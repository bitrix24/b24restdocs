# Get Currency Localizations crm.currency.localizations.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with access to CRM settings

This method retrieves existing currency localizations.

## Method Parameters

#|
||  **Name**
`type`| **Description** ||
|| **id**
[`string`](../../../data-types.md) | Currency identifier. 

Corresponds to the ISO 4217 standard.

The identifier can be obtained using the [crm.currency.list](../crm-currency-list.md) method.
 ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"RUB"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.localizations.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":"RUB","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.currency.localizations.get
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.currency.localizations.get",
        {
            id: "RUB"
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
                console.log(result);
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
        'crm.currency.localizations.get',
        [
            'id' => 'RUB'
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
        "en": {
            "FORMAT_STRING": "&#8381;#",
            "FULL_NAME": "Russian Ruble",
            "DEC_POINT": ".",
            "THOUSANDS_SEP": null,
            "DECIMALS": "2",
            "THOUSANDS_VARIANT": "C",
            "HIDE_ZERO": "Y"
        },
        "ru": {
            "FORMAT_STRING": "# &#8381;",
            "FULL_NAME": "Russian Ruble",
            "DEC_POINT": ".",
            "THOUSANDS_SEP": "&nbsp;",
            "DECIMALS": "2",
            "THOUSANDS_VARIANT": "B",
            "HIDE_ZERO": "Y"
        }
    },
    "time": {
        "start": 1718114356.076467,
        "finish": 1718114356.682042,
        "duration": 0.6055748462677002,
        "processing": 0.03888106346130371,
        "date_start": "2024-06-11T15:59:16+02:00",
        "date_finish": "2024-06-11T15:59:16+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object in the format `{"lang_1": "value_1", ... "lang_N": "value_N"}`, where `lang_N` is the language identifier, and `value` is an object of type [crm_currency_localization](../../data-types.md#crm_currency_localization). ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
	"error": "",
	"error_description": "The parameter id is invalid or not defined."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | Insufficient access rights ||
|| Empty string | The parameter id is invalid or not defined. | Empty currency identifier ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-currency-localizations-set.md)
- [{#T}](./crm-currency-localizations-delete.md)
- [{#T}](./crm-currency-localizations-fields.md)