# NoshUp MCP — Gemini Setup

NoshUp's MCP server works with Gemini CLI, Antigravity CLI, and Gemini Enterprise.

## Endpoint

```
https://api.noshup.ca/mcp
```

**Transport:** Streamable HTTP (required by Gemini — SSE is not supported)

## Authentication

OAuth 2.0 with PKCE. NoshUp's authorization server is at `https://api.noshup.ca/`.

### Discovery endpoints

| Endpoint | URL |
|---|---|
| Authorization server metadata | `https://api.noshup.ca/.well-known/oauth-authorization-server` |
| Protected resource metadata | `https://api.noshup.ca/.well-known/oauth-protected-resource/mcp` |

### Scopes

- `mcp` — access to all MCP tools (meal plans, grocery lists, budget, recipes, flyer deals)

## Gemini CLI / Antigravity CLI

```bash
gemini mcp add noshup https://api.noshup.ca/mcp
# Follow the OAuth flow in your browser — log in to NoshUp, approve consent
# Then use:
#   "Show my meal plan"
#   "What's on my grocery list?"
#   "Search for vegetarian meals"
```

## Gemini Enterprise (Google Cloud)

Admins can add NoshUp as a custom MCP data store:

1. Google Cloud Console → **Gemini Enterprise** → **Data stores** → **Create data store**
2. Source: **Custom MCP Server** → **Add MCP server**
3. Configure:
   - **MCP Server URL:** `https://api.noshup.ca/mcp`
   - **Transport:** Streamable HTTP
   - **Authentication:** OAuth 2.0
   - **Authorization URL:** `https://api.noshup.ca/authorize`
   - **Token URL:** `https://api.noshup.ca/token`
   - **Client ID:** _(your registered NoshUp OAuth client ID)_
   - **Client Secret:** _(your registered NoshUp OAuth client secret)_
   - **Scopes:** `mcp`
4. Click **Verify Auth** and complete the NoshUp login + consent
5. Click **Create** — the data store is ready when status shows `Active`

## Available tools

| Tool | Description | Access |
|---|---|---|
| `search_meals` | Search meals by cuisine, dietary preference, ingredient | All users |
| `get_recipe` | Get full recipe details by meal ID | All users |
| `get_flyer_deals` | Browse current grocery flyer deals | All users |
| `get_meal_plan` | View household's weekly meal plan | Paid (Basic+) |
| `create_meal_plan` | Generate a new weekly meal plan | Paid (Basic+) |
| `get_grocery_list` | View household's grocery list | Paid (Basic+) |
| `update_grocery_list` | Add/remove grocery list items | Paid (Premium) |
| `get_budget_summary` | View grocery budget status | Paid (Basic+) |
| `set_budget` | Update grocery budget | Paid (Premium) |

## Privacy

- All tools operate only on data already in NoshUp's database — no outbound third-party API calls
- Household-scoped: a user can only access their own household's data
- Privacy policy: https://noshup.ca/privacy
- Terms: https://noshup.ca/terms
