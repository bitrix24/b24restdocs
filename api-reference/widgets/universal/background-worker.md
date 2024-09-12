# Invisible Widget on Every Page PAGE_BACKGROUND_WORKER

> Scope: [`placement`](../../scopes/permissions.md)

You can add an "invisible" widget that will be displayed on all pages of Bitrix24. This widget enables the implementation of scenarios with an external [WebRTC client](../ui-interaction/page-background-worker/index.md) in telephony integrations, but this is not the only possible use case.

For example, using the [interactive interaction](../../interactivity/index.md) mechanism between the backend and frontend applications, you can send a "signal" to the `PAGE_BACKGROUND_WORKER` widget, and upon receiving the "signal," open the application slider using the [openApplication](../open-application.md) method.

The widget embedding code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

## Features of Widget Handler Registration

Unlike other types of widgets, for `PAGE_BACKGROUND_WORKER`, the application can register only one handler.

Since this widget loads on all pages, a handler that takes longer than 3-5 seconds to load may cause delays in rendering the Bitrix24 user interface. If this occurs more than 10 times within a day on the same Bitrix24, the handler will be automatically disabled.

Bitrix24 will notify the application about the handler's disablement. To do this, in the [placement.bind](../placement-bind.md) method, you need to specify the URL in the `OPTIONS[errorHandlerUrl]` parameter. Bitrix24 will call this URL in case the `PAGE_BACKGROUND_WORKER` widget handler is disabled.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"PAGE_BACKGROUND_WORKER","HANDLER":"http://myapp.com/handler/?type=1","OPTIONS":{"errorHandlerUrl":"http://myapp.com/error/"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/placement.bind
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"PAGE_BACKGROUND_WORKER","HANDLER":"http://myapp.com/handler/?type=1","OPTIONS":{"errorHandlerUrl":"http://myapp.com/error/"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/placement.bind
    ```

- JS

    ```js
    BX24.callMethod(
        "placement.bind",
        { 
            PLACEMENT: "PAGE_BACKGROUND_WORKER",
            HANDLER: "http://myapp.com/handler/?type=1",
            OPTIONS: {
                errorHandlerUrl: "http://myapp.com/error/"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'PAGE_BACKGROUND_WORKER',
            'HANDLER' => 'http://myapp.com/handler/?type=1',
            'OPTIONS' => [
                'errorHandlerUrl' => 'http://myapp.com/error/'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

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

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **ID***
[`string`](../../data-types.md) | Always equals `PAGE_BACKGROUND_WORKER` and is used for internal purposes

||
|| **URI***
[`string`](../../data-types.md) | URL-encoded address of the current page where the widget was opened.

||
|#

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../ui-interaction/page-background-worker/index.md)

{% endnote %}

## Continue Learning

- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../ui-interaction/crm-card.md)
- [{#T}](../../interactivity/index.md)
- [{#T}](../open-application.md)
- [{#T}](../open-path.md)