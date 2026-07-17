# Pinpoint Smoke Test

Use this checklist after changes, extension reloads, broker updates, or environment changes.

## 1. Start The Broker

For local testing:

```powershell
cd "C:\Users\User\Documents\Codex\QA-Capture-Tool"
.\start-broker.bat
```

Confirm the broker starts without errors.

## 2. Check Settings

1. Open Pinpoint Settings.
2. Confirm Connector URL is correct.
3. Select Test connector.
4. Confirm:

   ```text
   Broker: Reachable
   Jira: Connected
   ```

5. If Jira is not connected, select Connect Jira and complete sign-in.

## 3. Create A Test Issue

1. Open a page that is safe to capture.
2. Select the Pinpoint extension.
3. Select Capture current tab.
4. Add one box annotation.
5. Enter a summary:

   ```text
   Pinpoint smoke test - please ignore
   ```

6. Select a safe test project.
7. Select issue type Task.
8. Add a short Description.
9. Leave Assignee as Unassigned unless testing assignment.
10. Select Create Jira Issue.

## 4. Verify Jira

Confirm the created issue has:

- Correct summary.
- Description or Work instructions populated.
- Environment Info shown once.
- Clickable page URL.
- Screenshot attached.
- Annotation visible.
- Assignee unassigned unless selected.

## 5. Copy Diagnostics If Needed

If anything fails:

1. Open Pinpoint Settings.
2. Select Copy diagnostics.
3. Paste the diagnostics into the support note or Codex chat.

Diagnostics should not include secrets, OAuth tokens, or session tokens.
