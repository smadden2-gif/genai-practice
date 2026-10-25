# Context Bridge

**Context Bridge helps AI use the work and personal context you approve, across the tools where your life is split.**

Your AI starts from zero in each tool, while the context it needs is scattered across work and personal systems. Context Bridge is a permission-first way to carry the right context to a task while keeping the boundary visible.

- [Open the live Context Bridge prototype](https://smadden2-gif.github.io/genai-practice/)
- [View the public GitHub repository](https://github.com/smadden2-gif/genai-practice)

## The first slice

The page centers on one simulated flow: enter a work question, add optional personal context, explicitly approve whether that context may be considered, and optionally write a separate note to keep private. The result shows what personal context was used and what stayed out. Approval is unchecked by default.

## AI role and transparency

The prototype does not call an AI model. Its response is a fixed browser-side template that includes the work question and, only if the approval box is checked, the personal context. The separate private-note field is never rendered into the result. The simulation has no backend, API key, storage, or network transfer of entered text. The separate real-calendar section is currently unavailable and sends no requests. When configured, it will require internet access and send only approved availability parameters and necessary OAuth authorization to Google. Work-question text and private notes are never sent. This tests the permission and audit interaction, not the quality of an AI-generated answer or a real cross-tool integration.

Note: The assignment suggested Replit, but I did not have access, so I built this with AI-assisted coding (Instinct's agent) and deployed it on GitHub Pages instead.

## Quick tests

These three cases were run against the live prototype after the UI redesign.

### Typical: approved context

**Input.** Work: "I could lead a launch while keeping weekly family commitments."
Approved context: "I have a standing appointment on Thursday evenings."
Private note: "Family health details I will not share here."
Approval was checked.

**Expected.** The work question and approved context appear in the response.
Used lists the approved context. Kept out excludes the private note
without revealing its text.

**Actual. Pass.** The response included the work question and approved
context. Used showed the Thursday commitment. Kept out said the note
stayed out without showing its text.

### Challenge: approval left unchecked

**Input.** Work: "Should I accept a role with frequent travel?"
Personal context: "I may need to care for my parent."
Private note: "Do not use this." Approval was left unchecked.

**Expected.** No personal context is used. Used says none was used.
The private note stays out.

**Actual. Pass.** The no-context response included the work question.
Used said no personal context was used. Kept out described the private
note without exposing it.

### Empty work question

**Input.** Reload the page to start fresh, leave required work input
empty, and submit.

**Expected.** Browser validation blocks submission and the result stays
hidden.

**Actual. Pass.** The browser displayed "Please fill out this field."
The result remained hidden.

## Known limitations

- The response is a fixed template. It does not understand the work question or reason over context. Next: test with people, then evaluate a model using synthetic examples and a clear boundary for approved input.
- The demo does not save decisions or remember permission between visits. Next: define user controls and deletion before considering storage.
- The simulation does not connect to other tools or move context between them. The separate Calendar section is disabled pending secure authorization and a verified primary-calendar time zone. Next: test the permission model before adding an integration, and show exactly what data an integration would send.

## Run locally

Clone the repository and open `index.html` in a browser. No build step or server is required. The page uses no external JavaScript. Its existing design requests a display font from Google Fonts; that request is separate from OAuth and Calendar and sends no form input.

```bash
git clone https://github.com/smadden2-gif/genai-practice.git
cd genai-practice
open index.html
```


## 1. real-calendar slice status

**The Calendar section is deliberately nonfunctional until its security prerequisites are met.**

- The existing simulation still works independently.
- Calendar approval and the action are disabled. No Google scripts, OAuth flows or Calendar requests run.
- Pure window calculation, approval gating, request construction, response validation, outcome and receipt helpers are in `index.html`. The request helper is not wired to the UI.
- No token, client secret, API key, consent or result is persisted. Receipts are rendered only.
- `calendar.freebusy` is accepted by Google's published FreeBusy API discovery contract. A live grant and request have **not** been verified.

What prevents the Calendar action from starting?

## 2. Google Cloud configuration

**A client ID alone cannot activate this static page.**

1. Open Google Cloud Console and create or select a project.
2. Under APIs & Services → Library, enable Google Calendar API.
3. In Google Auth Platform, configure Branding, Audience and Data Access. For an external testing app, add the intended Google account as a test user.
4. Request only `https://www.googleapis.com/auth/calendar.freebusy`. Do not add `calendar.readonly`, event scopes or other calendar scopes. Existing OAuth permission does not approve a query.
5. Under Clients, create a Web application OAuth client. Register `https://smadden2-gif.github.io` as an authorized JavaScript origin. For local testing, register the exact local HTTP origin, such as `http://localhost:8000`.
6. A supported GIS authorization-code architecture needs a secure backend exchange. Choose and verify a supported PKCE design before implementation. Configure its exact HTTPS callback as an authorized redirect URI if the chosen flow uses redirects. No callback URL exists in this repository; do not point a callback at the static page and assume exchange works.
7. Keep any backend client secret exclusively in server-side secret configuration. Keep tokens and codes transient, out of URLs, logs and persistent browser storage. Never commit credentials. A public OAuth client ID is configuration, not a secret.
8. Establish an authoritative time zone for the signed-in user's primary calendar **before** query approval. FreeBusy cannot discover this setting. `calendars.get` does not accept the required narrow scope, so do not add it or broaden scopes. A separately approved design for obtaining and binding this metadata is still required.
9. Once both architectural prerequisites are resolved, implement and verify the authorization adapter and orchestration before changing the capability gate. Do not enable the button merely because a client ID is present.

OAuth authorization traffic is separate from the single Calendar POST. The eventual request body contains only `items: [{"id":"primary"}]`, `timeMin`, `timeMax` and `timeZone`. Its window is 15:00 today through next local midnight, exclusive, even when the query runs after 15:00.

Which two prerequisites remain after creating a Google OAuth client?

## 3. validation and future activation

**Local tests exercise isolated helpers; they do not establish live Google access.**

Run a static server from the checkout:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

Run the browser checks with Node.js, Playwright and Chromium available:

```bash
node tests/calendar.cjs
```

The test uses the server above and defaults to `/usr/bin/chromium`. Set `CHROMIUM_PATH` if Chromium is elsewhere. No framework or build step is needed for the application. Playwright is a development test tool only; it is preinstalled in the cloud environment.

Before live activation, verify GIS code flow with PKCE, the exact scope grant, account-bound primary-calendar time zone, zero Google activity without per-query approval, one Calendar POST after approval, no private fields in its body, every error path, consent reset, receipt accuracy and desktop/phone keyboard behavior. Do not use simulated success in the application or silently retry.

What do local fixture tests leave unverified?
