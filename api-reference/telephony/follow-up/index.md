# Call Follow-up in REST 3.0: Methods Overview

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

CoPilot Follow-Up is an AI assistant for Bitrix24 video calls. After a call ends, it analyzes the meeting, identifies key topics, and generates materials to help the team record the outcome.

The following may be available in a Follow-up:

- Call summary
- Agreements
- Recommendations and efficiency rating
- Summary and meeting transcript

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [CoPilot Follow-Up: AI Speech Analytics for Video Calls](https://helpdesk.bitrix24.com/open/23529044/)

{% note info "" %}

The methods in this section belong to REST 3.0. Calling specifics and the response format for the new API version are described in the [REST 3.0 Overview](../../rest-v3.md).

{% endnote %}

## When to Use Each Method

Use [call.followup.list](./call-followup-list.md) when you need to retrieve a list of Follow-ups for a specific period, filter calls by attendee, or paginate through the results.

Use [call.followup.get](./call-followup-get.md) when you need to retrieve the Follow-up for a single call using the `callId` identifier.

Use [call.followup.field.list](./call-followup-field-list.md) and [call.followup.field.get](./call-followup-field-get.md) when you need to retrieve a list of available Follow-up fields or the description of a specific field for `select` configuration.

## Workflow for Call Follow-ups

1. Retrieve a list of Follow-ups for the required period via [call.followup.list](./call-followup-list.md)
2. Take the `callId` from the list result
3. Retrieve the full call Follow-up via [call.followup.get](./call-followup-get.md)
4. Configure `select` to retrieve only the required fields and AI blocks
5. If necessary, clarify the field structure via `*.field.list` and `*.field.get`

## Data Access

A standard User receives Follow-ups for calls they participated in or for which they are part of a linked chat. An Administrator has access to all Follow-ups.

The `call` scope grants the application access to the methods but does not expand data visibility. The method still verifies the access rights of the User on whose behalf the request is being executed.

## Field Selection {#select}

The `select` parameter controls the composition of the response. You can request root Follow-up fields, an entire AI block, or a specific subfield using dot notation.

See the [Call Follow-up Fields](./fields.md) article for a complete list of fields and nested objects.

## Connection with Other Objects

**Calls.** The `callId` identifier links a Follow-up to a specific call. You can retrieve the `callId` from [call.followup.list](./call-followup-list.md), then pass it to [call.followup.get](./call-followup-get.md).

**Chats.** Access to a Follow-up depends not only on participation in the call but also on the User's participation in the linked chat. If a User did not participate in the call and is not part of the chat, the method will return an access error.

**Users.** The call attendees are returned in the `participants` field. You can map Follow-ups to Employees and their meeting participation using User identifiers.

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

## Continue Learning

- [{#T}](./call-followup-list.md)
- [{#T}](./call-followup-get.md)
- [{#T}](./fields.md)
- [{#T}](./call-followup-field-list.md)
- [{#T}](./call-followup-field-get.md)
- [{#T}](../../rest-v3.md)