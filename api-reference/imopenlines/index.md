# Open Channels in Bitrix24: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Open Channels collect customer inquiries from websites, messengers, and social networks into a single queue and pass them on to operators. Three related method groups work with them in REST:

- [Open Channels](./openlines/index.md) — configure a channel and an operator queue, and process a dialog
- [Open Channels Connectors](./imconnector/index.md) — connect your own communication channel
- [Open Channels Statistics](./statistics/index.md) — export metrics for reports in an external system

Your own communication channel requires the first two groups: the connector registers the channel, and Open Channels process the inquiries coming from it. An Open Channel from another Bitrix24 is connected without a connector of your own.

{% note info "" %}

The `imconnector.*` methods work only in the context of an [application](../../settings/app-installation/index.md). A call through a webhook returns an authorization error.

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Contact Center](https://helpdesk.bitrix24.com/open/24095446/)

{% note tip "Common Use Cases and Scenarios" %}

- [How to Create an Open Channels Connector for Website Chat](../../tutorials/openlines/example-connector.md)
- [How to Find a CRM Object Created from an Open Channel Conversation](../../tutorials/openlines/find-crm-object-by-dialog.md)

{% endnote %}

## Relationships with Other Objects

Open Channels are related to chats and sessions, dialog participants, messages and chatbots, CRM, universal lists, and statistics data.

**Chat.** A customer inquiry is processed in an Open Channel chat. The chat links messages, operators, and sessions, and its `CHAT_ID` identifier is an end-to-end parameter across the methods of this section, starting with the [Open Channels Dialogs](./openlines/sessions/index.md) group.

**Session.** Within the chat, an inquiry proceeds as a session and receives its own `SESSION_ID`. To start a session, read the history, connect an operator, and switch the mode, use the methods of the same [Open Channels Dialogs](./openlines/sessions/index.md) group.

**Operator.** The channel distributes inquiries among the operators in the queue. The queue is configured by the methods of the [Open Channels](./openlines/index.md) group, while operator actions in an active dialog are performed by the methods of the [Open Channel Operators](./openlines/operators/index.md) group on behalf of the authorized user. Within this group, the transfer recipient is accepted by the [imopenlines.operator.transfer](./openlines/operators/imopenlines-operator-transfer.md) method: an employee in `USER_ID`, and the channel whose queue the dialog goes to in `QUEUE_ID`.

**External User.** A customer of an external channel is linked to the dialog through `USER_CODE`. Using this code, the methods of the [Open Channels Dialogs](./openlines/sessions/index.md) group find the customer's chat and session.

**Message.** The correspondence between a customer and an operator is linked to the chat and the session. To send a message to a chat linked to a CRM object, use the [imopenlines.crm.message.add](./openlines/messages/imopenlines-crm-message-add.md) method, and to retain a quick reply, use the [imopenlines.message.quick.save](./openlines/messages/imopenlines-message-quick-save.md) method. To react to messages being added, edited, and deleted, use [Open Channel events](./openlines/events/index.md).

**Chatbot.** A bot sends messages, transfers a dialog to an operator, and ends a session using the methods of the [Chatbots in Open Channels](./openlines/chat-bots/index.md) group.

**CRM.** An Open Channel chat is linked to a CRM object — a lead, a deal, a contact, or a company. The methods of the [CRM Chats](./openlines/chats/index.md) group find such chats and manage their participants: they accept the object type in `CRM_ENTITY_TYPE` and the identifier in `CRM_ENTITY`. The [imopenlines.crm.lead.create](./openlines/sessions/imopenlines-crm-lead-create.md) method creates a lead based on the dialog outcome.

**Universal Lists.** The quick replies of a channel are retained in a universal list. Its identifier is passed in the `QUICK_ANSWERS_IBLOCK_ID` parameter. You can read the value with the [imopenlines.config.get](./openlines/imopenlines-config-get.md) method, and set it when creating or updating a channel with the [imopenlines.config.add](./openlines/imopenlines-config-add.md) and [imopenlines.config.update](./openlines/imopenlines-config-update.md) methods.

**Statistics Data.** The accumulated metrics are related to sessions, channels, and operators. The methods of the [Open Channels Statistics](./statistics/index.md) group narrow the selection by channel and operator, and provide details on individual sessions by `sessionId`.

## Key Identifiers

#|
|| **Identifier** | **Description** | **Example** | **How to Obtain** ||
|| `CHAT_ID` | Identifier of an Open Channel chat. Needed to work with a dialog, messages, operators, and chatbots | `1763` | [imopenlines.session.open](./openlines/sessions/imopenlines-session-open.md) by the `USER_CODE` code — in the `chatId` field, [imopenlines.dialog.get](./openlines/sessions/imopenlines-dialog-get.md) — in the `id` field ||
|| `SESSION_ID` | Identifier of a session within a chat. Needed to read the history, process an inquiry, and retrieve detailed statistics | `321` | [imopenlines.session.history.get](./openlines/sessions/imopenlines-session-history-get.md) — in the `sessionId` field, by a known `CHAT_ID`. By a selection based on channel, source, and period — [imopenlines.v2.Session.list](./statistics/imopenlines-v2-session-list.md). The identifier is also included in the `entity_data_1` string of the [imopenlines.dialog.get](./openlines/sessions/imopenlines-dialog-get.md) response. The rule for parsing this string is described on the page of the `imopenlines.session.history.get` method ||
|| `USER_CODE` | External code of a communication channel user. Needed to uniquely link an external customer with a chat and a session in the channel. A composite value in the format ```<connector>|<LINE_ID>|<CONNECTOR_CHAT_ID>|<CONNECTOR_USER_ID>``` | ```livechat|22|1761|587``` | The external channel assembles it from four parts: the connector code, the channel identifier, the chat identifier, and the identifier of the interlocutor in the channel itself. The ready value is returned by [imopenlines.dialog.get](./openlines/sessions/imopenlines-dialog-get.md) in the `entity_id` field and accepted by [imopenlines.session.open](./openlines/sessions/imopenlines-session-open.md) ||
|| `USER_ID` | Identifier of a Bitrix24 user. Needed to assign the recipient when transferring a dialog, specify the sender in [imopenlines.crm.message.add](./openlines/messages/imopenlines-crm-message-add.md), and select sessions by operator. In most statistics methods the same identifier is named `operatorId` and `operatorIdList`, and in [imopenlines.v2.Operator.list](./statistics/imopenlines-v2-operator-list.md) — `userId` and `userIdList` | `1` | [user.get](../user/user-get.md), [user.search](../user/user-search.md) ||
|| `LINE`/`CONFIG_ID` | Identifier of an Open Channel. Needed to read channel data, change settings, link connector messages to the channel, and narrow the statistics selection. In the `imconnector.*` methods it is named `LINE`, in the methods that read, modify, and delete a specific channel — `CONFIG_ID`, and in the statistics methods — `configId` and `configIdList` | `22` | [imopenlines.config.add](./openlines/imopenlines-config-add.md) returns the identifier of the created channel in `result`, [imopenlines.config.list.get](./openlines/imopenlines-config-list-get.md) returns the identifiers of existing channels ||
|| `CONNECTOR` | Identifier of a connector. Needed to specify which connected channel an `imconnector.*` call refers to. Retained in lowercase | `myconnector` | Set in the `ID` parameter of the [imconnector.register](./imconnector/imconnector-register.md) method, then passed, for example, to the [imconnector.activate](./imconnector/imconnector-activate.md) and [imconnector.send.messages](./imconnector/imconnector-send-messages.md) methods ||
|| `source` | Code of an Open Channel source in the statistics methods. Needed to select sessions by inquiry source. For your own connector it matches its `CONNECTOR`. Built-in sources have their own code, which may differ from the first part of `USER_CODE` | `livechat` | Ready values arrive in the `source` field of the [imopenlines.v2.Session.list](./statistics/imopenlines-v2-session-list.md) response. The codes of available sources are returned by [imconnector.list](./imconnector/imconnector-list.md) ||
|#

The `CHAT_ID` and `USER_ID` identifiers are given in the table with the meaning that the `imopenlines.*` methods assign to them. In the [imconnector.chat.name.set](./imconnector/imconnector-chat-name-set.md) method the same names mean something else — these are the identifiers of the chat and the interlocutor in the external system, not in Bitrix24.

The [imconnector.send.messages](./imconnector/imconnector-send-messages.md) method returns the Bitrix24 `CHAT_ID` in the `session` object — this value is then passed on to the `imopenlines.*` methods. A full breakdown of connector identifiers is available in the [Open Channels Connectors](./imconnector/index.md) section.

## How to Get Started

### Connect Your Own Communication Channel via a Connector

1. Create an Open Channel with the [imopenlines.config.add](./openlines/imopenlines-config-add.md) method, or retrieve the identifier of an existing channel with the [imopenlines.config.list.get](./openlines/imopenlines-config-list-get.md) method — pass it in the `LINE` parameter starting from the third step
2. Register the connector with the [imconnector.register](./imconnector/imconnector-register.md) method
3. Activate the connector on the channel with the [imconnector.activate](./imconnector/imconnector-activate.md) method
4. Set the channel configurations with the [imconnector.connector.data.set](./imconnector/imconnector-connector-data-set.md) method. The order matters: the configurations appear in the operator interface only after the connector is activated
5. Check that the channel is ready with the [imconnector.status](./imconnector/imconnector-status.md) method
6. Pass a customer message with the [imconnector.send.messages](./imconnector/imconnector-send-messages.md) method
7. Subscribe to [connector events](./imconnector/events/index.md) to receive operator replies and pass them on to the external channel

### Connect an Open Channel from Another Bitrix24

1. Connect the channel with the [imopenlines.network.join](./openlines/imopenlines-network-join.md) method, using the 32-character `CODE` from the channel card in Contact Center
2. Send a message to a user on behalf of the connected channel with the [imopenlines.network.message.add](./openlines/imopenlines-network-message-add.md) method, using the same `CODE`

### Process an Inquiry in Your Own Channel

1. Retrieve the chat identifier with the [imopenlines.session.open](./openlines/sessions/imopenlines-session-open.md) method by the `USER_CODE` code
2. Accept the dialog with the [imopenlines.operator.answer](./openlines/operators/imopenlines-operator-answer.md) method
3. Read the correspondence with the [imopenlines.session.history.get](./openlines/sessions/imopenlines-session-history-get.md) method
4. Close the inquiry with the [imopenlines.operator.finish](./openlines/operators/imopenlines-operator-finish.md) method

You can transfer a dialog to another operator with the [imopenlines.operator.transfer](./openlines/operators/imopenlines-operator-transfer.md) method. For automation, connect [Open Channel events](./openlines/events/index.md).

### Build an Open Channels Report in an External System

1. Retrieve the aggregate metrics for the period from `dateFrom` to `dateTo` with the [imopenlines.v2.Stat.get](./statistics/imopenlines-v2-stat-get.md) method
2. Retrieve the list of sessions with the [imopenlines.v2.Session.list](./statistics/imopenlines-v2-session-list.md) method — its period is set by the pair `dateCreateFrom` and `dateCreateTo`, or `dateCloseFrom` and `dateCloseTo`
3. Get details on the selected sessions with the [imopenlines.v2.Session.Stat.get](./statistics/imopenlines-v2-session-stat-get.md) and [imopenlines.v2.Session.Transfer.list](./statistics/imopenlines-v2-session-transfer-list.md) methods — both accept the session identifiers from the second step
4. Export the sessions rated by customers with the [imopenlines.v2.Session.Rating.list](./statistics/imopenlines-v2-session-rating-list.md) method — its period is set by the required `dateVoteFrom` and `dateVoteTo`, which are the rating dates. Along with the customer rating, the method returns the supervisor rating if it is available under the user's plan and permissions
5. Retrieve the current operator load with the [imopenlines.v2.Operator.list](./statistics/imopenlines-v2-operator-list.md) method

## Limitations and Versions

- The `imopenlines.v2.*` branch is the only one for statistics. Here `v2` is part of the method name, not an API version prefix: there is no previous version of these methods in REST
- The section has no parallel or outdated method branches
- The [imopenlines.session.mode.silent](./openlines/sessions/imopenlines-session-mode-silent.md) method is marked with the `DEPRECATED` label but continues to work. It is the only method in the section whose development has been discontinued
- In the cloud, the statistics methods work if the plan allows using Open Channels reports. In the on-premise version, the plan restriction does not apply
- The [imopenlines.network.message.add](./openlines/imopenlines-network-message-add.md) method does not work with session authorization, and it sends no more than one message per week to each user. On accounts with a Partner license (NFR), the limit does not apply

## Overview of Methods and Events {#all-methods}

> Scope: [`imopenlines`](../scopes/permissions.md)
>
> Who can execute the method: depends on the method
>
> Who can subscribe: any user

The connector methods belong to the same `imopenlines` scope. The methods of the [Chatbots in Open Channels](./openlines/chat-bots/index.md) group require two scopes: `imopenlines` and `imbot`. You can subscribe to events only from an application, using the [event.bind](../events/event-bind.md) method. Working with events is described in the article [Concept and Benefits of Event Processing](../events/index.md).

Below are the reference articles of the section and the key methods and events of each group. Full lists of methods and events, as well as permissions, parameters, response schemas, examples, and error codes, are available in the subsection overviews and on the method pages.

### Reference Materials

#|
|| **Article** | **Description** ||
|| [Open Channels Statistics Data Types](./statistics/data-types.md) | Structures of the `session`, `rating`, `transfer`, and other objects returned by the `imopenlines.v2.*` methods ||
|#

### Configuring Channels and Working with Dialogs

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [Open Channels](./openlines/index.md) | To configure channels and operator queues, and to connect Bitrix24 Network | [imopenlines.config.add](./openlines/imopenlines-config-add.md), [imopenlines.config.list.get](./openlines/imopenlines-config-list-get.md), [imopenlines.network.join](./openlines/imopenlines-network-join.md) ||
|| [Open Channels Dialogs](./openlines/sessions/index.md) | To work with chats, sessions, correspondence history, dialog modes, and the supervisor rating for a closed session | [imopenlines.session.open](./openlines/sessions/imopenlines-session-open.md), [imopenlines.dialog.get](./openlines/sessions/imopenlines-dialog-get.md), [imopenlines.session.history.get](./openlines/sessions/imopenlines-session-history-get.md) ||
|| [Open Channel Operators](./openlines/operators/index.md) | For operator actions in an active dialog: accept, transfer, close | [imopenlines.operator.answer](./openlines/operators/imopenlines-operator-answer.md), [imopenlines.operator.transfer](./openlines/operators/imopenlines-operator-transfer.md), [imopenlines.operator.finish](./openlines/operators/imopenlines-operator-finish.md) ||
|| [Open Channels Messages](./openlines/messages/index.md) | To send messages to a chat linked to a CRM object and to retain quick replies | [imopenlines.crm.message.add](./openlines/messages/imopenlines-crm-message-add.md), [imopenlines.message.quick.save](./openlines/messages/imopenlines-message-quick-save.md) ||
|| [CRM Chats](./openlines/chats/index.md) | To find chats linked to CRM objects and manage their participants | [imopenlines.crm.chat.get](./openlines/chats/imopenlines-crm-chat-get.md), [imopenlines.crm.chat.user.add](./openlines/chats/imopenlines-crm-chat-user-add.md) ||
|| [Chatbots in Open Channels](./openlines/chat-bots/index.md) | To automate a dialog with a bot: reply, transfer to an operator, close the session | [imopenlines.bot.session.message.send](./openlines/chat-bots/imopenlines-bot-session-message-send.md), [imopenlines.bot.session.operator](./openlines/chat-bots/imopenlines-bot-session-operator.md), [imopenlines.bot.session.finish](./openlines/chat-bots/imopenlines-bot-session-finish.md) ||
|#

#|
|| **Section** | **When to Use** | **Key Events** ||
|| [Open Channel Events](./openlines/events/index.md) | To react to messages and to the start and end of a session | [OnOpenLineMessageAdd](./openlines/events/on-open-line-message-add.md), [OnSessionStart](./openlines/events/on-session-start.md), [OnSessionFinish](./openlines/events/on-session-finish.md) ||
|#

### Connectors and Statistics

#|
|| **Section** | **When to Use** | **Key Methods** ||
|| [Open Channels Connectors](./imconnector/index.md) | To connect your own communication channel and exchange messages with it | [imconnector.register](./imconnector/imconnector-register.md), [imconnector.activate](./imconnector/imconnector-activate.md), [imconnector.send.messages](./imconnector/imconnector-send-messages.md) ||
|| [Open Channels Statistics](./statistics/index.md) | For reports and dashboards in an external system: aggregates, sessions, ratings, transfers, operator load | [imopenlines.v2.Stat.get](./statistics/imopenlines-v2-stat-get.md), [imopenlines.v2.Session.list](./statistics/imopenlines-v2-session-list.md), [imopenlines.v2.Operator.list](./statistics/imopenlines-v2-operator-list.md) ||
|#

#|
|| **Section** | **When to Use** | **Key Events** ||
|| [Connector Events](./imconnector/events/index.md) | To react to operator replies, the creation and closing of a dialog, source status removal, and channel deletion | [OnImConnectorMessageAdd](./imconnector/events/on-im-connector-message-add.md), [OnImConnectorDialogStart](./imconnector/events/on-im-connector-dialog-start.md), [OnImConnectorStatusDelete](./imconnector/events/on-im-connector-status-delete.md) ||
|#
