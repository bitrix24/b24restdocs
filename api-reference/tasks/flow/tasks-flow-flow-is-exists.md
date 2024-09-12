# Check for the existence of the flow tasks.flow.flow.isExists

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.flow.flow.isExists` checks whether a flow with the specified name exists. If an `id` is provided, it checks for flows with the same name, excluding the specified one.

## Method Parameters

#|
|| **Name** `type` | **Description** ||
|| **flowData^*^** [`object`](../../data-types.md) | Object containing data to check for the existence of the flow ||
|| **name^*^** [`string`](../../data-types.md) | The name of the flow to check ||
|| **id** [`integer`](../../data-types.md) | The identifier of the flow to exclude from the check (optional). You can obtain the id using the [tasks.task.get](../tasks-task-get.md) method for a task already added to the flow, or create a new flow using the [tasks.flow.flow.create](./tasks-flow-flow-create.md) method ||
|#

## Code Examples

{% list tabs %}

- JS
    ```js
    BX24.callMethod(
        'tasks.flow.flow.isExists',
        {
            flowData: {
                name: 'Flow Name'
            }
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
        "flowData": {
            "name": "Flow Name"
        }
    }' \
    https://your-domain.bitrix24.com/rest/tasks.flow.flow.isExists
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
        "flowData": {
            "name": "Flow Name"
        }
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.flow.flow.isExists
    ```

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    $flowData = [
        "name" => "Flow Name"
    ];

    // executing the request to the REST API
    $result = CRest::call(
        'tasks.flow.flow.isExists',
        [
            'flowData' => $flowData
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

HTTP status: **200**

```json
{
"result": {
    "exists": true
}
}
```

### Returned Data

#|
|| **Name** `type` | **Description** ||
|| **result** [`object`](../../data-types.md) | Object containing the result of the operation ||
|| **exists** [`boolean`](../../data-types.md) | Indicates whether a flow with the specified name exists ||
|#

## Error Handling

HTTP status: **200**

```json
{
    "error": "0",
    "error_description": "Flow not found"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** | **Additional Information** ||
|| `0` | Access denied or flow not found | The account's plan may not allow working with flows, or the user may not have permission to perform the check ||
|| `0` | Unknown error | An unknown error occurred ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}