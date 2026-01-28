# Delete Company crm.company.delete

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Delete" access permission for companies

{% note warning "Method development has been halted" %}

The method `crm.company.delete` is still operational, but there is a more relevant alternative [crm.item.delete](../universal/crm-item-delete.md).

{% endnote %}

The method `crm.company.delete` removes a company.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Company identifier ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":50}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":50,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.delete
    ```

- JS

    ```js
    try
    {
    	const id = prompt("Enter ID");
    	const response = await $b24.callMethod(
    		"crm.company.delete",
    		{ id: id }
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.info(result);
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    $id = readline("Enter ID");
    
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.delete',
                [
                    'id' => $id,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    var id = prompt("Enter ID");
    BX24.callMethod(
        "crm.company.delete",
        { id: id },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.delete',
        [
            'ID' => 50
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
    "result": true,
    "time": {
        "start": 1769497083,
        "finish": 1769497084.786988,
        "duration": 1.7869880199432373,
        "processing": 1,
        "date_start": "2026-01-27T09:58:03+01:00",
        "date_finish": "2026-01-27T09:58:04+01:00",
        "operating_reset_at": 1769497683,
        "operating": 1.564450979232788
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, returns `true` on success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | `Access denied` | User does not have permission for "Delete" companies ||
|| `-` | `Company is not found` | Company not found ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-company-add.md)
- [{#T}](./crm-company-update.md)
- [{#T}](./crm-company-get.md)
- [{#T}](./crm-company-list.md)
- [{#T}](./crm-company-fields.md)