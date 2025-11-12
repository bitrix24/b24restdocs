# MCP Server for Working with Bitrix24 REST API

When a developer asks the AI assistant in the development environment to write code for integration with Bitrix24, the neural network may suggest non-existent methods or pass unnecessary parameters. MCP provides the assistant with direct access to up-to-date documentation, making the responses more accurate.

## What is MCP

MCP (Model Context Provider) is a server that transmits structured data to the AI assistant: method descriptions, parameter lists, acceptable values, and usage hints.

MCP allows:

- obtaining relevant API methods and fields for a specific task,

- working with structured data instead of free text,

- reducing the number of errors and code corrections.

## How to Connect to the MCP Server

Specify the server address <https://mcp-dev.bitrix24.com/mcp> in the development environment settings.

The server is accessible without authorization.

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

3. Save the file. On the *File > Preferences > Cursor Settings > Tools & MCP* page, a green indicator will appear next to the server along with a list of available tools.

4. When composing a request, add the `mcp.json` file to the context.

Alternative way to add MCP:

```html
<a href="https://cursor.com/en-US/install-mcp?name=b24-dev-mcp&config=eyJ1cmwiOiJodHRwczovL21jcC1kZXYuYml0cml4MjQuY29tL21jcCIsInRpbWVvdXQiOjMwMDAwfQ%3D%3D"><img src="https://cursor.com/deeplink/mcp-install-dark.svg" alt="Add b24-dev-mcp MCP server to Cursor" height="32" /></a>
```

### GitHub Copilot Chat, VS Code

1. For setup, use the instructions from [GitHub](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp/extend-copilot-chat-with-mcp#configuring-mcp-servers-manually).

2. Create a file `.vscode/mcp.json` in the root of the project. The content of the file:

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

### Gemini Code Assist CLI

1. Add the MCP server with the command:

  ```bash
  gemini mcp add --transport http b24-dev-mcp https://mcp-dev.bitrix24.com/mcp
  ```

2. Check that the server appears in the list with the command `gemini mcp list`. Gemini will automatically determine when to use MCP.

## Request Examples

The MCP server automatically provides the assistant with up-to-date Bitrix24 REST API data, but the ways to interact between the development environment and MCP vary.

### Cursor

**Feature**: It is necessary to connect the MCP configuration in the chat context.

1. Add the `mcp.json` file to the chat context.

2. Send a request in the chat with the AI assistant — "Write a `curl` request to create a lead in Bitrix24."

3. The assistant will refer to the MCP server.

4. MCP will return information about the method and its parameters from the documentation.

5. The assistant will generate code based on the MCP response.

### GitHub Copilot Chat, VS Code

**Feature**: It is necessary to select MCP as the agent to execute the request.

1. Select the MCP agent and send a request in the chat with the AI assistant — "Create a lead in Bitrix24 with contact information and source 'website'. Show an example in JavaScript."

2. Copilot will automatically use the selected MCP agent.

3. MCP will return information about the method and its parameters from the documentation.

4. The assistant will generate code based on the MCP response.

### Claude Desktop, Anthropic

**Feature**: Automatic determination of the need to use MCP.

1. Send a request in the chat with the AI assistant — "Write a request to create a new lead in Bitrix24. The lead will include the data: name, company, and phone."

2. Claude will determine that data from the connected MCP is needed and will send the request.

3. MCP will return information about the method and its parameters from the documentation.

4. The assistant will generate code based on the MCP response.

### Gemini Code Assist CLI

**Feature**: Automatic determination of the need to use MCP.

1. Execute the command `gemini chat` and send a request — "Gather a `curl` request to create a lead with fields name, phone, and source 'site'."

2. Gemini will determine that data from the connected MCP is needed and will send the request.

3. MCP will return a description of the appropriate method and its parameters from the documentation.

4. Gemini will generate code based on the MCP response.