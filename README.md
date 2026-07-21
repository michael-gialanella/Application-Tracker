# ApplyFlow

A browser-based job-application tracker that imports relevant application emails from Microsoft Outlook. It supports manual entries, status tracking, search and filters, an application funnel, editable records, and direct links back to source emails.

## Files

- `application-tracker.html` — the complete application. It is intentionally a single self-contained file: HTML, CSS, and JavaScript.
- `application-tracker-template.html` — a shareable copy with no Entra client ID. Replace `PASTE_YOUR_ENTRA_APPLICATION_CLIENT_ID_HERE` with the client ID from your own app registration.
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

## Publish with GitHub Pages

Create a public GitHub repository, then run:

```powershell
git init
git add application-tracker.html README.md
git commit -m "Build Outlook application tracker"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/applyflow.git
git push -u origin main
```

In the GitHub repository, open **Settings → Pages**, choose **Deploy from a branch**, select `main` and `/ (root)`, then save.

The published URL is normally:

```text
https://YOUR-USERNAME.github.io/applyflow/application-tracker.html
```

Add that exact URL as another **Single-page application** redirect URI in the Entra app registration. Redirect URIs must match exactly.

## How the importer works

The app reads recent Outlook messages through Microsoft Graph and identifies confirmation, interview, offer, and rejection language. It includes specific handling for LinkedIn confirmations sent by `jobs-noreply@linkedin.com`, extracting the company and job title from the email.

Imported applications can always be edited manually in the tracker. Use **Reset Outlook imports** to delete only Outlook-imported entries and rescan; manual entries stay intact.

## Security notes

- The Entra application client ID is public by design. It is not a password.
- Never place OpenAI, Microsoft client-secret, GitHub, or other private API keys in this repository.
- Any visitor can view the public source code. Outlook data remains in the signed-in visitor's own browser and is fetched only after that visitor signs in.

## Resume-project description

> Built a client-side job application tracker with Microsoft Outlook / Graph API integration, application-status workflow, local persistence, and responsive dark-mode UI. Implemented email classification and LinkedIn application confirmation parsing to turn inbox activity into actionable application records.
