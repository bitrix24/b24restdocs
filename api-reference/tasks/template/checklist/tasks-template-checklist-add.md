# Add Checklist Item to Task Template tasks.template.checklist.add

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: user with permissions to modify the task template

The method `tasks.template.checklist.add` adds a checklist item to a task template.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../../data-types.md) | Identifier of the task template.

The task template identifier can be obtained when [creating a new template](../tasks-template-add.md) ||
|| **fields***
[`object`](../../../data-types.md) | Fields of the checklist item being created [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE***
[`string`](../../../data-types.md) | Text of the checklist item

If `PARENT_ID` is passed with a value of `0`, then `TITLE` is the name of the checklist ||
|| **PARENT_ID***
[`integer`](../../../data-types.md) | Identifier of the parent item.

To create a new checklist, pass `PARENT_ID` with a value of `0`.

If there is no checklist item in the template with the specified `PARENT_ID`, the system will create a new checklist ||
|| **SORT_INDEX**
[`integer`](../../../data-types.md) | Sort index. The lower the value, the higher the item in the list or sublist ||
|| **IS_COMPLETE**
[`boolean`](../../../data-types.md) | Status of the item. Possible values:
- `Y` — completed
- `N` — not completed

Default is `N` ||
|| **IS_IMPORTANT**
[`boolean`](../../../data-types.md) | Mark indicating that the item is important. Possible values:
- `Y` — important
- `N` — regular ||
|| **MEMBERS**
[`object`](../../../data-types.md) | Object describing the participants of the checklist item. Key — user identifier, value — object with the participant type parameter `TYPE`. Possible participant type values:
- `'TYPE': 'A'` — Participant
- `'TYPE': 'U'` — Observer

The system will add the participants of the checklist item to the task in the same roles ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "fields": {
        "TITLE": "4. Prepare Dashboard",
        "PARENT_ID": 23,
        "SORT_INDEX": 200,
        "IS_COMPLETE": "N",
        "IS_IMPORTANT": "Y",
        "MEMBERS": {
          "547": {
            "TYPE": "A"
          }
        }
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.checklist.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "fields": {
        "TITLE": "4. Prepare Dashboard",
        "PARENT_ID": 23,
        "SORT_INDEX": 200,
        "IS_COMPLETE": "N",
        "IS_IMPORTANT": "Y",
        "MEMBERS": {
          "547": {
            "TYPE": "A"
          }
        }
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.checklist.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.checklist.add',
            {
                templateId: 139,
                fields: {
                    TITLE: '4. Prepare Dashboard',
                    PARENT_ID: 23,
                    SORT_INDEX: 200,
                    IS_COMPLETE: 'N',
                    IS_IMPORTANT: 'Y',
                    MEMBERS: {
                        547: {
                            TYPE: 'A'
                        }
                    }
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
                'tasks.template.checklist.add',
                [
                    'templateId' => 139,
                    'fields' => [
                        'TITLE' => '4. Prepare Dashboard',
                        'PARENT_ID' => 23,
                        'SORT_INDEX' => 200,
                        'IS_COMPLETE' => 'N',
                        'IS_IMPORTANT' => 'Y',
                        'MEMBERS' => [
                            547 => [
                                'TYPE' => 'A'
                            ]
                        ]
                    ]
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
        'tasks.template.checklist.add',
        {
            templateId: 139,
            fields: {
                TITLE: '4. Prepare Dashboard',
                PARENT_ID: 23,
                SORT_INDEX: 200,
                IS_COMPLETE: 'N',
                IS_IMPORTANT: 'Y',
                MEMBERS: {
                    547: {
                        TYPE: 'A'
                    }
                }
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
        'tasks.template.checklist.add',
        [
            'templateId' => 139,
            'fields' => [
                'TITLE' => '4. Prepare Dashboard',
                'PARENT_ID' => 23,
                'SORT_INDEX' => 200,
                'IS_COMPLETE' => 'N',
                'IS_IMPORTANT' => 'Y',
                'MEMBERS' => [
                    547 => [
                        'TYPE' => 'A'
                    ]
                ],
            ]
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "checkListItem": {
            "id": 37,
            "copiedId": null,
            "userId": 503,
            "createdBy": 503,
            "parentId": 23,
            "title": "4. Prepare Dashboard",
            "sortIndex": 200,
            "displaySortIndex": "",
            "isComplete": false,
            "isImportant": true,
            "completedCount": 0,
            "members": [
                {
                    "type": "A",
                    "id": 547
                }
            ],
            "attachments": [],
            "nodeId": null,
            "templateId": 139
        }
    },
    "time": {
        "start": 1773235651,
        "finish": 1773235651.960878,
        "duration": 0.9608778953552246,
        "processing": 0,
        "date_start": "2026-03-11T16:27:31+01:00",
        "date_finish": "2026-03-11T16:27:31+01:00",
        "operating_reset_at": 1773236251,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with response data [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **checkListItem**
[`object`](../../../data-types.md) | Created checklist item [(detailed description)](#checklistitem) ||
|#

{% include [Decoding the checkListItem object](./_includes/checklist-item-response.md) %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Invalid value [] for field [PARENT_ID] in item [, New item]"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` not provided ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `400` | `0` | Item name not specified | Required parameter `fields.TITLE` not provided or invalid value passed ||
|| `400` | `0` | Invalid value [] for field [PARENT_ID] in item [, Item name] | Required parameter `fields.PARENT_ID` not provided or invalid value passed ||
|| `400` | `0` | Adding item: action not available | User does not have permission to add checklist item in the task template ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-checklist-update.md)
- [{#T}](./tasks-template-checklist-get.md)
- [{#T}](./tasks-template-checklist-list.md)
- [{#T}](./tasks-template-checklist-delete.md)
- [{#T}](./tasks-template-checklist-complete.md)