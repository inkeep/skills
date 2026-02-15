---
title: "CLI inkeep profile Command"
description: "Manage named CLI profiles for multiple remotes, credentials, and environments"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep profile Command

### `inkeep profile`

Manage named CLI profiles for multiple remotes, credentials, and environments. Profiles are stored in `~/.inkeep/profiles.yaml`. A default `cloud` profile points to hosted endpoints.

```yaml
# ~/.inkeep/profiles.yaml
activeProfile: cloud
profiles:
  cloud:
    remote: cloud
    credential: inkeep-cloud
    environment: production
  local:
    remote:
      api: http://localhost:3002
      manageUi: http://localhost:3000
    credential: none
    environment: development
```

**Subcommands:**

* `inkeep profile list` — Show all profiles and the active one.
* `inkeep profile add <name>` — Interactive profile creation. Choose between Inkeep Cloud, Local (localhost defaults, no auth required), or Custom (self-hosted/staging URLs).
* `inkeep profile use <name>` — Switch the active profile.
* `inkeep profile current` — Show the active profile details and credential key.
* `inkeep profile remove <name>` — Delete a profile (cannot remove the active one).

**Examples:**

```bash
# Create a local profile and switch to it
inkeep profile add local
inkeep profile use local

# Inspect which profile is active
inkeep profile current

# List all profiles
inkeep profile list

# Remove an unused profile
inkeep profile remove staging
```

**How profiles are used:**

* `inkeep login --profile <name>` stores credentials under the profile’s credential key.
* Commands that talk to the APIs (`push`, `pull`, `status`, `login`, `logout`) honor `--profile <name>`; if omitted, the active profile is used.
* Each profile bundles remote URLs, an environment name, and a credential reference so you can move between cloud and self-hosted deployments without editing config files.

#### Using profiles with authenticated deployments

If your deployment requires authentication (cloud or self-hosted), run `inkeep login` for each profile so its credential slot in `profiles.yaml` has a token in the system keychain:

```bash
# Authenticate against your secured deployment
inkeep login --profile local

# Validate which profile is active before running commands
inkeep profile current

# Use the profile when pushing/pulling
inkeep push --profile local
inkeep pull --profile local
```

* The `credential` field in the profile is the key used to store the token; `login` writes to that key and `push/pull/status` read from it.
* Repeat `login --profile <name>` for every profile/remote you operate against.
* Use `inkeep logout --profile <name>` to clear a profile’s stored credentials if you need to rotate access.
