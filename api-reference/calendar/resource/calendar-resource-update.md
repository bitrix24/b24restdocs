# Update Resource calendar.resource.update

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates a resource.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **resourceId***
[`integer`](../../data-types.md) | Resource identifier.

You can obtain the identifier using the resource creation method [calendar.resource.add](./calendar-resource-add.md) or the resource list retrieval method [calendar.resource.list](./calendar-resource-list.md) ||
|| **name***
[`string`](../../data-types.md) | New name for the resource ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"resourceId":197,"name":"Changed Resource Name"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.resource.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"resourceId":197,"name":"Changed Resource Name","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.resource.update
    ```

- JS

    ```js
    BX24.callMethod(
        'calendar.resource.update',
        {
            resourceId: 197,
            name: 'Changed Resource Name'
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.resource.update',
        [
            'resourceId' => 197,
            'name' => 'Changed Resource Name'
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
    "result": 197,
    "time": {
        "start": 1733318565.183275,
        "finish": 1733318565.695058,
        "duration": 0.5117831230163574,
        "processing": 0.29406094551086426,
        "date_start": "2024-12-04T13:22:45+00:00",
        "date_finish": "2024-12-04T13:22:45+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the updated resource ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "name" for the method "calendar.resource.update" is missing"
}
```
{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "id" for the method "calendar.resource.update" is missing | The required parameter `resourceId` was not provided ||
|| Empty string | The required parameter "name" for the method "calendar.resource.update" is missing | The required parameter `name` was not provided ||
|| Empty string | Access denied | The method is called by an external user or the user is not allowed to modify resources ||
|| Empty string | An error occurred while modifying the resource | Another error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-resource-add.md)
- [{#T}](./calendar-resource-list.md)
- [{#T}](./calendar-resource-booking-list.md)
- [{#T}](./calendar-resource-delete.md)