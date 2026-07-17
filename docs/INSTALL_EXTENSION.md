# Install The Pinpoint Extension

These steps install Pinpoint as an unpacked browser extension for Chrome or Microsoft Edge.

## Before You Start

You need:

- Chrome or Microsoft Edge.
- Access to the Pinpoint source files.
- A working Pinpoint broker URL.
- Jira Cloud access.

For local development, the broker URL is usually:

```text
http://127.0.0.1:8788
```

After Azure hosting is available, use the hosted broker URL instead.

## Install From A Downloaded ZIP

1. Download the repo ZIP from GitHub.
2. Extract it to a normal folder, for example:

   ```text
   C:\Users\YourName\Documents\Pinpoint
   ```

3. Open the browser extensions page:

   ```text
   chrome://extensions
   ```

   or:

   ```text
   edge://extensions
   ```

4. Turn on Developer mode.
5. Select Load unpacked.
6. Choose the extracted extension folder:

   ```text
   C:\Users\YourName\Documents\Pinpoint\extension
   ```

7. Pin the extension to the browser toolbar if desired.

## Configure Pinpoint

1. Open Pinpoint Settings.
2. Set Connector URL to the broker URL.
3. Select Test connector.
4. Select Connect Jira.
5. Complete the Atlassian sign-in flow.
6. Confirm the Settings page shows:

   ```text
   Broker: Reachable
   Jira: Connected
   ```

## Local Broker Note

Until the broker is hosted, each machine needs a local broker to create Jira issues. That requires the local setup documented in [Local Development](LOCAL_DEVELOPMENT.md).

Once Azure hosting is available, users should not need Node.js, Docker, or the local startup script.
