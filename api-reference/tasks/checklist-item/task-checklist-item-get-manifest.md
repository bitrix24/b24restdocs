# Get a list of methods and their description task.checklistitem.getmanifest

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.checklistitem.getmanifest` retrieves information about the methods for working with task checklist items `task.checklistitem.*`.

It is recommended to use the result only as a reference, as the structure of the method's response can be changed by the developer at any time.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/task.checklistitem.getmanifest
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/task.checklistitem.getmanifest
    ```

- JS

    ```javascript
    try
    {
        const response = await $b24.callMethod(
            'task.checklistitem.getmanifest',
            []
        );
        
        const result = response.getData().result;
        console.log('Manifest data:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'task.checklistitem.getmanifest',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting manifest: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'task.checklistitem.getmanifest',
        {},
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'task.checklistitem.getmanifest',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "Manifest version": "2.0",
        "Warning": "don't rely on format of this manifest, it can be changed without any notification",
        "REST: shortname alias to class": "checklistitem",
        "REST: writable checklistitem data fields": [
            "PARENT_ID",
            "TITLE",
            "SORT_INDEX",
            "IS_COMPLETE",
            "IS_IMPORTANT",
            "MEMBERS"
        ],
        "REST: readable checklistitem data fields": [
            "ID",
            "TASK_ID",
            "PARENT_ID",
            "CREATED_BY",
            "TITLE",
            "SORT_INDEX",
            "IS_COMPLETE",
            "IS_IMPORTANT",
            "TOGGLED_BY",
            "TOGGLED_DATE",
            "MEMBERS",
            "ATTACHMENTS"
        ],
        "REST: sortable checklistitem data fields": [
            "ID",
            "PARENT_ID",
            "CREATED_BY",
            "TITLE",
            "SORT_INDEX",
            "IS_COMPLETE",
            "IS_IMPORTANT",
            "TOGGLED_BY",
            "TOGGLED_DATE"
        ],
        "REST: date fields": [
            "TOGGLED_DATE"
        ],
        "REST: available methods": {
            "getmanifest": {
                "staticMethod": true,
                "params": []
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
                    "PARENT_ID",
                    "CREATED_BY",
                    "TITLE",
                    "SORT_INDEX",
                    "IS_COMPLETE",
                    "IS_IMPORTANT",
                    "TOGGLED_BY",
                    "TOGGLED_DATE",
                    "MEMBERS",
                    "ATTACHMENTS"
                ]
            },
            "getlist": {
                "staticMethod": true,
                "mandatoryParamsCount": 1,
                "params": [
                    {
                        "description": "taskId",
                        "type": "integer"
                    },
                    {
                        "description": "arOrder",
                        "type": "array",
                        "allowedKeys": [
                            "ID",
                            "PARENT_ID",
                            "CREATED_BY",
                            "TITLE",
                            "SORT_INDEX",
                            "IS_COMPLETE",
                            "IS_IMPORTANT",
                            "TOGGLED_BY",
                            "TOGGLED_DATE"
                        ]
                    }
                ],
                "allowedKeysInReturnValue": [
                    "ID",
                    "TASK_ID",
                    "PARENT_ID",
                    "CREATED_BY",
                    "TITLE",
                    "SORT_INDEX",
                    "IS_COMPLETE",
                    "IS_IMPORTANT",
                    "TOGGLED_BY",
                    "TOGGLED_DATE",
                    "MEMBERS",
                    "ATTACHMENTS"
                ],
                "collectionInReturnValue": true
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
                        "description": "arFields",
                        "type": "array",
                        "allowedKeys": [
                            "PARENT_ID",
                            "TITLE",
                            "SORT_INDEX",
                            "IS_COMPLETE",
                            "IS_IMPORTANT",
                            "MEMBERS"
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
                        "description": "arFields",
                        "type": "array",
                        "allowedKeys": [
                            "PARENT_ID",
                            "TITLE",
                            "SORT_INDEX",
                            "IS_COMPLETE",
                            "IS_IMPORTANT",
                            "MEMBERS"
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
            "complete": {
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
            "renew": {
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
            "moveafteritem": {
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
                        "description": "afterItemId",
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
    "time": {
        "start": 1769070876,
        "finish": 1769070877.009113,
        "duration": 1.009113073348999,
        "processing": 0,
        "date_start": "2026-01-22T11:34:36+01:00",
        "date_finish": "2026-01-22T11:34:37+01:00",
        "operating_reset_at": 1769071477,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object describing the methods `task.checklistitem.*` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include notitle [error handling](../../../_includes/error-info.md) %}

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./task-checklist-item-add.md)
- [{#T}](./task-checklist-item-update.md)
- [{#T}](./task-checklist-item-get.md)
- [{#T}](./task-checklist-item-get-list.md)
- [{#T}](./task-checklist-item-delete.md)
- [{#T}](./task-checklist-item-move-after-item.md)
- [{#T}](./task-checklist-item-complete.md)
- [{#T}](./task-checklist-item-renew.md)
- [{#T}](./task-checklist-item-is-action-allowed.md)