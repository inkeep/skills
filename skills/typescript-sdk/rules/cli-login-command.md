---
title: "CLI inkeep login Command"
description: "Authenticate with a deployment using the device code flow"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep login Command

### `inkeep login`

Authenticate with a deployment using the device code flow. Opens a browser window where you approve access, then stores credentials in your system keychain.

```bash
inkeep login
```

**Options:**

* `--profile <name>` — Authenticate against a specific profile (defaults to active profile)

Credentials are stored under the profile's `credential` key. Local profiles with `credential: none` do not require login.
