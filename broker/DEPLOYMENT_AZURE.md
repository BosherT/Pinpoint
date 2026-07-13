# Azure App Service Deployment Notes

These notes are specific to deploying the hosted broker to Azure App Service. The provider-neutral checklist is in [DEPLOYMENT.md](DEPLOYMENT.md).

## Runtime

Use Node.js 20 or newer.

The broker starts with:

```text
npm start
```

which runs:

```text
node src/server.js
```

## App Settings

Set production configuration in Azure App Service application settings:

```text
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

Do not commit production values to the repo.

## Database

Use Azure Database for PostgreSQL or another managed PostgreSQL database reachable by the App Service.

Run the migration before production testing:

```text
npm run db:migrate
```

Check database mode:

```text
npm run db:status
```

The broker should report Postgres mode. Memory mode is only acceptable for temporary local development.

## OAuth Callback

Add this callback URL in the Atlassian Developer Console:

```text
https://your-app-service-url/auth/callback
```

The value must exactly match `ATLASSIAN_REDIRECT_URI`.

## Health Check

Use:

```text
https://your-app-service-url/health
```

This should be available without Jira being connected.

## Production Notes

- Keep `TOKEN_ENCRYPTION_KEY` stable across deployments.
- Rotate Atlassian secrets only with a planned reconnect window.
- Set `SESSION_COOKIE_SECURE=true` for HTTPS.
- Set `CORS_ORIGIN` to the final extension origin once the extension ID is known.
- Keep the database when rolling the app back so users do not need to reconnect unnecessarily.
