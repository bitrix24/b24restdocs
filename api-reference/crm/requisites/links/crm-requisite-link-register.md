# Register Requisite Link with Object crm.requisite.link.register

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method registers a link between requisites and an object.

For successful registration, the requisite IDs must belong to the client and seller selected in the linked object. If a requisite is not available, its ID is passed as `0`. You can even specify all requisite IDs as zero. In this case, it is considered that the requisites are not linked to the object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields for the requisite link — an object of the form `{"field": "value"[, ...]}` ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the object type to which the link belongs.

The following types can be used:
- deal (value `2`)
- old invoice (value `5`)
- estimate (value `7`)
- new invoice (value `31`)
- other dynamic objects (for possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md)).

You can obtain CRM object type identifiers using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier of the object to which the link belongs. 

Object identifiers can be obtained using the following methods: [crm.deal.list](../../deals/crm-deal-list.md), [crm.quote.list](../../quote/crm-quote-list.md), [crm.item.list](../../universal/crm-item-list.md) ||
|| **REQUISITE_ID***
[`integer`](../../../data-types.md) | Identifier of the client's requisite selected for the object. 

Requisite identifiers can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **BANK_DETAIL_ID***
[`integer`](../../../data-types.md) | Identifier of the client's bank requisite selected for the object.

Bank requisite identifiers can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|| **MC_REQUISITE_ID***
[`integer`](../../../data-types.md) | Identifier of my company's requisite selected for the object. 

Requisite identifiers can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **MC_BANK_DETAIL_ID***
[`integer`](../../../data-types.md) | Identifier of my company's bank requisite selected for the object. 

Bank requisite identifiers can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_TYPE_ID":31,"ENTITY_ID":315,"REQUISITE_ID":60,"BANK_DETAIL_ID":24,"MC_REQUISITE_ID":2,"MC_BANK_DETAIL_ID":2}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.link.register
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_TYPE_ID":31,"ENTITY_ID":315,"REQUISITE_ID":60,"BANK_DETAIL_ID":24,"MC_REQUISITE_ID":2,"MC_BANK_DETAIL_ID":2},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.link.register
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.requisite.link.register", {
    			fields: {
    				ENTITY_TYPE_ID: 31,
    				ENTITY_ID: 315,
    				REQUISITE_ID: 60,
    				BANK_DETAIL_ID: 24,
    				MC_REQUISITE_ID: 2,
    				MC_BANK_DETAIL_ID: 2
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
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
                'crm.requisite.link.register',
                [
                    'fields' => [
                        'ENTITY_TYPE_ID'  => 31,
                        'ENTITY_ID'       => 315,
                        'REQUISITE_ID'    => 60,
                        'BANK_DETAIL_ID'  => 24,
                        'MC_REQUISITE_ID' => 2,
                        'MC_BANK_DETAIL_ID' => 2,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error registering requisite link: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.link.register", {
            fields: {
                ENTITY_TYPE_ID: 31,
                ENTITY_ID: 315,
                REQUISITE_ID: 60,       // Identifier of the requisite belonging to the buyer
                BANK_DETAIL_ID: 24,     // Identifier of the bank requisite belonging to the buyer
                MC_REQUISITE_ID: 2,     // Identifier of the requisite belonging to the selling company
                MC_BANK_DETAIL_ID: 2    // Identifier of the bank requisite belonging to the selling company
            }
        },
        function (result)
        {
            if (result.error())
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
        'crm.requisite.link.register',
        [
            'fields' =>
            [
                'ENTITY_TYPE_ID' => 31,
                'ENTITY_ID' => 315,
                'REQUISITE_ID' => 60,
                'BANK_DETAIL_ID' => 24,
                'MC_REQUISITE_ID' => 2,
                'MC_BANK_DETAIL_ID' => 2
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
    "result": true,
    "time": {
        "start": 1718729280.575321,
        "finish": 1718729281.296214,
        "duration": 0.720893144607544,
        "processing": 0.1967611312866211,
        "date_start": "2024-06-18T18:48:00+02:00",
        "date_finish": "2024-06-18T18:48:01+02:00",
        "operating": 0.1967298984527588
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the link registration:
- `true` — link registered
- `false` — link not registered
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "Entity is not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `ENTITY_TYPE_ID is not defined or invalid` | Object type identifier is not set or has an invalid value ||
|| `ENTITY_ID is not defined or invalid` | Object identifier is not set or has an invalid value ||
|| `REQUISITE_ID is not defined or invalid` | Client requisite identifier is not set or has an invalid value ||
|| `BANK_DETAIL_ID is not defined or invalid` | Client bank requisite identifier is not set or has an invalid value ||
|| `MC_REQUISITE_ID is not defined or invalid` | My company requisite identifier is not set or has an invalid value ||
|| `MC_BANK_DETAIL_ID is not defined or invalid` | My company bank requisite identifier is not set or has an invalid value ||
|| `The Requisite with ID '60' is not found` | Client requisite with the specified identifier not found ||
|| `The Requisite with ID '60' is not assigned to Company with ID '5'` | Client requisite with the specified identifier does not belong to the company specified in the object ||
|| `The BankDetail with ID '24' is not found` | Client bank requisite with the specified identifier not found ||
|| `The BankDetail with ID '24' is not assigned to Requisite with ID '60'` | Client bank requisite with the specified identifier does not belong to the specified requisite ||
|| `The Requisite of your company with ID '2' is not found` | My company requisite with the specified identifier not found ||
|| `The Requisite with ID '2' is not assigned to your company with ID '3010'` | My company requisite with the specified identifier does not belong to my company specified in the object ||
|| `The BankDetail of your company with ID '2' is not found` | My company bank requisite with the specified identifier not found ||
|| `The BankDetail of your company with ID '2' is not assigned to Requisite of your company with ID '2'` | My company bank requisite with the specified identifier does not belong to the specified requisite ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-link-get.md)
- [{#T}](./crm-requisite-link-list.md)
- [{#T}](./crm-requisite-link-unregister.md)
- [{#T}](./crm-requisite-link-fields.md)