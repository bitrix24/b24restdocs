# BX24.js: Library Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

BX24.js is a JavaScript library for applications embedded within the Bitrix24 interface. It allows you to call Bitrix24 methods from the client side of your application and automatically adds OAuth 2.0 authorization data.

The library also links the application interface with the Bitrix24 interface. Using it, you can manage the frame size, open standard Bitrix24 dialogs and pages, retrieve runtime environment data, and store user and application configurations.

> Quick links: [BX24.js Page Overview](#all-pages)

## Getting Started

1. Include the library on the application page:

   ```html
   <script src="//api.bitrix24.com/api/v1/"></script>
   ```

2. Wait for the library to initialize using [BX24.init](./system-functions/bx24-init.md)
3. Select a function group from the [BX24.js Page Overview](#all-pages) table
4. Call the required function and handle the result in your application's client-side code

## Integration With Other Tools

**Placements.** An application can be opened in a selected location within the Bitrix24 interface. To register a handler, use the [placement.bind](../../api-reference/widgets/placement-bind.md) method: pass the placement code in the `PLACEMENT` parameter and the external widget handler URL in the `HANDLER` parameter. This method is available to an administrator.

**UI Kit.** While BX24.js handles the interaction between the application and Bitrix24, the [Bitrix24 UI Kit](../../api-reference/widgets/ui-kit/index.md) helps you build an interface using ready-made components, design tokens, and icons.

## Key Considerations

- For external applications and webhooks, select a different library in the [SDK Overview](../index.md)
- The required scope depends on the method being called. See scope values in the [Permissions](../../api-reference/scopes/permissions.md) guide
- Data access depends on the permissions of the user on whose behalf the application performs the request

## BX24.js Page Overview {#all-pages}

#|
|| **If you need to** | **Open the page** ||
|| Call one or more Bitrix24 methods from the client side of the application | [Calling REST methods](./how-to-call-rest-methods/index.md) ||
|| Initialize the library, handle the first run, or get authorization data | [Initialization and authorization](./system-functions/index.md) ||
|| Save user settings or general application settings | [Application settings](./options/index.md) ||
|| Open the standard dialog for selecting users, permissions, or CRM items | [System dialogs](./system-dialogues/index.md) ||
|| Manage the application window, navigation, page events, and call context | [Interface, navigation, and context](./additional-functions/index.md) ||
|#