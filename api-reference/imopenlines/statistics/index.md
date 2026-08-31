# Open Channels Statistics: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Open Channels statistics show how a team processes customer inquiries from online chat, messengers, social networks, and other Contact Center channels. They help supervisors see how many dialogs are currently in progress, where delays occur, how quickly operators respond, and which channels generate more inquiries.

Customer and supervisor ratings help monitor service quality. A customer can leave a like or dislike in the chat, and a supervisor can rate the operator's work on a five-point scale and leave a comment.

The `imopenlines.v2.*` methods provide access to this data for external dashboards, reports, and load monitoring. The methods only read accumulated statistics: sessions, ratings, transfers, current operator load, and aggregate metrics.

{% note info "" %}

In the cloud, the methods are available if the plan allows using Open Channels reports. On-premise installations are not subject to plan restrictions.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Contact Center](https://helpdesk.bitrix24.com/open/24095446/)

## What You Can Analyze

#|
|| **Metric** | **What It Shows** ||
|| Inquiry volume | How many sessions were created, closed, or marked as spam ||
|| Response speed | Time to the first operator response and first response KPI performance ||
|| Communication quality | Customer likes and dislikes, supervisor ratings, and supervisor comments ||
|| Inquiry channels | Which channels customers come from: online chat, messengers, social networks, and other connectors ||
|| Operator load | Operator status, active sessions, and free slots ||
|| Session transfers | Who the dialog was transferred to and when ||
|#

## How to Get Started

1. Retrieve summary metrics using [imopenlines.v2.Stat.get](./imopenlines-v2-stat-get.md)
2. Retrieve session details using [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md)
3. For selected sessions, request metrics using [imopenlines.v2.Session.Stat.get](./imopenlines-v2-session-stat-get.md) or transfer history using [imopenlines.v2.Session.Transfer.list](./imopenlines-v2-session-transfer-list.md)
4. For a real-time load widget, retrieve operators using [imopenlines.v2.Operator.list](./imopenlines-v2-operator-list.md)
5. For a CSAT report, retrieve rated sessions using [imopenlines.v2.Session.Rating.list](./imopenlines-v2-session-rating-list.md)

## Data Format

All parameters and response fields use `camelCase`. Dates are passed as ISO 8601 strings, for example `2026-06-15T14:32:10+02:00`.

Response object structures `session`, `operatorLoad`, `sessionStat`, `rating`, `transfer`, and `statResult` are described in [Open Channels Statistics Data Types](./data-types.md).

List methods use `offset` and `limit` pagination. The `limit` value must be from `1` to `200`; the default value is `50`. The response contains the `hasNextPage` field.

Batch methods [imopenlines.v2.Session.Stat.get](./imopenlines-v2-session-stat-get.md) and [imopenlines.v2.Session.Transfer.list](./imopenlines-v2-session-transfer-list.md) do not use pagination. They accept a `sessionId` array with a limited size.

The `voteHead` and `commentHead` fields in responses of [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md), [imopenlines.v2.Session.Stat.get](./imopenlines-v2-session-stat-get.md), and [imopenlines.v2.Session.Rating.list](./imopenlines-v2-session-rating-list.md) are returned as `null` if the plan or user permissions do not allow viewing supervisor ratings.

## Relationship with Other Objects

**Open Channel.** The line ID is passed in the `configId` and `configIdList` parameters. You can obtain it using [imopenlines.config.get](../openlines/imopenlines-config-get.md) and [imopenlines.config.list.get](../openlines/imopenlines-config-list-get.md).

**Session.** Open Channel sessions are returned by [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md). Session IDs are needed for the batch methods [imopenlines.v2.Session.Stat.get](./imopenlines-v2-session-stat-get.md) and [imopenlines.v2.Session.Transfer.list](./imopenlines-v2-session-transfer-list.md).

**Operator.** An operator is identified by the Bitrix24 user ID. Use `operatorId`, `operatorIdList`, `userId`, or `userIdList` depending on the method. You can obtain the user ID using [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md).

**CRM.** A session can be linked to a lead, deal, contact, or company. The `crmEntityType` and `crmEntityId` fields in `session` items returned by [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md) are returned only if the user has read permission for the linked CRM object.

## Overview of Methods {#all-methods}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: a user with access to Open Channels reports

#|
|| **Method** | **Description** ||
|| [imopenlines.v2.Stat.get](./imopenlines-v2-stat-get.md) | Retrieves aggregate Open Channels statistics for a period ||
|| [imopenlines.v2.Operator.list](./imopenlines-v2-operator-list.md) | Retrieves operators with their current status and load ||
|| [imopenlines.v2.Session.list](./imopenlines-v2-session-list.md) | Retrieves sessions with filters and pagination ||
|| [imopenlines.v2.Session.Stat.get](./imopenlines-v2-session-stat-get.md) | Retrieves batch session metrics ||
|| [imopenlines.v2.Session.Rating.list](./imopenlines-v2-session-rating-list.md) | Retrieves sessions with customer ratings ||
|| [imopenlines.v2.Session.Transfer.list](./imopenlines-v2-session-transfer-list.md) | Retrieves session transfer history ||
|#
