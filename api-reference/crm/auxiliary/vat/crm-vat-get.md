# Get VAT Rate by ID crm.vat.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.vat.get` returns the VAT rate parameters by ID.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Identifier of the VAT rate. You can get a list of rates using the [crm.vat.list](./crm-vat-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.vat.get",
        {
            id: 7
        },
        function(result) {
            if(result.error())
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
         -d '{"id":7}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.vat.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.vat.get
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.vat.get',
        [
            'id' => 7
        ]
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
        "ACTIVE": "Y",
        "NAME": "VAT 20%",
        "RATE": "20.00",
        "C_SORT": "100"
    },
    "time": {
        "start": 1752043855.534128,
        "finish": 1752043855.598301,
        "duration": 0.06417298316955566,
        "processing": 0.008214235305786133,
        "date_start": "2025-07-09T09:50:55+02:00",
        "date_finish": "2025-07-09T09:50:55+02:00",
        "operating_reset_at": 1752044455,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
 `type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the VAT rate ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Activity status of the rate ||
|| **C_SORT**
[`integer`](../../../data-types.md) | Sorting order ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the rate ||
|| **RATE**
[`double`](../../../data-types.md) | Value of the VAT rate, % ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "VAT rate not found.",
    "error_description": "VAT rate not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `The Commercial Catalog module is not installed.` | The catalog module is not installed ||
|| `400`     | `Access denied.` | No permission to perform the operation ||
|| `400`     | `VAT rate not found.` | VAT rate not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-vat-fields.md)
- [{#T}](./crm-vat-list.md)
- [{#T}](./crm-vat-add.md)
- [{#T}](./crm-vat-update.md)
- [{#T}](./crm-vat-delete.md)