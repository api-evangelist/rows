---
name: Write data to a Rows table
description: Locate a spreadsheet and table, then append rows or overwrite a cell range in the Rows API.
api: openapi/rows-openapi.yml
operations: [listSpreadsheets, getSpreadsheet, appendValues, overwriteCells]
auth: Bearer API key (Authorization: Bearer <API_KEY>)
base_url: https://api.rows.com/v1
---

# Write data to a Rows table

Use this skill to push data into a Rows spreadsheet table.

## Auth
Send every request with `Authorization: Bearer <API_KEY>`. Generate the key in
Rows account settings. All calls are on `https://api.rows.com/v1`.

## Steps
1. **Find the spreadsheet.** Call `listSpreadsheets` (`GET /spreadsheets`,
   optionally `?folder_id=<id>`) and pick the target `spreadsheetId`.
2. **Find the table.** Call `getSpreadsheet` (`GET /spreadsheets/{spreadsheetId}`)
   and read the tables it returns to get the `tableId`.
3. **Choose the write mode:**
   - **Append rows** — `appendValues`
     (`POST /spreadsheets/{spreadsheetId}/tables/{tableId}/values/{range}:append`).
     Body: `{ "values": [["a","b"], ["c","d"]] }`. Use an A1 range like `A1:B1000`.
   - **Overwrite a range** — `overwriteCells`
     (`POST /spreadsheets/{spreadsheetId}/tables/{tableId}/cells/{range}`).
     Body: `{ "cells": [[{"value":"a"},{"value":"b"}]] }`.

## Rules
- Ranges use spreadsheet **A1 notation** (e.g. `A1:B1000`).
- `values` is an array of string rows; `cells` is an array of rows of `{ value }` objects.
- Writes are **not documented as idempotent** — do not blindly retry a failed
  append without checking the table, or you may duplicate rows.
- `401` means a missing/invalid key; `404` means the spreadsheet or table id is wrong.
- Respect your plan's monthly API-call quota (see pricing).
