# Data Handling and Participant Notice

## Current behavior

The page does not currently show an independent notice or consent screen. When a participant presses **Start Speed Test**, it immediately calls `navigator.geolocation.getCurrentPosition()` with high accuracy requested and no cached position accepted. The browser then displays its own permission prompt when applicable. If permission is granted, the page displays latitude, longitude, and accuracy and submits a JSON object to its configured Apps Script endpoint.

The submitted fields are timestamp, latitude, longitude, accuracy, altitude, speed, and a map URL. Some browser location fields can be `null`; the current page substitutes `N/A` for missing altitude and speed. The speed-test figures on screen are random demo values and are not network measurements.

The sample Apps Script writes submitted values to a Google Sheet. The repository does not define a retention period, access roster, deletion process, or automated cleanup. Sheet copies, exports, and platform logs may also exist depending on the operator's configuration.

## Participant notice checklist

Before a real exercise, provide a notice before the browser asks for location. State plainly:

1. The organization and exercise owner.
2. The purpose and authorization for the exercise.
3. Which browser/device data may be collected, especially precise coordinates.
4. The Apps Script / Google Sheet destination and authorized viewers.
5. The retention deadline, deletion contact, and backup handling.
6. Whether participation is optional, how to decline, and whom to contact.

A browser permission prompt only controls access to a browser API. It does not explain the exercise, identify the data recipient, or define retention. The current code has no pre-permission notice, so the documentation must not imply otherwise.

## Operator checklist

- Obtain written approval and define a limited participant and time scope.
- Use a dedicated Sheet and least-privilege Google account.
- Limit Sheet and Apps Script access to named operators.
- Set a deletion date before launch and assign an owner to execute it.
- Avoid copying coordinates into tickets, chats, or unrelated systems.
- Document any incident or accidental collection and follow organizational policy.
- Disable the deployment and remove access when the exercise ends.

This document describes existing behavior and operating responsibilities. It does not implement consent UI or technical retention controls.
