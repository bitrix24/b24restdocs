# MCP Server for Working with Bitrix24 REST API

When a developer asks an AI assistant in an IDE, such as Cursor or VS Code, to write code for integration with Bitrix24, the neural network may suggest non-existent methods or pass unnecessary parameters. MCP provides the assistant with direct access to up-to-date documentation, making the responses more accurate.

## What is MCP

Model Context Provider is a server that transmits structured data to the AI assistant: method descriptions, parameter lists, acceptable values, and usage hints.

MCP allows you to:

- obtain relevant API methods and fields for a specific task,

- work with structured data instead of free text,

- reduce the number of errors and code corrections.

## How to Connect to the MCP Server

Server address: <https://mcp-dev.bitrix24.com/mcp>

The server is accessible without authorization.

### Cursor

1. Open *Settings > Tools > New MCP server*.

2. Add the configuration:

  ```json
  "b24-dev-mcp": {
    "url": "https://mcp-dev.bitrix24.com/mcp",
    "timeout": 30000
  }
  ```

3. Save the settings. A green indicator and a list of available tools will appear next to the server.

4. When composing a request, explicitly instruct the assistant: "Use `b24-dev-mcp`."

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

1. Go to *Settings > Integrations*.

2. Click `Add integration`.

3. Fill in the fields:

   - `Name`: `b24-dev-mcp`;

   - `URL`: `https://mcp-dev.bitrix24.com/mcp`.

4. Save the settings. Claude will automatically determine when to use MCP.

## Example Requests

The MCP server automatically provides the assistant with up-to-date Bitrix24 REST API data, but the ways to interact between the IDE and MCP vary.

### Cursor

**Feature**: requires explicit instruction to use the MCP server.

1. Send a request in the chat with the AI assistant — "Write a `curl` request to create a lead in Bitrix24. Use `b24-dev-mcp`."

2. The assistant will refer to the MCP server.

3. MCP will return information about the method and its parameters from the documentation.

4. The assistant will generate code based on the MCP response.

### GitHub Copilot Chat, VS Code

**Feature**: it is necessary to select MCP as the agent to execute the request.

1. Select the MCP agent and send a request in the chat with the AI assistant — "Create a lead in Bitrix24 with contact information and source 'website'. Show an example in JavaScript."

2. Copilot will automatically use the selected MCP agent.

3. MCP will return information about the method and its parameters from the documentation.

4. The assistant will generate code based on the MCP response.

### Claude Desktop, Anthropic

**Feature**: automatic determination of the need to use MCP.

1. Send a request in the chat with the AI assistant — "Write a request to create a new lead in Bitrix24. The lead will include the following data: name, company, and phone."

2. Claude will determine that data from the connected MCP is needed and will send the request.

3. MCP will return information about the method and its parameters from the documentation.

4. The assistant will generate code based on the MCP response.

## Conclusion

The MCP server helps AI assistants work with the current methods of the Bitrix24 REST API. This reduces the number of errors when generating code. Connect the MCP server to speed up the development of integrations.