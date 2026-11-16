# CloudSpot MCP server through Cotlet.dev

Connect CloudSpot to ChatGPT, Claude, Codex and Claude Code through Cotlet.dev. Manage galleries, clients, projects and documents in your own signed-in CloudSpot account.

This is an unofficial integration, not affiliated with CloudSpot. Cotlet.dev hosts the connection through your Cloud Browser. This repository provides an optional Python stdio MCP bridge.

## Connect your account

1. [Request access to CloudSpot](https://cotlet.dev/signup?app=cloudspot).
2. Open https://app.cloudspot.io/login in your Cotlet.dev Cloud Browser, sign in yourself and enable CloudSpot for agents.
3. Connect your agent. Refresh the connector if you enabled CloudSpot after connecting.

### ChatGPT

Open Settings > Apps & Connectors > Create. Enter `https://cotlet.dev/mcp/v1/connect` as the MCP server URL and choose OAuth.

### Claude

Open Settings > Connectors > Add custom connector and enter `https://cotlet.dev/mcp/v1/connect`.

### Codex

Create a Cotlet.dev API key in your console and set `COTLET_API_KEY` in your environment. Add:

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

## Photography studio workflows

Examples based on the [live CloudSpot integration reference](https://cotlet.dev/integrations/cloudspot-api-mcp-server):

- Read galleries and see the favorites and downloads in Collection Analytics.
- Create a Collection and deliver a Gallery with an optional Zip download.
- Turn an inquiry into a Project and prepare contracts and questionnaires.
- Draft an invoice and payment schedule, then approve sending it.
- Set up mini-sessions and reschedule bookings.
- Review pending print orders before sending them to the lab.

The live reference lists 17 capabilities: 2 read and 15 write. The hosted groups are `cloudspot_read` and `cloudspot_write`.

- `read_galleries`
- `view_collection_analytics`
- `create_collection`
- `deliver_images`
- `set_gallery_expiration`
- `manage_leads`
- `manage_clients`
- `manage_projects`
- `manage_to_dos`
- `manage_contracts`
- `manage_questionnaires`
- `manage_invoices`
- `record_manual_payment`
- `manage_mini_sessions`
- `manage_bookings`
- `manage_discount_codes`
- `review_pending_orders`

`read_galleries` and `view_collection_analytics` only read. Other workflows can include read actions such as `list` and `get`; writes ask for confirmation by default, configurable per tool. Review recipients, amounts and order details before approving. Features depend on your CloudSpot plan: Studio workflows require Full Suite features. Uploading photos and crop edits remain with you. CloudSpot does not email clients when a booking moves; arrange a separate message. Submitted print orders cannot be undone through the tools.

## Optional local stdio bridge

Python 3.10 or newer is required, with no runtime dependencies. Clone this repository and configure a client that supports stdio:

```json
{"mcpServers":{"cloudspot":{"command":"python3","args":["/absolute/path/to/cloudspot-mcp/cloudspot_agent.py"],"env":{"COTLET_API_KEY":"your-cotlet-key"}}}}
```

The local bridge lists seven browser helpers: `cloudspot_guide`, `cloudspot_status`, `cloudspot_open`, `cloudspot_read`, `cloudspot_click`, `cloudspot_type` and `cloudspot_close`. They forward to Cotlet.dev's shared browser tools and are distinct from the 17 hosted workflow capabilities. Call the guide first and close the browser when finished. Discovery works without a key; executing a tool requires your Cotlet.dev key and connected account.

```bash
python3 cloudspot_agent.py connect
python3 cloudspot_agent.py guide
python3 cloudspot_agent.py status
python3 cloudspot_agent.py serve
```

You can also build the included Dockerfile and pass `COTLET_API_KEY` as an environment variable.

## Reference and access

- [CloudSpot connection guide and operation reference](https://cotlet.dev/integrations/cloudspot-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=cloudspot)
- [Request access](https://cotlet.dev/signup?app=cloudspot)
- [Cotlet.dev console](https://cotlet.dev/app)

Licensed under Elastic License 2.0. See LICENSE.
