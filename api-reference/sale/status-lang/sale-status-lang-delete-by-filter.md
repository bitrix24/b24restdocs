# Delete Localization sale.statusLang.deleteByFilter

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method removes localization records for order or delivery status by status ID and language.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`| **Description** ||
|| **fields***
[`object`](../../data-types.md) | Values of the filter fields (detailed description provided [below](#parametr-fields)) for deleting the localization record in the following structure:

```js
fields: {
    statusId: 'value',
    lid: 'value',
    name: 'value'
}
```

||
|#

### Parameter fields

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type`| **Description** ||
|| **statusId***
[`sale_status.id`](../data-types.md) | Symbolic identifier of the status ||
|| **lid***
[`sale_lang.lid`](../data-types.md) | Language identifier. Current possible values are listed below:

- ar — Arabic
- br — Portuguese
- de — German
- en — English
- fr — French
- hi — Hindi
- id — Indonesian
- in — English (India)
- it — Italian
- ja — Japanese
- la — Spanish
- ms — Malay
- pl — Polish
- ru — Russian
- sc — Chinese (Simplified)
- tc — Chinese (Traditional)
- th — Thai
- tr — Turkish
- ua — Ukrainian
- vn — Vietnamese

The list of available languages can be obtained using the method [sale.statuslang.getlistlangs](./sale-status-lang-get-list-langs.md) ||
|| **name***
[`string`](../../data-types.md) | 

{% note alert "Attention" %}

To ensure the method works correctly, please provide any non-empty value for this parameter.

{% endnote %}

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
    -d '{"fields":{"statusId":"RD","lid":"la","name":"-"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.statusLang.deleteByFilter
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"statusId":"RD","lid":"la","name":"-"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.statusLang.deleteByFilter
    ```

- JS

    ```js
    BX24.callMethod(
        'sale.statusLang.deleteByFilter',
        {
            fields: {
                statusId: 'RD',
                lid: 'la',
                name: '-',
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sale.statusLang.deleteByFilter',
        [
            'fields' =>
            [
                'statusId' => 'RD',
                'lid' => 'la',
                'name' => '-',
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
    "result":true,
    "time":{
        "start":1712223882.72792,
        "finish":1712223882.978806,
        "duration":0.2508859634399414,
        "processing":0.022897958755493164,
        "date_start":"2024-04-04T12:44:42+03:00",
        "date_finish":"2024-04-04T12:44:42+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the status localization deletion ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":0,
    "error_description":"Required fields: name"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `201740400001` | Localization for deletion by the specified filter not found ||
|| `201750000004` | Unknown localization language identifier `lid` provided ||
|| `200040300010` | Insufficient permissions to delete the status localization ||
|| `100` | Parameter `fields` not specified or empty ||
|| `0` | Required fields in the `fields` structure not provided ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-status-lang-get-list-langs.md)
- [{#T}](./sale-status-lang-add.md)
- [{#T}](./sale-status-lang-list.md)
- [{#T}](./sale-status-lang-get-fields.md)