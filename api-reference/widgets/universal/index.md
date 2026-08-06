# Universal Widgets: Overview of Placements

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Universal widgets are not tied to a specific Bitrix24 tool: they have neither their own button nor their own menu. The handler is called either through a link that the application itself placed in the content, or on every Bitrix24 page without any user action. Both placements require the `placement` scope.

To register a widget, use the [placement.bind](../placement-bind.md) method and pass the required code in the `PLACEMENT` parameter.

> Quick navigation: [all placements](#all-placements)

## How to Choose a Placement

The placements differ in who launches the handler.

#|
|| **Placement** | **Who Launches It** | **When to Use** ||
|| [REST_APP_URI](./app-url.md) | The user — by following the application link | The application places a link in a message, comment, task, or other content, and the handler opens in a slider over the current page. Custom parameters can be passed in the link ||
|| [PAGE_BACKGROUND_WORKER](./background-worker.md) | Bitrix24 — on every page load | The application needs code that runs on all Bitrix24 pages: receiving signals from its own backend, telephony integration, opening the application interface automatically. The placement has no visible element ||
|#

## How to Get Started

1. Choose the placement by who launches the handler: the user through a link or Bitrix24 on every page
2. Register the handler with the [placement.bind](../placement-bind.md) method and pass the placement code in `PLACEMENT`. For `PAGE_BACKGROUND_WORKER`, `OPTIONS[errorHandlerUrl]` is mandatory
3. Complete the application installation — until then the handler is not called
4. Call the placement: follow the link `/marketplace/view/#APP_CODE#/` or open any Bitrix24 page
5. Parse `PLACEMENT_OPTIONS` to get the launch context
6. If the widget has to open the application interface, use the [JavaScript methods for widgets](../bx24-widget-methods.md)

Both placements are registered in a single copy: a repeated `placement.bind` for the same application returns the error `ERROR_PLACEMENT_MAX_COUNT`.

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

{% list tabs %}

- REST_APP_URI

    ```php
    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 9ecab44f06b9efb6c37d7b02180422b2
        [AUTH_ID] => 913374660070f28d001e30ba00000001f0f1073c8a5e2b7d94f16c0a3e58d271
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 81b29b660070f28d001e30ba00000001f0f107e4d1a9b3f508c72e6d95af3b04
        [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
        [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
        [APPLICATION_SCOPE] => placement
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => REST_APP_URI
        [PLACEMENT_OPTIONS] => {"test":"y","docId":"42","URI":"\/company\/personal\/user\/1\/blog\/"}
    )
    ```

- PAGE_BACKGROUND_WORKER

    ```php
    Array
    (
        [DOMAIN] => xxx.bitrix24.com
        [PROTOCOL] => 1
        [LANG] => en
        [APP_SID] => 588b8a98e848778a4ffb38fbcf70f2b9
        [AUTH_ID] => 4172bb660070f28d001e30ba00000001f0f107c42ca5bd5f61030c5d9c3e4d60
        [AUTH_EXPIRES] => 3600
        [REFRESH_ID] => 31f1e2660070f28d001e30ba00000001f0f107b1918506d8a2ed9ecf76e8fdac
        [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
        [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
        [APPLICATION_SCOPE] => placement
        [member_id] => da45a03b265edd8787f8a258d793cc5d
        [status] => L
        [PLACEMENT] => PAGE_BACKGROUND_WORKER
        [PLACEMENT_OPTIONS] => {"ID":"PAGE_BACKGROUND_WORKER","URI":"\/company\/personal\/user\/1\/blog\/"}
    )
    ```

{% endlist %}

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the call context.

#|
|| **Placement** | **Keys** | **What They Contain** ||
|| [REST_APP_URI](./app-url.md) | Keys from the link `params`, `URI` | The parameters the application set in the link itself and the address of the page the user came from ||
|| [PAGE_BACKGROUND_WORKER](./background-worker.md) | `ID`, `URI` | The placement code and the address of the page where the handler loaded ||
|#

The `URI` key is universal: it is passed to any placement and carries the path with the query string of the Bitrix24 page the widget is opened from.

## Relationship With Other Objects

**Application interface.** Both placements open the application in its own frame, so the [JavaScript methods for widgets](../bx24-widget-methods.md) work further: `openApplication` opens the application slider, `closeApplication` closes it.

**Signal exchange.** `PAGE_BACKGROUND_WORKER` is the only placement that works without any user action, so the application backend passes a signal to the browser through it using the [interactive interaction](../../../settings/interactivity/index.md) mechanism.

**Call card.** Telephony applications control the call card from the background handler — the methods and events are described in the [{#T}](../ui-interaction/page-background-worker/index.md) section.

## Common Mistakes

#|
|| **Mistake** | **Solution** ||
|| `placement.bind` returns `EMPTY_ERROR_HANDLER_URL` | The `PAGE_BACKGROUND_WORKER` placement requires an address for deactivation messages. Pass `OPTIONS[errorHandlerUrl]` ||
|| `placement.bind` returns `ERROR_PLACEMENT_MAX_COUNT` | A handler for this placement is already registered. Remove the old registration with the [placement.unbind](../placement-unbind.md) method ||
|| The `/marketplace/view/#APP_CODE#/` link opens an empty slider | Check that the application installation is complete and that the link contains the application code, not the handler registration identifier ||
|| The background handler stopped being called | The registration is deleted if the handler took longer than five seconds to load more than ten times a day. Bitrix24 reports this to the address from `errorHandlerUrl` ||
|#

## Overview of Placements {#all-placements}

> Scope: [`placement`](../../scopes/permissions.md)

#|
|| **Placement** | **When to Use** ||
|| [REST_APP_URI](./app-url.md) | Open the application in a slider through a link in a message, comment, task, or other content ||
|| [PAGE_BACKGROUND_WORKER](./background-worker.md) | Run a background scenario on all Bitrix24 pages without a visible interface element ||
|#

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../placement-get.md)
- [{#T}](../placement-unbind.md)
- [{#T}](../bx24-widget-methods.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
