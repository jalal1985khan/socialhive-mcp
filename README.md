# SocialHive MCP Server

Official Model Context Protocol (MCP) server definition for **[SocialHive](https://socialhive.pro)**.

[![MCP Registry](https://img.shields.io/badge/MCP%20Registry-io.github.jalal1985khan%2Fsocialhive-blue)](https://registry.modelcontextprotocol.io/v0.1/servers?search=socialhive)
[![Status](https://img.shields.io/badge/status-active-brightgreen)](https://registry.modelcontextprotocol.io/v0.1/servers?search=socialhive)

SocialHive enables AI assistants (Claude, Cursor, Windsurf, Cline, ChatGPT, and custom LLM agents) to draft, schedule, and publish content across social platforms, generate AI-powered text and image variations, inspect performance analytics, and automate social workflows.

- **Official Registry Identifier**: `io.github.jalal1985khan/socialhive`
- **Remote Streamable HTTP Endpoint**: `https://platform.socialhive.pro/api/mcp`
- **Supported Transports**: Streamable HTTP (`POST`), Server-Sent Events (`GET` with `Accept: text/event-stream`)
- **Protocol Versions**: `2026-07-28`, `2025-11-25`, `2025-06-18`, `2025-03-26`, `2024-11-05`

---

## Capabilities & Available Tools

SocialHive exposes workspace-scoped tools that respect your team roles and workspace limits:

| Category | Tools | Description |
|---|---|---|
| **Channels & Accounts** | `list_channels`<br>`get_connected_social_accounts` | Discover connected platforms (LinkedIn, X, Bluesky, Mastodon, Facebook, Instagram, Threads, TikTok, YouTube, Pinterest) and their target IDs. |
| **Publishing & Scheduling** | `create_post`<br>`schedule_post`<br>`publish_post`<br>`get_post`<br>`list_posts` | Draft, schedule (ISO 8601), immediately publish, or query status of social posts. |
| **AI Content Studio** | `generate_text`<br>`adapt_post`<br>`generate_image` | Generate platform-tailored copy, adapt posts to character limits, and generate brand images. |
| **Media Library** | `list_media`<br>`import_media` | Browse assets or ingest images/video from remote URLs into your SocialHive asset library. |
| **Curation & Sources** | `list_sources`<br>`search_source_items` | Query RSS feeds, newsletters, and ingested web content for fresh source material. |
| **Articles & Long-form** | `list_articles`<br>`get_article`<br>`create_article` | Manage long-form content, blog posts, and newsletter drafts. |
| **Analytics & Insights** | `get_analytics` | Retrieve engagement summaries, top-performing posts, and AI-driven growth recommendations. |
| **Workflows** | `list_workflows`<br>`list_node_types`<br>`propose_workflow` | Propose automated workflow graphs to Workflow Studio for human approval. |

---

## Authentication

All requests to the SocialHive MCP server require a workspace API key:
- Header: `Authorization: Bearer shp_<your_api_key>`
- You can generate your API key in **SocialHive Dashboard > Settings > API Keys** (`https://socialhive.pro/app/settings/api`).

---

## Client Setup

### 1. Claude Desktop (`claude_desktop_config.json`)

Add the following to your Claude Desktop configuration file:

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "socialhive": {
      "url": "https://platform.socialhive.pro/api/mcp",
      "headers": {
        "Authorization": "Bearer shp_YOUR_API_KEY"
      }
    }
  }
}
```

### 2. Cursor / Windsurf / Cline / VS Code

In your client's MCP configuration file (e.g. `.cursor/mcp.json` or Cline MCP settings):

```json
{
  "mcpServers": {
    "socialhive": {
      "type": "streamable-http",
      "url": "https://platform.socialhive.pro/api/mcp",
      "headers": {
        "Authorization": "Bearer shp_YOUR_API_KEY"
      }
    }
  }
}
```

### 3. Programmatic Usage (TypeScript SDK)

```typescript
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StreamableHttpClientTransport } from "@modelcontextprotocol/sdk/client/streamableHttp.js";

const transport = new StreamableHttpClientTransport(
  new URL("https://platform.socialhive.pro/api/mcp"),
  {
    headers: {
      Authorization: "Bearer shp_YOUR_API_KEY",
    },
  }
);

const client = new Client({ name: "my-agent", version: "1.0.0" }, { capabilities: {} });
await client.connect(transport);

const tools = await client.listTools();
console.log("Connected to SocialHive MCP! Available tools:", tools.tools.map(t => t.name));
```

---

## Publishing to the Official MCP Registry

### 1. Install `mcp-publisher`
```bash
brew install mcp-publisher
```

### 2. Validate
```bash
mcp-publisher validate
```

### 3. Authenticate
- **Domain Verification** (for `pro.socialhive/platform`):
  ```bash
  mcp-publisher login domain --domain socialhive.pro
  ```
- **GitHub Verification** (if using `io.github.<user>/socialhive`):
  ```bash
  mcp-publisher login github
  ```

### 4. Publish
```bash
mcp-publisher publish
```

### 5. Check Registry Listing
```bash
curl -s "https://registry.modelcontextprotocol.io/v0.1/servers?search=socialhive"
```
