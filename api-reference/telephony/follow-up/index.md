# Call Follow-up in REST 3.0: Methods Overview

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

CoPilot Follow-Up is an AI assistant for Bitrix24 video calls. After a call, it analyzes the meeting and identifies key topics. The result of the analysis is the call's Follow-up: materials the team uses to record the outcome of the meeting.

The following may be available in a Follow-up:

- Call summary
- Agreements
- Recommendations and efficiency rating
- Summary and meeting transcript

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [CoPilot Follow-Up: AI Speech Analytics for Video Calls](https://helpdesk.bitrix24.com/open/25345201/)

{% note info "" %}

The methods in this section belong to REST 3.0. Calling specifics and the response format for the new API version are described in the [REST 3.0 Overview](../../rest-v3.md).

{% endnote %}

## When to Use Each Method

Use [call.followup.list](./call-followup-list.md) when you need to retrieve a list of Follow-ups for a specific period, filter calls by attendee, or paginate through the results.

Use [call.followup.get](./call-followup-get.md) when you need to retrieve the Follow-up for a single call using the `callId` identifier.

Use [call.followup.field.list](./call-followup-field-list.md) and [call.followup.field.get](./call-followup-field-get.md) when you need to retrieve a list of available Follow-up fields or the description of a specific field for `select` configuration.

The [call.followup.*](#all-methods) methods return Follow-ups of Bitrix24 calls and Bitrix24 Sync calls. If you need only Sync calls, use the [Bitrix24 Sync methods](#sync-methods).

## Workflow for Call Follow-ups

1. Retrieve a list of Follow-ups for the required period via [call.followup.list](./call-followup-list.md)
2. Take the `callId` from the list result
3. Retrieve the full call Follow-up via [call.followup.get](./call-followup-get.md)
4. Configure `select` to retrieve only the required fields and AI blocks
5. If necessary, clarify the field structure via `*.field.list` and `*.field.get`

## Data Access {#access}

An Administrator receives all Follow-ups. A standard User receives Follow-ups of calls they participated in or whose chat they belong to. For a Bitrix24 Sync call, participation means that the User spoke in the call part the Follow-up refers to.

The `call` scope grants the application access to the methods but does not expand data visibility. The method still verifies the access rights of the User on whose behalf the request is being executed.

## Field Selection {#select}

The `select` parameter specifies which fields are returned in the response. AI blocks are what the AI has prepared: the transcript, meeting overview, summary, insights, and efficiency rating. You can request root Follow-up fields, an entire AI block, or a part of it using dot notation. For example, `overview` returns the whole meeting overview, while `overview.topic` returns only the topic.

See the [Call Follow-up Fields](./fields.md) article for a complete list of fields and nested objects.

## Connection with Other Objects

**Calls.** The `callId` identifier links a Follow-up to a call, and for a Sync call — to a detailed analysis page. See [Bitrix24 Sync Call Follow-ups](#sync) for details. You can retrieve the `callId` from [call.followup.list](./call-followup-list.md), then pass it to [call.followup.get](./call-followup-get.md).

**Chats.** Members of the call chat also receive the call's Follow-up, even if they did not attend the call. If a User did not participate in the call and is not part of the chat, the method will return an access error.

**Users.** The `participants` field returns the call attendees: User identifier, name, position, and time in seconds. For a Bitrix24 call, this is the time spent in the call; for a Sync call, it is how long the User spoke. You can use the identifier to find the Employee.

## Bitrix24 Sync Call Follow-ups {#sync}

Bitrix24 Sync is a separate calls service that connects to Bitrix24. Follow-ups of its calls are returned by two sets of methods:

- [call.followup.*](#all-methods) — together with Follow-ups of Bitrix24 calls. A Sync call can be identified by the `source` field with the value `sync`
- [syncconnector.followup.*](#sync-methods) — Follow-ups of Sync calls only

A Sync call Follow-up is linked not to the call but to a detailed analysis page. The "Detailed analysis" link in the chat opens this page. One Sync call can produce several pages, one for each call part. Both sets build `callId` from the identifier of this page:

- [syncconnector.followup.*](#sync-methods) return the page identifier as is, for example `128`
- [call.followup.*](#all-methods) add `10000000000` to it, for example `10000000128`. The offset ensures that the `callId` of a Sync call does not coincide with a Bitrix24 call identifier

The parameters and response fields of [syncconnector.followup.*](#sync-methods) are the same as those of [call.followup.*](#all-methods). The Sync methods require the `syncconnector` scope, while the `call` scope is enough to retrieve Sync Follow-ups via [call.followup.*](#all-methods). Who receives which Follow-ups is described in [Data Access](#access).

The methods are listed in [Bitrix24 Sync Methods](#sync-methods).

## Overview of Methods {#all-methods}

> Scope: [`call`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [call.followup.list](./call-followup-list.md) | Returns a list of Follow-up calls for a period ||
|| [call.followup.get](./call-followup-get.md) | Returns the Follow-up of a single call ||
|| [call.followup.field.list](./call-followup-field-list.md) | Returns a list of Follow-up fields ||
|| [call.followup.field.get](./call-followup-field-get.md) | Returns the description of a Follow-up field by name ||
|#

### Bitrix24 Sync Methods {#sync-methods}

> Scope: [`syncconnector`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [syncconnector.followup.list](./syncconnector-followup-list.md) | Returns a list of Sync call Follow-ups for a period ||
|| [syncconnector.followup.get](./syncconnector-followup-get.md) | Returns a Follow-up by the detailed analysis page identifier ||
|| [syncconnector.followup.field.list](./syncconnector-followup-field-list.md) | Returns a list of Sync call Follow-up fields ||
|| [syncconnector.followup.field.get](./syncconnector-followup-field-get.md) | Returns the description of a Sync call Follow-up field by name ||
|#

## Continue Learning

- [{#T}](./call-followup-list.md)
- [{#T}](./call-followup-get.md)
- [{#T}](./fields.md)
- [{#T}](./call-followup-field-list.md)
- [{#T}](./call-followup-field-get.md)
- [{#T}](./syncconnector-followup-list.md)
- [{#T}](./syncconnector-followup-get.md)
- [{#T}](../../rest-v3.md)