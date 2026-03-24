# Gong Transcript Analysis — Reference

## Snowflake Schema

**Database:** `PROCORE_IT`
**Schema:** `GONG_DATA_CLOUD_PREP`
**Warehouse:** `PNT_PRODUCT_MGMT_WH`

### Views

#### `GONG_CALL_TRANSCRIPTS_PREP`

Primary transcript data. ~852K+ rows.

| Column | Type | Notes |
|--------|------|-------|
| `CONVERSATION_KEY` | VARCHAR | FK to conversations |
| `TRANSCRIPT` | VARIANT (JSON) | Full transcript: array of speaker turns with sentences, topics, timestamps |

The `TRANSCRIPT` column is a JSON array. To search within it, cast to string:

```sql
LOWER(t.TRANSCRIPT::STRING) ILIKE '%search term%'
```

#### `GONG_CONVERSATIONS_PREP`

Call-level metadata.

| Column | Type | Notes |
|--------|------|-------|
| `CONVERSATION_KEY` | VARCHAR | PK |
| `STARTED_DATE` | TIMESTAMP | When the call began |
| `CONVERSATION_TYPE` | VARCHAR | e.g. "Meeting", "Call" |

#### `GONG_CONVERSATION_PARTICIPANTS_PREP`

Who was on each call.

| Column | Type | Notes |
|--------|------|-------|
| `CONVERSATION_KEY` | VARCHAR | FK |
| `PARTICIPANT_NAME` | VARCHAR | Display name |
| `AFFILIATION` | VARCHAR | Company or "Internal" |
| `TITLE` | VARCHAR | Job title |
| `EMAIL` | VARCHAR | Email address |

---

## SQL Templates

### Set warehouse (always run first)

```sql
USE WAREHOUSE PNT_PRODUCT_MGMT_WH;
```

### Discovery: count matching calls

```sql
SELECT COUNT(DISTINCT c.CONVERSATION_KEY) AS call_count,
       MIN(c.STARTED_DATE) AS earliest,
       MAX(c.STARTED_DATE) AS latest
FROM PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CALL_TRANSCRIPTS_PREP t
JOIN PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CONVERSATIONS_PREP c
  ON t.CONVERSATION_KEY = c.CONVERSATION_KEY
WHERE (
    LOWER(t.TRANSCRIPT::STRING) ILIKE '%{{PRIMARY_TERM_1}}%'
    OR LOWER(t.TRANSCRIPT::STRING) ILIKE '%{{PRIMARY_TERM_2}}%'
    OR (LOWER(t.TRANSCRIPT::STRING) ILIKE '%{{GENERIC_TERM}}%'
        AND LOWER(t.TRANSCRIPT::STRING) ILIKE '%{{QUALIFIER}}%')
  )
  AND c.STARTED_DATE >= DATEADD('day', -{{DAYS}}, CURRENT_DATE());
```

### Discovery: list matching calls with dates

```sql
SELECT DISTINCT c.CONVERSATION_KEY, c.STARTED_DATE
FROM PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CALL_TRANSCRIPTS_PREP t
JOIN PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CONVERSATIONS_PREP c
  ON t.CONVERSATION_KEY = c.CONVERSATION_KEY
WHERE (
    LOWER(t.TRANSCRIPT::STRING) ILIKE '%{{PRIMARY_TERM_1}}%'
    OR LOWER(t.TRANSCRIPT::STRING) ILIKE '%{{PRIMARY_TERM_2}}%'
    OR (LOWER(t.TRANSCRIPT::STRING) ILIKE '%{{GENERIC_TERM}}%'
        AND LOWER(t.TRANSCRIPT::STRING) ILIKE '%{{QUALIFIER}}%')
  )
  AND c.STARTED_DATE >= DATEADD('day', -{{DAYS}}, CURRENT_DATE())
ORDER BY c.STARTED_DATE DESC;
```

### Pull full transcripts (batch by conversation keys)

```sql
SELECT t.CONVERSATION_KEY, c.STARTED_DATE, t.TRANSCRIPT
FROM PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CALL_TRANSCRIPTS_PREP t
JOIN PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CONVERSATIONS_PREP c
  ON t.CONVERSATION_KEY = c.CONVERSATION_KEY
WHERE t.CONVERSATION_KEY IN ('{{KEY_1}}', '{{KEY_2}}', '{{KEY_3}}')
ORDER BY c.STARTED_DATE DESC;
```

### Pull participants for attribution

```sql
SELECT CONVERSATION_KEY, PARTICIPANT_NAME, AFFILIATION, TITLE
FROM PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CONVERSATION_PARTICIPANTS_PREP
WHERE CONVERSATION_KEY IN ('{{KEY_1}}', '{{KEY_2}}', '{{KEY_3}}')
ORDER BY CONVERSATION_KEY, AFFILIATION;
```

---

## Search Term Design Patterns

### Product with a unique name

Products like "Timesheets", "Submittal Log", "Drawing Management" are specific
enough to search directly:

```sql
LOWER(t.TRANSCRIPT::STRING) ILIKE '%timesheets%'
```

### Product with a generic name

Products like "Pages", "Tasks", "Forms" are too common. Always pair with a
qualifier that indicates the Procore feature context:

```sql
(LOWER(t.TRANSCRIPT::STRING) ILIKE '%pages%'
 AND (LOWER(t.TRANSCRIPT::STRING) ILIKE '%data grid%'
      OR LOWER(t.TRANSCRIPT::STRING) ILIKE '%data canvas%'
      OR LOWER(t.TRANSCRIPT::STRING) ILIKE '%data connectors%'
      OR LOWER(t.TRANSCRIPT::STRING) ILIKE '%embedded chart%'))
```

### Alternate spellings and phrasings

Always check for:
- Hyphenated vs. space-separated: `"dataset editor"` / `"data set editor"`
- Plural vs. singular: `"timesheet"` / `"timesheets"`
- Abbreviations: `"RFI"` / `"request for information"`
- Feature sub-components: `"ai generated column"` / `"ai column"`

---

## Confluence Publishing Reference

### Procore Atlassian Cloud ID

```
cdf4443f-e3ed-4a9e-9567-bb704cf01f1f
```

### Finding spaceId from a folder URL

Folder IDs sometimes return 404 from `getConfluencePage`. Workaround:

1. Search for children: `searchConfluenceUsingCql` with
   `cql: "ancestor = {{FOLDER_ID}}"`
2. Get any child page: `getConfluencePage` with one of the result page IDs
3. Read `spaceId` from that child page response
4. Use the folder ID as `parentId` when creating — `createConfluencePage`
   accepts folder IDs even though `getConfluencePage` doesn't

### Creating the page

```json
{
  "cloudId": "cdf4443f-e3ed-4a9e-9567-bb704cf01f1f",
  "spaceId": "{{SPACE_ID}}",
  "title": "{{PRODUCT}}: {{ANALYSIS_TYPE}}",
  "parentId": "{{PARENT_ID}}",
  "body": "{{MARKDOWN_CONTENT}}",
  "contentFormat": "markdown"
}
```

---

## Transcript JSON Structure

Each `TRANSCRIPT` value is a JSON array of speaker segments:

```json
[
  {
    "speakerId": "12345",
    "topic": "Dataset Editor walkthrough",
    "sentences": [
      {
        "start": 120.5,
        "end": 125.3,
        "text": "The sync never fully synced for us."
      }
    ]
  }
]
```

When extracting quotes, use the `text` field from `sentences` and attribute
via participant metadata joined on `speakerId`.
