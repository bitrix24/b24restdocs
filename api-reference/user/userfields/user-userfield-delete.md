# Delete Custom Field user.userfield.delete

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a custom field.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md)| Identifier of the custom field.

To obtain the identifiers of custom fields, use the [user.userfield.list](./user-userfield-list.md) method.
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
    -d '{"id":123}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.userfield.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":123,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/user.userfield.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'user.userfield.delete',
        {
            id: 123,
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
        'user.userfield.delete',
        [
            'id' => 123
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
        "start":1747310916.827703,
        "finish":1747310918.05313,
        "duration":1.2254269123077393,
        "processing":0.2228090763092041,
        "date_start":"2025-05-15T14:08:36+02:00",
        "date_finish":"2025-05-15T14:08:38+02:00",
        "operating":0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of deleting the custom field ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{	
   "error":"",
   "error_description":"Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty string | Access denied. | Field with such `id` does not exist or access is denied ||
|| Empty string | ID is not defined or invalid | `id` is not specified or is incorrect ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-userfield-add.md)
- [{#T}](./user-userfield-update.md)
- [{#T}](./user-userfield-list.md)