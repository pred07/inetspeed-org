# Internet Speed Checker | Authorized Awareness Lab

A small static browser demo for **authorized, opt-in security-awareness exercises**. It combines a lightweight HTML page, the browser's native geolocation permission prompt, and a Google Apps Script web app that appends submitted location fields to a Google Sheet.

> **Important: the current page is a prototype.** The “speed test” readings are randomized demo values; this repository does not measure real network performance. Clicking **Start Speed Test** immediately requests precise browser location. The page has no separate consent or privacy notice before that request. A browser permission prompt is not a substitute for informed consent. Do not deploy this to participants until they have received a clear notice and the exercise has been approved by the system owner and privacy/security stakeholders.

## What it does

- Runs as a static HTML page in a modern browser; no local agent, browser extension, tunnel, proxy, or build step is required.
- Uses the browser Geolocation API after the participant clicks the start button and responds to the browser permission prompt.
- Displays latitude, longitude, and reported accuracy in the page.
- Sends a timestamp, latitude, longitude, accuracy, altitude, speed, and a Google Maps URL to a configured Apps Script endpoint. The endpoint example in the original setup appends these fields to a Google Sheet.
- Animates **simulated** ping, download, and upload values. These numbers are generated randomly and are not measurements.

The static HTML page is about 8.5 KB and needs no package install, build pipeline, or local service; the decorative GIF is about 1.1 MB. Location access requires a supported secure context, and logging depends on the configured Google endpoint and spreadsheet.

## Architecture

```text
Participant browser
  ├─ Static page (index.html + speed.gif)
  ├─ Native geolocation permission and coordinates
  └─ HTTPS POST (no-cors) ──> Google Apps Script web app
                                  └─> Google Sheet
```

There is no tunnel or inbound connection to a participant's device. The browser makes an outbound request to the configured Apps Script URL. This simplifies deployment compared with a tunnel-based local service, but it does **not** make the activity invisible or bypass security monitoring. See [Defender and monitoring notes](docs/DEFENDER_AND_MONITORING.md).

## Deployment overview

1. Host the static page on an HTTPS origin, such as GitHub Pages.
2. Create a dedicated Google Sheet and Apps Script project for the exercise.
3. Review [Apps Script setup](docs/APPS_SCRIPT_SETUP.md) and [data handling](docs/DATA_HANDLING.md) before deploying the web app.
4. Confirm the participant notice, approval, collection scope, access list, retention period, and deletion owner with the organization.
5. Configure the page to call the intended endpoint and test only with synthetic data and consenting testers.
6. Restrict access to the Sheet and Apps Script project; do not use a personal or production spreadsheet.

The current browser request uses `no-cors`, so the page cannot read the endpoint's response or reliably confirm that a row was stored. A resolved `fetch()` is not proof of successful collection. The Apps Script source is not included in this repository, so its authentication, validation, rate limits, and retention behavior cannot be verified here. The previous quick-start sample showed none of those controls. Review the deployed Apps Script project before use; do not treat this repository as evidence that the live endpoint is protected.

## Privacy and authorized use

Use only in a controlled exercise with written authorization and an advance notice that states:

- who is running the exercise and why;
- that precise device location may be requested and recorded;
- the fields collected and where they are stored;
- who can access the Sheet and Apps Script project;
- how long records are retained and how deletion requests are handled;
- how participants can decline or ask questions.

Do not collect location covertly, use the tool to track people outside the approved scope, or reuse records for unrelated purposes. The repository contains no consent screen, retention scheduler, or deletion workflow. Because the Apps Script source is absent, server-side authentication cannot be assessed from this repository. The organization operating the exercise must define and carry out retention and deletion manually.

## Security and monitoring posture

This is a transparent browser-to-cloud demo, not a stealth tool. It can produce observable browser geolocation permission events, requests to the hosted page and Apps Script endpoint, Apps Script executions, and spreadsheet changes. Monitoring coverage depends on the browser, Google Workspace configuration, endpoint controls, and the organization's SIEM/EDR telemetry. Do not claim that it avoids, defeats, or is undetectable to SIEM or EDR. See [Defender and monitoring notes](docs/DEFENDER_AND_MONITORING.md).

## Repository contents

- `index.html` — static interface and browser-side behavior
- `speed.gif` — decorative animated asset
- `docs/APPS_SCRIPT_SETUP.md` — collector setup and deployment notes
- `docs/DATA_HANDLING.md` — collection, access, retention, and deletion checklist
- `docs/DEFENDER_AND_MONITORING.md` — observable events and safe validation guidance

## License

The repository currently describes itself as MIT licensed, but a `LICENSE` file is not present. Do not assume the code is formally licensed for reuse until the owner adds the license text.

## Project status

Prototype / educational demonstration. Not a real speed-test service, a production telemetry platform, a consent-management system, or a security control. Review the implementation and obtain organizational approval before any deployment.
