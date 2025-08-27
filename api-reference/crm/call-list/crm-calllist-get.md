# Get Information About the Call List crm.calllist.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with read access permission for CRM entities

The method `crm.calllist.get` returns information about the call list by its identifier, without the list of participants.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the call list ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"ID":123}' \
         https://**your_bitrix24**/rest/**user_id**/**webhook**/crm.calllist.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d '{"ID":123,"auth":"**put_access_token_here**"}' \
         https://**your_bitrix24**/rest/crm.calllist.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.calllist.get',
    		{ ID: 123 }
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
                'crm.calllist.get',
                [
                    'ID' => 123,
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting call list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.calllist.get",
        { ID: 123 },
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
        'crm.calllist.get',
        [ 'ID' => 123 ]
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
    "result": {
        "ID": "13",
        "DATE_CREATE": "2025-07-14 08:47:19",
        "CREATED_BY_ID": "29",
        "WEBFORM_ID": "1",
        "ENTITY_TYPE_ID": "3",
        "ENTITY_TYPE": "CONTACT"
    },
    "time": {
        "start": 1752472572.278248,
        "finish": 1752472572.332555,
        "duration": 0.054306983947753906,
        "processing": 0.004546165466308594,
        "date_start": "2025-07-14T08:56:12+02:00",
        "date_finish": "2025-07-14T08:56:12+02:00",
        "operating_reset_at": 1752473172,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | Identifier of the call list ||
|| **DATE_CREATE**
[`datetime`](../../data-types.md) | Date and time of call list creation ||
|| **CREATED_BY_ID**
[`integer`](../../data-types.md) | Identifier of the user who created the call list ||
|| **WEBFORM_ID**
[`integer`](../../data-types.md) | Identifier of the associated CRM form ||
|| **ENTITY_TYPE_ID**
[`integer`](../../data-types.md) | Identifier of the object type ||
|| **ENTITY_TYPE**
[`string`](../../data-types.md) | Type of the object ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "Incorrect list id",
    "error_description": "An incorrect list identifier was provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `Incorrect list id` | Incorrect list identifier or the user does not have read access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-calllist-add.md)
- [{#T}](./crm-calllist-items-get.md)
- [{#T}](./crm-calllist-list.md)
- [{#T}](./crm-calllist-statuslist.md)
- [{#T}](./crm-calllist-update.md)