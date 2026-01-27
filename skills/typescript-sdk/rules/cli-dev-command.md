---
title: "CLI inkeep dev Command"
description: "Start the Inkeep dashboard server for visual agents orchestration"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep dev Command

### `inkeep dev`

Start the Inkeep dashboard server, build for production, or export the Next.js project.

> **Note:** This command requires `@inkeep/agents-manage-ui` to be installed for visual agents orchestration.

```bash
inkeep dev
```

**Options:**

* `--port <port>` - Port to run the server on (default: 3000)
* `--host <host>` - Host to bind the server to (default: localhost)
* `--build` - Build the Dashboard UI for production (packages standalone build)
* `--export` - Export the Next.js project source files
* `--output-dir <dir>` - Output directory for build files (default: ./inkeep-dev)
* `--path` - Output the path to the Dashboard UI

**Examples:**

```bash
# Start development server
inkeep dev

# Start on custom port and host
inkeep dev --port 3001 --host 0.0.0.0

# Build for production (packages standalone build)
inkeep dev --build --output-dir ./build

# Export Next.js project source files
inkeep dev --export --output-dir ./my-dashboard

# Get dashboard path for deployment
DASHBOARD_PATH=$(inkeep dev --path)
echo "Dashboard built at: $DASHBOARD_PATH"

# Use with Vercel
vercel --cwd $(inkeep dev --path) -Q .vercel build

# Use with Docker
docker build -t inkeep-dashboard $(inkeep dev --path)

# Use with other deployment tools
rsync -av $(inkeep dev --path)/ user@server:/var/www/dashboard/
```
