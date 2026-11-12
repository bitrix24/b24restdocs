# Open Channels Dialogs: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Open Channels dialogs are chats with clients via messengers, social networks, or online chats.

A dialog can consist of multiple sessions. Each session represents one cycle of handling requests: connecting an operator, exchanging messages, concluding, and evaluating.

The methods in this section allow you to:

- open a dialog based on external client data
- initiate and manage sessions
- connect operators
- change dialog modes
- transfer communication results to CRM

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to work with the dialog list in Open Channels](https://helpdesk.bitrix24.com/open/25768607/)

## Connection of Dialogs with Other Objects

**Open Channels.** A dialog belongs to an open channel and inherits its processing rules: queues, routing, and operating modes. Change the channel settings using the [imopenlines.config.*](../index.md#config) methods. The channel ID `LINE_ID` is part of the user code `USER_CODE`.

**Chat.** Each dialog is an open channel chat of the `lines` type. Most methods in this section accept the numeric identifier of this chat, `CHAT_ID`. To message a client from a CRM card and save a message as a canned response, use the methods in the [Messages](../messages/index.md) section.

**External Channel User.** The client is linked to the dialog through the user code `USER_CODE`: the connector, channel, chat, and user in the communication channel. You can find the chat by this code using the [imopenlines.session.open](./imopenlines-session-open.md) method.

**Operators.** Operators participate in handling sessions: they accept dialogs, transfer them, and conclude requests. For operator scenarios, use the methods in the [Operators](../operators/index.md) section.

**CRM.** A dialog can be linked to CRM: the [imopenlines.crm.lead.create](./imopenlines-crm-lead-create.md) method creates a lead from the client data. The `entity_data_2` field of the [imopenlines.dialog.get](./imopenlines-dialog-get.md) method returns the links of the dialog to a lead, contact, company, and deal. You can then work with the lead using the [crm.lead.*](../../../crm/leads/index.md) methods.

## Main Identifiers

#|
|| **Identifier** | **Description** ||
|| `USER_CODE` | User code from the external communication channel. Format: ```<connector>|<LINE_ID>|<CONNECTOR_CHAT_ID>|<CONNECTOR_USER_ID>```. Example: ```livechat|22|1761|587```. Returned in the `entity_id` field of the [imopenlines.dialog.get](./imopenlines-dialog-get.md) method ||
|| `CHAT_ID` | Internal identifier of the chat in Bitrix24. The main parameter for methods working with the dialog. Format: integer. Example: `2043` ||
|| `DIALOG_ID` | String representation of the chat. Used only in [`imopenlines.dialog.get`](./imopenlines-dialog-get.md). Format: `chat<ID>`. Example: `chat2043` ||
|| `SESSION_ID` | Identifier of a separate session within the dialog. Format: integer. Example: `3567`. You can find the current session in the sixth element of the `entity_data_1` field of the [imopenlines.dialog.get](./imopenlines-dialog-get.md) method or in the `sessionId` field of the [imopenlines.session.history.get](./imopenlines-session-history-get.md) method ||
|#

## How to Work with Dialogs

### Retrieve Dialog Data

To retrieve dialog data, use [imopenlines.dialog.get](./imopenlines-dialog-get.md).

If the dialog needs to be found based on external client data, first call [imopenlines.session.open](./imopenlines-session-open.md), then pass the obtained identifier to `imopenlines.dialog.get`.

### Start a Session

To initiate a new processing cycle in the current chat, call [imopenlines.session.start](./imopenlines-session-start.md).

If the client has raised a new question in the current session, close it and start a new one from the client's message using the [imopenlines.message.session.start](./imopenlines-message-session-start.md) method.

### Connect an Operator

To have an operator join the dialog, use [imopenlines.session.join](./imopenlines-session-join.md).

To take over a dialog from another operator or chatbot, use [imopenlines.session.intercept](./imopenlines-session-intercept.md).

### Retrieve History

Call [imopenlines.session.history.get](./imopenlines-session-history-get.md) to retrieve the session messages and participant data. The method returns messages page by page, up to 200 per request.

### Manage Dialog Modes

To pin and unpin dialogs, use [imopenlines.session.mode.pin](./imopenlines-session-mode-pin.md), [imopenlines.session.mode.pinAll](./imopenlines-session-mode-pin-all.md), [imopenlines.session.mode.unpinAll](./imopenlines-session-mode-unpin-all.md).

Silent mode is toggled by the [imopenlines.session.mode.silent](./imopenlines-session-mode-silent.md) method. The method is no longer being developed; do not use it in new integrations.

### Conclude Processing and Transfer Data to CRM

A supervisor can rate the operator's work in a session using the [imopenlines.session.head.vote](./imopenlines-session-head-vote.md) method. To save the client in CRM, create a lead using the [imopenlines.crm.lead.create](./imopenlines-crm-lead-create.md) method. If the channel is configured to create deals, the method creates a deal with a contact.

{% note tip "User Documentation" %}

- [Service Quality Evaluation in Open Channels of Bitrix24](https://helpdesk.bitrix24.com/open/25786667/)
- [Limit on Unclosed Dialogs for Open Channel Operators](https://helpdesk.bitrix24.com/open/25830887/)
- [How to Work with Dialog Statistics in Open Channels](https://helpdesk.bitrix24.com/open/25781307/)

{% endnote %}

## Response Format and Error Handling

Most methods in this section return a `result` object and a service block `time`.

When integrating, check:

- HTTP response status
- `error` field and `error_description` text for status `400`
- error codes described on the method page

Some methods return the same `ACCESS_DENIED` error both when permissions are missing and when an identifier is invalid. If you get this error, check both the user's permissions for the dialog and the values of `CHAT_ID`, `SESSION_ID`, `DIALOG_ID`, and `USER_CODE`.

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

### Start and Retrieve Dialog

#|
|| **Method** | **Description** ||
|| [imopenlines.session.open](./imopenlines-session-open.md) | Retrieves the chat identifier by user code ||
|| [imopenlines.dialog.get](./imopenlines-dialog-get.md) | Returns open channel chat data by one of the identifiers ||
|| [imopenlines.session.start](./imopenlines-session-start.md) | Starts a new session in the current chat ||
|| [imopenlines.message.session.start](./imopenlines-message-session-start.md) | Closes the current session and starts a new one from a client message ||
|| [imopenlines.session.history.get](./imopenlines-session-history-get.md) | Returns session messages page by page ||
|#

### Connect Operator to Dialog

#|
|| **Method** | **Description** ||
|| [imopenlines.session.join](./imopenlines-session-join.md) | Joins the current operator to the dialog ||
|| [imopenlines.session.intercept](./imopenlines-session-intercept.md) | Takes over a dialog from the current operator or chatbot ||
|#

### Manage Dialog Modes

#|
|| **Method** | **Description** ||
|| [imopenlines.session.mode.pin](./imopenlines-session-mode-pin.md) | Pins or unpins the selected dialog ||
|| [imopenlines.session.mode.pinAll](./imopenlines-session-mode-pin-all.md) | Pins all available dialogs to the current operator ||
|| [imopenlines.session.mode.unpinAll](./imopenlines-session-mode-unpin-all.md) | Unpins all pinned dialogs from the current operator ||
|| [imopenlines.session.mode.silent](./imopenlines-session-mode-silent.md) | Turns the dialog's silent mode on or off. The method is no longer being developed; do not use it in new integrations ||
|#

### Save Results and Create Lead

#|
|| **Method** | **Description** ||
|| [imopenlines.session.head.vote](./imopenlines-session-head-vote.md) | Saves the supervisor's rating and comment on the operator's work ||
|| [imopenlines.crm.lead.create](./imopenlines-crm-lead-create.md) | Creates a CRM lead from the client data in a dialog ||
|#
