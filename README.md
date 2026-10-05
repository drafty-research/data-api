# data-api

Use this repository as a versioned store for observations collected from public
web apps and systems. Each record is a JSON file; Git commits provide the write
history, while GitHub's APIs and raw file URLs provide read access. There is no
separate database service.

## Record format

Store records at `data/<source>/<record-id>.json`. Use lowercase slugs for
`<source>` and stable identifiers for `<record-id>`. Each file contains an
object with collection metadata and the source data:

```json
{
  "source": "example-service",
  "id": "item-123",
  "observed_at": "2026-10-05T15:00:00Z",
  "source_url": "https://example.com/items/123",
  "data": {
    "status": "active"
  }
}
```

Keep `source`, `id`, `observed_at`, `source_url`, and `data` in this envelope;
the `data` object can follow the source system's own shape. Use UTC RFC 3339
timestamps for `observed_at`. Keep record IDs stable so later changes to a
record are visible as new versions in Git history.

## Writing records

Commit new or updated JSON files under `data/`, with commit messages that name
the source and describe the change. Records can be committed directly or
written with GitHub's Contents API (`PUT /repos/drafty-research/data-api/contents/{path}`).
The API requires a token with repository contents write access; when updating
an existing file, include its current `sha` in the request body.

## Reading records

- Fetch an individual file from
  `https://raw.githubusercontent.com/drafty-research/data-api/<branch>/data/<source>/<record-id>.json`.
- List or fetch files using GitHub's Contents API:
  `GET /repos/drafty-research/data-api/contents/data`.
- Use the Git commit history for a record's prior versions and provenance.

GitHub API rate limits apply. Only commit data that is permitted to be
redistributed, and do not store credentials or private information.
