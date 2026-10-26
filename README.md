# FraudLabs Pro MCP Server
[![mcp-fraudlabspro MCP server](https://glama.ai/mcp/servers/fraudlabspro/mcp-fraudlabspro/badges/score.svg)](https://glama.ai/mcp/servers/fraudlabspro/mcp-fraudlabspro)

An MCP-compliant server that integrates the FraudLabs Pro fraud detection system into AI assistants like Claude Desktop. This server enables real-time screening of order transactions and user-related events (like logins or registrations) to identify and prevent fraudulent activities.

## Features
- Order Screening: Validate e-commerce orders using IP addresses, billing/shipping details, and credit card information.

- User Screening: Analyze account-level events (registrations/logins) for suspicious patterns.

- Transaction Management: Retrieve historical results for orders or user screenings.

- Feedback Loop: Approve, Reject, or Blacklist transactions directly through the AI interface to improve the detection engine.

## Prerequisites
- Python 3.10+ installed (only needed for the local MCP server).
- FraudLabs Pro API Key: Sign up for a Free Micro Plan API Key at [FraudLabs Pro](https://www.fraudlabspro.com/checkout-micro).

## Installation

There are two ways to use this MCP server:

- **Cloud-hosted MCP server**: Connect directly to the hosted FraudLabs Pro MCP server. Nothing needs to be installed locally.
- **Local MCP server**: Download and run the server on your own machine.

### Option 1: Cloud-hosted MCP server

Use this if you want to connect directly to the hosted server.

#### Method A: Direct HTTP connection

Use this if your MCP client supports remote servers over HTTP. No Node.js or `npx` is needed.

```json
{
  "mcpServers": {
    "fraudlabspro": {
      "type": "http",
      "url": "https://mcp.fraudlabspro.com/",
      "headers": {
        "X-API-Key": "YOUR_API_KEY_HERE"
      }
    }
  }
}
```

If your client is configured from the command line, such as Claude Code, you can add the server with:

```bash
claude mcp add --transport http fraudlabspro https://mcp.fraudlabspro.com/ --header "X-API-Key: YOUR_API_KEY_HERE"
```

#### Method B: Connection through `npx`

Use this if your MCP client only supports local (stdio) servers. The `mcp-remote` package runs locally and forwards requests to the hosted server. Node.js is required.

```json
{
  "mcpServers": {
    "fraudlabspro": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote@latest",
        "https://mcp.fraudlabspro.com/",
        "--header",
        "X-API-Key:YOUR_API_KEY_HERE"
      ]
    }
  }
}
```

Restart your MCP client after saving the changes, and you should see the server appear in the tools menu.

### Option 2: Local MCP server

1. Install Dependencies
Ensure you have the required libraries installed in your environment:
```bash
pip install mcp httpx uv
```
2. Configuration
To use this server with an MCP client (such as Claude Desktop), add the following entry to your configuration file:
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "fraudlabspro": {
      "command": "uvx",
      "args": [
        "mcp-fraudlabspro"
      ],
      "env": {
        "FRAUDLABSPRO_API_KEY": "YOUR_API_KEY_HERE"
      }
    }
  }
}
```

## Available Tools

📦 Order Management

| Tool | Description | Key Arguments |
|------|-------------|----------|
| screen_order | Screen an order for fraud. | ip, email, amount, bin_no, bill_country, ship_country |
| get_order_result | Retrieve the validation result for a previous order. | transaction_id |
| feedback_order | Update order status (APPROVE, REJECT, BLACKLIST). | transaction_id, action, note |

👤 User Management

| Tool | Description | Key Arguments |
|------|-------------|----------|
| screen_user | Screen user events like logins or signups. | email, ip, phone, first_name, last_name |
| get_user_result | Retrieve results for a previous user screening. | user_transaction_id |
| feedback_user | Update user event status based on manual review. | user_transaction_id, action, reason |

## Development & Logging
The server uses FastMCP and sends logs through the MCP context. You can view logs in the Claude Desktop "Developer Console" to inspect outgoing payloads and API responses for debugging.

Common Error: If you receive "An API key is needed," ensure the `FRAUDLABSPRO_API_KEY` environment variable is correctly set in your configuration file and that you have restarted the MCP client.

## License

See the LICENSE file.

<!-- mcp-name: io.github.fraudlabspro/mcp-fraudlabspro -->
