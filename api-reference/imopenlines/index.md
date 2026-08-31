# Open Channels in Bitrix24: Overview of Methods and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Open Channels bring customer inquiries from websites, messengers, and social networks into a single processing flow. Three related method groups are available for integrations:

- [Open Channels](./openlines/index.md) — configuring lines, operator queues, sessions, messages, chatbots, events, and CRM links
- [Connectors](./imconnector/index.md) — connecting and maintaining external communication channels through which customers write to open channels
- [Open Channels Statistics](./statistics/index.md) — retrieving aggregates, sessions, ratings, transfers, and current operator load

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Contact Center](https://helpdesk.bitrix24.com/open/24095446/)

## How to Choose a Section

#| 
|| **Scenario** | **What to Use** ||
|| Set up a line, operator queues, and inquiry processing rules | The [Open Channels](./openlines/index.md) method group ||
|| Connect your own external communication channel to Bitrix24 | The [Open Channels Connectors](./imconnector/index.md) method group ||
|| Build an external dashboard or export data for analytics | The [Open Channels Statistics](./statistics/index.md) method group ||
|| Connect an external open channel from another Bitrix24 | Method [imopenlines.network.join](./openlines/imopenlines-network-join.md) ||
|| Subscribe to message, dialog, and status events | Events from the [Connectors Events](./imconnector/events/index.md) and [Open Channels Events](./openlines/events/index.md) sections ||
|#

## How Open Channels and Connectors are Related

Both sets of methods are used for your own external channel: `imconnector.*` for registering and maintaining the connector, and `imopenlines.*` for configuring the line and processing dialogs. If you need to connect an external open channel from another Bitrix24 without registering your own connector, use the method [imopenlines.network.join](./openlines/imopenlines-network-join.md).

## Relationships of Open Channels and Connectors with Other Objects

**Chat.** The system processes inquiries as an open channel chat. Operations with messages, operators, and sessions are performed through the chat.

**Session.** Within the chat, the inquiry proceeds as a session. To start a session, read history, connect an operator, and manage modes, use the group of methods [imopenlines.session.*](./openlines/sessions/index.md).

**Operator.** The line distributes inquiries among operators from the queue. Queue configuration is done through `imopenlines.config.*`, while operator actions in an active dialog are handled through the group of methods [imopenlines.operator.*](./openlines/operators/index.md). To transfer a dialog to an operator, use `USER_ID`, which can be obtained through the methods [user.get](../user/user-get.md) and [user.search](../user/user-search.md).

**External User.** A client from the external channel is linked to the dialog through `USER_CODE`. This identifier allows the system to determine which chat and session the inquiry belongs to.

**Message.** The correspondence between the customer and the operator is associated with the chat and session. Send a message using [imopenlines.crm.message.add](./openlines/messages/imopenlines-crm-message-add.md), and save a quick reply using [imopenlines.message.quick.save](./openlines/messages/imopenlines-message-quick-save.md). To react to message changes, use [Open Channels events](./openlines/events/index.md).

**Chatbot.** The bot can send messages, transfer the dialog to an operator, and end the session using methods from the group [imopenlines.bot.*](./openlines/chat-bots/index.md).

**CRM.** Open channel dialogs can be linked to CRM objects. To retrieve CRM chats, use the group of methods [imopenlines.crm.chat.*](./openlines/chats/index.md), and to create a lead based on the dialog outcome — the method [imopenlines.crm.lead.create](./openlines/sessions/imopenlines-crm-lead-create.md).

**Universal Lists.** Quick replies are associated with the identifier `QUICK_ANSWERS_IBLOCK_ID`. You can obtain it using the method [imopenlines.config.get](./openlines/imopenlines-config-get.md), and set it when creating or updating a line using the methods [imopenlines.config.add](./openlines/imopenlines-config-add.md) and [imopenlines.config.update](./openlines/imopenlines-config-update.md).

**Statistics Data.** Accumulated data on sessions, ratings, transfers, and operator load is available through the [Open Channels Statistics](./statistics/index.md) method group. Response object structures are described in [Open Channels Statistics Data Types](./statistics/data-types.md).

## Key Identifiers

#| 
|| **Identifier** | **Description** | **How to Obtain** ||
|| `CHAT_ID` | Identifier of the open channel chat. Needed for actions with dialogs, messages, operators, and chatbots | [imopenlines.session.open](./openlines/sessions/imopenlines-session-open.md), [imopenlines.dialog.get](./openlines/sessions/imopenlines-dialog-get.md) ||
|| `SESSION_ID` | Identifier of the session within the chat. Needed for reading history, managing the inquiry processing flow, and retrieving detailed statistics | [imopenlines.session.history.get](./openlines/sessions/imopenlines-session-history-get.md), [imopenlines.v2.Session.list](./statistics/imopenlines-v2-session-list.md) ||
|| `USER_CODE` | External code of the user from the communication channel. Needed to uniquely link the external client with the chat and session in the line | Generated by the external channel, used in [imopenlines.session.open](./openlines/sessions/imopenlines-session-open.md) ||
|| `USER_ID` | Identifier of the Bitrix24 user. Needed for routing the dialog between operators and operator actions | [user.get](../user/user-get.md), [user.search](../user/user-search.md) ||
|| `LINE`/`CONFIG_ID` | Identifier of the open channel. In the methods `imconnector.*`, the parameter `LINE` is usually used, in the methods `imopenlines.config.*`, `CONFIG_ID` is used, and in statistics methods, `configId` and `configIdList` are used. This identifier is needed to retrieve line data, modify settings, link connector messages, and filter statistics | [imopenlines.config.get](./openlines/imopenlines-config-get.md), [imopenlines.config.list.get](./openlines/imopenlines-config-list-get.md), [imopenlines.config.update](./openlines/imopenlines-config-update.md) ||
|| `CONNECTOR` | Identifier of the connector. Needed for the methods `imconnector.*` to work with the correct connected connector | Set during registration in [imconnector.register](./imconnector/imconnector-register.md) in the `ID` parameter, then passed, for example, in [imconnector.activate](./imconnector/imconnector-activate.md) and [imconnector.send.messages](./imconnector/imconnector-send-messages.md) ||
|| `source` | Open channel source code. Needed to filter statistics by inquiry channel | [imconnector.list](./imconnector/imconnector-list.md) ||
|#

## How to Get Started

### Connect Your Own External Channel via Connector

1. Register and activate the connector: [imconnector.register](./imconnector/imconnector-register.md), [imconnector.activate](./imconnector/imconnector-activate.md)
2. Create an open channel and link it to the channel: [imopenlines.config.add](./openlines/imopenlines-config-add.md), [imconnector.connector.data.set](./imconnector/imconnector-connector-data-set.md)
3. Check message sending and connector status: [imconnector.send.messages](./imconnector/imconnector-send-messages.md), [imconnector.status](./imconnector/imconnector-status.md)

### Connect an External Open Channel from Another Bitrix24

1. Connect the line using the method [imopenlines.network.join](./openlines/imopenlines-network-join.md)
2. Check dialog processing: [imopenlines.session.open](./openlines/sessions/imopenlines-session-open.md), [imopenlines.operator.answer](./openlines/operators/imopenlines-operator-answer.md), [imopenlines.crm.message.add](./openlines/messages/imopenlines-crm-message-add.md)

To automate, connect events: [Open Channels Events](./openlines/events/index.md), [Connectors Events](./imconnector/events/index.md)

### Build an External Open Channels Report

1. Retrieve aggregate metrics using [imopenlines.v2.Stat.get](./statistics/imopenlines-v2-stat-get.md)
2. Retrieve the list of sessions using [imopenlines.v2.Session.list](./statistics/imopenlines-v2-session-list.md)
3. For details on selected sessions, use [imopenlines.v2.Session.Stat.get](./statistics/imopenlines-v2-session-stat-get.md) or [imopenlines.v2.Session.Transfer.list](./statistics/imopenlines-v2-session-transfer-list.md)
4. To monitor current operator load, use [imopenlines.v2.Operator.list](./statistics/imopenlines-v2-operator-list.md)

## Overview of Methods and Events {#all-methods}

> Scope: [`imopenlines`](../scopes/permissions.md), [`imconnector`](../scopes/permissions.md)
>
> Who can execute methods and events: depending on the method, event, and access permissions to open channels

### Open Channels

#| 
|| **Section** | **Description** ||
|| [Open Channels](./openlines/index.md) | Methods for configuring lines, queues, dialogues, messages, and CRM links ||
|| [Open Channel Dialogues](./openlines/sessions/index.md) | Methods for working with chats, sessions, history, and dialogue modes ||
|| [Open Channel Operators](./openlines/operators/index.md) | Methods for transferring, closing, and distributing dialogues between operators ||
|| [Open Channel Messages](./openlines/messages/index.md) | Methods for sending messages and saving quick replies ||
|| [CRM Chats](./openlines/chats/index.md) | Methods for searching CRM chats and managing participants ||
|| [Chatbots in Open Channels](./openlines/chat-bots/index.md) | Methods for automating dialogues using chatbots ||
|| [Open Channel Events](./openlines/events/index.md) | Events for Open Channel messages and sessions ||
|#

### Open Channels Statistics

#|
|| **Section** | **Description** ||
|| [Open Channels Statistics](./statistics/index.md) | Methods for retrieving aggregates, sessions, ratings, transfers, and operator load ||
|#

### Open Channels Connectors

#| 
|| **Section** | **Description** ||
|| [Open Channels Connectors](./imconnector/index.md) | Methods for registering connectors, connecting channels, and exchanging messages ||
|| [Connector Events](./imconnector/events/index.md) | Events for connector messages, dialogues, lines, and statuses ||
|#
