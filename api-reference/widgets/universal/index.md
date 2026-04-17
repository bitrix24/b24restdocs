# Universal Widgets: Overview of Embedding Points

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This section describes universal embedding points that are not tied to a specific Bitrix24 interface. Through these points, the application can be opened via a special link in the content or operate in the background on all pages of the account.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

> Quick Navigation: [All Widgets](#all-widgets)

## How to Choose an Embedding Point

- The user must open the application via a special link in a message, comment, task, or other content — [REST_APP_URI](./app-url.md)
- The application should operate in the background on all pages of the account and respond to events without a separate visible interface element — [PAGE_BACKGROUND_WORKER](./background-worker.md)

## Getting Started

1. Define the launch scenario: opening via a link or background operation on the account pages.
2. Register the handler through [placement.bind](../placement-bind.md) and pass the appropriate `PLACEMENT`.
3. Parse `PLACEMENT_OPTIONS` in the handler to obtain the launch context.
4. If the widget needs to open the application interface, use the [JavaScript methods for widgets](../bx24-widget-methods.md).
5. If the scenario requires signal exchange between the backend and frontend, connect to [interactive interaction](../../../settings/interactivity/index.md).

## Features of Universal Widgets

**REST_APP_URI.** The widget opens via a link in the format `/marketplace/view/#APP_CODE#/` and can accept arbitrary parameters through `params`. This scenario is suitable when the application needs to be launched from text or other content where a link can be placed.

**PAGE_BACKGROUND_WORKER.** The widget loads on all pages of the account without a separate visual element. The handler must respond quickly; otherwise, Bitrix24 may disable it and notify the application via `OPTIONS[errorHandlerUrl]`.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

{% list tabs %}

- REST_APP_URI

    ```php
    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 195ec4ee87932d8f9bbbd6a2f0a83553
        [AUTH_ID] => f27bbb6600705a0700005a4b00000001f0f107398c3f17f5fc48d5ce194d5c65de7cfb
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => e2fae26600705a0700005a4b00000001f0f1075f986dbd8dff24c36c2ad9bb0816a665
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => REST_APP_URI
        [PLACEMENT_OPTIONS] => {"test":"y"}
    )
    ```

- PAGE_BACKGROUND_WORKER

    ```php
    Array
    (
        [handler] => 1
        [DOMAIN] => restapi.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 588b8a98e848778a4ffb38fbcf70f2b9
        [AUTH_ID] => 4172bb6600705a0700005a4b00000001f0f107c42ca5bd5f61030c5d9c3e4d60d11b5a
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 31f1e26600705a0700005a4b00000001f0f107b1918506d8a2ed9ecf76e8fdac962471
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => PAGE_BACKGROUND_WORKER
        [PLACEMENT_OPTIONS] => {"ID":"PAGE_BACKGROUND_WORKER","URI":"\/company\/personal\/user\/1\/blog\/"}
    )
    ```

{% endlist %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the launch context.

#|
|| **Widget** | **Keys** | **Description** ||
|| [REST_APP_URI](./app-url.md) | Arbitrary keys from `params` | Parameters that the application passes in a special link in the format `/marketplace/view/#APP_CODE#/` ||
|| [PAGE_BACKGROUND_WORKER](./background-worker.md) | `ID`, `URI` | Widget code and the address of the current page where the handler was launched ||
|#

## Connection with Other Objects

**Application Slider.** For scenarios where the application interface needs to be opened after a background signal, use the [JavaScript methods for widgets](../bx24-widget-methods.md).

**Interactivity.** The `PAGE_BACKGROUND_WORKER` widget is often used in conjunction with the [interactive interaction](../../../settings/interactivity/index.md) mechanism to transmit signals between the backend and frontend without manual initiation from the user.

## Overview of Widgets {#all-widgets}

> Scope: [`placement`](../../scopes/permissions.md)

#|
|| **Widget** | **When to Use** ||
|| [REST_APP_URI](./app-url.md) | Open the application via a special link in text, comment, task, or other content ||
|| [PAGE_BACKGROUND_WORKER](./background-worker.md) | Execute a background scenario on all pages of the account without a separate interface element ||
|#