# Clear the set of companies associated with the specified contact crm.contact.company.items.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "edit" access permission for contacts

The method `crm.contact.company.items.delete` clears the set of companies associated with the specified contact.


## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`][1] | Identifier of the contact.

Can be obtained using the methods [`crm.contact.list`](../crm-contact-list.md) or [`crm.contact.add`](../crm-contact-add.md) ||
|#


## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

Example of removing all linked companies from a contact with `id = 54`

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
            'crm.contact.company.items.delete',
            {
                id: 54,
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
		"start": 1724075916.767978,
		"finish": 1724075917.346106,
		"duration": 0.5781280994415283,
		"processing": 0.2096860408782959,
		"date_start": "2024-08-19T15:58:36+02:00",
		"date_finish": "2024-08-19T15:58:37+02:00"
	}
}
```

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response. Contains `true` - In case of success ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#


## Error Handling

HTTP status: **400**

```json
{
	"error": "",
	"error_description": "The parameter ownerEntityID is invalid or not defined."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | The parameter 'ownerEntityID' is invalid or not defined. | The provided `id` is less than 0 or not provided at all ||
|| `ACCESS_DENIED` | Access denied! | The user does not have permission to edit contacts ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}


## Continue Learning

TODO

[1]: ../../../data-types.md