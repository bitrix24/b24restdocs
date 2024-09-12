# Get Description of Bank Details Fields crm.requisite.bankdetail.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a formal description of the bank details fields.

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.bankdetail.fields
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.bankdetail.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.bankdetail.fields",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.bankdetail.fields',
        []
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
        "ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ID"
        },
        "ENTITY_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Object ID"
        },
        "COUNTRY_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Country ID"
        },
        "DATE_CREATE": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Creation Date"
        },
        "DATE_MODIFY": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Modification Date"
        },
        "CREATED_BY_ID": {
            "type": "user",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Created By"
        },
        "MODIFY_BY_ID": {
            "type": "user",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Modified By"
        },
        "NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Name"
        },
        "CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Code"
        },
        "XML_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "External Code"
        },
        "ACTIVE": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Active"
        },
        "SORT": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Sorting"
        },
        "RQ_BANK_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Bank Name"
        },
        "RQ_BANK_ADDR": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Bank Address"
        },
        "RQ_BANK_CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_BANK_CODE"
        },
        "RQ_AGENCY_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_AGENCY_NAME"
        },
        "RQ_BANK_ROUTE_NUM": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_BANK_ROUTE_NUM"
        },
        "RQ_BIK": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "BIK"
        },
        "RQ_MFO": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_MFO"
        },
        "RQ_ACC_NAME": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_ACC_NAME"
        },
        "RQ_ACC_NUM": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Account Number"
        },
        "RQ_ACC_TYPE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_ACC_TYPE"
        },
        "RQ_IIK": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_IIK"
        },
        "RQ_ACC_CURRENCY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Account Currency"
        },
        "RQ_COR_ACC_NUM": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Correspondent Account"
        },
        "RQ_IBAN": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "IBAN"
        },
        "RQ_SWIFT": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "SWIFT"
        },
        "RQ_BIC": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_BIC"
        },
        "RQ_CODEB": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_CODEB"
        },
        "RQ_CODEG": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_CODEG"
        },
        "RQ_RIB": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "RQ_RIB"
        },
        "COMMENTS": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Comment"
        },
        "ORIGINATOR_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "External Source"
        }
    },
    "time": {
        "start": 1717409814.796487,
        "finish": 1717409815.225673,
        "duration": 0.4291858673095703,
        "processing": 0.013273000717163086,
        "date_start": "2024-06-03T12:16:54+02:00",
        "date_finish": "2024-06-03T12:16:55+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the field identifier and `value` is the object with [field attributes](#attributes) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

### Description of Bank Details Fields

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the bank detail. Created automatically and unique within the account ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object's type. Can only be `Requisite` (value `8`).

Object type identifiers are returned by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object. Currently, it can only be the identifier of the requisite. 

Requisite identifiers can be obtained using the method [`crm.requisite.list`](../universal/crm-requisite-list.md) ||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of bank detail fields (see the method [crm.requisite.preset.countries](../presets/crm-requisite-preset-countries.md) for available values).

The country code of the bank detail matches the country code in the linked requisite template, the identifier of which is specified in the `ENTITY_ID` field 
||
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
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the object in the external information base. 

The purpose of the field may change by the final developer. Each application ensures the uniqueness of values in this field. 

It is recommended to use a unique prefix to avoid collisions with other applications ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Values `Y` or `N` are used. 

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
[`string`](../../../data-types.md) | Account Currency ||
|| **RQ_COR_ACC_NUM**
[`string`](../../../data-types.md) | Correspondent Account ||
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

### Description of Attributes {#attributes}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Field type ||
|| **isRequired**
[`boolean`](../../../data-types.md) | Required attribute. Possible values:
- true — yes
- false — no
||
|| **isReadOnly**
[`boolean`](../../../data-types.md) | Read-only attribute. Possible values:
- true — yes
- false — no 
||
|| **isImmutable**
[`boolean`](../../../data-types.md) | Immutable attribute. Possible values:
- true — yes
- false — no 
||
|| **isMultiple**
[`boolean`](../../../data-types.md) | Multi-field attribute. Possible values:
- true — yes
- false — no
||
|| **isDynamic**
[`boolean`](../../../data-types.md) | Custom attribute. Possible values:
- true — yes
- false — no
||
|| **title**
[`string`](../../../data-types.md) | Field identifier ||
|#

## Error Handling

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-bank-detail-add.md)
- [{#T}](./crm-requisite-bank-detail-update.md)
- [{#T}](./crm-requisite-bank-detail-get.md)
- [{#T}](./crm-requisite-bank-detail-list.md)
- [{#T}](./crm-requisite-bank-detail-delete.md)