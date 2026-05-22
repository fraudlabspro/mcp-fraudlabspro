# FraudLabs Pro MCP Server
[![mcp-fraudlabspro MCP server](https://glama.ai/mcp/servers/fraudlabspro/mcp-fraudlabspro/badges/score.svg)](https://glama.ai/mcp/servers/fraudlabspro/mcp-fraudlabspro)

An MCP-compliant server that integrates the FraudLabs Pro fraud detection system into AI assistants like Claude Desktop. This server enables real-time screening of order transactions and user-related events (like logins or registrations) to identify and prevent fraudulent activities.

## Features
- Order Screening: Validate e-commerce orders using IP addresses, billing/shipping details, and credit card information.

- User Screening: Analyze account-level events (registrations/logins) for suspicious patterns.

- Transaction Management: Retrieve historical results for orders or user screenings.

- Feedback Loop: Approve, Reject, or Blacklist transactions directly through the AI interface to improve the detection engine.

## Prerequisites
- Python 3.10+ installed.
- FraudLabs Pro API Key: You can obtain a free or paid API key at [FraudLabs Pro](https://www.fraudlabspro.com/pricing).

## Installation
1. Install Dependencies
Ensure you have the required libraries installed in your environment:
```bash
pip install mcp httpx
```
2. Configuration
To use this server with an MCP client (such as Claude Desktop), add the following entry to your configuration file:
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "fraudlabspro": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/ip2locationio/src",
        "run",
        "server.py"
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