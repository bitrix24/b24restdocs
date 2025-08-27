# Get a list of fields for duplicate search crm.duplicate.volatileType.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.duplicate.volatileType.fields` returns a list of standard and custom fields that can be used for finding duplicates in leads, contacts, and companies.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Identifier of the object type. Possible values:
- `1` — [lead](../../leads/index.md)
- `3` — [contact](../../contacts/index.md)
- `4` — [company](../../companies/index.md)

If not specified, fields for all types will be returned ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"entityTypeId":1}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.duplicate.volatileType.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.duplicate.volatileType.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.duplicate.volatileType.fields",
    		{
    			entityTypeId: 1
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
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
                'crm.duplicate.volatileType.fields',
                [
                    'entityTypeId' => 1,
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
        echo 'Error calling crm.duplicate.volatileType.fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.duplicate.volatileType.fields",
        {
            entityTypeId: 1
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
        'crm.duplicate.volatileType.fields',
        [
            'entityTypeId' => 1
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
    "result": [
        {
            "entityTypeId": 1,
            "fieldCode": "TITLE",
            "fieldTitle": "Lead Name"
        },
        {
            "entityTypeId": 1,
            "fieldCode": "ADDRESS",
            "fieldTitle": "Address"
        },
        {
            "entityTypeId": 1,
            "fieldCode": "UF_CRM_1750854801",
            "fieldTitle": "String"
        },
        // ... other fields ...
    ],
    "time": {
        "start": 1750854837.219779,
        "finish": 1750854837.296077,
        "duration": 0.07629799842834473,
        "processing": 0.028430938720703125,
        "date_start": "2025-06-25T12:33:57+00:00",
        "date_finish": "2025-06-25T12:33:57+00:00" 
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ||
|| **fieldCode**
[`string`](../../../data-types.md) | Field code ||
|| **fieldTitle**
[`string`](../../../data-types.md) | Field name ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md)
- [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md)
- [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md)