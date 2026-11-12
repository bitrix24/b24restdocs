# Open Channels: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Open channels in Bitrix24 consolidate requests from external channels into a single stream and direct them to employees. The channel maintains routing rules, operator queues, auto-response scripts, work hours, and CRM integration.

The workflow operates in a chain: a customer writes to a connected channel, the system creates a chat and opens a session, after which the dialogue is assigned to an employee according to the channel's rules. Within the session, the operator responds to the customer, transfers the dialogue, connects a colleague, or concludes the request.

Open Channels methods and events manage each step in this chain: configuring channels, working with chats and sessions, operator actions, messages, chatbots, and events.

> Quick navigation: [All Methods and Events](#all-methods)
>
> User documentation: [How to Create and Configure an Open Channel](https://helpdesk.bitrix24.com/open/25385203/)

## How to Choose a Subsection

#|
|| **Scenario** | **What to use** ||
|| Create a line, get a list of lines, update or delete settings | Methods [imopenlines.config.*](#config) ||
|| Add another company's open channel to your Bitrix24 so that employees can message it | Method [imopenlines.network.join](./imopenlines-network-join.md) ||
|| Open a dialog, work with a session, and get message history | Methods from the [Dialogs](./sessions/index.md) section ||
|| Accept a dialog, transfer it to another employee, or end it | Methods from the [Operators](./operators/index.md) section ||
|| Send a message in a dialog or save a quick reply | Methods from the [Messages](./messages/index.md) section ||
|| Manage a chat associated with CRM | Methods from the [CRM Chats](./chats/index.md) section ||
|| Automate a dialog using a chatbot | Methods from the [Chatbots](./chat-bots/index.md) section ||
|| Subscribe to events for messages and sessions | [Events](./events/index.md) ||
|| Connect your own external communication channel from an application | Methods from the [Open Channels Connectors](../imconnector/index.md) section ||
|| Retrieve dialog statistics and ratings | Methods from the [Open Channels Statistics](../statistics/index.md) section ||
|#

## Connection with Other Objects

**Chat.** Requests in an open channel are processed in a chat. To find the chat and retrieve its data, use the methods [imopenlines.session.open](./sessions/imopenlines-session-open.md) and [imopenlines.dialog.get](./sessions/imopenlines-dialog-get.md). For operations with CRM chats, use methods from the [CRM Chats](./chats/index.md) section. The main identifier is `CHAT_ID`.

**Session.** Within the chat, requests are treated as sessions. The [Dialogs](./sessions/index.md) section provides methods for starting a session, connecting an operator, reading history, and managing modes. The main identifier is `SESSION_ID`.

**Employee.** The channel distributes requests among employees from the queue. The queue and distribution rules are configured through `imopenlines.config.*`, while the employee's work in an active dialogue is managed through the [Operators](./operators/index.md) section.

**External User.** Messages from the customer in the external channel create a chat and session in the channel. The `USER_CODE` is used to communicate with the user.

**Message.** Message exchanges in the open channel are conducted using methods from the [Messages](./messages/index.md) section. Messages are linked to the chat, session, and participate in [events](./events/index.md).

**Chatbot.** To automate the dialogue, use methods from the [Chatbots](./chat-bots/index.md) section. A chatbot can send messages, transfer the dialogue, and conclude the session.

**CRM.** Open channels are connected to CRM. Using methods from the [CRM Chats](./chats/index.md) section, you can retrieve a chat linked to a CRM object. The method [imopenlines.crm.lead.create](./sessions/imopenlines-crm-lead-create.md) allows you to create a lead based on the dialogue. If the channel is configured for deals, the method creates a deal with a contact.

## Getting Started

1. Create an open channel using the [imopenlines.config.add](./imopenlines-config-add.md) method or retrieve the identifier of an existing one using the [imopenlines.config.list.get](./imopenlines-config-list-get.md) method
2. Connect an external channel to the open channel via a connector using methods from the [Open Channels Connectors](../imconnector/index.md) section
3. Open a conversation and retrieve the `CHAT_ID` and `SESSION_ID` identifiers using methods from the [Dialogs](./sessions/index.md) section
4. Process the conversation on behalf of an operator using methods from the [Operators](./operators/index.md) section
5. For additional scenarios, use methods from the [Messages](./messages/index.md) and [Chatbots](./chat-bots/index.md) sections. To receive notifications about messages and sessions, subscribe to [events](./events/index.md)

{% note tip "User documentation" %}

- [Use chats in Open Channels](https://helpdesk.bitrix24.com/open/25967455/)
- [Canned Responses in Open Channels: How to Create and Configure](https://helpdesk.bitrix24.com/open/25760371/)
- [Open Channels: How to Obtain Consent for Personal Data Processing](https://helpdesk.bitrix24.com/open/25828127/)

{% endnote %}

## Limits and Response Format

**Limits.** General REST limitations and open channel restrictions apply to methods, such as the limit on unclosed conversations for operators. For details, refer to the article [Limit on Unclosed Conversations for Open Channel Operators](https://helpdesk.bitrix24.com/open/25830887/).

**Response Format.** A successful response typically contains `result` and `time`, while errors are returned in the format `error` and `error_description`. Examples of response structure and error codes can be found on the specific method's page.

## Overview of Methods and Events {#all-methods}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method and access permissions to open channels

### Configuration of Open Channels and Integration {#config}

#|
|| **Method** | **Description** ||
|| [imopenlines.config.add](./imopenlines-config-add.md) | Adds a new open channel ||
|| [imopenlines.config.update](./imopenlines-config-update.md) | Modifies the settings of an open channel ||
|| [imopenlines.config.get](./imopenlines-config-get.md) | Retrieves an open channel by identifier ||
|| [imopenlines.config.list.get](./imopenlines-config-list-get.md) | Retrieves a list of open channels ||
|| [imopenlines.config.delete](./imopenlines-config-delete.md) | Deletes an open channel ||
|| [imopenlines.config.path.get](./imopenlines-config-path-get.md) | Retrieves the Bitrix24 address and the path to the public page of open channels ||
|| [imopenlines.network.join](./imopenlines-network-join.md) | Connects an external open channel to Bitrix24 ||
|| [imopenlines.network.message.add](./imopenlines-network-message-add.md) | Sends a message to the user on behalf of the open channel ||
|| [imopenlines.revision.get](./imopenlines-revision-get.md) | Retrieves information about API revisions ||
|#

### Chatbots in Open Channels

#|
|| **Method** | **Description** ||
|| [imopenlines.bot.session.message.send](./chat-bots/imopenlines-bot-session-message-send.md) | Sends an automatic message in the dialogue ||
|| [imopenlines.bot.session.operator](./chat-bots/imopenlines-bot-session-operator.md) | Switches the dialogue to a free operator ||
|| [imopenlines.bot.session.transfer](./chat-bots/imopenlines-bot-session-transfer.md) | Transfers the dialogue to an operator or queue ||
|| [imopenlines.bot.session.finish](./chat-bots/imopenlines-bot-session-finish.md) | Ends the dialogue ||
|#

### CRM Chats in Open Channels

#|
|| **Method** | **Description** ||
|| [imopenlines.crm.chat.get](./chats/imopenlines-crm-chat-get.md) | Retrieves the chats of a CRM object ||
|| [imopenlines.crm.chat.getLastId](./chats/imopenlines-crm-chat-get-last-id.md) | Retrieves the identifier of the last chat of a CRM object ||
|| [imopenlines.crm.chat.user.add](./chats/imopenlines-crm-chat-user-add.md) | Adds a user to a CRM object chat ||
|| [imopenlines.crm.chat.user.delete](./chats/imopenlines-crm-chat-user-delete.md) | Removes a user from a CRM object chat ||
|#

### Messages in Open Channels

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [imopenlines.crm.message.add](./messages/imopenlines-crm-message-add.md) | Sends a message to a CRM item chat ||
    || [imopenlines.message.quick.save](./messages/imopenlines-message-quick-save.md) | Saves a message as a quick reply ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnOpenLineMessageAdd](./events/on-open-line-message-add.md) | When adding a message to a chat ||
    || [OnOpenLineMessageUpdate](./events/on-open-line-message-update.md) | When changing a message in a chat ||
    || [OnOpenLineMessageDelete](./events/on-open-line-message-delete.md) | When deleting a message in a chat ||
    |#

{% endlist %}

### Operators of Open Channels

#|
|| **Method** | **Description** ||
|| [imopenlines.operator.answer](./operators/imopenlines-operator-answer.md) | Takes the dialogue from the queue or from a chatbot ||
|| [imopenlines.operator.finish](./operators/imopenlines-operator-finish.md) | Concludes the operator's own dialogue ||
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
    || [imopenlines.dialog.get](./sessions/imopenlines-dialog-get.md) | Retrieves information about a dialog ||
    || [imopenlines.session.start](./sessions/imopenlines-session-start.md) | Starts a new dialog in a chat ||
    || [imopenlines.message.session.start](./sessions/imopenlines-message-session-start.md) | Closes the current dialog and starts a new one from the specified message ||
    || [imopenlines.session.history.get](./sessions/imopenlines-session-history-get.md) | Gets message history and session data ||
    || [imopenlines.session.join](./sessions/imopenlines-session-join.md) | Attaches an operator to a dialog ||
    || [imopenlines.session.intercept](./sessions/imopenlines-session-intercept.md) | Takes the dialog from the current operator ||
    || [imopenlines.session.mode.pin](./sessions/imopenlines-session-mode-pin.md) | Pins or unpins the selected dialog ||
    || [imopenlines.session.mode.pinAll](./sessions/imopenlines-session-mode-pin-all.md) | Assigns all available dialogs to an operator ||
    || [imopenlines.session.mode.unpinAll](./sessions/imopenlines-session-mode-unpin-all.md) | Unassigns all assigned dialogs from an operator ||
    || [imopenlines.session.mode.silent](./sessions/imopenlines-session-mode-silent.md) | Turns the dialog's silent mode on or off. The method is no longer being developed; do not use it in new integrations ||
    || [imopenlines.session.head.vote](./sessions/imopenlines-session-head-vote.md) | Saves the supervisor's rating of the employee's work in a dialog ||
    || [imopenlines.crm.lead.create](./sessions/imopenlines-crm-lead-create.md) | Creates a CRM lead or deal based on a dialog ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnSessionStart](./events/on-session-start.md) | When creating an Open Channel session ||
    || [OnSessionFinish](./events/on-session-finish.md) | When closing an Open Channel session ||
    |#

{% endlist %}
