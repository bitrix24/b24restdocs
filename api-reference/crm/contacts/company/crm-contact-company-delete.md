# Delete Company from Specified Contact crm.contact.company.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "edit" access permission for contacts

The method `crm.contact.company.delete` removes a company from the specified contact.


## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`][1] | Identifier of the contact

Can be obtained using the methods [`crm.contact.list`](../crm-contact-list.md) or [`crm.contact.add`](../crm-contact-add.md) ||
|| **fields**^*^
[`object`][1] | An object containing information about which company needs to be removed from the associations

Contains a single key `COMPANY_ID` ||
|| **fields.COMPANY_ID**^*^
[`integer`][1] | Identifier of the company to be removed from the associations ||
|#

{% note info "Removing Primary Association" %}

When the primary association is removed, the first available association will become the primary one.

{% endnote %}


## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of removing the contact-company association, where
* Contact ID - `54`
* Company ID - `32`

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
            'crm.contact.company.delete',
            {
                id: 54,
                fields: {
                    COMPANY_ID: 32,
                },
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
		"start": 1724072653.79827,
		"finish": 1724072717.749956,
		"duration": 63.95168590545654,
		"processing": 63.63148093223572,
		"date_start": "2024-08-19T15:04:13+02:00",
		"date_finish": "2024-08-19T15:05:17+02:00"
	}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`][1] | Root element of the response
Contains:

- `true` - In case of success
- `false` - In case of failure (Most likely, the company you are trying to remove is not associated with the contact)
||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#


## Error Handling

HTTP status: **400**

```json
{
	"error": "",
	"error_description": "The parameter 'ownerEntityID' is invalid or not defined."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | The parameter 'ownerEntityID' is invalid or not defined. | The provided `id` is less than 0 ||
|| `-`     | The parameter 'fields' must be an array. | The `fields` provided is not an object ||
|| `ACCESS_DENIED` | Access denied! | The user does not have permission to edit contacts ||
|| `-`     | Not found. | Contact with the provided `id` not found ||
|| `-`     | The parameter 'fields' is not valid. | Can occur for several reasons:
* If the required parameter `fields.COMPANY_ID` is not provided
* If the provided `fields.COMPANY_ID` is less than or equal to 0 ||
||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}


## Continue Learning
TODO

[1]: ../../../data-types.md