# Create a new bank detail crm.requisite.bankdetail.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method creates a new bank detail.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields — an object of the form `{"field": "value"[, ...]}` for adding a bank detail ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID***
[`integer`](../../../data-types.md) | Identifier of the parent object. Currently, it can only be the identifier of the detail. Identifiers of details can be obtained using the method [`crm.requisite.list`](../universal/crm-requisite-list.md) ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of bank detail fields (see the method [crm.requisite.preset.countries](../presets/crm-requisite-preset-countries.md) for available values).

The country code of the bank detail matches the country code in the linked detail template, the identifier of which is specified in the `ENTITY_ID` field ||
|| **NAME***
[`string`](../../../data-types.md) | Name of the bank detail ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the detail ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the external information base object. 

The purpose of the field may change by the final developer. Each application ensures the uniqueness of values in this field. 

It is recommended to use a unique prefix to avoid collisions with other applications ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Values `Y` or `N` are used. 

Currently, the field does not actually affect anything ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **RQ_BANK_NAME**
[`string`](../../../data-types.md) | Bank name ||
|| **RQ_BANK_ADDR**
[`string`](../../../data-types.md) | Bank address ||
|| **RQ_BANK_CODE**
[`string`](../../../data-types.md) | Bank code (for country BR) ||
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
[`string`](../../../data-types.md) | Account currency ||
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

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":27,"COUNTRY_ID":1,"NAME":"Superbank","RQ_BANK_NAME":"Ltd. Superbank","RQ_BANK_ADDR":"117312, New York, 19 Vavilova St.","RQ_BIK":"044525225","RQ_ACC_NUM":"40702810938000060473","RQ_ACC_CURRENCY":"USD","RQ_COR_ACC_NUM":"30101810400000000225","XML_ID":"1e4641fd-2dd9-31e6-b2f2-105056c00008","ACTIVE":"Y","SORT":600}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.bankdetail.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ENTITY_ID":27,"COUNTRY_ID":1,"NAME":"Superbank","RQ_BANK_NAME":"Ltd. Superbank","RQ_BANK_ADDR":"117312, New York, 19 Vavilova St.","RQ_BIK":"044525225","RQ_ACC_NUM":"40702810938000060473","RQ_ACC_CURRENCY":"USD","RQ_COR_ACC_NUM":"30101810400000000225","XML_ID":"1e4641fd-2dd9-31e6-b2f2-105056c00008","ACTIVE":"Y","SORT":600},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.bankdetail.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.requisite.bankdetail.add",
    		{
    			fields:
    			{
    				"ENTITY_ID": 27,                 // Identifier of the detail
    				"COUNTRY_ID": 1,                 // Country code (USA)
    				"NAME": "Superbank",              // Name of the bank detail
    				"RQ_BANK_NAME": "Ltd. Superbank",  // Bank name
    				"RQ_BANK_ADDR": "117312, New York, 19 Vavilova St.",
    				"RQ_BIK": "044525225",
    				"RQ_ACC_NUM": "40702810938000060473",
    				"RQ_ACC_CURRENCY": "USD",
    				"RQ_COR_ACC_NUM": "30101810400000000225",
    				"XML_ID":"1e4641fd-2dd9-31e6-b2f2-105056c00008",
    				"ACTIVE":"Y",
    				"SORT":600
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.info("Created bank detail with ID " + result);
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
                'crm.requisite.bankdetail.add',
                [
                    'fields' => [
                        'ENTITY_ID'       => 27,
                        'COUNTRY_ID'      => 1,
                        'NAME'            => 'Superbank',
                        'RQ_BANK_NAME'    => 'Ltd. Superbank',
                        'RQ_BANK_ADDR'    => '117312, New York, 19 Vavilova St.',
                        'RQ_BIK'          => '044525225',
                        'RQ_ACC_NUM'      => '40702810938000060473',
                        'RQ_ACC_CURRENCY' => 'USD',
                        'RQ_COR_ACC_NUM'  => '30101810400000000225',
                        'XML_ID'          => '1e4641fd-2dd9-31e6-b2f2-105056c00008',
                        'ACTIVE'          => 'Y',
                        'SORT'            => 600,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Created bank detail with ID ' . $result;
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error creating bank detail: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.bankdetail.add",
        {
            fields:
            {
                "ENTITY_ID": 27,                 // Identifier of the detail
                "COUNTRY_ID": 1,                 // Country code (USA)
                "NAME": "Superbank",              // Name of the bank detail
                "RQ_BANK_NAME": "Ltd. Superbank",  // Bank name
                "RQ_BANK_ADDR": "117312, New York, 19 Vavilova St.",
                "RQ_BIK": "044525225",
                "RQ_ACC_NUM": "40702810938000060473",
                "RQ_ACC_CURRENCY": "USD",
                "RQ_COR_ACC_NUM": "30101810400000000225",
                "XML_ID":"1e4641fd-2dd9-31e6-b2f2-105056c00008",
                "ACTIVE":"Y",
                "SORT":600
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Created bank detail with ID " + result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.bankdetail.add',
        [
            'fields' => [
                'ENTITY_ID' => 27,
                'COUNTRY_ID' => 1,
                'NAME' => 'Superbank',
                'RQ_BANK_NAME' => 'Ltd. Superbank',
                'RQ_BANK_ADDR' => '117312, New York, 19 Vavilova St.',
                'RQ_BIK' => '044525225',
                'RQ_ACC_NUM' => '40702810938000060473',
                'RQ_ACC_CURRENCY' => 'USD',
                'RQ_COR_ACC_NUM' => '30101810400000000225',
                'XML_ID' => '1e4641fd-2dd9-31e6-b2f2-105056c00008',
                'ACTIVE' => 'Y',
                'SORT' => 600
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
    "result": 357,
    "time": {
        "start": 1717429942.060649,
        "finish": 1717429942.626925,
        "duration": 0.5662760734558105,
        "processing": 0.09111285209655762,
        "date_start": "2024-06-03T17:52:22+02:00",
        "date_finish": "2024-06-03T17:52:22+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created bank detail ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "ENTITY_ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Error Text** | **Description** ||
|| `ENTITY_ID is not defined or invalid` | The identifier of the detail is not defined or has an invalid value ||
|| `Access denied` | Insufficient access permissions to add a bank detail ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-bank-detail-update.md)
- [{#T}](./crm-requisite-bank-detail-get.md)
- [{#T}](./crm-requisite-bank-detail-list.md)
- [{#T}](./crm-requisite-bank-detail-delete.md)
- [{#T}](./crm-requisite-bank-detail-fields.md)