# Get the list of currencies crm.currency.list

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with access to CRM settings

This method retrieves the list of currencies created in the account.

{% note info %}

Localization parameters (settings dependent on language) will be returned for the current language of the account.

{% endnote %}

## Method Parameters

#|
||  **Name**
`type`| **Description** ||
|| **order**
[`object`](../../data-types.md) | An object for sorting records in the format `{"field_1": "order_1", ... "field_N": "order_N"}`, where `field_N` is the identifier of the field [crm_currency](../data-types.md#crm_currency).

Possible values for `order_N`:

- `asc` — in ascending order
- `desc` — in descending order

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
    -d '{"order":{"sort":"asc","currency":"asc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.currency.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"order":{"sort":"asc","currency":"asc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.currency.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.currency.list",
        {
            order: {
                sort: 'asc',
                currency: 'asc',
            },
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
                console.log(result.data);
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
        'crm.currency.list',
        [
            'order' => [
                'sort' => 'asc',
                'currency' => 'asc',
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
    "result": [
        {
            "CURRENCY": "USD",
            "AMOUNT_CNT": "1",
            "AMOUNT": "68.7900",
            "SORT": "200",
            "BASE": "N",
            "FULL_NAME": "US Dollar",
            "LID": "com",
            "FORMAT_STRING": "$#",
            "DEC_POINT": ".",
            "THOUSANDS_SEP": null,
            "DECIMALS": "2",
            "DATE_UPDATE": "2023-03-21T15:19:50+02:00"
        },
        {
            "CURRENCY": "EUR",
            "AMOUNT_CNT": "1",
            "AMOUNT": "78.3200",
            "SORT": "300",
            "BASE": "N",
            "FULL_NAME": "Euro",
            "LID": "com",
            "FORMAT_STRING": "# &euro;",
            "DEC_POINT": ".",
            "THOUSANDS_SEP": "&nbsp;",
            "DECIMALS": "2",
            "DATE_UPDATE": "2023-03-21T15:19:50+02:00"
        }
    ],
    "total": 0,
    "time": {
        "start": 1716986687.629166,
        "finish": 1716986688.481436,
        "duration": 0.8522701263427734,
        "processing": 0.014969825744628906,
        "date_start": "2024-05-29T14:44:47+02:00",
        "date_finish": "2024-05-29T14:44:48+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`crm_currency[]`](../data-types.md#crm_currency) | An array of objects containing information about the selected currencies ||
|| **total**
[`integer`](../../data-types.md) | Currently always has a value of `0` ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
	"error": "",
	"error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | Insufficient access permissions ||
|| Empty string | Failed to get list. General error. | Currency module is not installed ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-currency-add.md)
- [{#T}](./crm-currency-update.md)
- [{#T}](./crm-currency-get.md)
- [{#T}](./crm-currency-delete.md)
- [{#T}](./crm-currency-fields.md)