# Delete the correspondence with a natural or legal person sale.businessValuePersonDomain.deleteByFilter

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sale.businessValuePersonDomain.deleteByFilter` removes the correspondence with a natural or legal person.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for deleting the correspondence with a natural or legal person ||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **personTypeId***
[`sale_person_type.id`](../data-types.md) | ID of the payer type. Unique parameter.

You can obtain the identifiers of payer types using the method [sale.persontype.list](../person-type/sale-person-type-list.md) ||
|| **domain***
[`string`](../../data-types.md) | Value corresponding to the payer type: natural person or legal entity.
- `I` — natural person
- `E` — legal entity ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"personTypeId":3,"domain":"I"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.businessValuePersonDomain.deleteByFilter
    ```

- cURL (OAuth)

    ```curl
    -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"personTypeId":3,"domain":"I"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.businessValuePersonDomain.deleteByFilter
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.businessValuePersonDomain.deleteByFilter', 
    		{
    			fields: {
    				personTypeId: 3,
    				domain: "I"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch(error)
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
                'sale.businessValuePersonDomain.deleteByFilter',
                [
                    'fields' => [
                        'personTypeId' => 3,
                        'domain'      => "I",
                    ],
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
        echo 'Error deleting business value person domain: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.businessValuePersonDomain.deleteByFilter', 
        {
            fields: {
                personTypeId: 3,
                domain: "I"
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.businessValuePersonDomain.deleteByFilter',
        [
            'fields' =>
            [
                'personTypeId' => 3,
                'domain' => 'I'
            ]
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
    "result":true,
    "time":{
        "start":1712571751.487018,
        "finish":1712571752.329445,
        "duration":0.8424267768859863,
        "processing":0.6462001800537109,
        "date_start":"2024-04-08T13:22:31+03:00",
        "date_finish":"2024-04-08T13:22:32+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of deleting the correspondence with a natural or legal person ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 201440400003,
    "error_description": "business value person domain does not exist"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201450000002` | Parameter `personTypeId` not provided ||
|| `201450000003` | Parameter `domain` not provided ||
|| `201440400003` | The specified `domain` does not exist for the given `personTypeId` ||
|| `200040300020` | Access error for deletion ||
|| `100` | Parameter `fields` not specified or empty ||
|| `0` | Required parameter not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-business-value-person-domain-add.md)
- [{#T}](./sale-business-value-person-domain-list.md)
- [{#T}](./sale-business-value-person-domain-get-fields.md)