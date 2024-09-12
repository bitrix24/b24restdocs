# Delete Cash Register sale.cashbox.delete

> Scope: [`sale, cashbox`](../../scopes/permissions.md)
>
> Who can execute the method: CRM administrator (permission "Allow changing settings")

This method deletes a cash register.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`sale_cashbox.ID`](../cashbox/#sale_cashbox) | Identifier of the cash register to be deleted ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.cashbox.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.cashbox.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "sale.cashbox.delete",
        {
            "ID": 1,
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
        'sale.cashbox.delete',
        [
            'ID' => 1,
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
[`boolean`](../../data-types.md) | Result of the cash register deletion ||
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
|| `ACCESS_DENIED` | Insufficient permissions to delete the cash register or the application is trying to delete a cash register added by another application | 403 ||
|| `ERROR_CHECK_FAILURE` | The `ID` field value is not specified | 400 ||
|| `ERROR_CASHBOX_NOT_FOUND` | Cash register with the specified `ID` not found | 400 ||
|| `ERROR_CASHBOX_DELETE` | Other errors. More detailed information about the error can be found in `error_description` | 400 ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sale-cashbox-handler-add.md)
- [{#T}](./sale-cashbox-handler-update.md)
- [{#T}](./sale-cashbox-handler-list.md)
- [{#T}](./sale-cashbox-handler-delete.md)
- [{#T}](./sale-cashbox-add.md)
- [{#T}](./sale-cashbox-update.md)
- [{#T}](./sale-cashbox-list.md)
- [{#T}](./sale-cashbox-check-apply.md)