# Get bank details by id crm.requisite.bankdetail.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns bank details by identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the bank details. 

Identifiers of bank details can be obtained using the method [`crm.requisite.bankdetail.list`](./crm-requisite-bank-detail-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":357}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.bankdetail.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":357,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.bankdetail.get
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.requisite.bankdetail.get',
    		{ id: 357 }
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
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
                'crm.requisite.bankdetail.get',
                [
                    'id' => 357,
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
        echo 'Error getting bank detail: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.bankdetail.get",
        { id: 357 },
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
        'crm.requisite.bankdetail.get',
        ['id' => 357]
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
        "ID": "357",
        "ENTITY_ID": "27",
        "COUNTRY_ID": "1",
        "DATE_CREATE": "2024-06-03T17:52:22+02:00",
        "DATE_MODIFY": "",
        "CREATED_BY_ID": "1",
        "MODIFY_BY_ID": null,
        "NAME": "Ltd. Superbank",
        "CODE": null,
        "XML_ID": "1e4641fd-2dd9-31e6-b2f2-105056c00008",
        "ORIGINATOR_ID": null,
        "ACTIVE": "Y",
        "SORT": "600",
        "RQ_BANK_NAME": "Ltd. Superbank",
        "RQ_BANK_CODE": null,
        "RQ_BANK_ADDR": "117312, New York, Vavilova Street, House 19",
        "RQ_BANK_ROUTE_NUM": null,
        "RQ_BIK": "044525225",
        "RQ_MFO": null,
        "RQ_ACC_NAME": null,
        "RQ_ACC_NUM": "40702810938000060473",
        "RQ_ACC_TYPE": null,
        "RQ_IIK": null,
        "RQ_ACC_CURRENCY": "USD",
        "RQ_COR_ACC_NUM": "30101810400000000225",
        "RQ_IBAN": null,
        "RQ_SWIFT": null,
        "RQ_BIC": null,
        "RQ_CODEB": null,
        "RQ_CODEG": null,
        "RQ_RIB": null,
        "RQ_AGENCY_NAME": null,
        "COMMENTS": null
    },
    "time": {
        "start": 1717495619.077607,
        "finish": 1717495619.708617,
        "duration": 0.6310100555419922,
        "processing": 0.07691788673400879,
        "date_start": "2024-06-04T12:06:59+02:00",
        "date_finish": "2024-06-04T12:06:59+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object containing the values of the bank details fields ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

### Description of Bank Details Fields

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the bank details. Automatically created and unique within the account ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object's type. Can only be `Requisite` (value `8`).

Identifiers of object types are returned by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object. Currently can only be the identifier of the requisite. 

Identifiers of requisites can be obtained using the method [`crm.requisite.list`](../universal/crm-requisite-list.md) ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of bank details fields (see method [crm.requisite.preset.countries](../presets/crm-requisite-preset-countries.md) for available values).

The country code of the bank details matches the country code in the linked requisite template, the identifier of which is specified in the `ENTITY_ID` field 
||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite ||
|| **NAME^*^**
[`string`](../../../data-types.md) | Name of the bank details ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer. Each application ensures the uniqueness of values in this field. 

It is recommended to use a unique prefix to avoid collisions with other applications 
||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity indicator. Uses values `Y` or `N`. 

Currently, the field does not actually affect anything ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **RQ_BANK_NAME**
[`string`](../../../data-types.md) | Name of the bank ||
|| **RQ_BANK_ADDR**
[`string`](../../../data-types.md) | Address of the bank ||
|| **RQ_BANK_CODE**
[`string`](../../../data-types.md) | Bank Code (for country BR) ||
|| **RQ_BANK_ROUTE_NUM**
[`string`](../../../data-types.md) | Bank Routing Number ||
|| **RQ_BIK**
[`string`](../../../data-types.md) | BIK ||
|| **RQ_CODEB**
[`string`](../../../data-types.md) | Code Banque (for country FR) ||
|| **RQ_CODEG**
[`string`](../../../data-types.md) | Code Guichet (for country FR) ||
|| **RQ_RIB**
[`string`](../../../data-types.md) | Clé RIB (for country FR) ||
|| **RQ_MFO**
[`string`](../../../data-types.md) | MFO ||
|| **RQ_ACC_NAME**
[`string`](../../../data-types.md) | Bank Account Holder Name ||
|| **RQ_ACC_NUM**
[`string`](../../../data-types.md) | Bank Account Number ||
|| **RQ_ACC_TYPE**
[`string`](../../../data-types.md) | Tipo da conta (for country BR) ||
|| **RQ_AGENCY_NAME**
[`string`](../../../data-types.md) | Agência (for country BR) ||
|| **RQ_IIK**
[`string`](../../../data-types.md) | IIK ||
|| **RQ_ACC_CURRENCY**
[`string`](../../../data-types.md) | Currency of the account ||
|| **RQ_COR_ACC_NUM**
[`string`](../../../data-types.md) | Correspondent account ||
|| **RQ_IBAN**
[`string`](../../../data-types.md) | IBAN ||
|| **RQ_SWIFT**
[`string`](../../../data-types.md) | SWIFT ||
|| **RQ_BIC**
[`string`](../../../data-types.md) | BIC ||
|| **COMMENTS**
[`string`](../../../data-types.md) | Comment ||
|| **ORIGINATOR_ID**
[`string`](../../../data-types.md) | Identifier of the external information base. The purpose of the field may change by the final developer ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The RequisiteBankDetail with ID '357' is not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Error Text** | **Description** ||
|| `ID is not defined or invalid` | The identifier of the bank details is not defined or has an invalid value ||
|| `The RequisiteBankDetail with ID '357' is not found` | The bank details with the specified identifier were not found ||
|| `Access denied` | Insufficient access permissions to retrieve bank details ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-bank-detail-add.md)
- [{#T}](./crm-requisite-bank-detail-update.md)
- [{#T}](./crm-requisite-bank-detail-list.md)
- [{#T}](./crm-requisite-bank-detail-delete.md)
- [{#T}](./crm-requisite-bank-detail-fields.md)