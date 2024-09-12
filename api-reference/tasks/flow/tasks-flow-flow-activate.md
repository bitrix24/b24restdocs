# Activate/Deactivate Flow tasks.flow.flow.activate

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: Creator or Administrator of the flow

The method `tasks.flow.flow.activate` turns a flow on or off by its identifier. If the flow is off, it turns it on. If it is on, it turns it off.

## Method Parameters

#|
|| **Name** `type` | **Description** ||
|| **flowId^*^** [`integer`](../../data-types.md) | The identifier of the flow to be activated or deactivated. You can obtain the flowId using the [tasks.task.get](../tasks-task-get.md) method for a task that has already been added to the flow, or create a new flow using the [tasks.flow.flow.create](./tasks-flow-flow-create.md) method ||
|#

## Code Examples

{% list tabs %}

- JS
    ```js
    BX24.callMethod(
        'tasks.flow.flow.activate',
        {
            flowId: 517
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- cURL (oAuth)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
    -d '{
        "flowId": 517
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.flow.activate
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "flowId": 517
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.flow.activate
    ```

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $flowId = 517;

    // executing the request to the REST API
    $result = CRest::call(
        'tasks.flow.flow.activate',
        [
            'flowId' => $flowId
        ]
    );

    // Handling the response from Bitrix24
    if ($result['error']) {
        echo 'Error: '.$result['error_description'];
    } else {
        print_r($result['result']);
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
"result": true
}
```

### Returned Data

#|
|| **Name** `type` | **Description** ||
|| **result** [`boolean`](../../data-types.md) | Success of the operation ||
|#

## Error Handling

HTTP Status: **200**

```json
{
    "error": "0",
    "error_description": "Flow not found"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Additional Information** ||
|| `0` | Access denied or flow not found | The account plan may not allow working with flows, or the user does not have permission to perform the operation ||
|| `0` | Flow not found | The flow with the specified identifier was not found ||
|| `0` | Unknown error | An unknown error occurred ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}