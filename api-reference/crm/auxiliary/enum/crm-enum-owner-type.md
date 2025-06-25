# Get CRM Object Types crm.enum.ownertype

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.enum.ownertype` returns the identifiers of CRM object types and smart processes. Use the `ID` of the object type as the value for the `entityTypeId` parameter in the methods [crm.item.*](../../universal/index.md), [crm.activity.*](../../timeline/activities/index.md).

{% note info " " %}

The identifiers for smart processes in each Bitrix24 are unique and may differ from those provided in the example.

{% endnote %}

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.enum.ownertype",
        {},
        function(result) {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.enum.ownertype
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.enum.ownertype
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.enum.ownertype',
        []
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
     "ID": 1,
     "NAME": "Lead",
     "SYMBOL_CODE": "LEAD",
     "SYMBOL_CODE_SHORT": "L"
    },
    {
     "ID": 2,
     "NAME": "Deal",
     "SYMBOL_CODE": "DEAL",
     "SYMBOL_CODE_SHORT": "D"
    },
    {
     "ID": 3,
     "NAME": "Contact",
     "SYMBOL_CODE": "CONTACT",
     "SYMBOL_CODE_SHORT": "C"
    },
    {
     "ID": 4,
     "NAME": "Company",
     "SYMBOL_CODE": "COMPANY",
     "SYMBOL_CODE_SHORT": "CO"
    },
    {
     "ID": 5,
     "NAME": "Invoice (old version)",
     "SYMBOL_CODE": "INVOICE",
     "SYMBOL_CODE_SHORT": "I"
    },
    {
     "ID": 31,
     "NAME": "Invoice",
     "SYMBOL_CODE": "SMART_INVOICE",
     "SYMBOL_CODE_SHORT": "SI"
    },
    {
     "ID": 7,
     "NAME": "Estimate",
     "SYMBOL_CODE": "QUOTE",
     "SYMBOL_CODE_SHORT": "Q"
    },
    {
     "ID": 8,
     "NAME": "Requisites",
     "SYMBOL_CODE": "REQUISITE",
     "SYMBOL_CODE_SHORT": "RQ"
    },
    {
     "ID": 36,
     "NAME": "Document",
     "SYMBOL_CODE": "SMART_DOCUMENT",
     "SYMBOL_CODE_SHORT": "DO"
    },
    {
     "ID": 39,
     "NAME": "Company Document",
     "SYMBOL_CODE": "SMART_B2E_DOC",
     "SYMBOL_CODE_SHORT": "SBD"
    },
    {
     "ID": 177,
     "NAME": "Equipment Purchase",
     "SYMBOL_CODE": "DYNAMIC_177",
     "SYMBOL_CODE_SHORT": "Tb1"
    },
    {
     "ID": 156,
     "NAME": "Purchase",
     "SYMBOL_CODE": "DYNAMIC_156",
     "SYMBOL_CODE_SHORT": "T9c"
    }
],
"time": {
    "start": 1750153184.228934,
    "finish": 1750153184.262921,
    "duration": 0.03398704528808594,
    "processing": 0.0008471012115478516,
    "date_start": "2025-06-17T12:39:44+02:00",
    "date_finish": "2025-06-17T12:39:44+02:00",
    "operating_reset_at": 1750153784,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | Array of owner types [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the owner type ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the owner type ||
|| **SYMBOL_CODE**
[`string`](../../../data-types.md) | Symbolic code ||
|| **SYMBOL_CODE_SHORT**
[`string`](../../../data-types.md) | Short symbolic code ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../../../../tutorials/tasks/how-to-connect-task-to-spa.md)