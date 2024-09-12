# Delete Custom Type crm.type.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with administrative access to the CRM section

This method deletes an existing SPA by its `id`.

You can only delete an SPA if there are no associated CRM entities. If there are any, you must delete them first before deleting the SPA.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`][1] | Identifier of the SPA. It can be obtained using the methods: [`crm.type.list`](./crm-type-list.md), [`crm.type.add`](./crm-type-add.md) ||
|#

## Code Examples

Delete the SPA with `id = 16`

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":16}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.type.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":16,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.type.delete
    ```

- JS

    ```js
    BX24.callMethod(
        'crm.type.delete',
        {
            id: 16,
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error());

                return;
            }

            console.info(result.data());
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.type.delete',
        [
            'id' => 16
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
    "result": [],
    "time": {
        "start": 1720441523.621191,
        "finish": 1720441528.162992,
        "duration": 4.5418009757995605,
        "processing": 4.141964912414551,
        "date_start": "2024-07-08T14:25:23+02:00",
        "date_finish": "2024-07-08T14:25:28+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`][1] | Root element of the response. On success, `result = []`.

If `result` returns `null`, it is likely that the required parameter `id` was not provided. In this case, the object will not be deleted ||
|| **time**
[`time`][1] | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "BX_ERROR",
    "error_description": "You cannot delete an entity type that has elements"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `allowed_only_intranet_user` | Action allowed only for intranet users | Occurs if the user is not an intranet user ||
|| `400` | `ACCESS_DENIED` | Access denied | Occurs if the user does not have administrative rights in CRM ||

|| `400` | `BX_ERROR` | You cannot delete an entity type that has elements | Occurs when trying to delete an SPA with associated CRM entities ||
|| `400` | `0` | SPA not found | The SPA with the provided `id` was not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-type-add.md)
- [{#T}](./crm-type-update.md)
- [{#T}](./crm-type-get.md)
- [{#T}](./crm-type-list.md)
- [{#T}](./crm-type-fields.md)

[1]: ../../../data-types.md