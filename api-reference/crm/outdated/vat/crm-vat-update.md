# Update Existing VAT Rate crm.vat.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with CRM administrator rights

The method `crm.vat.update` updates the parameters of an existing VAT rate.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Identifier of the VAT rate to be updated. You can get a list of rates using the [crm.vat.list](./crm-vat-list.md) method. ||
|| **fields*** 
[`object`](../../../data-types.md) | Array of fields to update. The list of available fields is described [below](#fields)  ||
|#

### Fields Parameter {#fields}

#|
|| **Name**
 `type` | **Description** ||
|| **ACTIVE** 
[`string`](../../../data-types.md) | Activity status of the rate:
- `Y` — active,
- `N` — inactive.
||
|| **C_SORT** 
[`integer`](../../../data-types.md) | Sorting ||
|| **NAME*** 
[`string`](../../../data-types.md) | Name of the rate ||
|| **RATE*** 
[`double`](../../../data-types.md) | Value of the VAT rate, % ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.vat.update",
        {
            id: 7,
            fields: {
                ACTIVE: "N",
                NAME: "VAT 20% (inactive)"
            }
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
         -d '{"id":7,"fields":{"ACTIVE":"N","NAME":"VAT 20% (inactive)"}}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.vat.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"fields":{"ACTIVE":"N","NAME":"VAT 20% (inactive)"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.vat.update
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.vat.update',
        [
            'id' => 7,
            'fields' => [
                'ACTIVE' => 'N',
                'NAME' => 'VAT 20% (inactive)'
            ]
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
    "result": 13,
    "time": {
        "start": 1752045209.496356,
        "finish": 1752045209.539975,
        "duration": 0.04361891746520996,
        "processing": 0.009280920028686523,
        "date_start": "2025-07-09T10:13:29+02:00",
        "date_finish": "2025-07-09T10:13:29+02:00",
        "operating_reset_at": 1752045809,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response, contains the identifier of the updated rate ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "Invalid identifier.",
    "error_description": "An invalid identifier was provided."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `The Commercial Catalog module is not installed.` | The catalog module is not installed ||
|| `400`     | `Invalid parameters.` | Invalid parameters were provided ||
|| `400`     | `Access denied.` | No rights to perform the operation ||
|| `400`     | `Invalid identifier.` | An invalid identifier was provided ||
|| `400`     | `Error on updating VAT rate.` | Error while updating the VAT rate ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-vat-fields.md)
- [{#T}](./crm-vat-list.md)
- [{#T}](./crm-vat-get.md)
- [{#T}](./crm-vat-add.md)
- [{#T}](./crm-vat-delete.md)