# Azure App Service Prep

This guide lists what needs to be ready before deploying the hosted broker to Azure App Service.

## Target Shape

- Azure App Service running the Node.js broker.
- Azure Database for PostgreSQL storing OAuth connections.
- Azure App Settings storing all secrets and runtime configuration.
- Atlassian OAuth app using the Azure callback URL.
- Browser extension configured to use the Azure broker URL.

## Azure Resources Needed

- App Service plan suitable for a small internal tool.
- Linux or Windows App Service capable of running Node.js 20 or newer.
- PostgreSQL database.
- A way to set application settings/secrets.
- Outbound HTTPS access to Atlassian.
- HTTPS public URL for the broker.

## Required App Settings

Set these in Azure App Service configuration, not in a committed file:

```text
PORT=8080
PUBLIC_BASE_URL=https://your-app-service-url
ATLASSIAN_CLIENT_ID=...
ATLASSIAN_CLIENT_SECRET=...
ATLASSIAN_REDIRECT_URI=https://your-app-service-url/auth/callback
ATLASSIAN_SCOPES=read:jira-work write:jira-work read:jira-user offline_access
DATABASE_URL=postgres://...
TOKEN_ENCRYPTION_KEY=...
SESSION_COOKIE_NAME=qa_capture_session
SESSION_COOKIE_SECURE=true
CORS_ORIGIN=chrome-extension://your-real-extension-id
```

Use the App Service port expected by the Azure runtime. If Azure injects `PORT`, let the platform value win.

Generate `TOKEN_ENCRYPTION_KEY` locally with:

```powershell
cd "C:\Users\User\Documents\Codex\QA-Capture-Tool\broker"
npm.cmd run token-key:generate
```

Store the generated value in Azure App Settings.

## Database Migration

After the App Service can reach PostgreSQL, run:

```powershell
npm run db:migrate
```

Depending on the deployment process, this can be run from:

- Azure SSH/Kudu console.
- A one-off deployment job.
- A local machine with access to the production database.

## Atlassian OAuth Update

Add the production callback URL to the Atlassian OAuth app:

```text
https://your-app-service-url/auth/callback
```

Keep the local callback if local development still needs to work:

```text
http://127.0.0.1:8788/auth/callback
```

## Extension Update

Once Azure is live, update the extension Connector URL to:

```text
https://your-app-service-url
```

For early testing this can be changed manually in extension settings. Later, the extension default can be updated in code and packaged for users.

## Smoke Test

1. Open `https://your-app-service-url/health`.
2. Confirm the broker is using Postgres, not memory fallback.
3. Open `https://your-app-service-url`.
4. Connect Jira.
5. Confirm `/auth/status` returns connected.
6. Update the extension Connector URL to the Azure URL.
7. Create a test issue.
8. Restart the App Service.
9. Confirm the Jira connection still works.

## Rollback

The safest rollback is to point the extension Connector URL back to the previous working broker.

If the app deployment needs to roll back:

1. Redeploy the previous broker version.
2. Keep the PostgreSQL database intact.
3. Keep `TOKEN_ENCRYPTION_KEY` unchanged so existing encrypted tokens remain readable.
