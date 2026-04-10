# CALL_CARD: Overview of Methods and Events

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The `CALL_CARD` placement is used by applications that are embedded in the call card within the CRM. Through the JavaScript interface of the placement, the application can access data about the current call, manage the auto-closing of the card, and respond to changes in the client or the call state.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Call Card](https://helpdesk.bitrix24.com/open/18493064/)

## Getting Started with CALL_CARD

1. Open the application in the `CALL_CARD` placement.
2. Retrieve the list of available commands and events via [BX24.placement.getInterface](../bx24-placement-get-interface.md).
3. Invoke placement commands through [BX24.placement.call](../bx24-placement-call.md).
4. Subscribe to call card events using [BX24.placement.bindEvent](../bx24-placement-bind-event.md).

## How the Placement API Call Works

In `CALL_CARD`, the commands and events of the placement are not standalone REST methods. The application interacts with them through a common JavaScript embedding interface.

To execute a placement command, pass its name to [BX24.placement.call](../bx24-placement-call.md). For `CALL_CARD`, these commands are `getStatus`, `disableAutoClose`, and `enableAutoClose`. The `getStatus` command returns data about the current call, while the auto-close management commands return an empty array upon successful invocation.

To respond to changes in the card without making repeated requests, subscribe to events via [BX24.placement.bindEvent](../bx24-placement-bind-event.md). In `CALL_CARD`, the available events are `CallCard::EntityChanged`, `CallCard::CallStateChanged`, and `CallCard::BeforeClose`. The event handler may receive client data, call state with additional parameters, or a call without data if the card is closing.

## Overview of Methods and Events {#all-methods}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [getStatus](./get-status.md) | Retrieves information about the current call ||
    || [disableAutoClose](./disable-auto-close.md) | Disables automatic closing of the card after the call ends ||
    || [enableAutoClose](./enable-auto-close.md) | Enables automatic closing of the card after the call ends ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [CallCard::EntityChanged](./call-card-entity-changed.md) | When the current client changes during a call campaign ||
    || [CallCard::BeforeClose](./call-card-before-close.md) | Before the call card closes ||
    || [CallCard::CallStateChanged](./call-card-call-state-changed.md) | When the state of the current call changes ||
    |#

{% endlist %}