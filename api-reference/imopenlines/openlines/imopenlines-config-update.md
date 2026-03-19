# Update Open Channel imopenlines.config.update

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to modify open channels

The method `imopenlines.config.update` updates an open channel.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONFIG_ID***
[`integer`](../../data-types.md) | Identifier of the open channel.

You can obtain the identifier of the open channel when [creating an open channel](./imopenlines-config-add.md) or by using the [method to get the list of open channels](./imopenlines-config-list-get.md) ||
|| **PARAMS**
[`object`](../../data-types.md) | Object with settings for the update. The set of fields corresponds to the [PARAMS](./imopenlines-config-add.md#params) parameter of the `imopenlines.config.add` method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONFIG_ID": 15,
        "PARAMS": {
          "LINE_NAME": "Online Store Support Line (VIP)",
          "QUEUE": [
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "1"
            },
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "15"
            },
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "23"
            }
          ],
          "QUEUE_TYPE": "strictly",
          "QUEUE_TIME": 45,
          "NO_ANSWER_TIME": 120,
          "WELCOME_MESSAGE": "Y",
          "WELCOME_MESSAGE_TEXT": "Hello! We will respond within a couple of minutes",
          "WORKTIME_ENABLE": "Y",
          "WORKTIME_FROM": "09:00",
          "WORKTIME_TO": "21:00",
          "WORKTIME_TIMEZONE": "Europe/Berlin"
        }
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.config.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "CONFIG_ID": 15,
        "PARAMS": {
          "LINE_NAME": "Online Store Support Line (VIP)",
          "QUEUE": [
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "1"
            },
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "15"
            },
            {
              "ENTITY_TYPE": "user",
              "ENTITY_ID": "23"
            }
          ],
          "QUEUE_TYPE": "strictly",
          "QUEUE_TIME": 45,
          "NO_ANSWER_TIME": 120,
          "WELCOME_MESSAGE": "Y",
          "WELCOME_MESSAGE_TEXT": "Hello! We will respond within a couple of minutes",
          "WORKTIME_ENABLE": "Y",
          "WORKTIME_FROM": "09:00",
          "WORKTIME_TO": "21:00",
          "WORKTIME_TIMEZONE": "Europe/Berlin"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/imopenlines.config.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'imopenlines.config.update',
            {
                CONFIG_ID: 15,
                PARAMS: {
                    LINE_NAME: 'Online Store Support Line (VIP)',
                    QUEUE: [
                        {
                            ENTITY_TYPE: 'user',
                            ENTITY_ID: '1'
                        },
                        {
                            ENTITY_TYPE: 'user',
                            ENTITY_ID: '15'
                        },
                        {
                            ENTITY_TYPE: 'user',
                            ENTITY_ID: '23'
                        }
                    ],
                    QUEUE_TYPE: 'strictly',
                    QUEUE_TIME: 45,
                    NO_ANSWER_TIME: 120,
                    WELCOME_MESSAGE: 'Y',
                    WELCOME_MESSAGE_TEXT: 'Hello! We will respond within a couple of minutes',
                    WORKTIME_ENABLE: 'Y',
                    WORKTIME_FROM: '09:00',
                    WORKTIME_TO: '21:00',
                    WORKTIME_TIMEZONE: 'Europe/Berlin'
                }
            }
        );

        const result = response.getData().result;
        console.log(result);
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
                'imopenlines.config.update',
                [
                    'CONFIG_ID' => 15,
                    'PARAMS' => [
                        'LINE_NAME' => 'Online Store Support Line (VIP)',
                        'QUEUE' => [
                            [
                                'ENTITY_TYPE' => 'user',
                                'ENTITY_ID' => '1',
                            ],
                            [
                                'ENTITY_TYPE' => 'user',
                                'ENTITY_ID' => '15',
                            ],
                            [
                                'ENTITY_TYPE' => 'user',
                                'ENTITY_ID' => '23',
                            ],
                        ],
                        'QUEUE_TYPE' => 'strictly',
                        'QUEUE_TIME' => 45,
                        'NO_ANSWER_TIME' => 120,
                        'WELCOME_MESSAGE' => 'Y',
                        'WELCOME_MESSAGE_TEXT' => 'Hello! We will respond within a couple of minutes',
                        'WORKTIME_ENABLE' => 'Y',
                        'WORKTIME_FROM' => '09:00',
                        'WORKTIME_TO' => '21:00',
                        'WORKTIME_TIMEZONE' => 'Europe/Berlin',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.config.update',
        {
            CONFIG_ID: 15,
            PARAMS: {
                LINE_NAME: 'Online Store Support Line (VIP)',
                QUEUE: [
                    {
                        ENTITY_TYPE: 'user',
                        ENTITY_ID: '1'
                    },
                    {
                        ENTITY_TYPE: 'user',
                        ENTITY_ID: '15'
                    },
                    {
                        ENTITY_TYPE: 'user',
                        ENTITY_ID: '23'
                    }
                ],
                QUEUE_TYPE: 'strictly',
                QUEUE_TIME: 45,
                NO_ANSWER_TIME: 120,
                WELCOME_MESSAGE: 'Y',
                WELCOME_MESSAGE_TEXT: 'Hello! We will respond within a couple of minutes',
                WORKTIME_ENABLE: 'Y',
                WORKTIME_FROM: '09:00',
                WORKTIME_TO: '21:00',
                WORKTIME_TIMEZONE: 'Europe/Berlin'
            }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.config.update',
        [
            'CONFIG_ID' => 15,
            'PARAMS' => [
                'LINE_NAME' => 'Online Store Support Line (VIP)',
                'QUEUE' => [
                    [
                        'ENTITY_TYPE' => 'user',
                        'ENTITY_ID' => '1',
                    ],
                    [
                        'ENTITY_TYPE' => 'user',
                        'ENTITY_ID' => '15',
                    ],
                    [
                        'ENTITY_TYPE' => 'user',
                        'ENTITY_ID' => '23',
                    ],
                ],
                'QUEUE_TYPE' => 'strictly',
                'QUEUE_TIME' => 45,
                'NO_ANSWER_TIME' => 120,
                'WELCOME_MESSAGE' => 'Y',
                'WELCOME_MESSAGE_TEXT' => 'Hello! We will respond within a couple of minutes',
                'WORKTIME_ENABLE' => 'Y',
                'WORKTIME_FROM' => '09:00',
                'WORKTIME_TO' => '21:00',
                'WORKTIME_TIMEZONE' => 'Europe/Berlin',
            ],
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773667094,
        "finish": 1773667094.239236,
        "duration": 0.23923611640930176,
        "processing": 0,
        "date_start": "2026-03-16T16:18:14+01:00",
        "date_finish": "2026-03-16T16:18:14+01:00",
        "operating_reset_at": 1773667694,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns:
- `true` — if the channel was successfully updated
- `false` — if the channel with the specified `CONFIG_ID` does not exist ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CONFIG_ID_EMPTY",
    "error_description": "Config ID can't be empty"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CONFIG_ID_EMPTY` | Config ID can't be empty | The `CONFIG_ID` parameter is not provided or is incorrect ||
|| `403` | `CONFIG_WRONG_USER_PERMISSION` | Permission denied | Insufficient rights to modify the channel ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./imopenlines-config-add.md)
- [{#T}](./imopenlines-config-get.md)
- [{#T}](./imopenlines-config-list-get.md)
- [{#T}](./imopenlines-config-delete.md)
- [{#T}](./imopenlines-config-path-get.md)