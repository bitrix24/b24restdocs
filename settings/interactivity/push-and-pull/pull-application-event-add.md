# Send Events to the RT Channel of the pull.application.event.add

> Scope: [`pull`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: a user authorized in the application

The method `pull.application.event.add` sends an event to the RT channel of the application.

{% note info "" %}

The method works only in the context of the [application](../../app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMMAND**^*^
[`string`](../../../api-reference/data-types.md) | The event command.

The parameter accepts any command value as long as it adheres to the format.

Allowed characters: `A-Z`, `a-z`, `0-9`, `_`, `:`, `|`, `.`, `-` ||
|| **PARAMS**
[`object`](../../../api-reference/data-types.md) | Event parameters in the format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n
}
```

where:
- `field_n` - the name of the event parameter
- `value_n` - the value of the event parameter

The parameter accepts any name and value.

Example:

```json
{
    "grid_id": 15,
    "status": "done"
}
``` ||
|| **MODULE_ID**
[`string`](../../../api-reference/data-types.md) | The identifier of the event module.

The parameter accepts any identifier value as long as it adheres to the format.

Allowed characters: `a-z`, `0-9`, `.`, `_`.

If the parameter is not provided, `application` is used as the default event module identifier. ||
|| **USER_ID**
[`integer`](../../../api-reference/data-types.md) \| [`integer[]`](../../../api-reference/data-types.md) | The user identifier or an array of user identifiers.

`USER_ID` can be obtained by:
- using the [user.get](../../../api-reference/user/user-get.md) method
- using the [user.current](../../../api-reference/user/user-current.md) method for the current user

A user without admin rights can only specify their own identifier.

If the parameter is not provided, the event is sent to the general channel. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of sending an event to the general channel of the application, where:
- `COMMAND` - the event command
- `PARAMS` - the event parameters
- `MODULE_ID` - the identifier of the event module

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "COMMAND": "test_event",
        "PARAMS": {
          "param1": "value1"
        },
        "MODULE_ID": "application",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/pull.application.event.add.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'pull.application.event.add',
    		{
    			COMMAND: 'test_event',
    			PARAMS: {
    				param1: 'value1'
    			},
    			MODULE_ID: 'application'
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
                'pull.application.event.add',
                [
                    'COMMAND' => 'test_event',
                    'PARAMS' => [
                        'param1' => 'value1',
                    ],
                    'MODULE_ID' => 'application',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error sending event: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'pull.application.event.add',
        {
            COMMAND: 'test_event',
            PARAMS: {
                param1: 'value1'
            },
            MODULE_ID: 'application'
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
        'pull.application.event.add',
        [
            'COMMAND' => 'test_event',
            'PARAMS' => [
                'param1' => 'value1',
            ],
            'MODULE_ID' => 'application',
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
    "result": true,
    "time": {
        "start": 1743495945,
        "finish": 1743495945.285066,
        "duration": 0.2850658893585205,
        "processing": 0.008597135543823242,
        "date_start": "2025-04-01T11:52:25+02:00",
        "date_finish": "2025-04-01T11:52:25+02:00",
        "operating_reset_at": 1743496545,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../api-reference/data-types.md) | Indicator of successful event sending ||
|| **time**
[`time`](../../../api-reference/data-types.md#time) | Information about the request execution time ||
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
|| `403` | `WRONG_AUTH_TYPE` | Get access to application config available only for application authorization. | Method call not from the application authorization context ||
|| `400` | `USER_ID_ACCESS_ERROR` | Only admin can send notifications to other channels | A user without admin rights attempts to send an event to another channel ||
|| `400` | `MODULE_ID_ERROR` | Module ID format error | An incorrect type of `MODULE_ID` was provided ||
|| `400` | `COMMAND_ERROR` | Command format error | An incorrect type of `COMMAND` was provided ||
|| `400` | `PARAMS_ERROR` | Params format error | An incorrect type of `PARAMS` was provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../../interactivity/index.md)
- [{#T}](./pull-application-config-get.md)
- [{#T}](./pull-application-push-add.md)