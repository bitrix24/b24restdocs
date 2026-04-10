# Open Channels: Overview of Methods and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Open channels in Bitrix24 consolidate requests from external channels into a single stream and direct them to employees. The channel maintains routing rules, operator queues, auto-response scripts, working hours, and CRM integration.

The workflow operates in a chain: a client writes to a connected channel, the system creates a chat and opens a session, after which the dialogue is assigned to an employee according to the channel's rules. Within the session, the operator responds to the client, transfers the dialogue, connects a colleague, or concludes the request.

This section describes the management of each step in this chain: configuring channels, working with chats and sessions, operator actions, messages, chatbots, and events.

> Quick Navigation: [All Methods and Events](#all-methods)
>
> User Documentation: [How to Create and Configure an Open Channel](https://helpdesk.bitrix24.com/open/25385203/)

## How to Choose a Subsection

#|
|| **Scenario** | **What to Use** ||
|| Create a channel, get a list of channels, update or delete settings | Methods `imopenlines.config.*` ||
|| Connect an external open channel from another Bitrix24 to the current account | Method [imopenlines.network.join](./imopenlines-network-join.md) ||
|| Open a dialogue, work with a session, and retrieve message history | Methods from the [Dialogs](./sessions/index.md) section ||
|| Accept a dialogue, transfer it to another employee, or conclude it | Methods from the [Operators](./operators/index.md) section ||
|| Send a message in a dialogue or save a quick response | Methods from the [Messages](./messages/index.md) section ||
|| Manage the chat linked to CRM | Methods from the [CRM Chats](./chats/index.md) section ||
|| Automate the dialogue using a chatbot | Methods from the [Chatbots](./chat-bots/index.md) section ||
|| Subscribe to events related to messages and sessions | [Events](./events/index.md) ||
|#

## Connection of Open Channels with Other Objects

**Chat.** Requests in an open channel are processed in a chat. To find the chat and retrieve its data, use the methods [imopenlines.session.open](./sessions/imopenlines-session-open.md) and [imopenlines.dialog.get](./sessions/imopenlines-dialog-get.md). For operations with CRM chats, refer to the [CRM Chats](./chats/index.md) section. The main identifier is `CHAT_ID`.

**Session.** Within the chat, requests are treated as sessions. The [Dialogs](./sessions/index.md) section provides methods for starting a session, connecting an operator, reading history, and managing modes. The main identifier is `SESSION_ID`.

**Employee.** The channel distributes requests among employees from the queue. The queue and distribution rules are configured through `imopenlines.config.*`, while the employee's work in an active dialogue is managed through the [Operators](./operators/index.md) section.

**External User.** Messages from the client in the external channel create a chat and session in the channel. The `USER_CODE` is used to communicate with the user.

**Message.** Message exchanges in the open channel are conducted using methods from the [Messages](./messages/index.md) section. Messages are linked to the chat, session, and participate in [events](./events/index.md).

**Chatbot.** To automate the dialogue, use methods from the [Chatbots](./chat-bots/index.md) section. A chatbot can send messages, transfer the dialogue, and conclude the session.

**CRM.** Open channels are connected to CRM. Using methods from the [CRM Chats](./chats/index.md) section, you can retrieve a chat linked to a CRM object. The method [imopenlines.crm.lead.create](./sessions/imopenlines-crm-lead-create.md) allows you to create a lead based on the dialogue.

## How to Get Started

1. Connect the source of requests:
   - external channel via a connector according to the scenario from the article [Open Channels in Bitrix24: API of Channels and Connectors](../index.md),
   - external open channel from another Bitrix24 using the method [imopenlines.network.join](./imopenlines-network-join.md).
2. Open a dialogue and retrieve the identifiers `CHAT_ID` and `SESSION_ID` in the [Dialogs](./sessions/index.md) section.
3. Configure the processing of the dialogue by employees in the [Operators](./operators/index.md) section.
4. For additional scenarios, use the sections [Messages](./messages/index.md), [Chatbots](./chat-bots/index.md), and [Events](./events/index.md).

{% note tip "User Documentation" %}

- [How to Work with Chats in Open Channels](https://helpdesk.bitrix24.com/open/25761311/)
- [Quick Responses in Open Channels: How to Create and Configure](https://helpdesk.bitrix24.com/open/25760371/)
- [Open Channels: How to Obtain Consent for Personal Data Processing](https://helpdesk.bitrix24.com/open/25828127/)

{% endnote %}

## Limits and Response Format

**Limits.** General REST limitations and open channel restrictions apply to methods, such as the limit on unclosed dialogues for operators. For details, refer to the article [Limit on Unclosed Dialogues for Open Channel Operators](https://helpdesk.bitrix24.com/open/26928654/).

**Response Format.** A successful response typically contains `result` and `time`, while errors are returned in the format `error` and `error_description`. Examples of response structure and error codes can be found on the specific method's page.

## Overview of Methods and Events {#all-methods}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method and access permissions to open channels

### Configuration of Open Channels and Integration

#|
|| **Method** | **Description** ||
|| [imopenlines.config.add](./imopenlines-config-add.md) | Adds a new open channel ||
|| [imopenlines.config.update](./imopenlines-config-update.md) | Modifies the settings of an open channel ||
|| [imopenlines.config.get](./imopenlines-config-get.md) | Retrieves an open channel by identifier ||
|| [imopenlines.config.list.get](./imopenlines-config-list-get.md) | Retrieves a list of open channels ||
|| [imopenlines.config.delete](./imopenlines-config-delete.md) | Deletes an open channel ||
|| [imopenlines.config.path.get](./imopenlines-config-path-get.md) | Retrieves the link to the public page of the open channels on the account ||
|| [imopenlines.network.join](./imopenlines-network-join.md) | Connects an external open channel to the account ||
|| [imopenlines.network.message.add](./imopenlines-network-message-add.md) | Sends a message to the user on behalf of the open channel ||
|| [imopenlines.revision.get](./imopenlines-revision-get.md) | Retrieves information about API revisions ||
|#

### Chatbots in Open Channels

#|
|| **Method** | **Description** ||
|| [imopenlines.bot.session.message.send](./chat-bots/imopenlines-bot-session-message-send.md) | Sends an automated message in the dialogue ||
|| [imopenlines.bot.session.operator](./chat-bots/imopenlines-bot-session-operator.md) | Switches the dialogue to an available operator ||
|| [imopenlines.bot.session.transfer](./chat-bots/imopenlines-bot-session-transfer.md) | Transfers the dialogue to an operator or queue ||
|| [imopenlines.bot.session.finish](./chat-bots/imopenlines-bot-session-finish.md) | Concludes the dialogue ||
|#

### CRM Chats in Open Channels

#|
|| **Method** | **Description** ||
|| [imopenlines.crm.chat.get](./chats/imopenlines-crm-chat-get.md) | Retrieves a chat by CRM object ||
|| [imopenlines.crm.chat.getLastId](./chats/imopenlines-crm-chat-get-last-id.md) | Retrieves the identifier of the last chat ||
|| [imopenlines.crm.chat.user.add](./chats/imopenlines-crm-chat-user-add.md) | Adds a user to an existing chat ||
|| [imopenlines.crm.chat.user.delete](./chats/imopenlines-crm-chat-user-delete.md) | Removes a user from the chat ||
|#

### Messages in Open Channels

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [imopenlines.crm.message.add](./messages/imopenlines-crm-message-add.md) | Sends a message in the open channel ||
    || [imopenlines.message.quick.save](./messages/imopenlines-message-quick-save.md) | Saves a message as a quick response ||
    |#

- Events

    #|
    || **Event** | **Triggered By** ||
    || [OnOpenLineMessageAdd](./events/on-open-line-message-add.md) | When a message is added to the chat ||
    || [OnOpenLineMessageUpdate](./events/on-open-line-message-update.md) | When a message is updated in the chat ||
    || [OnOpenLineMessageDelete](./events/on-open-line-message-delete.md) | When a message is deleted from the chat ||
    |#

{% endlist %}

### Operators of Open Channels

#|
|| **Method** | **Description** ||
|| [imopenlines.operator.answer](./operators/imopenlines-operator-answer.md) | Transfers the dialogue to the current operator ||
|| [imopenlines.operator.finish](./operators/imopenlines-operator-finish.md) | Concludes the dialogue on behalf of the current operator ||
|| [imopenlines.operator.another.finish](./operators/imopenlines-operator-another-finish.md) | Concludes the dialogue of another operator ||
|| [imopenlines.operator.skip](./operators/imopenlines-operator-skip.md) | Transfers the dialogue to the next operator in the queue ||
|| [imopenlines.operator.spam](./operators/imopenlines-operator-spam.md) | Marks the dialogue as spam ||
|| [imopenlines.operator.transfer](./operators/imopenlines-operator-transfer.md) | Transfers the dialogue to another operator or to another channel ||
|#

### Dialogues of Open Channels

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [imopenlines.session.open](./sessions/imopenlines-session-open.md) | Retrieves the chat identifier by user code ||
    || [imopenlines.dialog.get](./sessions/imopenlines-dialog-get.md) | Retrieves the operator's dialogue data ||
    || [imopenlines.session.start](./sessions/imopenlines-session-start.md) | Starts a new session in the chat ||
    || [imopenlines.message.session.start](./sessions/imopenlines-message-session-start.md) | Starts a new session based on a message ||
    || [imopenlines.session.history.get](./sessions/imopenlines-session-history-get.md) | Retrieves message history and session data ||
    || [imopenlines.session.join](./sessions/imopenlines-session-join.md) | Joins the operator to the dialogue ||
    || [imopenlines.session.intercept](./sessions/imopenlines-session-intercept.md) | Transfers the dialogue to the current operator ||
    || [imopenlines.session.mode.pin](./sessions/imopenlines-session-mode-pin.md) | Pins or unpins the selected dialogue ||
    || [imopenlines.session.mode.pinAll](./sessions/imopenlines-session-mode-pin-all.md) | Pins all available dialogues to the operator ||
    || [imopenlines.session.mode.unpinAll](./sessions/imopenlines-session-mode-unpin-all.md) | Unpins all pinned dialogues of the operator ||
    || [imopenlines.session.mode.silent](./sessions/imopenlines-session-mode-silent.md) | Enables or disables silent mode for the dialogue ||
    || [imopenlines.session.head.vote](./sessions/imopenlines-session-head-vote.md) | Saves the supervisor's rating for the completed session ||
    || [imopenlines.crm.lead.create](./sessions/imopenlines-crm-lead-create.md) | Creates a CRM lead based on the dialogue ||
    |#

- Events

    #|
    || **Event** | **Triggered By** ||
    || [OnSessionStart](./events/on-session-start.md) | When a chat is created ||
    || [OnSessionFinish](./events/on-session-finish.md) | When a chat is closed ||
    |#

{% endlist %}