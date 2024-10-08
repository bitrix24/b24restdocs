# Get a List of Methods and Their Descriptions task.elapseditem.getmanifest

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of methods in the form of `task.elapseditem.*` along with their descriptions.

The return value of this method is not intended for automated processing, as its format may change without notice.

This method can be useful as reference information, as it always contains up-to-date information.

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.elapseditem.getmanifest
    ```

- cURL (OAuth)

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.elapseditem.getmanifest
    ```

- JS

    ```js
    BX24.callMethod(
        'task.elapseditem.getmanifest',
        {},
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.elapseditem.getmanifest',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result":{
        "Manifest version": "1.2",
        "Manifest change date": "22 Feb 2018",
        "Warning": "Don't rely on the format of this manifest, it can be changed without any notifications!",
        "REST: shortname alias to class": "elapseditem",
        "REST: writable elapseditem data fields": [
            "USER_ID",
            "COMMENT_TEXT",
            "SECONDS",
            "SOURCE",
            "CREATED_DATE",
            "DATE_START",
            "DATE_STOP"
        ],
        "REST: readable elapseditem data fields": [
            "ID",
            "TASK_ID",
            "USER_ID",
            "COMMENT_TEXT",
            "SECONDS",
            "MINUTES",
            "SOURCE",
            "CREATED_DATE",
            "DATE_START",
            "DATE_STOP"
        ],
        "REST: sortable elapseditem data fields": [
            "ID",
            "TASK_ID",
            "USER_ID",
            "SECONDS",
            "MINUTES",
            "CREATED_DATE",
            "DATE_START",
            "DATE_STOP"
        ],
        "REST: filterable elapseditem data fields": [
            "ID",
            "TASK_ID",
            "USER_ID",
            "CREATED_DATE"
        ],
        "REST: date fields": [
            "CREATED_DATE",
            "DATE_START",
            "DATE_STOP"
        ],
        "REST: available methods": {
            "getmanifest": {
                "staticMethod": true,
                "params": []
            },
            "getlist": {
                "staticMethod": true,
                "params": [
                    {
                        "description": "taskId",
                        "type": "integer"
                    },
                    {
                        "description": "order",
                        "type": "array",
                        "allowedKeys": [
                            "ID",
                            "TASK_ID",
                            "USER_ID",
                            "SECONDS",
                            "MINUTES",
                            "CREATED_DATE",
                            "DATE_START",
                            "DATE_STOP"
                        ]
                    },
                    {
                        "description": "filter",
                        "type": "array",
                        "allowedKeys": [
                            "ID",
                            "TASK_ID",
                            "USER_ID",
                            "CREATED_DATE"
                        ],
                        "allowedKeyPrefixes": [
                            "!",
                            "<=",
                            "<",
                            ">=",
                            ">"
                        ]
                    },
                    {
                        "description": "select",
                        "type": "array",
                        "allowedValues": [
                            "",
                            "*",
                            "ID",
                            "TASK_ID",
                            "USER_ID",
                            "COMMENT_TEXT",
                            "SECONDS",
                            "MINUTES",
                            "SOURCE",
                            "CREATED_DATE",
                            "DATE_START",
                            "DATE_STOP"
                        ],
                        "allowedAggregations": [
                            "MAX",
                            "MIN",
                            "COUNT",
                            "SUM",
                            "AVG"
                        ],
                        "allowedValuesInAggregation": [
                            "ID",
                            "USER_ID",
                            "SECONDS",
                            "MINUTES"
                        ]
                    },
                    {
                        "description": "params",
                        "type": "array",
                        "allowedKeys": [
                            "NAV_PARAMS"
                        ]
                    }
                ],
                "allowedKeysInReturnValue": [
                    "ID",
                    "TASK_ID",
                    "USER_ID",
                    "COMMENT_TEXT",
                    "SECONDS",
                    "MINUTES",
                    "SOURCE",
                    "CREATED_DATE",
                    "DATE_START",
                    "DATE_STOP"
                ],
                "allowedAggregations": [
                    "MAX",
                    "MIN",
                    "COUNT",
                    "SUM",
                    "AVG"
                ],
                "collectionInReturnValue": true
            },
            "get": {
                "mandatoryParamsCount": 2,
                "params": [
                    {
                        "description": "taskId",
                        "type": "integer"
                    },
                    {
                        "description": "itemId",
                        "type": "integer"
                    }
                ],
                "allowedKeysInReturnValue": [
                    "ID",
                    "TASK_ID",
                    "USER_ID",
                    "COMMENT_TEXT",
                    "SECONDS",
                    "MINUTES",
                    "SOURCE",
                    "CREATED_DATE",
                    "DATE_START",
                    "DATE_STOP"
                ]
            },
            "add": {
                "staticMethod": true,
                "mandatoryParamsCount": 2,
                "params": [
                    {
                        "description": "taskId",
                        "type": "integer"
                    },
                    {
                        "description": "fields",
                        "type": "array",
                        "allowedKeys": [
                            "USER_ID",
                            "COMMENT_TEXT",
                            "SECONDS",
                            "SOURCE",
                            "CREATED_DATE",
                            "DATE_START",
                            "DATE_STOP"
                        ]
                    }
                ]
            },
            "update": {
                "staticMethod": false,
                "mandatoryParamsCount": 3,
                "params": [
                    {
                        "description": "taskId",
                        "type": "integer"
                    },
                    {
                        "description": "itemId",
                        "type": "integer"
                    },
                    {
                        "description": "fields",
                        "type": "array",
                        "allowedKeys": [
                            "USER_ID",
                            "COMMENT_TEXT",
                            "SECONDS",
                            "SOURCE",
                            "CREATED_DATE",
                            "DATE_START",
                            "DATE_STOP"
                        ]
                    }
                ]
            },
            "delete": {
                "staticMethod": false,
                "mandatoryParamsCount": 2,
                "params": [
                    {
                        "description": "taskId",
                        "type": "integer"
                    },
                    {
                        "description": "itemId",
                        "type": "integer"
                    }
                ]
            },
            "isactionallowed": {
                "staticMethod": false,
                "mandatoryParamsCount": 3,
                "params": [
                    {
                        "description": "taskId",
                        "type": "integer"
                    },
                    {
                        "description": "itemId",
                        "type": "integer"
                    },
                    {
                        "description": "actionId",
                        "type": "integer"
                    }
                ]
            }
        }
    },
    "time":{
        "start":1712137817.343984,
        "finish":1712137817.605804,
        "duration":0.26182007789611816,
        "processing":0.018325090408325195,
        "date_start":"2024-04-03T12:50:17+03:00",
        "date_finish":"2024-04-03T12:50:17+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Descriptions of the methods `task.elapseditem.*` ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./task-elapsed-item-add.md)
- [{#T}](./task-elapsed-item-update.md)
- [{#T}](./task-elapsed-item-get.md)
- [{#T}](./task-elapsed-item-get-list.md)
- [{#T}](./task-elapsed-item-delete.md)
- [{#T}](./task-elapsed-item-is-action-allowed.md)