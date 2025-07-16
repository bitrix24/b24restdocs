# Delete CRM Status Element `crm.status.delete`

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with CRM administrator rights

The method `crm.status.delete` removes a status element by its identifier.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md) | Identifier of the status element to be deleted. You can obtain a list of identifiers using the [crm.status.list](./crm-status-list.md) method ||
|| **params**
[`string`](../../data-types.md) | Available flags:

- `FORCED` — flag for forcibly deleting system elements. Default is `N`.
Pass `Y` to delete the element even if it is a system one ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Deleting a regular element.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.status.delete",
        {
            id: 123
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
         -d '{"id":123}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.status.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":123,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.status.delete
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.status.delete',
        [
            'id' => 123
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

Deleting a system element.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.status.delete",
        {
            id: 123,
            params: {
                FORCED: "Y"
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":123,"params":{"FORCED":"Y"}}' https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.status.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -d '{"id":123,"params":{"FORCED":"Y"},"auth":"**put_access_token_here**"}' https://**put_your_bitrix24_address**/rest/crm.status.delete
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.status.delete',
        [
            'id' => 123,
            'params' => [
                'FORCED' => 'Y'
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
        "start": 1752145688.808395,
        "finish": 1752145688.842539,
        "duration": 0.03414416313171387,
        "processing": 0.010373115539550781,
        "date_start": "2025-07-10T14:08:08+02:00",
        "date_finish": "2025-07-10T14:08:08+02:00",
        "operating_reset_at": 1752146288,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result** 
[`boolean`](../../data-types.md) | Root element of the response, contains `true` on success ||
|| **time** 
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Invalid identifier.",
    "error_description": "An invalid identifier was provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `Access denied.` | No rights to perform the operation ||
|| `400`     | `Invalid identifier.` | An invalid identifier was provided ||
|| `400`     | `Status is not found.` | Element not found ||
|| `400`     | `CRM System Status can be deleted only if parameter FORCED is specified and equal to "Y".` | A system element can only be deleted with the `FORCED = "Y"` parameter ||
|| `400`     | `There are active items in this status.` | There are related items, deletion is not possible ||
|| `400`     | `Error on deleting status.` | Error while deleting the element ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-status-fields.md)
- [{#T}](./crm-status-list.md)
- [{#T}](./crm-status-get.md)
- [{#T}](./crm-status-add.md)
- [{#T}](./crm-status-update.md)