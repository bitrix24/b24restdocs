# Get Currency by ID crm.currency.get

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with access to CRM settings

This method retrieves currency data by its symbolic identifier `id` according to ISO 4217.

{% note info %}

Localization parameters (settings that depend on the language) will be returned for the current account language.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
||  **Name**
`type`| **Description** ||
|| **id**
[`crm_currency.CURRENCY`](../data-types.md#crm_currency) | Symbolic identifier of the currency.

Corresponds to the ISO 4217 standard.

Can be obtained using the [crm.currency.list](./crm-currency-list.md) method.
 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"USD"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"USD","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.currency.get
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.currency.get",
        {
            id: "USD"
        },
    )
    .then(
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        },
        function(error)
        {
            console.info(error);
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.currency.get',
        [
            'id' => 'USD'
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
        "CURRENCY": "USD",
        "AMOUNT_CNT": "1",
        "AMOUNT": "1.0000",
        "SORT": "100",
        "BASE": "Y",
        "FULL_NAME": "United States Dollar",
        "LID": "us",
        "FORMAT_STRING": "# $",
        "DEC_POINT": ".",
        "THOUSANDS_SEP": ",",
        "DECIMALS": "2",
        "DATE_UPDATE": "2024-01-29T12:28:40+02:00",
        "LANG": {
            "en": {
                "FORMAT_STRING": "$#",
                "FULL_NAME": "United States Dollar",
                "DEC_POINT": ".",
                "THOUSANDS_SEP": null,
                "DECIMALS": "2",
                "THOUSANDS_VARIANT": "C",
                "HIDE_ZERO": "Y"
            }
        }
    },
    "time": {
        "start": 1718357792.091095,
        "finish": 1718357792.889212,
        "duration": 0.79811692237854,
        "processing": 0.10800814628601074,
        "date_start": "2024-06-14T11:36:32+02:00",
        "date_finish": "2024-06-14T11:36:32+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`crm_currency`](../data-types.md#crm_currency) | Object containing currency data ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
	"error": "",
	"error_description": "Not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | Insufficient access permissions ||
|| Empty string | Not found | Currency with the specified code not found ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-currency-add.md)
- [{#T}](./crm-currency-update.md)
- [{#T}](./crm-currency-list.md)
- [{#T}](./crm-currency-delete.md)
- [{#T}](./crm-currency-fields.md)