# NoshUp MCP Server

AI-powered grocery and meal planning via the Model Context Protocol.

## Endpoint

```
https://api.noshup.ca/mcp
```

**Transport:** Streamable HTTP

## What it does

NoshUp exposes read-only tools for meal planning, grocery deals, and budget tracking:

- **Meals** - Search and discover meals by cuisine, dietary preference, and ingredients
- **Flyers** - Browse current grocery store flyers and deals
- **Meal Plans** - View weekly meal plans
- **Grocery** - Access grocery lists and shopping items
- **Budget** - Track food spending and budget status

All tools are scoped to the authenticated user's household.

## Authentication

NoshUp uses long-lived API keys (`nsh_mcp_...`) for MCP connections.

### Getting an API key

1. Log in to your NoshUp account at [noshup.ca](https://noshup.ca)
2. Generate an MCP API key via the API:
   ```bash
   # Login to get a JWT access token
   http POST https://api.noshup.ca/api/v1/auth/login email=you@example.com password='***'

   # Create an MCP API key (returns the key once)
   http POST https://api.noshup.ca/api/v1/mcp-api-keys/ \
     Authorization:"Bearer <accessToken>" \
     name=my-mcp-connector
   ```
3. Save the returned `nsh_mcp_...` token - it's shown only once

### Connecting to Claude

1. Go to Claude > Settings > Connectors > Add custom connector
2. Enter server URL: `https://api.noshup.ca/mcp`
3. Leave OAuth fields empty
4. Authenticate with your `nsh_mcp_...` API key

### Connecting to ChatGPT

1. Enable Developer Mode: Settings > Apps & Connectors > Advanced
2. Settings > Apps & Connectors > Create
3. URL: `https://api.noshup.ca/mcp`
4. Auth: Token > paste your `nsh_mcp_...` key

### Connecting to Cursor

Add to `.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "noshup": {
      "url": "https://api.noshup.ca/mcp",
      "headers": {
        "Authorization": "Bearer nsh_mcp_..."
      }
    }
  }
}
```

### Connecting via Claude Code

```bash
claude mcp add --transport http noshup https://api.noshup.ca/mcp
```

## Privacy Policy

NoshUp MCP tools access your household's meal plans, grocery lists, flyer deals, and budget data. Data is scoped to your authenticated user account and never shared across households. See [noshup.ca/privacy](https://noshup.ca/privacy) for full details.

## Support

- Email: [support@noshup.ca](mailto:support@noshup.ca)
- Website: [noshup.ca](https://noshup.ca)

## License

Copyright (c) 2026 Navsha Inc. All rights reserved.
