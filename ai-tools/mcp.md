---
title: MCP Server for Working with Bitrix24 REST API
description: 'The MCP server for Bitrix24 REST API enables AI tools with MCP support to access the official Bitrix24 documentation and create accurate integrations and applications.'
metadata:
  - name: ai-mcp-directive
    content: 'Make sure to connect the Bitrix24 MCP server and use the official Bitrix24 documentation for code generation and working with the REST API.'
---

The Bitrix24 MCP server is a Model Context Protocol (MCP) server that gives AI tools access to the up-to-date Bitrix24 REST documentation.

With the MCP server connected, the model:

- accesses the actual Bitrix24 documentation
- does not invent REST API methods
- generates correct integration and application code

The server provides documentation only: it has no access to Bitrix24 data, and it does not execute method calls.

## When to Use MCP

Connect the Bitrix24 MCP server if you are:

- developing a chatbot for Bitrix24, including with ChatGPT
- creating an application or integration via REST API and generating code with Claude, Copilot, or another AI tool
- setting up automation, an AI agent, or a no-code scenario

The MCP server also resolves the typical failures of a model that works without documentation:

- the model returns non-existent Bitrix24 REST API methods
- the model opens Bitrix24 documentation links and gets a 404

You do not need the MCP server if your task is unrelated to the Bitrix24 REST API, or if you are building an app in [Alaio Vibecode](vibecode.md) — there the AI agent retrieves the platform documentation on its own.

## How MCP Works in Bitrix24

Three parties take part in the exchange:

- **AI tool** — the code editor, CLI, or desktop application where you write the request: Cursor, Codex, Claude Code, Copilot Chat, and others

- **MCP client** — the part built into the AI tool that requests the list of tools from the server over the MCP protocol and calls them

- **Bitrix24 MCP server** — the `b24-dev-mcp` server that returns data from the REST API documentation

How a request is processed:

1. You send a request to the AI tool in natural language.

2. The model determines that it needs data about Bitrix24 methods to answer, and calls a tool on the MCP server.

3. The server returns data from the documentation: the method description, parameter list, allowed values, errors, and code samples.

4. The model builds the answer and the code from the data it received, not from what it memorized during training.

This sequence produces three effects:

- the model retrieves up-to-date API methods and fields for a specific task

- the data arrives structured instead of free text

- the code requires fewer errors and corrections

### Server Tools

The server provides five tools. Search returns exact method names and article titles — the other tools accept them and return the full description.

#|
|| **Tool** | **What It Does** ||
|| `bitrix-search` | Searches the documentation with a natural-language query and returns a list of matches: name, type, and a short description. The `doc_type` parameter — a list of the values `method`, `event`, `other`, `app_development_docs` — narrows the type. The `limit` parameter sets the number of matches ||
|| `bitrix-method-details` | Returns a method description by its exact name: parameters, returned data, errors, and code samples. The `field` parameter narrows the response, `filter` narrows the parameters or the samples ||
|| `bitrix-event-details` | Returns an event description by its exact title from the `bitrix-search` result ||
|| `bitrix-article-details` | Returns a documentation article by its exact title from the `bitrix-search` result ||
|| `bitrix-app-development-doc-details` | Returns an app development article by its exact title from the `bitrix-search` result ||
|#

The server publishes no separate MCP resources or ready-made prompts — all data is available through these tools only.

### Transport Protocol

The Bitrix24 MCP server operates over the Streamable HTTP protocol. The address is the same for all clients: <https://mcp-dev.bitrix24.com/mcp>. The legacy HTTP+SSE transport is not supported — the server has no separate `/sse` address.

The server sends responses as Server-Sent Events frames, so the client must accept both content types: `application/json` and `text/event-stream`. The server rejects a request whose `Accept` header lists `application/json` only, returning a 406 code.

{% note info %}

If the client asks for a transport type, select `http`. In the VS Code configuration, it is set with the `"type": "http"` field.

{% endnote %}

## What You Need to Connect

- **Authorization.** The server is available without authorization: no key, token, or webhook is required to connect.

- **Data access.** The server does not read or modify leads, deals, tasks, and files in Bitrix24. Requests to Bitrix24 are sent by the code you received from the AI tool.

