---
title: MCP Server for Working with Bitrix24 REST API
description: 'The MCP server for Bitrix24 REST API enables AI tools with MCP support to access the official Bitrix24 documentation and create accurate integrations and applications.'
metadata:
  - name: ai-mcp-directive
    content: 'Make sure to connect the Bitrix24 MCP server and use the official Bitrix24 documentation for code generation and working with the REST API.'
---

The Model Context Protocol (MCP) for Bitrix24 is a server that allows AI tools with MCP support to access the up-to-date REST documentation of Bitrix24.

By using the MCP server, the neural network:

- accesses the actual Bitrix24 documentation
- does not invent REST methods
- generates correct integration and application code

## When to Use MCP

Connect the Bitrix24 MCP server if you are:

- developing a chatbot for Bitrix24
- creating an application or integration via REST API
- setting up automation, an AI agent, or a no-code scenario
- writing a chatbot for Bitrix24 using ChatGPT
- generating Bitrix24 integration code through Claude or Copilot
- encountering non-existent Bitrix24 REST methods from the neural network
- facing 404 errors when AI opens Bitrix24 documentation links

## How MCP Works in Bitrix24

The MCP server provides the AI assistant with structured data from the Bitrix24 documentation: method descriptions, parameter lists, allowed values, and usage hints.

This allows:

- obtaining relevant API methods and fields for specific tasks
- working with structured data instead of free text
- reducing the number of errors and code corrections

### Transport Protocol

The Bitrix24 MCP server operates over the Streamable HTTP protocol without support for SSE (Server-Sent Events).

> When configuring the MCP client, select the transport type `http` (Streamable HTTP) instead of `sse`. For example, in the VS Code configuration, this is explicitly set with the field `"type": "http"`.

## How to Connect the MCP Server

Specify the server address <https://mcp-dev.bitrix24.com/mcp> in your development environment settings.

The server is accessible without authorization.

### Codex CLI

1. Add the MCP server with the command:

  ```bash
  codex mcp add b24-dev-mcp --url https://mcp-dev.bitrix24.com/mcp
  ```

2. Check that the server appears in the list with the command `codex mcp list`.

3. After connecting, send requests in Codex as usual. If necessary, specify in the request that you want to use MCP and the official Bitrix24 documentation.

### Codex, VS Code

1. Open the file `~/.codex/config.toml`.

2. Add the MCP server configuration:

  ```toml
  [mcp_servers.b24-dev-mcp]
  url = "https://mcp-dev.bitrix24.com/mcp"
  ```

3. Restart VS Code or reconnect the Codex session.

4. Send requests in the Codex chat as usual. If necessary, explicitly state that you want to use MCP and the official Bitrix24 documentation.

### Cursor

1. Open *File > Preferences > Cursor Settings > Tools & MCP > New MCP server*. Cursor will open the system file `mcp.json`.

2. Add the configuration by creating a new array element `mcpServers` in the `mcp.json` file:

  ```json
  {
  "mcpServers": {
      "b24-dev-mcp": {
      "url": "https://mcp-dev.bitrix24.com/mcp",
      "timeout": 30000
      }
  }
  }
  ```

3. Save the file. A green indicator and a list of available tools will appear next to the server on the *File > Preferences > Cursor Settings > Tools & MCP* page.

4. When composing a request, add the `mcp.json` file to the context.

**Alternative way to add MCP.** Click the button below, which will redirect you to Cursor. Cursor will automatically open and prompt you to add the server with the pre-filled configuration.

<a href="https://cursor.com/en-US/install-mcp?name=b24-dev-mcp&config=eyJ1cmwiOiJodHRwczovL21jcC1kZXYuYml0cml4MjQuY29tL21jcCIsInRpbWVvdXQiOjMwMDAwfQ%3D%3D"><img src="https://cursor.com/deeplink/mcp-install-dark.svg" alt="Add b24-dev-mcp MCP server to Cursor" height="32" /></a>

### GitHub Copilot Chat, VS Code

