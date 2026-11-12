# Open Channel Operators: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The methods in this section perform operator actions on an open channel dialogue on behalf of the user calling the method: take the dialogue, skip it, transfer it, complete it, or mark it as spam.

> Quick Navigation: [All Methods](#all-methods)
>
> User documentation: [How to Set Up Employee Queues in Open Channels](https://helpdesk.bitrix24.com/open/25782447/)

## Operators' Interaction with Other Objects

**Chats.** All methods in this section work with an open channel chat by its numeric identifier `CHAT_ID` without the `chat` prefix. This identifier can be obtained through the methods [imopenlines.session.open](../sessions/imopenlines-session-open.md) and [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md).

**User.** The [imopenlines.operator.transfer](./imopenlines-operator-transfer.md) method transfers the dialogue to an employee by the identifier `USER_ID`. The user identifier can be retrieved using the methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md).

**Open Channels.** The [imopenlines.operator.transfer](./imopenlines-operator-transfer.md) method transfers the dialogue to an open channel queue by the identifier `QUEUE_ID` or a string in the format `queue<channel ID>`. The channel identifier is the `ID` field in the response of the [imopenlines.config.list.get](../imopenlines-config-list-get.md) method.

{% note tip "User Documentation" %}

- [How to Set Up Employee Queues in Open Channels](https://helpdesk.bitrix24.com/open/25782447/)
- [Why Operator Availability Check is Needed in Open Channels](https://helpdesk.bitrix24.com/open/25283590/)
- [Limit on Unclosed Dialogues for Open Channel Operators](https://helpdesk.bitrix24.com/open/25830887/)

{% endnote %}

## Getting Started

1. Get the chat identifier `CHAT_ID` using the [imopenlines.session.open](../sessions/imopenlines-session-open.md) or [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) method
2. To take the dialogue, call [imopenlines.operator.answer](./imopenlines-operator-answer.md). The method takes the dialogue from the queue or from a chatbot. To take the dialogue from another operator, use [imopenlines.session.intercept](../sessions/imopenlines-session-intercept.md)
3. If the dialog needs to be transferred to another employee or queue, call [imopenlines.operator.transfer](./imopenlines-operator-transfer.md)
4. If the operator cannot process the request, transfer it to the next operator using [imopenlines.operator.skip](./imopenlines-operator-skip.md)
5. If the request is complete, close it using [imopenlines.operator.finish](./imopenlines-operator-finish.md). Another operator's dialogue is closed by the [imopenlines.operator.another.finish](./imopenlines-operator-another-finish.md) method
6. If the request is inappropriate, mark it as spam using [imopenlines.operator.spam](./imopenlines-operator-spam.md)

### Rules for Calling imopenlines.operator.transfer

- Only pass one parameter: `TRANSFER_ID`, `USER_ID`, or `QUEUE_ID`
- Use the employee identifier or a string in the format `queue<channel ID>`, for example `queue3`, as `TRANSFER_ID`
- The employee must be active; you cannot transfer the dialogue to yourself

## Response Format of the Methods

All methods in this section return `true` in the `result` field if the action is performed. On error, the methods return the error code and text.

The [imopenlines.operator.spam](./imopenlines-operator-spam.md) method returns `true` right after the access check, even if the chat has no active dialogue.

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#| 
|| **Method** | **Description** ||
|| [imopenlines.operator.answer](./imopenlines-operator-answer.md) | Assigns the user as the person responsible for the dialogue ||
|| [imopenlines.operator.finish](./imopenlines-operator-finish.md) | Completes the dialogue handled by the user ||
|| [imopenlines.operator.another.finish](./imopenlines-operator-another-finish.md) | Completes another operator's dialogue ||
|| [imopenlines.operator.skip](./imopenlines-operator-skip.md) | Passes the dialogue to the next operator in the queue ||
|| [imopenlines.operator.spam](./imopenlines-operator-spam.md) | Marks the dialogue as spam ||
|| [imopenlines.operator.transfer](./imopenlines-operator-transfer.md) | Transfers the dialogue to another operator or to another line ||
|#
