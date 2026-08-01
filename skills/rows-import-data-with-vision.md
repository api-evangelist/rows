---
name: Import data from files with Rows AI Vision
description: Extract structured tabular data from image/document files and import it into a Rows spreadsheet.
api: openapi/rows-openapi.yml
operations: [listFolders, importVisionData]
auth: Bearer API key (Authorization: Bearer <API_KEY>)
base_url: https://api.rows.com/v1
---

# Import data from files with Rows AI Vision

Use this skill to turn images/documents (receipts, tables, screenshots) into
structured rows via Rows AI Vision.

## Auth
Send `Authorization: Bearer <API_KEY>` on every request. Base URL
`https://api.rows.com/v1`.

## Steps
1. **(Optional) Pick a destination folder.** Call `listFolders`
   (`GET /folders`) to get a `folder_id` if you want the import placed in a folder.
2. **Import the file(s).** Call `importVisionData`
   (`POST /vision/import`) as **multipart/form-data**:
   - `files` — one or more binary files (required).
   - `folder_id` — target folder (optional).
   - `app_id` — target spreadsheet id (optional; required if `table_id` is set).
   - `table_id` — target table id (optional).
   - `mode` — import mode.
   - `merge` — boolean, merge into existing data.
   - `instructions` — free-text extraction guidance for the AI.

## Rules
- Send as `multipart/form-data`; the file field is `files` (repeat for many files).
- If you set `table_id`, you must also set `app_id`.
- `401` means a missing/invalid API key.
- Vision imports consume AI Tasks and count against your plan quota.
