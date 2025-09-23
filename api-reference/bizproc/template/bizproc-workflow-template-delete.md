# Delete Business Process Template bizproc.workflow.template.delete

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a business process template.

It allows you to remove templates that were created using the method [bizproc.workflow.template.add](./bizproc-workflow-template-add.md). These templates are tied to the application and can only be deleted in the context of the same [application](../../../settings/app-installation/index.md) in which they were created.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description**||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the business process template ||
|#	

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":525,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.workflow.template.delete
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'bizproc.workflow.template.delete',
            {
                ID: 525
            }
        );
        
        const result = response.getData().result;
        if(result.error())
            alert("Error: " + result.error());
        else
            console.log(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $templateId = 123; // Replace with the actual template ID you want to delete
        $result = $serviceBuilder
            ->getBizProcScope()
            ->template()
            ->delete($templateId);
        if ($result->isSuccess()) {
            print("Template with ID {$templateId} deleted successfully.\n");
        } else {
            print("Failed to delete template with ID {$templateId}.\n");
        }
    } catch (\Throwable $e) {
        print("An error occurred: " . $e->getMessage() . "\n");
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'bizproc.workflow.template.delete',
        {
            ID: 525
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.template.delete',
        [
            'ID' => 525
        ]
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
    "result": null,
    "time": {
        "start": 1737536737.1245451,
        "finish": 1737536737.3437879,
        "duration": 0.21924281120300293,
        "processing": 0.18391799926757812,
        "date_start": "2025-01-22T12:05:37+02:00",
        "date_finish": "2025-01-22T12:05:37+02:00",
        "operating_reset_at": 1737537337,
        "operating": 0.18389892578125
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`null` | Returns `null` if the template is successfully deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_TEMPLATE_NOT_FOUND",
    "error_description": "Workflow template not found."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ACCESS_DENIED` | Application context required | Access token not from the application ||
|| `ACCESS_DENIED` | Access denied! | Method was not executed by an administrator ||
|| `ERROR_TEMPLATE_NOT_FOUND` | Workflow template not found. | Template with the specified `ID` not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-workflow-template-add.md)
- [{#T}](./bizproc-workflow-template-update.md)
- [{#T}](./bizproc-workflow-template-list.md)