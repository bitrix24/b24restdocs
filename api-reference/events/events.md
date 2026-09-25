# Get a List of Available Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Who can execute the method: any user

Retrieves Bitrix24 event codes. The application uses this list to choose which events to subscribe to with the [event.bind](./event-bind.md) method. The events included in the list depend on the `SCOPE` and `FULL` parameters.

The method works only in the context of authorizing the [application](../../settings/app-installation/index.md). When called through a webhook, it returns the `WRONG_AUTH_TYPE` error.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **SCOPE**
[`string`](../data-types.md) | [Scope](../scopes/permissions.md) whose events you need to retrieve, for example `crm` or `user`. The method returns events of this scope only, even if the application does not have this permission.

If you pass an empty string, the method returns only the common application events. For an unknown scope, the method returns an empty array without an error ||
|| **FULL**
[`boolean`](../data-types.md) | If you pass `true`, the method returns all Bitrix24 events regardless of the application's permissions.

The parameter has no effect if `SCOPE` is passed, even an empty one ||
|#

If no parameters are passed, the method returns events from the application's scope and common events available to any application: for example, [ONAPPINSTALL](../common/events/on-app-install.md) and [ONOFFLINEEVENT](./on-offline-event.md).

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    Example №1

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "SCOPE": "user",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/events
    ```

    Example №2

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "FULL": true,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/events
    ```

- BX24.js

    Example №1

    ```js
    BX24.callMethod(
        "events",
        {
            "SCOPE": "user"
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

    Example №2

    ```js
    BX24.callMethod(
        "events",
        {
            "FULL": true
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- Python

    Example №1

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.events(
            scope="user",
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

    Example №2

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.events(
            full=True,
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP CRest

    Example №1

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'events',
        [
            'SCOPE' => 'user'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

    Example №2

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'events',
        [
            'FULL' => true
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

Response to the first example — a request with `SCOPE: "user"`:

```json
{
    "result": [
        "ONUSERADD"
    ],
    "time": {
        "start": 1790304784,
        "finish": 1790304784.638336,
        "duration": 0.6383359432220459,
        "processing": 0,
        "date_start": "2026-09-25T05:53:04+03:00",
        "date_finish": "2026-09-25T05:53:04+03:00",
        "operating_reset_at": 1790305384,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | An array of strings — symbolic event codes in uppercase, for example `ONCRMDEALADD`. The code is passed in the `event` parameter of the [event.bind](./event-bind.md) method.

The codes included in the array depend on the `SCOPE` and `FULL` parameters ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method"
}
```

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Error message** | **Description** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was called outside an application, for example, through a webhook ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./event-bind.md)
- [{#T}](./event-get.md)
- [{#T}](./event-unbind.md)
- [{#T}](./safe-event-handlers.md)
- [{#T}](./offline-events.md)
- [{#T}](./event-offline-list.md)
- [{#T}](./event-offline-get.md)
- [{#T}](./event-offline-clear.md)
- [{#T}](./event-offline-error.md)
- [{#T}](./on-offline-event.md)
