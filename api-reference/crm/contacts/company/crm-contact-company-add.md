# Add Company to Specified Contact crm.contact.company.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "edit" access permission for contacts

The method `crm.contact.company.add` adds a company to the specified contact.


## Method Parameters

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`][1] | Identifier of the contact.

Can be obtained using the methods [`crm.contact.list`](../crm-contact-list.md) or [`crm.contact.add`](../crm-contact-add.md)
||
|| **fields**^*^
[`object`][1] | Format object.

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where
- `field_n` — field name
- `value_n` — field value

The list of available fields is described [below](#parametr-fields). ||
|#

### Parameter fields

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMPANY_ID**^*^
[`crm_entity`][1] | Identifier of the company to be linked to the contact

Can be obtained using the method [`crm.item.list`](../../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **IS_PRIMARY**
[`boolean`][1] | Indicates if the link is primary

Possible values:
- `Y` - Yes
- `N` - No

* The first added element's `IS_PRIMARY` is `Y` by default
* Passing `IS_PRIMARY = Y` for a new and non-first link overrides the existing primary link ||
|| **SORT**
[`integer`][1] | Sort index

Default is `i + 10`, where `i` is the maximum sort index of existing links for the current contact or 0 if none exist ||
|#


## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

Example of adding a Contact-Company link, where
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
            'crm.contact.company.add',
            {
                id: 54,
                fields: {
                    COMPANY_ID: 32,
                    IS_PRIMARY: "Y",
                    SORT: 1000,
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
		"start": 1724068028.331234,
		"finish": 1724068028.726591,
		"duration": 0.3953571319580078,
		"processing": 0.13033390045166016,
		"date_start": "2024-08-19T13:47:08+02:00",
		"date_finish": "2024-08-19T13:47:08+02:00"
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

- `true` - On success
- `false` - On failure (Most likely, the company you are trying to add already exists in the links)
||
|| **time**
[`time`][1] | Information about the request execution time ||
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
|| `-`     | The parameter 'ownerEntityID' is invalid or not defined. | The provided `id` is less than 0 or not provided at all ||
|| `-`     | The parameter 'fields' must be an array. | The `fields` parameter is not an object ||
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