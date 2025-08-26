# Get the link of the requisite with the object crm.requisite.link.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns the link of the requisites with the object.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId***
[`integer`](../../../data-types.md) | 
Identifier of the object type to which the link belongs.

The following types can be used:
- deal (value `2`)
- old invoice (value `5`)
- estimate (value `7`)
- new invoice (value `31`)
- other dynamic objects (to get possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md)).

Identifiers of CRM object types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **entityId***
[`integer`](../../../data-types.md) | Identifier of the object to which the link belongs. 

Identifiers of objects can be obtained using the following methods: [crm.deal.list](../../deals/crm-deal-list.md), [crm.quote.list](../../quote/crm-quote-list.md), [crm.item.list](../../universal/crm-item-list.md). ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":31,"entityId":315}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.link.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":31,"entityId":315,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.link.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.requisite.link.get", {
    			entityTypeId: 31,        // Identifier of the type (Invoice)
    			entityId: 315,             // Identifier of the invoice
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
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
                'crm.requisite.link.get',
                [
                    'entityTypeId' => 31,
                    'entityId'     => 315,
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
        echo 'Error getting requisite link: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.link.get", {
            entityTypeId: 31,        // Identifier of the type (Invoice)
            entityId: 315,             // Identifier of the invoice
        },
        function(result)
        {
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
        'crm.requisite.link.get',
        [
            'entityTypeId' => 31,
            'entityId' => 315
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
        "ENTITY_TYPE_ID": 31,
        "ENTITY_ID": 315,
        "REQUISITE_ID": "60",
        "BANK_DETAIL_ID": "24",
        "MC_REQUISITE_ID": "2",
        "MC_BANK_DETAIL_ID": "2"
    },
    "time": {
        "start": 1718378061.651857,
        "finish": 1718378062.098295,
        "duration": 0.4464380741119385,
        "processing": 0.07567882537841797,
        "date_start": "2024-06-14T17:14:21+02:00",
        "date_finish": "2024-06-14T17:14:22+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
`Object`| An object containing the values of the requisite link fields ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

### Description of the requisite link fields with the CRM object

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the object type to which the link belongs.

The following types can be used:
- deal (value `2`)
- old invoice (value `5`)
- estimate (value `7`)
- new invoice (value `31`)
- other dynamic objects (to get possible values, see the method [crm.type.list](../../universal/user-defined-object-types/crm-type-list.md)).

Identifiers of CRM object types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the object to which the link belongs. 

Identifiers of objects can be obtained using the following methods: [crm.deal.list](../../deals/crm-deal-list.md), [crm.quote.list](../../quote/crm-quote-list.md), [crm.item.list](../../universal/crm-item-list.md) ||
|| **REQUISITE_ID**
[`integer`](../../../data-types.md) | Identifier of the client's requisite selected for the object. 

Identifiers of requisites can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | Identifier of the client's bank requisite selected for the object.

Identifiers of bank requisites can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|| **MC_REQUISITE_ID**
[`integer`](../../../data-types.md) | Identifier of my company's requisite selected for the object. 

Identifiers of requisites can be obtained using the method [crm.requisite.list](../universal/crm-requisite-list.md) ||
|| **MC_BANK_DETAIL_ID**
[`integer`](../../../data-types.md) | Identifier of my company's bank requisite selected for the object. 

Identifiers of bank requisites can be obtained using the method [crm.requisite.bankdetail.list](../bank-detail/crm-requisite-bank-detail-list.md) ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "Not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `entityTypeId is not defined or invalid` | The object type identifier is not set or has an invalid value ||
|| `entityId is not defined or invalid` | The object identifier is not set or has an invalid value ||
|| `Not found` | The requisite link was not found ||
|| `Access denied` | Insufficient access permissions to retrieve the requisite link ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-link-register.md)
- [{#T}](./crm-requisite-link-list.md)
- [{#T}](./crm-requisite-link-unregister.md)
- [{#T}](./crm-requisite-link-fields.md)