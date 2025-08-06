# Create VAT Rate crm.vat.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with CRM administrator rights

The method `crm.vat.add` creates a new VAT rate in CRM.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields*** 
[`object`](../../../data-types.md) | Object format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — field name
- `value_n` — field value

The list of available fields is described [below](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
 `type` | **Description** ||
|| **ACTIVE** 
[`string`](../../../data-types.md) | Activity of the rate:
- `Y` — active,
- `N` — inactive.

Default — `Y` ||
|| **C_SORT** 
[`integer`](../../../data-types.md) | Sorting. 
Default — 100 ||
|| **NAME*** 
[`string`](../../../data-types.md) | Name of the rate ||
|| **RATE*** 
[`double`](../../../data-types.md) | VAT rate value, % ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.vat.add",
        {
            fields: {
                ACTIVE: "Y",
                C_SORT: 100,
                NAME: "VAT 20%",
                RATE: 20.0
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
         -d '{"fields":{"ACTIVE":"Y","C_SORT":100,"NAME":"VAT 20%","RATE":20.0}}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webbhook_here**/crm.vat.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"ACTIVE":"Y","C_SORT":100,"NAME":"VAT 20%","RATE":20.0},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.vat.add
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.vat.add',
        [
            'fields' => [
                'ACTIVE' => 'Y',
                'C_SORT' => 100,
                'NAME' => 'VAT 20%',
                'RATE' => 20.0
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
    "result": 13,
    "time": {
        "start": 1751982588.245153,
        "finish": 1751982588.287266,
        "duration": 0.04211306571960449,
        "processing": 0.005285978317260742,
        "date_start": "2025-07-08T16:49:48+02:00",
        "date_finish": "2025-07-08T16:49:48+02:00",
        "operating_reset_at": 1751983188,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`integer`](../../../data-types.md) | Root element of the response, contains the identifier of the created VAT rate ||
|| **time** 
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Invalid parameters.",
    "error_description": "Invalid parameters provided."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `The Commercial Catalog module is not installed.` | Catalog module is not installed ||
|| `400`     | `Invalid parameters.` | Invalid parameters provided ||
|| `400`     | `Access denied.` | No rights to perform the operation ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-vat-list.md)
- [{#T}](./crm-vat-get.md)
- [{#T}](./crm-vat-update.md)
- [{#T}](./crm-vat-delete.md) 
- [{#T}](./crm-vat-fields.md)