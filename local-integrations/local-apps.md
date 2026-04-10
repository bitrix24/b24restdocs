# Local Applications

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Local applications are those that are described and added directly to a specific Bitrix24. This is the essence of the term "local."

The portal administrator decides what access permissions to grant to such an application and how it will be named in the interface.

There are two types of such applications:

- "Static" applications based on HTML/JS. Essentially, using these technologies, you can implement single-page applications that interact with the Bitrix24 REST API via the JS SDK. In the interface, they are presented as a separate page (with a link from the left menu). Static applications cannot receive Bitrix24 events.
- "Server" applications with a back-end in any suitable programming languages (PHP, Python, etc.). They can access the REST API using the OAuth 2.0 protocol and can also receive events from Bitrix24 in their handlers. They are represented in the interface as a separate page, as well as in the form of embedded popup dialogs in available Bitrix24 embed objects. There is an option where the application does not manifest itself in the interface but uses REST.

## Continue Your Exploration

- [{#T}](static-local-app.md)
- [{#T}](serverside-local-app-with-ui.md)
- [{#T}](serverside-local-app-with-no-ui.md)