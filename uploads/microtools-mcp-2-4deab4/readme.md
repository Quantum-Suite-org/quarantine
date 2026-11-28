MCP Microtools

A small stdio MCP server with eight lightweight tools for common developer tasks. It processes inputs locally and does not access files or call external services.

Requirements

\- Node.js 20 or later

\- An MCP client that supports stdio servers

Install and run

npm install

npm start

To connect it from an MCP client, configure a stdio server with the command node and the argument pointing to this project’s index.js:

{

&#x20; "mcpServers": {

&#x20;   "microtools": {

&#x20;     "command": "node",

&#x20;     "args": \["/path/to/mcp-microtools/index.js"]

&#x20;   }

&#x20; }

}

Tools

\- json\_format — Validate and pretty-print JSON. Choose an indentation width from 0 to 8 spaces.

\- json\_query — Read a value from JSON using a path such as user.name, items\[0].id, or $ for the root.

\- url\_parse — Parse an absolute URL into its components and query parameters. Repeated query keys are returned as arrays.

\- url\_build — Set, append, or remove query parameters on an absolute URL. Arrays create repeated keys; null removes a key.

\- base64\_encode — Encode UTF-8 text as standard or URL-safe Base64.

\- base64\_decode — Decode Base64 as UTF-8 text. Rejects malformed data and invalid UTF-8.

\- text\_hash — Return the hexadecimal SHA-256 or SHA-512 digest of UTF-8 text.

\- timestamp\_convert — Convert an ISO 8601 date or Unix timestamp to ISO 8601, seconds, and milliseconds. Numeric inputs default to seconds; set numeric\_unit to milliseconds when needed.

Text inputs are limited to 1,000,000 characters; URL inputs are limited to 4,096 characters.

Dependencies

The server uses @modelcontextprotocol/server and Zod for tool input validation.