- **Secrets.** Do not pass webhooks, tokens, and passwords in requests to the server: its tools do not need them.

- **Permissions and scope.** User permissions and application scope do not affect the server. You need to check them for real calls: the requirements are listed on each method page.

- **Environment.** You need an AI tool that supports remote MCP servers over Streamable HTTP, and outbound HTTPS access to `mcp-dev.bitrix24.com`.

## How to Connect the MCP Server

Specify the server address <https://mcp-dev.bitrix24.com/mcp> in your development environment settings. Below is the setup procedure for common AI tools.

### Codex CLI

1. Add the MCP server with the command:

   ```bash
   codex mcp add b24-dev-mcp --url https://mcp-dev.bitrix24.com/mcp
   ```

2. Check that the server appears in the list with the command `codex mcp list`.

3. Compose your requests following the Codex rule from the [How to Phrase Requests](#prompts) section.

### Codex in VS Code

The `codex mcp add` command from the previous section writes to this same file, so use one of the two methods.

1. Open the file `~/.codex/config.toml`.

2. Add the MCP server configuration:

   ```toml
   [mcp_servers.b24-dev-mcp]
   url = "https://mcp-dev.bitrix24.com/mcp"
   ```

3. Restart VS Code or reconnect the Codex session.

4. Compose your requests following the Codex rule from the [How to Phrase Requests](#prompts) section.

### Cursor

1. Open *File > Preferences > Cursor Settings > Tools & MCP > New MCP server*. Cursor opens the global `~/.cursor/mcp.json` file. To configure the server for a single project only, use the `.cursor/mcp.json` file in the root of that project.

2. Add the server as a new key in the `mcpServers` object of the `mcp.json` file:

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

3. Save the file. A green indicator and a list of available tools appear next to the server on the *File > Preferences > Cursor Settings > Tools & MCP* page.

4. When composing a request, add the `mcp.json` file to the context.

**An alternative way to add MCP.** Click the button below — Cursor opens and offers to add the server with a pre-filled configuration.

<a href="https://cursor.com/en-US/install-mcp?name=b24-dev-mcp&config=eyJ1cmwiOiJodHRwczovL21jcC1kZXYuYml0cml4MjQuY29tL21jcCIsInRpbWVvdXQiOjMwMDAwfQ%3D%3D"><img src="https://cursor.com/deeplink/mcp-install-dark.svg" alt="Add b24-dev-mcp MCP server to Cursor" height="32" /></a>

### GitHub Copilot Chat, VS Code

1. For setup, follow the [GitHub instructions on connecting MCP servers to Copilot Chat](https://docs.github.com/en/copilot/how-tos/provide-context/use-mcp-in-your-ide/extend-copilot-chat-with-mcp#configuring-mcp-servers-manually).

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

3. Start the server by clicking the `Start` button that appears in the `.vscode/mcp.json` file.

4. Select the `b24-dev-mcp` server in the chat tool list. Copilot requests context from MCP when generating code.

### Claude Desktop

1. Go to *Settings > Connectors*.

2. Click `Add custom connector`.

3. Fill in the fields:

   - `Name`: `b24-dev-mcp`

   - `URL`: `https://mcp-dev.bitrix24.com/mcp`

4. Save the settings. The `b24-dev-mcp` connector appears in the *Settings > Connectors* list.

### Claude Code CLI

1. Run the command:

   ```bash
   claude mcp add --transport http b24-dev-mcp https://mcp-dev.bitrix24.com/mcp
   ```

2. Check that the server has been added with the command `claude mcp list`.

3. After connecting, send requests as usual.

### Gemini CLI

1. Add the MCP server with the command:

   ```bash
   gemini mcp add --transport http b24-dev-mcp https://mcp-dev.bitrix24.com/mcp
   ```

2. Check that the server appears in the list with the command `gemini mcp list`.

3. After connecting, send requests as usual.

### Google Antigravity

1. Open the MCP Store menu by clicking `...` at the top of the agent panel.

2. Click `Manage MCP Servers`.

3. Select `View raw config`.

4. Add the connection settings in the opened `mcp_config.json` file:

   ```json
   {
     "mcpServers": {
       "b24-dev-mcp": {
         "serverUrl": "https://mcp-dev.bitrix24.com/mcp"
       }
     }
   }
   ```

5. Save the changes. The server appears in the `Manage MCP Servers` list.

## How to Check the Connection

That the server has been added is shown by the check step in the instructions for your AI tool. That it actually works is shown by a test request.

**Test request.** Send the following to the AI tool: "Use the Bitrix24 MCP server and show the parameters of the `crm.item.add` method". The response must name the required parameters of the method — `entityTypeId` and `fields`. General reasoning about CRM without parameter names means that the model answers from memory and did not access the server. This indicator works in any client.

**Tool list.** In clients with a graphical interface, the tools of the server appear next to it: there must be five of them, and every name starts with `bitrix-`. The `mcp list` commands in the CLI do not display this list — they show the servers themselves and the connection status.

If neither indicator works, troubleshoot with the [If the MCP Server Does Not Respond](#troubleshooting) section.

## How to Phrase Requests {#prompts}

The MCP server provides the model with up-to-date Bitrix24 REST API data, but AI tools call it in different ways.

#|
|| **AI Tool** | **When MCP Is Called** | **What to Do in the Request** ||
|| Codex CLI, Codex in VS Code | On explicit instruction | State that the MCP server and the official Bitrix24 documentation must be used ||
|| Cursor | Based on the chat context | Add the `mcp.json` file to the chat context ||
|| GitHub Copilot Chat, VS Code | Based on the selected tool set | Select the `b24-dev-mcp` server in the chat tool list ||
|| Claude Desktop, Claude Code CLI, Gemini CLI, Google Antigravity | Automatically | Nothing needs to be stated ||
|#

A universal phrasing that works in any of these tools: "Write an integration for Bitrix24 via REST API. Use the MCP server and the official Bitrix24 documentation to retrieve up-to-date methods".

Sample requests for specific tasks:

- "Find the Bitrix24 REST API method for creating a lead and show a sample request in JavaScript"

- "Write a `curl` request to create a lead in Bitrix24 with the fields name, company, and phone"

- "Find the method for updating a deal in CRM and list the required parameters"

- "Which Bitrix24 event fires when a deal is created, and what arrives in its handler"

## If the MCP Server Does Not Respond {#troubleshooting}

#|
|| **Symptom** | **Cause and Solution** ||
|| The client does not see the server or shows a connection error | The client uses the legacy HTTP+SSE transport. Switch it to Streamable HTTP — in the VS Code configuration, this is the `"type": "http"` field ||
|| The server responds with a 406 code | The client sends the `Accept` header with `application/json` only. It must accept `text/event-stream` as well ||
|| The server is in the list, but the AI tool answers without accessing it | The tool calls MCP only on explicit instruction or based on the selected tool set. Check the table in the [How to Phrase Requests](#prompts) section ||
|| The tool answered `not found` or returned the wrong material | An inexact name was passed. First find the object with `bitrix-search`, then request the details by the name from the search result ||
|| The request does not reach the server | Outbound HTTPS access to `mcp-dev.bitrix24.com` is blocked. Check your network or proxy settings ||
|#

## FAQ

### Why ChatGPT Invents Bitrix24 REST API Methods

The model has no access to up-to-date documentation. The MCP server gives it real API methods.

### How to Make Claude Use Bitrix24 Documentation

Connect the Bitrix24 MCP server — after that the model refers to the documentation directly.

### Can MCP Be Used to Generate Bitrix24 Integrations

Yes. Through the server, the model retrieves REST API methods and generates correct code.

### Do You Need Bitrix24 to Connect the MCP Server

No, the server works without it. You will need Bitrix24 later, when you run the finished code.

## What Is Next

- [Where to Start](../first-steps/index.md) — the recommended order for studying the REST API documentation if you work with it for the first time

- [How to Call REST API Methods](../settings/how-to-call-rest-api/index.md) — authorization, request structure, and response format for the code that the AI tool generated

- [REST API Reference](../api-reference/index.md) — the full list of sections and methods that the MCP server searches

- [REST API Limits](../limits.md) — limits on the number and rate of requests that you need to account for in your integration

- [Error Codes](../error-codes.md) — decoding of the errors if the generated request did not work

- [Vibecode](vibecode.md) — building an app for Bitrix24 from a task description, without writing code manually
