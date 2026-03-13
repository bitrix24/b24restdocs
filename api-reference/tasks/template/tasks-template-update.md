# Update Task Template tasks.template.update

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: user with template editing access permission

The method `tasks.template.update` updates an existing task template.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **templateId***
[`integer`](../../data-types.md) | Identifier of the task template.

The task template identifier can be obtained when [creating a new template](./tasks-template-add.md) ||
|| **fields***
[`object`](../../data-types.md) | Fields of the template that need to be modified.

The fields and value types correspond to the description on the [Task Template Fields](./fields.md) page ||
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
      "templateId": 139,
      "fields": {
        "TITLE": "Preparation of Weekly Project Status and Approval",
        "DESCRIPTION": "Updated task template for preparing the weekly project status and final approval before submission",
        "PRIORITY": 1,
        "TASK_CONTROL": "Y",
        "ADD_IN_REPORT": "Y",
        "UF_CRM_TASK": ["L_1179", "D_1833"]
      }
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/tasks.template.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
      "templateId": 139,
      "fields": {
        "TITLE": "Preparation of Weekly Project Status and Approval",
        "DESCRIPTION": "Updated task template for preparing the weekly project status and final approval before submission",
        "PRIORITY": 1,
        "TASK_CONTROL": "Y",
        "ADD_IN_REPORT": "Y",
        "UF_CRM_TASK": ["L_1179", "D_1833"]
      },
      "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/tasks.template.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'tasks.template.update',
            {
                templateId: 139,
                fields: {
                    TITLE: 'Preparation of Weekly Project Status and Approval',
                    DESCRIPTION: 'Updated task template for preparing the weekly project status and final approval before submission',
                    PRIORITY: 1,
                    TASK_CONTROL: 'Y',
                    ADD_IN_REPORT: 'Y',
                    UF_CRM_TASK: ['L_1179', 'D_1833']
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
                'tasks.template.update',
                [
                    'templateId' => 139,
                    'fields' => [
                        'TITLE' => 'Preparation of Weekly Project Status and Approval',
                        'DESCRIPTION' => 'Updated task template for preparing the weekly project status and final approval before submission',
                        'PRIORITY' => 1,
                        'TASK_CONTROL' => 'Y',
                        'ADD_IN_REPORT' => 'Y',
                        'UF_CRM_TASK' => ['L_1179', 'D_1833']
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);

    } catch (Throwable $e) {
        echo 'Error updating template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'tasks.template.update',
        {
            templateId: 139,
            fields: {
                TITLE: 'Preparation of Weekly Project Status and Approval',
                DESCRIPTION: 'Updated task template for preparing the weekly project status and final approval before submission',
                PRIORITY: 1,
                TASK_CONTROL: 'Y',
                ADD_IN_REPORT: 'Y',
                UF_CRM_TASK: ['L_1179', 'D_1833']
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
        'tasks.template.update',
        [
            'templateId' => 139,
            'fields' => [
                'TITLE' => 'Preparation of Weekly Project Status and Approval',
                'DESCRIPTION' => 'Updated task template for preparing the weekly project status and final approval before submission',
                'PRIORITY' => 1,
                'TASK_CONTROL' => 'Y',
                'ADD_IN_REPORT' => 'Y',
                'UF_CRM_TASK' => ['L_1179', 'D_1833']
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
    "result": true,
    "time": {
        "start": 1773221471,
        "finish": 1773221471.833299,
        "duration": 0.833298921585083,
        "processing": 0,
        "date_start": "2026-03-11T12:31:11+01:00",
        "date_finish": "2026-03-11T12:31:11+01:00",
        "operating_reset_at": 1773222071,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the template was successfully updated.

Returns `false`
- if the template with the specified `templateId` does not exist
- if the user does not have access permission to edit the template ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {templateId}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {templateId} | Required parameter `templateId` not provided ||
|| `400` | `100` | Invalid value {} to match with parameter {templateId}. Should be value of type int. | Parameter `templateId` was provided incorrectly or is empty ||
|| `400` | `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./tasks-template-add.md)
- [{#T}](./tasks-template-get.md)
- [{#T}](./tasks-template-delete.md)
- [{#T}](./tasks-template-fields.md)