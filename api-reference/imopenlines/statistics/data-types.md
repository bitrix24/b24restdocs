# Open Channels Statistics Data Types

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Data types are used in responses of [imopenlines.v2.*](./index.md) methods. All field names are passed in `camelCase` format.

## Where Types Are Used

#|
|| **Type** | **Where It Is Returned** ||
|| [`session`](#session) | Method [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md), field `sessions[]` ||
|| [`operatorLoad`](#operator-load) | Method [imopenlines.v2.Operator.list](./imopenlines-v2-operator-list.md), field `operators[]` ||
|| [`sessionStat`](#session-stat) | Method [imopenlines.v2.Session.Stat.get](./imopenlines-v2-session-stat-get.md), field `stats[]` ||
|| [`rating`](#rating) | Method [imopenlines.v2.Session.Rating.list](./imopenlines-v2-session-rating-list.md), field `ratings[]` ||
|| [`transfer`](#transfer) | Method [imopenlines.v2.Session.Transfer.list](./imopenlines-v2-session-transfer-list.md), field `transfers[]` ||
|| [`statResult`](#stat-result) | Method [imopenlines.v2.Stat.get](./imopenlines-v2-stat-get.md), root object `result` ||
|#

## Field Features

#|
|| **Fields** | **Where They Are Used** | **When They Are Returned as `null`** ||
|| `voteHead`, `commentHead` | Objects [`session`](#session), [`sessionStat`](#session-stat), and [`rating`](#rating). `sessionStat` has only the `voteHead` field | If the plan or user permissions do not allow viewing supervisor ratings ||
|| `crmEntityType`, `crmEntityId` | Object [`session`](#session) | If the user does not have read permission for the linked CRM object ||
|#

## Object session {#session}

#|
|| **Field**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Session ID ||
|| **configId**
[`integer`](../../data-types.md) | Open channel ID ||
|| **source**
[`string`](../../data-types.md) | Channel code, for example `livechat` ||
|| **operatorId**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Operator ID ||
|| **userId**
[`integer`](../../data-types.md) | Internal customer ID ||
|| **userCode**
[`string`](../../data-types.md) | External customer ID in the channel ||
|| **chatId**
[`integer`](../../data-types.md) | Chat ID ||
|| **dateCreate**
[`string`](../../data-types.md) | Session creation date in ISO 8601 format ||
|| **dateClose**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Session close date ||
|| **dateFirstAnswer**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Date of the first operator response ||
|| **dateOperatorAnswer**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Date of the first live operator response ||
|| **status**
[`string`](../../data-types.md) | Session status [sessionStatus](#session-status) ||
|| **closeReason**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Close reason [closeReason](#close-reason) ||
|| **vote**
[`string`](../../data-types.md) | Customer rating [vote](#vote) ||
|| **voteHead**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Supervisor rating from `1` to `5` ||
|| **commentHead**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Supervisor comment on the rating ||
|| **crmEntityType**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Linked CRM object type.

Possible values:

- `lead` — lead
- `deal` — deal
- `contact` — contact
- `company` — company ||
|| **crmEntityId**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Linked CRM object ID ||
|| **queueTransfers**
[`integer`](../../data-types.md) | Number of session transfers ||
|| **waitAnswer**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Time to first response, seconds ||
|| **waitClose**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Time to close, seconds ||
|| **kpiFirstAnswer**
[`boolean`](../../data-types.md) \| [`null`](../../data-types.md) | Whether the operator met the first response KPI ||
|| **messageCount**
[`integer`](../../data-types.md) | Number of messages in the dialog ||
|#

## Object operatorLoad {#operator-load}

#|
|| **Field**
`type` | **Description** ||
|| **userId**
[`integer`](../../data-types.md) | Operator ID ||
|| **configId**
[`integer`](../../data-types.md) | Open channel ID ||
|| **status**
[`string`](../../data-types.md) | Operator status [operatorStatus](#operator-status) ||
|| **activeSessions**
[`integer`](../../data-types.md) | Number of active operator sessions ||
|| **maxChat**
[`integer`](../../data-types.md) | Maximum number of simultaneous chats from line settings ||
|| **freeSlots**
[`integer`](../../data-types.md) | Number of free operator slots ||
|| **lastActivityDate**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Operator last activity date ||
|#

## Object sessionStat {#session-stat}

#|
|| **Field**
`type` | **Description** ||
|| **sessionId**
[`integer`](../../data-types.md) | Session ID ||
|| **waitAnswer**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Time to first response, seconds ||
|| **waitClose**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Time to close, seconds ||
|| **messagesCount**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Total number of messages, including system messages ||
|| **messagesOperatorCount**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Number of operator messages, excluding system messages ||
|| **messagesClientCount**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Number of customer messages, excluding system messages ||
|| **transfersCount**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Number of session transfers ||
|| **kpiFirstAnswer**
[`boolean`](../../data-types.md) \| [`null`](../../data-types.md) | Whether the operator met the first response KPI ||
|| **vote**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Customer rating [vote](#vote) ||
|| **voteHead**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Supervisor rating from `1` to `5` ||
|#

## Object rating {#rating}

#|
|| **Field**
`type` | **Description** ||
|| **sessionId**
[`integer`](../../data-types.md) | Session ID ||
|| **configId**
[`integer`](../../data-types.md) | Open channel ID ||
|| **operatorId**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Operator ID ||
|| **source**
[`string`](../../data-types.md) | Channel code ||
|| **vote**
[`string`](../../data-types.md) | Customer rating [vote](#vote) ||
|| **voteHead**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Supervisor rating from `1` to `5` ||
|| **commentHead**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Supervisor comment on the rating ||
|| **dateVote**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Customer rating date ||
|| **dateSessionClose**
[`string`](../../data-types.md) \| [`null`](../../data-types.md) | Session close date ||
|#

## Object transfer {#transfer}

#|
|| **Field**
`type` | **Description** ||
|| **sessionId**
[`integer`](../../data-types.md) | Session ID ||
|| **date**
[`string`](../../data-types.md) | Transfer date in ISO 8601 format ||
|| **fromOperatorId**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | ID of the operator from whom the session was transferred ||
|| **toOperatorId**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | ID of the operator to whom the session was transferred ||
|| **fromConfigId**
[`integer`](../../data-types.md) | Source line ID ||
|| **toConfigId**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Destination line ID ||
|| **reason**
[`string`](../../data-types.md) | Transfer reason [transferReason](#transfer-reason) ||
|| **mode**
[`string`](../../data-types.md) | Transfer mode.

Possible values:

- `MANUAL` — manual transfer
- `AUTO` — automatic transfer
- `BOT` — transfer by bot ||
|| **type**
[`string`](../../data-types.md) | Transfer destination type.

Possible values:

- `USER` — transfer to an operator
- `QUEUE` — transfer to a queue ||
|| **initiatorId**
[`integer`](../../data-types.md) \| [`null`](../../data-types.md) | Transfer initiator ID ||
|#

## Object statResult {#stat-result}

#|
|| **Field**
`type` | **Description** ||
|| **totalSessions**
[`integer`](../../data-types.md) | Total number of sessions ||
|| **closedSessions**
[`integer`](../../data-types.md) | Number of closed sessions ||
|| **spamSessions**
[`integer`](../../data-types.md) | Number of spam sessions ||
|| **avgWaitAnswer**
[`number`](../../data-types.md) | Average time to first response, seconds ||
|| **avgSessionDuration**
[`number`](../../data-types.md) | Average session duration, seconds ||
|| **likeCount**
[`integer`](../../data-types.md) | Number of likes ||
|| **dislikeCount**
[`integer`](../../data-types.md) | Number of dislikes ||
|| **votedSessions**
[`integer`](../../data-types.md) | Number of sessions with a customer rating ||
|| **positiveRate**
[`number`](../../data-types.md) | Share of positive ratings from `0` to `1` ||
|| **kpiFirstAnswerOk**
[`integer`](../../data-types.md) | Number of sessions that met the first response KPI ||
|| **kpiFirstAnswerFail**
[`integer`](../../data-types.md) | Number of sessions that did not meet the first response KPI ||
|| **sessionsBySource**
[`sourceCount[]`](#source-count) | Number of sessions by channel ||
|| **sessionsByHour**
[`integer[]`](../../data-types.md) | Number of sessions by hour of day, 24 items ||
|| **sessionsByOperator**
[`operatorCount[]`](#operator-count) | Statistics by operator ||
|#

## Object sourceCount {#source-count}

#|
|| **Field**
`type` | **Description** ||
|| **source**
[`string`](../../data-types.md) | Channel code, for example `livechat` ||
|| **count**
[`integer`](../../data-types.md) | Number of sessions from the channel ||
|#

## Object operatorCount {#operator-count}

#|
|| **Field**
`type` | **Description** ||
|| **operatorId**
[`integer`](../../data-types.md) | Operator ID ||
|| **count**
[`integer`](../../data-types.md) | Number of operator sessions ||
|| **avgWaitAnswer**
[`number`](../../data-types.md) | Average operator first response time, seconds ||
|| **positiveRate**
[`number`](../../data-types.md) | Share of positive customer ratings for the operator from `0` to `1` ||
|#

## sessionStatus Values {#session-status}

#|
|| **Value** | **Description** ||
|| `new` | Session is in the queue or missed ||
|| `answered` | Operator is handling the dialog ||
|| `closed` | Session is closed ||
|| `spam` | Session is marked as spam ||
|| `paused` | Session is paused ||
|#

## operatorStatus Values {#operator-status}

#|
|| **Value** | **Description** ||
|| `online` | Operator is online and not paused ||
|| `offline` | Operator is offline ||
|| `pause` | Operator has paused themselves ||
|#

## closeReason Values {#close-reason}

#|
|| **Value** | **Description** ||
|| `operator` | Session was closed by an operator ||
|| `auto` | Session was closed automatically by timeout ||
|| `spam` | Session was closed as spam ||
|| `client` | Session was closed because of customer inactivity ||
|| `replyLimit` | Session was closed after the channel response window expired ||
|#

## vote Values {#vote}

#|
|| **Value** | **Description** ||
|| `like` | Customer left a like ||
|| `dislike` | Customer left a dislike ||
|| `none` | No rating ||
|#

## transferReason Values {#transfer-reason}

#|
|| **Value** | **Description** ||
|| `manual` | Manual transfer by an operator ||
|| `queue` | Automatic return to the same line queue ||
|| `auto` | Automatic distribution ||
|| `line` | Transfer to another open channel ||
|#

## Continue Learning

- [Open Channels Statistics: Overview of Methods](./index.md)
- [Get Aggregate Statistics](./imopenlines-v2-stat-get.md)
- [Get Sessions](./imopenlines-v2-session-list.md)
