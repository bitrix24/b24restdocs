# Get Connection Configuration for RT Servers pull.application.config.get

> Scope: [`pull`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: a user authorized in the application

The method `pull.application.config.get` returns the connection configuration for Push&Pull servers for the current application.

{% note info "" %}

The method works only in the context of the [application](../../app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CACHE**
[`string`](../../../api-reference/data-types.md) | Use cached data:

- `Y` — use cache
- `N` — do not use cache

If the parameter is not provided, the cache is used ||
|| **REOPEN**
[`string`](../../../api-reference/data-types.md) | Refresh channels upon expiration:

- `Y` — refresh
- `N` — do not refresh

If the parameter is not provided, refreshing is enabled ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

Example of obtaining Push&Pull configuration for the application, where:
- `CACHE` — cached data is used
- `REOPEN` — refreshes channels

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "CACHE": "Y",
        "REOPEN": "Y",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/pull.application.config.get.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'pull.application.config.get',
    		{
    			CACHE: 'Y',
    			REOPEN: 'Y'
    		}
    	);

    	const result = response.getData().result;
    	console.info(result);
    }
    catch (error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'pull.application.config.get',
                [
                    'CACHE' => 'Y',
                    'REOPEN' => 'Y',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting pull config: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'pull.application.config.get',
        {
            CACHE: 'Y',
            REOPEN: 'Y'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    $result = CRest::call(
        'pull.application.config.get',
        [
            'CACHE' => 'Y',
            'REOPEN' => 'Y',
        ]
    );

    echo '<pre>';
    print_r($result);
    echo '</pre>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "server": {
            "version": 4,
            "server_enabled": true,
            "mode": "personal",
            "hostname": "your-account.bitrix24.com",
            "long_polling": "https://rtc-**.bitrix24.com/sub2/",
            "long_pooling_secure": "https://rtc-**.bitrix24.com/sub2/",
            "websocket_enabled": true,
            "websocket": "wss://rtc-**.bitrix24.com/subws2/",
            "websocket_secure": "wss://rtc-**.bitrix24.com/subws2/",
            "publish_enabled": true,
            "publish": "https://rtc-**.bitrix24.com/rest/",
            "publish_secure": "https://rtc-**.bitrix24.com/rest/",
            "config_timestamp": 1774886062
        },
        "api": {
            "revision_web": 19,
            "revision_mobile": 3
        },
        "channels": {
            "shared": {
                "id": "***masked***",
                "start": "2026-03-31T17:05:18+02:00",
                "end": "2026-04-01T05:05:23+02:00",
                "type": "shared"
            },
            "private": {
                "id": "***masked***",
                "public_id": "***masked***",
                "start": "2026-03-31T17:05:18+02:00",
                "end": "2026-04-01T05:05:23+02:00",
                "type": "private"
            }
        },
        "exp": 1775052318,
        "publicChannels": {
            "<user_id>": {
                "user_id": 577,
                "public_id": "***masked***",
                "signature": "***masked***",
                "start": "2026-03-31T10:06:39+02:00",
                "end": "2026-03-31T22:06:44+02:00"
            }
        }
    },
    "time": {
        "start": 1774965918,
        "finish": 1774965918.322255,
        "duration": 0.32225489616394043,
        "processing": 0,
        "date_start": "2026-03-31T17:05:18+02:00",
        "date_finish": "2026-03-31T17:05:18+02:00",
        "operating_reset_at": 1774966518,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../api-reference/data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` — field of the `result` object
- `value_n` — value of the `result` field

See the fields of the `result` object in the [Result Type](#result-type) section.

The composition of fields may vary and depends on the Push&Pull server configuration ||
|| **time**
[`time`](../../../api-reference/data-types.md#time) | Information about the request execution time ||
|#

#### Result Type {#result-type}

#|
|| **Name**
`type` | **Description** ||
|| **server**
[`object`](../../../api-reference/data-types.md) | Push&Pull server parameters [more details](#result-server-type) ||
|| **api**
[`object`](../../../api-reference/data-types.md) | Push&Pull API versions:

- **revision_web** [`integer`](../../../api-reference/data-types.md) — web API version
- **revision_mobile** [`integer`](../../../api-reference/data-types.md) — mobile API version ||
|| **channels**
[`object`](../../../api-reference/data-types.md) | Application channels.

Contains two channel types: `shared` and `private`.

See the channel field descriptions in the [Shared/Private Channel Type](#result-channel-type) section ||
|| **exp**
[`integer`](../../../api-reference/data-types.md) | Expiration time of the configuration in Unix timestamp format ||
|| **publicChannels**
[`object`](../../../api-reference/data-types.md) | Public user channels in the format:

```
{
    "<user_id>": {
        "user_id": user_id,
        "public_id": "string",
        "signature": "string",
        "start": "datetime",
        "end": "datetime"
    }
}
```

where:
- `<user_id>` — user identifier
- `user_id` — user identifier
- `public_id` — public channel identifier
- `signature` — channel signature
- `start` — channel activation start time
- `end` — channel activation end time ||
|| **clientId**
[`string`](../../../api-reference/data-types.md) | Public client identifier.

Returned in shared mode of the server ||
|| **jwt**
[`string`](../../../api-reference/data-types.md) | JWT token for connection.

Returned if JWT issuance is enabled in the server configuration ||
|#

### Server Type {#result-server-type}

#|
|| **Name**
`type` | **Description** ||
|| **version**
[`integer`](../../../api-reference/data-types.md) | Push&Pull server version ||
|| **server_enabled**
[`boolean`](../../../api-reference/data-types.md) | Server availability indicator ||
|| **mode**
[`string`](../../../api-reference/data-types.md) | Server mode ||
|| **hostname**
[`string`](../../../api-reference/data-types.md) | Account hostname ||
|| **long_polling**
[`string`](../../../api-reference/data-types.md) | Long polling URL ||
|| **long_pooling_secure**
[`string`](../../../api-reference/data-types.md) | Long polling URL for secure connection ||
|| **websocket_enabled**
[`boolean`](../../../api-reference/data-types.md) | WebSocket availability indicator ||
|| **websocket**
[`string`](../../../api-reference/data-types.md) | WebSocket URL ||
|| **websocket_secure**
[`string`](../../../api-reference/data-types.md) | WebSocket URL for secure connection ||
|| **publish_enabled**
[`boolean`](../../../api-reference/data-types.md) | Publish API availability indicator ||
|| **publish**
[`string`](../../../api-reference/data-types.md) | Publish API URL ||
|| **publish_secure**
[`string`](../../../api-reference/data-types.md) | Publish API URL for secure connection ||
|| **config_timestamp**
[`integer`](../../../api-reference/data-types.md) | Configuration version timestamp ||
|#

### Shared/Private Channel Type {#result-channel-type}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`string`](../../../api-reference/data-types.md) | Channel identifier ||
|| **public_id**
[`string`](../../../api-reference/data-types.md) \| [`null`](../../../api-reference/data-types.md) | Public channel identifier.

May be absent for `shared` ||
|| **start**
[`datetime`](../../../api-reference/data-types.md) | Channel activation start time ||
|| **end**
[`datetime`](../../../api-reference/data-types.md) | Channel activation end time ||
|| **type**
[`string`](../../../api-reference/data-types.md) | Channel type:

- `shared`
- `private` ||
  |#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Get access to application config available only for application authorization."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Get access to application config available only for application authorization. | Method call not from the context of an OAuth application ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../../interactivity/index.md)
- [{#T}](./pull-application-event-add.md)
- [{#T}](./pull-application-push-add.md)