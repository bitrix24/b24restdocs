# Open Channels Dialogs: Overview of Methods

Open Channels dialogs are chats with clients via messengers, social networks, or online chats.

A dialog can consist of multiple sessions. Each session represents one cycle of handling requests: connecting an operator, exchanging messages, concluding, and evaluating.

The methods in this section allow you to:
- open a dialog based on external client data,
- initiate and manage sessions,
- connect operators,
- change dialog modes,
- transfer communication results to CRM.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to work with the dialog list in Open Channels](https://helpdesk.bitrix24.com/open/25768607/)

## Connection of Dialogs with Other Objects

**Open Channels.** A dialog belongs to an open channel and inherits its processing rules: queues, routing, and operating modes. To work with channel settings, use the group of methods [imopenlines.config.*](../index.md).

**Chat.** The system represents each dialog as a chat in the open channel. Operations with messages, operators, and sessions are performed through the chat.

**External Channel User.** The external client is linked to the dialog through communication channel data. Based on this data, the system determines which dialog the request pertains to.

**Operators.** Operators participate in handling sessions: they accept dialogs, transfer them, and conclude requests. For operator scenarios, use the methods in the [Operators](../operators/index.md) section.

**CRM.** Communication results from the dialog can be transferred to CRM, allowing further work using the methods [crm.lead.*](../../../crm/leads/index.md).

## Main Identifiers

#|
|| **Identifier** | **Description** ||
|| `USER_CODE` | User code from the external communication channel. Format: ```<connector>|<LINE_ID>|<CONNECTOR_CHAT_ID>|<CONNECTOR_USER_ID>```. Example: ```livechat|22|1761|587``` ||
|| `CHAT_ID` | Internal identifier of the chat in Bitrix24. The main parameter for methods working with the dialog. Format: integer. Example: `2043` ||
|| `DIALOG_ID` | String representation of the chat. Used only in [`imopenlines.dialog.get`](./imopenlines-dialog-get.md). Format: `chat<ID>`. Example: `chat2043` ||
|| `SESSION_ID` | Identifier of a separate session within the dialog. Format: integer. Example: `3567` ||
|#

## How to Work with Dialogs

### Retrieve Dialog Data

To retrieve data for the current dialog, use [imopenlines.dialog.get](./imopenlines-dialog-get.md).

If the dialog needs to be found based on external client data, first call [imopenlines.session.open](./imopenlines-session-open.md), then pass the obtained identifier to `imopenlines.dialog.get`.

### Start a Session

To initiate a new processing cycle in the current chat, call [imopenlines.session.start](./imopenlines-session-start.md).

If you need to start a new session from a specific message, use [imopenlines.message.session.start](./imopenlines-message-session-start.md).

### Connect an Operator

To have an operator join the dialog, use [imopenlines.session.join](./imopenlines-session-join.md).

To have the current operator take over the dialog, use [imopenlines.session.intercept](./imopenlines-session-intercept.md).

### Retrieve History

Call [`imopenlines.session.history.get`](./imopenlines-session-history-get.md) to obtain messages, session status, and related data.

### Manage Dialog Modes

To pin and unpin dialogs, use [imopenlines.session.mode.pin](./imopenlines-session-mode-pin.md), [imopenlines.session.mode.pinAll](./imopenlines-session-mode-pin-all.md), [imopenlines.session.mode.unpinAll](./imopenlines-session-mode-unpin-all.md).

To toggle silent mode, use [imopenlines.session.mode.silent](./imopenlines-session-mode-silent.md).

### Conclude Processing and Transfer Data to CRM

After concluding the dialog, you can save the supervisor's evaluation using the method [imopenlines.session.head.vote](./imopenlines-session-head-vote.md). The communication result can be transferred to CRM using the method [imopenlines.crm.lead.create](./imopenlines-crm-lead-create.md).

{% note tip "User Documentation" %}

- [Service Quality Evaluation in Open Channels of Bitrix24](https://helpdesk.bitrix24.com/open/25786667/)
- [Limit on Unclosed Dialogs for Open Channel Operators](https://helpdesk.bitrix24.com/open/25830887/)
- [How to Work with Dialog Statistics in Open Channels](https://helpdesk.bitrix24.com/open/25781307/)

{% endnote %}

## Response Format and Error Handling

Most methods in this section return a `result` object and a service block `time`.

When integrating, check:

- HTTP response status,
- `error` field and `error_description` text for status `400`,
- method-specific error codes on the specific method page.

For access errors and incorrect identifiers, first check the user's permissions for the dialog and the correctness of the values for `CHAT_ID`, `SESSION_ID`, `DIALOG_ID`, and `USER_CODE`.

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with dialog permissions

### Start and Retrieve Dialog

#|
|| **Method** | **Description** ||
|| [imopenlines.session.open](./imopenlines-session-open.md) | Retrieves the chat identifier by user code ||
|| [imopenlines.dialog.get](./imopenlines-dialog-get.md) | Returns dialog data by one of the identifiers ||
|| [imopenlines.session.start](./imopenlines-session-start.md) | Starts a new session in the current chat ||
|| [imopenlines.message.session.start](./imopenlines-message-session-start.md) | Starts a new session based on the selected message ||
|| [imopenlines.session.history.get](./imopenlines-session-history-get.md) | Returns message history and session data ||
|#

### Connect Operator to Dialog

#|
|| **Method** | **Description** ||
|| [imopenlines.session.join](./imopenlines-session-join.md) | Joins the current operator to the dialog ||
|| [imopenlines.session.intercept](./imopenlines-session-intercept.md) | Transfers the dialog to the current operator ||
|#

### Manage Dialog Modes

#|
|| **Method** | **Description** ||
|| [imopenlines.session.mode.pin](./imopenlines-session-mode-pin.md) | Pins or unpins the selected dialog ||
|| [imopenlines.session.mode.pinAll](./imopenlines-session-mode-pin-all.md) | Pins all available dialogs to the current operator ||
|| [imopenlines.session.mode.unpinAll](./imopenlines-session-mode-unpin-all.md) | Unpins all pinned dialogs from the current operator ||
|| [imopenlines.session.mode.silent](./imopenlines-session-mode-silent.md) | Turns the dialog's silent mode on or off ||
|#

### Save Results and Create Lead

#|
|| **Method** | **Description** ||
|| [imopenlines.session.head.vote](./imopenlines-session-head-vote.md) | Saves the supervisor's evaluation for the completed session ||
|| [imopenlines.crm.lead.create](./imopenlines-crm-lead-create.md) | Creates a CRM lead from the open channel dialog ||
|#