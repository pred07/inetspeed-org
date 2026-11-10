# Apps Script Setup and Operations

This document describes how to review and operate a Google Apps Script collector for this demo. The `.gs` source is not present in this repository, so the deployed endpoint's behavior must be verified in its Apps Script project. This guide is not a claim that the current endpoint is production-ready.

## Data flow

The browser posts JSON to a configured Apps Script web-app URL. The previous README example parsed `e.postData.contents`, opened a spreadsheet by ID, and appended a row containing:

- submission timestamp;
- latitude and longitude;
- reported location accuracy;
- altitude and speed when available;
- a Google Maps URL derived from the coordinates.

The browser uses `fetch(..., { mode: 'no-cors' })`; it cannot inspect the web app's response. Do not treat a successful browser promise as confirmation that a row was written.

## Create an isolated collector

1. Create a new Google Sheet specifically for an approved exercise. Avoid a personal or production sheet.
2. Create an Apps Script project from that Sheet (Extensions → Apps Script), or create a standalone project and explicitly configure the spreadsheet ID.
3. Use a dedicated sheet tab with a neutral name such as `exercise_submissions`; the historical `locphish` name in older setup instructions is not required by the page.
4. Review the collector's permissions and the Google account under which it executes.
5. Set a named owner for access reviews, retention, deletion, and incident response.

## Deploy carefully

Apps Script web-app access settings determine who can invoke the endpoint. The previous README suggested **Execute as: Me** and **Who has access: Anyone**. With those settings, the deployed URL is publicly callable and can write to the owner's spreadsheet. Do not use them with sensitive data or an uncontrolled audience. The repository does not include the deployed script source or its current access configuration; inspect both directly. If the exercise requires a publicly reachable URL, get an explicit security and privacy review, use a dedicated low-privilege account and isolated Sheet, and monitor usage.

A token embedded in `index.html` is not a meaningful secret: anyone who can load the page can inspect its JavaScript and copy the token. Stronger controls must be enforced server-side and reviewed for the intended deployment model. Consider authentication, strict field/type/range validation, request-size limits, abuse controls, and a tested deletion process before using real participant data. The previous sample did not implement these protections; the live deployment must be reviewed independently.

## Configure the page

The current `index.html` contains a `SCRIPT_URL` constant. It must point to the reviewed deployment URL for the intended exercise. Keep configuration changes scoped to the correct deployment, and avoid publishing an endpoint that writes to a personal spreadsheet. Do not paste credentials, private keys, or secret tokens into the page or repository.

## Test without personal data

- Use a test Sheet with synthetic records and a small, explicitly approved test group.
- Verify the headers, field order, timestamps, and access permissions.
- Test malformed and missing fields against the server-side handler before real use.
- Confirm the configured retention period and practice deletion of a test row and its backups.
- Check Apps Script execution history and Sheet version/activity history after each test.
- Remember that the browser's `no-cors` request does not expose the collector's response to the page.

## Operations and offboarding

At the end of the exercise, disable or remove the web-app deployment, revoke unnecessary editor access, export or retain only records required by the approved policy, delete expired rows and copies, and record who completed the cleanup. The current project does not automate these tasks.
