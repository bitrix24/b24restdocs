# Update Existing Cash Register sale.cashbox.update

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow changing settings")

This method updates an existing cash register.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_cashbox.ID`](../data-types.md#sale_cashbox) | Identifier of the cash register to be updated ||
|| **FIELDS***
[`object`](../../data-types.md) | Values of the fields to be updated (detailed description provided [below](#fields)) ||
|#

### FIELDS Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../../data-types.md) | Name of the cash register ||
|| **EMAIL**
[`string`](../../data-types.md) | E-mail address to which notifications will be sent in case of errors during receipt printing ||
|| **OFD**
[`string`](../../data-types.md) | OFD handler code. Available OFD handlers: 
- `bx_firstofd` — First OFD 
- `bx_platformaofd` — Platform OFD 
- `bx_yarusofd` — YARUS OFD 
- `bx_taxcomofd` — Taxcom OFD 
- `bx_ofdruofd` — OFD.RU 
- `bx_tenzorofd` — Tensor OFD 
- `bx_conturofd` — Kontur OFD

By default, without OFD
||
|| **OFD_SETTINGS**
[`object`](../../data-types.md) | OFD settings (detailed description provided [below](#ofd_settings)) 
||
|| **NUMBER_KKM**
[`string`](../../data-types.md) | External identifier of the cash register ||
|| **ACTIVE**
[`string`](../../data-types.md) with value `Y/N` | Activity status of the cash register. Possible values:
- `Y` — yes
- `N` — no ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting ||
|| **USE_OFFLINE**
[`string`](../../data-types.md) | Whether the cash register is used offline. Possible values:
- `Y` — yes
- `N` — no ||
|| **SETTINGS**
[`object`](../../data-types.md) | Cash register settings according to the structure of settings provided in the `CONFIG` key of the `SETTINGS` field of the [sale.cashbox.handler.add](./sale-cashbox-handler-add.md) method ||
|#

### OFD_SETTINGS Parameter {#ofd_settings}

#|
|| **Name**
`type` | **Description** ||
|| **Settings for all OFDs** |  ||
|| **OFD_MODE**
[`object`](../../data-types.md) | Settings related to the OFD operation mode. The `IS_TEST` parameter is passed ([`string`](../../data-types.md) with values `Y/N`) — OFD operation mode: 
- `Y` — test mode 
- `N` — production mode ||
|| **Additional settings for OFD.RU** |  ||
|| **SELLER_INFO**
[`object`](../../data-types.md) | Settings for the "Seller Information" section. The required parameter `INN` ([`string`](../../data-types.md)) — seller's INN is passed
||
|| **Additional settings for YARUS OFD** |  ||
|| **AUTH**
[`object`](../../data-types.md) | Authorization settings. The `INN` parameter ([`string`](../../data-types.md)) — security key is passed
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
    -d '{"ID":1,"FIELDS":{"NAME":"New Name"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.cashbox.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"FIELDS":{"NAME":"New Name"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.cashbox.update
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.cashbox.update",
        {
            "ID": 1,
            "FIELDS": {
                "NAME": "New Name",
            }
        },
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
        'sale.cashbox.update',
        [
            'ID' => 1,
            'FIELDS' =>
            [
                'NAME' => 'New Name',
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
    "result": true,
    "time": {
        "start": 1712135335.026931,
        "finish": 1712135335.407762,
        "duration": 0.3808310031890869,
        "processing": 0.0336611270904541,
        "date_start": "2024-04-03T11:08:55+02:00",
        "date_finish": "2024-04-03T11:08:55+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of updating the cash register fields ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ERROR_CASHBOX_NOT_FOUND",
    "error_description": "Cash register not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ACCESS_DENIED` | Insufficient rights to update the cash register or the application is trying to modify a cash register added by another application | 403 ||
|| `ERROR_CHECK_FAILURE` | The values for the `ID` or `FIELDS` fields are not specified | 400 ||
|| `ERROR_CASHBOX_NOT_FOUND` | Cash register with the specified `ID` not found | 400 ||
|| `ERROR_CASHBOX_UPDATE` | Other errors. More detailed information about the error can be found in `error_description` | 400 ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-cashbox-handler-add.md)
- [{#T}](./sale-cashbox-handler-update.md)
- [{#T}](./sale-cashbox-handler-list.md)
- [{#T}](./sale-cashbox-handler-delete.md)
- [{#T}](./sale-cashbox-add.md)
- [{#T}](./sale-cashbox-list.md)
- [{#T}](./sale-cashbox-delete.md)
- [{#T}](./sale-cashbox-check-apply.md)