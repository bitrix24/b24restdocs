# Deprecated Chat Application Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "" %}

**DEPRECATED**

The development of `imbot.app.*` methods has been halted. The `imbot.app.*` methods only function in the old version of the messenger. Integrate applications through [Chat Widgets](../../widgets/im/index.md).

{% endnote %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

## Overview of Methods

#| 
|| **Method** | **Description** ||
|| [imbot.app.register](./create-app/imbot-app-register.md) | Registers a chat application ||
|| [imbot.app.update](./create-app/imbot-app-update.md) | Updates application data in the chat ||
|| [imbot.app.unregister](./create-app/imbot-app-unregister.md) | Removes the application from the chat ||
|#

## Overview of Pages

#| 
|| **Section** | **Pages** ||
|| About Chat Applications | [About Chat Applications](./chat-apps.md), [About Creating an Application](./create-app/index.md) ||
|| Context and IFRAME | [Contextual Applications](./context.md), [Create IFRAME Handler](./iframe.md) ||
|| Design | [Create Chat Application Icon](./icon.md) ||
|#