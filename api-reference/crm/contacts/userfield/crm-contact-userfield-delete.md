# Delete Custom Field for Contacts crm.contact.userfield.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: Administrator

The method `crm.contact.userfield.delete` removes a custom field for contacts.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id^*^**
[`integer`](../../../data-types.md) | Identifier of the custom field associated with the contact

This can be obtained using the methods [`crm.contact.userfield.add`](crm-contact-userfield-add.md) or [`crm.contact.userfield.list`](crm-contact-userfield-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Deleting a custom field with `id = 432`

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
        'crm.contact.userfield.delete',
        {
            id: 432,
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

HTTP Status: **200**

```json
{
	"result": true,
	"time": {
		"start": 1724316817.995457,
		"finish": 1724316818.640754,
		"duration": 0.6452970504760742,
		"processing": 0.3215677738189697,
		"date_start": "2024-08-22T10:53:37+02:00",
		"date_finish": "2024-08-22T10:53:38+02:00",
		"operating": 0
	}
}
```

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#


## Error Handling

HTTP Status: **400**

```json
{
  "error": "",
  "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-` | ID is not defined or invalid. | The provided `id` is either less than or equal to zero, or not provided at all ||
|| `-` | Access denied. | Occurs in cases:
* The user does not have administrative rights
* The user is trying to delete a custom field not associated with contacts ||

|| `ERROR_NOT_FOUND` | The entity with ID `id` is not found. | The custom field with the provided `id` does not exist ||
|| `-` | Error deleting `FIELD_NAME` for object `ENTITY_ID`. | Unknown error during deletion ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}


## Continue Learning

TODO