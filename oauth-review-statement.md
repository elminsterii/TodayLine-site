# TodayLine – Google OAuth Verification Review Statement

**Application Name:** TodayLine  
**Application Type:** Desktop Application (currently available on Windows; macOS planned)  
**Developer Contact:** support@todayline.app  
**Date:** April 10, 2026  

---

## 1. Application Overview

TodayLine is a lightweight desktop application that displays a user's Google Calendar events for the current day in a semi-transparent, always-on-top floating overlay window. The purpose of the application is to provide users with a quick, at-a-glance view of their daily schedule without requiring them to open a browser or switch applications.

---

## 2. Requested OAuth Scope

| Scope | Type | Purpose |
|---|---|---|
| `https://www.googleapis.com/auth/calendar.readonly` | Read-only | Retrieve today's calendar events for local display |

TodayLine requests only **one** OAuth scope. No write, edit, delete, or administrative scopes are requested. The application adheres strictly to the principle of least privilege.

---

## 3. Why This Scope Is Required

The `calendar.readonly` scope is the minimum permission necessary to fulfill TodayLine's sole function: **reading calendar events to display them on the user's desktop**.

Without this scope, TodayLine cannot retrieve event data from the Google Calendar API and therefore cannot perform its core functionality. A narrower scope (e.g., `calendar.events.readonly`) was evaluated; however, `calendar.readonly` was selected to ensure compatibility with calendar list retrieval, which is required to allow users to choose which calendars to display.

The application does **not** request any of the following scopes, as they are unnecessary:

- `calendar` (read/write) — TodayLine never modifies calendar data.
- `calendar.events` (read/write) — TodayLine never creates, updates, or deletes events.
- Any Drive, Gmail, or other Google service scope — TodayLine interacts only with Google Calendar.

---

## 4. How User Data Is Accessed and Used

### 4.1 Data Access Flow

1. The user launches TodayLine and signs in via the standard Google OAuth 2.0 authorization flow.
2. Upon granting permission, TodayLine calls the **Google Calendar API** (`calendar.events.list`) to retrieve the user's calendar events for the current day only (from midnight to 23:59:59 local time).
3. The retrieved events are rendered in the TodayLine overlay window as a visual timeline.
4. API calls are repeated at a configurable interval (default: every 5 minutes) to refresh the display with any newly added or updated events.

### 4.2 Scope of Data Used

TodayLine reads and displays the following fields from each calendar event:

- Event title (summary)
- Start and end date/time
- Calendar color (for visual grouping)

TodayLine does **not** read or display: event descriptions, attendee lists, conference links, attachments, recurrence rules, or any other metadata beyond what is necessary to render the timeline.

### 4.3 Local Processing Only

All data processing occurs **entirely on the user's local device**. Calendar data is:

- Fetched directly from the Google Calendar API over HTTPS.
- Processed in memory within the running application.
- Rendered in the on-screen overlay window.
- **Never written to disk** (beyond the OS secure credential storage for the OAuth token).
- **Never transmitted** to any server, database, analytics service, or third party.

---

## 5. Data Storage and Retention

| Data Type | Storage Location | Retention |
|---|---|---|
| OAuth Access Token | OS credential storage (local, encrypted) | Until user signs out or revokes access |
| OAuth Refresh Token | OS credential storage (local, encrypted) | Until user signs out or revokes access |
| Calendar Event Data | RAM only (in-process memory) | Cleared when app is closed or data is refreshed |
| User Identity (email) | Not stored | Never persisted |

TodayLine maintains **no persistent database** of any kind. There is no server-side component, no backend infrastructure, and no cloud storage associated with TodayLine.

---

## 6. Data Sharing

TodayLine does **not** share, sell, transfer, or disclose any user data — including Google Calendar data — to any third party for any purpose. This includes:

- No advertising networks or analytics platforms.
- No crash reporting services that include user data (if crash reporting is implemented in future, it will be opt-in and anonymized).
- No other applications, services, or individuals.

---

## 7. Compliance with Google API Services User Data Policy

TodayLine's use of Google user data complies with the [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy), including the **Limited Use** requirements:

- ✅ Data is used only to provide the user-facing feature described in this statement.
- ✅ Data is not transferred to third parties except as necessary to provide the app's feature.
- ✅ Data is not used for serving advertisements.
- ✅ Data is not used for determining creditworthiness or for lending purposes.
- ✅ Humans do not read user data unless required for security purposes, to comply with applicable law, or with the user's explicit permission.

---

## 8. Security Measures

- All API communication uses **HTTPS/TLS**.
- OAuth tokens are stored using the **operating system's secure credential storage** (e.g., Windows Credential Manager via DPAPI, or macOS Keychain), which encrypts them with keys tied to the user's OS account.
- The application does not log API responses or event data to any file or console output in production builds.
- The application undergoes code review before each release to ensure no accidental data leakage.

---

## 9. User Control

Users have full control over TodayLine's access to their Google account:

- Users can **sign out** from within the application at any time, which immediately clears all stored tokens.
- Users can **revoke access** at any time via [Google Account Permissions](https://myaccount.google.com/permissions).
- After revocation, TodayLine cannot access any Google data until the user re-authorizes.

---

## 10. Supporting Documents

| Document | Description | Location |
|---|---|---|
| Privacy Policy | Full data handling practices | `privacy-policy.html` |
| Terms of Service | User agreement | `terms-of-service.html` |
| OAuth Review Statement | This document | `oauth-review-statement.md` |

---

*This statement is accurate as of the date listed above and will be updated if the application's data practices change.*
