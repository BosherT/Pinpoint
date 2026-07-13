# Update Workflow

Use this workflow for normal changes after the first GitHub import.

## Start From Main

```powershell
cd "C:\Users\User\Documents\Codex\QA-Capture-Tool"
git switch main
git pull origin main
```

## Create A Branch

Use a short branch name that describes the change:

```powershell
git switch -c short-change-name
```

Examples:

```text
azure-deployment-prep
assignee-search-polish
extension-packaging
```

## Make And Check Changes

After editing, check what changed:

```powershell
git status --short
git diff
```

Run syntax checks when code changes:

```powershell
node --check .\extension\capture.js
node --check .\extension\options.js
node --check .\broker\src\server.js
node --check .\broker\src\services\jiraClient.js
node --check .\broker\src\services\tokenStore.js
```

For broker changes, also run:

```powershell
cd .\broker
npm.cmd run db:status
```

## Commit

```powershell
git add .
git commit -m "Describe the change"
```

Before committing, make sure no secrets are staged:

```powershell
git diff --cached --name-only
```

Do not commit:

- `.env`
- `.env.*`
- OAuth token files
- Jira API config files
- `node_modules/`
- logs

## Push And Pull Request

```powershell
git push -u origin short-change-name
```

Then open a pull request on GitHub and merge it after review.

## After Merge

```powershell
git switch main
git pull origin main
git branch -d short-change-name
```