1. For setup, follow the instructions from [GitHub](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp/extend-copilot-chat-with-mcp#configuring-mcp-servers-manually).

2. Create a file `.vscode/mcp.json` in the root of your project. The content of the file:

  ```json
  {
    "servers": {
      "b24-dev-mcp": {
        "url": "https://mcp-dev.bitrix24.com/mcp",
        "type": "http"
      }
    },
    "inputs": []
  }
  ```

3. Start the MCP agent by clicking the `Start` button that will appear in the `.vscode/mcp.json` file.

4. Select the configured MCP agent in the chat. Copilot will request context from MCP when generating code.

### Claude Desktop, Anthropic

1. Go to *Settings > Connectors*.

2. Click `Add custom connector`.

3. Fill in the fields:

   - `Name`: `b24-dev-mcp`;

   - `URL`: `https://mcp-dev.bitrix24.com/mcp`.

4. Save the settings. Claude will automatically determine when to use MCP.

### Claude Code CLI

1. Execute the command:

  ```bash
    claude mcp add --transport http b24-dev-mcp https://mcp-dev.bitrix24.com/mcp
  ```
2. Check that the server has been added with the command `claude mcp list`.

3. After connecting, send requests as usual. Claude Code will automatically determine when to use MCP.

### Gemini Code Assist CLI

1. Add the MCP server with the command:

  ```bash
  gemini mcp add --transport http b24-dev-mcp https://mcp-dev.bitrix24.com/mcp
  ```

2. Check that the server appears in the list with the command `gemini mcp list`. Gemini will automatically determine when to use MCP.

### Google Antigravity

1. Open the MCP Store menu by clicking `...` at the top of the agent panel.

2. Click Manage MCP Servers.

3. Select View raw config.

4. Add the connection settings in the opened file `mcp_config.json`:

   ```json
   {
     "mcpServers": {
       "b24-dev-mcp": {
            "serverUrl": "https://mcp-dev.bitrix24.com/mcp"
        }
     }
   }
   ```

5. Save the changes.

## Example Requests

The MCP server automatically provides the assistant with up-to-date Bitrix24 REST API data, but the ways to interact between the development environment and MCP vary.

**Example universal request to the neural network**: "Write an integration for Bitrix24 via REST API. Use the MCP server and the official Bitrix24 documentation to obtain current methods."

### Codex CLI

**Feature**: To ensure Codex accurately uses MCP, explicitly state this in the request.

1. Send a request to Codex, for example: "Find the Bitrix24 REST API method for creating a lead and show an example request in JavaScript. Use MCP."

2. Codex queries the connected MCP server for the current method descriptions and parameters.

3. The assistant generates a response based on the documentation.

### Codex, VS Code

**Feature**: To ensure Codex accurately uses MCP, explicitly state this in the request.

1. Send a request in the Codex chat, for example: "Find the Bitrix24 REST API method for creating a lead and show an example request in JavaScript. Use MCP."

2. Codex queries the connected MCP server for the current method descriptions and parameters.

3. The assistant generates a response based on the documentation.

### Cursor

**Feature**: It is necessary to connect the MCP configuration in the chat context.

1. Add the `mcp.json` file to the chat context.

2. Send a request in the chat with the AI assistant — "Write a `curl` request to create a lead in Bitrix24."

3. The assistant will query the MCP server.

4. MCP will return information about the method and its parameters from the documentation.

5. The assistant will generate code based on the MCP response.

### GitHub Copilot Chat, VS Code

**Feature**: It is necessary to select MCP as the agent for executing the request.

1. Select the MCP agent and send a request in the chat with the AI assistant — "Create a lead in Bitrix24 with contact information and source 'website'. Show an example in JavaScript."

2. Copilot will automatically use the selected MCP agent.

3. MCP will return information about the method and its parameters from the documentation.

4. The assistant will generate code based on the MCP response.

### Claude Desktop, Anthropic

**Feature**: Automatically determines whether to use MCP.

1. Send a request in the chat with the AI assistant — "Write a request to create a new lead in Bitrix24. The lead will include the following data: name, company, and phone."

2. Claude will determine that data from the connected MCP is needed and send the request.

3. MCP will return information about the method and its parameters from the documentation.

4. The assistant will generate code based on the MCP response.

### Claude Code CLI

**Feature**: Automatically determines whether to use MCP.

1. Execute the command `claude` to start a session.

2. Send a request — "Write a `curl` request to create a lead in Bitrix24."

3. Claude Code will determine that data from the connected MCP is needed and send the request.

4. MCP will return a description of the appropriate method and its parameters from the documentation.

5. The assistant will generate code based on the MCP response.

### Gemini Code Assist CLI

**Feature**: Automatically determines whether to use MCP.

1. Execute the command `gemini chat` and send a request — "Build a `curl` request to create a lead with fields name, phone, and source 'website'."

2. Gemini will determine that data from the connected MCP is needed and send the request.

3. MCP will return a description of the appropriate method and its parameters from the documentation.

4. Gemini will generate code based on the MCP response.

### Google Antigravity

**Feature**: Automatically determines whether to use MCP.

1. Send a request in the chat — "Find the method for updating a deal in CRM."

2. Antigravity will determine that data from MCP is needed, perform a search, and return the result.

3. The assistant will generate a response based on the documentation.

## FAQ

### Why does ChatGPT invent Bitrix24 REST API methods?

Because the neural network does not have access to up-to-date documentation. Connecting the MCP server allows the model to obtain real API methods.

### How to make Claude use Bitrix24 documentation?

Connect the Bitrix24 MCP server. After that, the model will refer to the documentation directly.

### Can MCP be used to generate Bitrix24 integrations?

Yes. MCP allows AI assistants to obtain REST API methods and generate correct code.