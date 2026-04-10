# Open Channel Operators: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Open channel operators work with dialogues: they accept them for processing, transfer them, complete them, or mark them as spam.

> Quick Navigation: [All Methods](#all-methods)

## Operators' Interaction with Other Objects

**Chats.** The operator's actions are applied to the chat using the identifier `CHAT_ID`. This identifier can be obtained through the methods [imopenlines.session.open](../sessions/imopenlines-session-open.md) and [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md).

**User.** The operator switches the dialogue to an employee with the identifier `USER_ID`. The user identifier can be retrieved using the methods [user.get](../../../user/user-get.md) and [user.search](../../../user/user-search.md).

**Open Channels.** The operator transfers the dialogue to the queue of another open channel using the identifier `QUEUE_ID` or a string in the format `queue<QUEUE_ID>`. The queue identifier can be obtained through the method [imopenlines.config.list.get](../imopenlines-config-list-get.md).

{% note tip "User Documentation" %}

- [How to Set Up Employee Queues in Open Channels](https://helpdesk.bitrix24.com/open/25782447/)
- [Why Operator Availability Check is Needed in Open Channels](https://helpdesk.bitrix24.com/open/25283590/)
- [Limit on Unclosed Dialogues for Open Channel Operators](https://helpdesk.bitrix24.com/open/25830887/)

{% endnote %}

## How to Use the Methods

Choose a method based on your workflow:
- start processing a new request — [imopenlines.operator.answer](./imopenlines-operator-answer.md)
- complete your dialogue — [imopenlines.operator.finish](./imopenlines-operator-finish.md)
- complete another operator's dialogue — [imopenlines.operator.another.finish](./imopenlines-operator-another-finish.md)
- transfer the dialogue to a specific employee or another queue — [imopenlines.operator.transfer](./imopenlines-operator-transfer.md)
- pass the dialogue to the next operator in the queue — [imopenlines.operator.skip](./imopenlines-operator-skip.md)
- close an inappropriate request — [imopenlines.operator.spam](./imopenlines-operator-spam.md)

### Rules for Calling imopenlines.operator.transfer

- Only pass one parameter: `TRANSFER_ID`, `USER_ID`, or `QUEUE_ID`. Do not pass `TRANSFER_ID` along with `USER_ID` or `QUEUE_ID` simultaneously.
- Use `USER_ID` of the employee or a string in the format `queue<QUEUE_ID>` as `TRANSFER_ID`.

## Response Format of the Methods

All methods in this section return the same response format:

- `result` — status of the operation,
- `time` — information about the execution time.

```json
{
    "result": true,
    "time": {
        "start": 1773663032,
        "finish": 1773663032.493037,
        "duration": 0.49303698539733887,
        "processing": 0,
        "date_start": "2026-03-16T15:10:32+01:00",
        "date_finish": "2026-03-16T15:10:32+01:00",
        "operating_reset_at": 1773663632,
        "operating": 0
    }
}
```

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with dialogue permissions

#| 
|| **Method** | **Description** ||
|| [imopenlines.operator.answer](./imopenlines-operator-answer.md) | Transfers the dialogue to the current operator ||
|| [imopenlines.operator.finish](./imopenlines-operator-finish.md) | Completes the dialogue on behalf of the current operator ||
|| [imopenlines.operator.another.finish](./imopenlines-operator-another-finish.md) | Completes another operator's dialogue ||
|| [imopenlines.operator.skip](./imopenlines-operator-skip.md) | Passes the dialogue to the next operator in the queue ||
|| [imopenlines.operator.spam](./imopenlines-operator-spam.md) | Marks the dialogue as spam ||
|| [imopenlines.operator.transfer](./imopenlines-operator-transfer.md) | Transfers the dialogue to another operator or to another line ||
|#