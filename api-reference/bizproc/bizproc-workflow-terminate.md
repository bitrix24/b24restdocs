# Stop Active Workflow bizproc.workflow.terminate

> Scope: [`bizproc`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method stops the specified workflow. All data related to the workflow will be preserved.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`string`](../data-types.md) | Identifier of the workflow to be stopped.

The identifier can be obtained using the [bizproc.workflow.instances](./bizproc-workflow-instances.md) method. ||
|| **STATUS**
[`string`](../data-types.md) | Set the status text. ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":"65e5a449e8f135.21284909","STATUS":"Terminated by rest app."}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/bizproc.workflow.terminate
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":"65e5a449e8f135.21284909","STATUS":"Terminated by rest app.","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/bizproc.workflow.terminate
    ```

- JS

    ```js
    BX24.callMethod(
        'bizproc.workflow.terminate',
        {
            ID: '65e5a449e8f135.21284909',
            STATUS: 'Terminated by rest app.',
        },
        function(result) {
            console.log('response', result.answer);
            if(result.error())
                alert("Error: " + result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'bizproc.workflow.terminate',
        [
            'ID' => '65e5a449e8f135.21284909',
            'STATUS' => 'Terminated by rest app.'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- PHP (B24PhpSdk)
  
    ```php       
    try {
        $workflowId = 'your_workflow_id'; // Replace with actual workflow ID
        $message = 'Workflow terminated'; // Replace with actual message
        $result = $serviceBuilder
            ->getBizProcScope()
            ->workflow()
            ->terminate($workflowId, $message);
        if ($result->isSuccess()) {
            print($result->getCoreResponse()->getResponseData()->getResult()[0]);
        } else {
            print('Termination failed.');
        }
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1726476060.581428,
        "finish": 1726476060.813776,
        "duration": 0.23234796524047852,
        "processing": 0.002630949020385742,
        "date_start": "2024-09-16T08:41:00+00:00",
        "date_finish": "2024-09-16T08:41:00+00:00",
        "operating_reset_at": 1726476660,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Root element of the response.

Contains `true` in case of success. ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time. ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied!"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** |**Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! | Method was not executed by an administrator. ||
|| `400` | `ERROR_WRONG_WORKFLOW_ID` | Empty workflow instance ID | An empty value was passed to the `ID` parameter. ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./bizproc-workflow-start.md)
- [{#T}](./bizproc-workflow-instances.md)
- [{#T}](./bizproc-workflow-kill.md)
- [{#T}](../../tutorials/bizproc/how-to-kill-workflows.md)
- [{#T}](../../tutorials/bizproc/how-to-filter-and-kill-workflows.md)