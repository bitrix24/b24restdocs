# Get a directory item by ID crm.status.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.status.get` returns the parameters of a directory item by its ID.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md) | Identifier of the directory item. You can get a list of items with identifiers using the method [crm.status.list](./crm-status-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"id":123}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.status.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":123,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.status.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.status.get',
    		{
    			id: 123
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch(error)
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
                'crm.status.get',
                [
                    'id' => 123,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting CRM status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.status.get",
        {
            id: 123
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.status.get',
        [
            'id' => 123
        ]
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
        "ID": "733",
        "ENTITY_ID": "DYNAMIC_1038_STAGE_37",
        "STATUS_ID": "DT1038_37:SUCCESS",
        "NAME": "Success",
        "NAME_INIT": "Success",
        "SORT": "40",
        "SYSTEM": "Y",
        "CATEGORY_ID": "37",
        "COLOR": "#00ff00",
        "SEMANTICS": "S"
    },
    "time": {
        "start": 1752133970.651926,
        "finish": 1752133970.690207,
        "duration": 0.03828096389770508,
        "processing": 0.0060749053955078125,
        "date_start": "2025-07-10T10:52:50+02:00",
        "date_finish": "2025-07-10T10:52:50+02:00",
        "operating_reset_at": 1752134570,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the directory item ||
|| **ENTITY_ID**
[`string`](../../data-types.md) | Identifier of the object to which the directory relates ||
|| **STATUS_ID**
[`string`](../../data-types.md) | Status value code ||
|| **NAME**
[`string`](../../data-types.md) | Name ||
|| **NAME_INIT**
[`string`](../../data-types.md) | Initial name ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting ||
|| **SYSTEM**
[`string`](../../data-types.md) | Indicator of system value ||
|| **CATEGORY_ID**
[`integer`](../../data-types.md) | Identifier of the funnel to which the status relates ||
|| **COLOR**
[`string`](../../data-types.md) | Status color for kanban ||
|| **SEMANTICS**
[`string`](../../data-types.md) | Group of stages ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Status is not found.",
    "error_description": "Directory item not found."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `Access denied.` | No permission to perform the operation ||
|| `400`     | `Status is not found.` | Directory item not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-status-fields.md)
- [{#T}](./crm-status-list.md)
- [{#T}](./crm-status-add.md)
- [{#T}](./crm-status-update.md)
- [{#T}](./crm-status-delete.md)