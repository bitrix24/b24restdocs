# Overview of Deprecated Chatbot Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% note warning "" %}

**DEPRECATED**

Development of this section has been halted.
Please refer to the current sections:
- [Chatbots 2.0](../chat-bots-v2/index.md)
- [Messenger im.v2](../chat-bots-v2/im.v2/index.md)

{% endnote %}

## Overview of Methods

#|
|| **Section** | **Methods** ||
|| Chatbots | [imbot.*](./bots/index.md) ||
|| Chats | [imbot.chat.*](./chats/index.md), [imbot.dialog.get](./chats/imbot-dialog-get.md) ||
|| Commands | [imbot.command.*](./commands/index.md) ||
|| Messages | [imbot.message.*](./messages/index.md) ||
|#

## Overview of Events

#|
|| **Section** | **Events** ||
|| Chatbot Events | [ONIMBOTDELETE](./events/on-imbot-delete.md) ||
|| Chat Events | [ONIMBOTJOINCHAT](./chats/events/on-imbot-join-chat.md) ||
|| Command Events | [ONIMCOMMANDADD](./commands/events/on-im-command-add.md) ||
|| Message Events | [ONIMBOTMESSAGEADD](./messages/events/on-imbot-message-add.md), [ONIMBOTMESSAGEUPDATE](./messages/events/on-imbot-message-update.md), [ONIMBOTMESSAGEDELETE](./messages/events/on-imbot-message-delete.md) ||
|#