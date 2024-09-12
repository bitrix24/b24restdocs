# Delete Contact crm.contact.delete

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "delete" access permission for contacts

The method `crm.contact.delete` removes a contact and all associated objects.

## Method Parameters

{% include [Footnote about parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **id^*^**
[`integer`][1] | Identifier of the contact. Can be obtained using the methods [`crm.contact.list`](crm-contact-list.md) or [`crm.contact.add`](crm-contact-add.md) ||
|#

## Code Examples

{% include [Footnote about examples](../../../_includes/examples.md) %}

Deleting a contact with `id = 50`

{% list tabs %}

- cURL (Webhook)

    ```bash
    todo
    ```

- cURL (OAuth)

    ```bash
    todo
    ```

- JS

    ```js
        BX24.callMethod(
            'crm.contact.delete',
            {
                id: 50,
            },
            (result) => {
                result.error()
                    ? console.error(result.error())
                    : console.info(result.data())
                ;
            },
        );
    ```

- PHP

    ```php
    todo
    ```

{% endlist %}


## Response Handling

HTTP status: **200**

```json
{
  "result": true,
  "time": {
    "start": 1723727109.430573,
    "finish": 1723727210.611973,
    "duration": 101.18140006065369,
    "processing": 100.78639197349548,
    "date_start": "2024-08-15T15:05:09+02:00",
    "date_finish": "2024-08-15T15:06:50+02:00"
  }
}
```


### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response.

Returns `true` on success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | ID is not defined or invalid. | The `id` parameter is missing or the provided value is not a positive integer ||
|| `-`     | Access denied. | The user does not have permission for "Delete" contact ||
|| `ERROR_CORE` | Element not found | Contact with the provided `id` was not found ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](crm-contact-fields.md)
- [{#T}](crm-contact-add.md)
- [{#T}](crm-contact-update.md)
- [{#T}](crm-contact-get.md)
- [{#T}](crm-contact-list.md)

[1]: ../../data-types.md