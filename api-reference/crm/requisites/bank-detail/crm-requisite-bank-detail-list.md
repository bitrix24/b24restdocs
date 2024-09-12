# Get a List of Bank Details by Filter crm.requisite.bankdetail.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a list of bank details based on the filter.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | The array contains a list of fields to select (see [bank detail fields](#fields)).

If the array is not provided or an empty array is passed, all available bank detail fields will be selected. ||
|| **filter**
[`object`](../../../data-types.md) | An object for filtering the selected bank details in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to [bank detail fields](#fields).

An additional prefix can be assigned to the key to specify the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol should not be included in the filter value. The search looks for the substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `!%` — NOT LIKE, substring search. The `%` symbol should not be included in the filter value. The search goes from both sides.
- `!=%` — NOT LIKE, substring search. The `%` symbol should be included in the value. Examples:
    - `"mol%"` — searches for values not starting with "mol"
    - `"%mol"` — searches for values not ending with "mol"
    - `"%mol%"` — searches for values where the substring "mol" is not present in any position
- `!%=` — NOT LIKE (similar to `!=%`)
- `=` — equals, exact match (used by default)
- `!=` — not equal
- `!` — not equal 
||
|| **order**
[`object`](../../../data-types.md) | An object for sorting the selected bank details in the format `{"field_1": "order_1", ... "field_N": "order_N"}`.

Possible values for `field` correspond to [bank detail fields](#fields).

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used for managing pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results, the value is `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page 
||
|#

### Description of Bank Detail Fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the bank detail. Automatically created and unique within the account. ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object's type. Can only be `Requisite` (value `8`).

Object type identifiers are returned by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md)
||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object. Currently, it can only be the identifier of the requisite. 

Requisite identifiers can be obtained using the method [`crm.requisite.list`](../universal/crm-requisite-list.md) ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of bank detail fields (see the method [crm.requisite.preset.countries](../presets/crm-requisite-preset-countries.md) for available values).

The country code of the bank detail matches the country code in the linked requisite template, the identifier of which is specified in the `ENTITY_ID` field. ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the bank detail ||
|| **CODE**
[`string`](../../../data-types.md) | Symbolic code of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information database. 

The purpose of the field may change by the final developer. Each application ensures the uniqueness of values in this field. 

It is recommended to use a unique prefix to avoid collisions with other applications. ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Values `Y` or `N` are used. 

Currently, the field does not actually affect anything. ||
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
[`string`](../../../data-types.md) | Identifier of the external information database. The purpose of the field may change by the final developer ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"DATE_CREATE":"ASC"},"filter":{"COUNTRY_ID":"1"},"select":["ENTITY_ID","ID","NAME"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.bankdetail.list
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"DATE_CREATE":"ASC"},"filter":{"COUNTRY_ID":"1"},"select":["ENTITY_ID","ID","NAME"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.bankdetail.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.bankdetail.list",
        {
            order: { "DATE_CREATE": "ASC" },
            filter: { "COUNTRY_ID": "1" },
            select: [ "ENTITY_ID", "ID", "NAME" ]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.dir(result.data());
                if(result.more())
                    result.next();
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.bankdetail.list',
        [
            'order' => ['DATE_CREATE' => 'ASC'],
            'filter' => ['COUNTRY_ID' => '1'],
            'select' => ['ENTITY_ID', 'ID', 'NAME']
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
            "ENTITY_ID": "27",
            "ID": "11",
            "NAME": "Tinkoff"
        },
        {
            "ENTITY_ID": "30",
            "ID": "15",
            "NAME": "AO \"ALPHA-BANK\""
        },
        {
            "ENTITY_ID": "30",
            "ID": "16",
            "NAME": "AO \"Tinkoff Bank\""
        },
        {
            "ENTITY_ID": "45",
            "ID": "17",
            "NAME": "AO \"Tinkoff Bank\""
        },
        {
            "ENTITY_ID": "45",
            "ID": "18",
            "NAME": "AO \"ALPHA-BANK\""
        }
    ],
    "total": 5,
    "time": {
        "start": 1717498684.70562,
        "finish": 1717498685.13295,
        "duration": 0.42733001708984375,
        "processing": 0.020370006561279297,
        "date_start": "2024-06-04T12:58:04+02:00",
        "date_finish": "2024-06-04T12:58:05+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md)| An array of objects with information from the selected bank details. Each element contains the selected [bank detail fields](#fields) ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|  
|| **Error Text** | **Description** ||
|| `Access denied` | Insufficient access permissions to retrieve the list of bank details ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-bank-detail-add.md)
- [{#T}](./crm-requisite-bank-detail-update.md)
- [{#T}](./crm-requisite-bank-detail-get.md)
- [{#T}](./crm-requisite-bank-detail-delete.md)
- [{#T}](./crm-requisite-bank-detail-fields.md)