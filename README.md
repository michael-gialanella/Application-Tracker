# ApplyFlow

A browser-based job-application tracker that imports relevant application emails from Microsoft Outlook. It supports manual entries, status tracking, search and filters, an application funnel, editable records, and direct links back to source emails.

## Files

- `application-tracker-template.html` — the complete application. It is intentionally a single self-contained file: HTML, CSS, and JavaScript.  a shareable copy with no Entra client ID. Replace `PASTE_YOUR_ENTRA_APPLICATION_CLIENT_ID_HERE` with the client ID from your own app registration.
- `start-applyflow.ps1` — optional local server launcher for Windows.

## Run locally

Open PowerShell in this folder and run:

```powershell
powershell -ExecutionPolicy Bypass -File .\start-applyflow.ps1
```

Then open `http://localhost:8080/application-tracker.html`.

The tracker stores applications in the browser's local storage. Clearing browser data removes locally saved applications.

## Connect Outlook

This project uses Microsoft Graph and the Microsoft Authentication Library (MSAL) in the browser.

1. Go to the Microsoft Entra admin center.
2. Open **App registrations** and create a new registration.
3. Choose **Accounts in any organizational directory and personal Microsoft accounts** if you want both work/school and personal Outlook accounts to work.
4. Under **Authentication**, add a **Single-page application** redirect URI:

   ```text
   http://localhost:8080/application-tracker.html
   ```

5. Under **API permissions**, add delegated Microsoft Graph permissions:

   - `User.Read`
   - `Mail.Read`

6. Copy the application (client) ID and replace the `clientId` value in `application-tracker.html` if using your own Entra app registration.

No client secret is needed or appropriate for this browser-only app.

