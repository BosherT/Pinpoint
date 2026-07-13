# Local Development

These steps run the extension and hosted broker locally for development and testing.

## Prerequisites

- Git for Windows
- Node.js 20 or newer
- Docker Desktop
- Chrome or Microsoft Edge
- Atlassian OAuth app configured for local testing

## Broker Setup

From the repo root:

```powershell
cd "C:\Users\User\Documents\Codex\QA-Capture-Tool\broker"
npm.cmd install
Copy-Item .env.example .env
```

Edit `broker\.env` and set:

```text
PUBLIC_BASE_URL=http://127.0.0.1:8788
ATLASSIAN_CLIENT_ID=your-client-id
ATLASSIAN_CLIENT_SECRET=your-client-secret
ATLASSIAN_REDIRECT_URI=http://127.0.0.1:8788/auth/callback
DATABASE_URL=postgres://qa_capture:qa_capture@localhost:5432/qa_capture
SESSION_COOKIE_SECURE=false
```

Generate a token encryption key:

```powershell
npm.cmd run token-key:generate
```

Copy the generated value into `.env` as `TOKEN_ENCRYPTION_KEY`.

Start Postgres:

```powershell
docker compose up -d
```

Run the migration:

```powershell
npm.cmd run db:migrate
npm.cmd run db:status
```

Start the broker:

```powershell
npm.cmd start
```

Open:

```text
http://127.0.0.1:8788
```

## Atlassian OAuth Local Callback

In the Atlassian Developer Console, the local callback URL should be:

```text
http://127.0.0.1:8788/auth/callback
```

## Extension Setup

1. Open Chrome or Edge extensions.
2. Enable Developer mode.
3. Choose Load unpacked.
4. Select:

   ```text
   C:\Users\User\Documents\Codex\QA-Capture-Tool\extension
   ```

5. Open the extension settings.
6. Set Connector URL to:

   ```text
   http://127.0.0.1:8788
   ```

7. Use Connect Jira.
8. Create a test issue in a safe Jira project.

## Quick Verification

Run these from the repo root:

```powershell
node --check .\extension\capture.js
node --check .\extension\options.js
node --check .\broker\src\server.js
node --check .\broker\src\services\jiraClient.js
node --check .\broker\src\services\tokenStore.js
```

The broker health page should also show:

```text
connected: true
```

after Jira has been connected.
