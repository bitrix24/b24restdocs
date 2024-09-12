# Set the set of companies associated with the specified contact crm.contact.company.items.set

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "edit" access permission for contacts

The method `crm.contact.company.items.set` sets the set of companies associated with the specified contact.


## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`][1] | Identifier of the contact.

Can be obtained using the methods [`crm.contact.list`](../crm-contact-list.md) or [`crm.contact.add`](../crm-contact-add.md)
||
|| **items**^*^
[`object[]`][1] | Set of objects that describe the associated companies for the contact. The structure of a single binding object is shown [below](#contact_company_binding) ||
|#

### Structure of the Binding Object {#contact_company_binding}

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMPANY_ID**^*^
[`crm_entity`][1] | Identifier of the company that will be associated with the contact

Can be obtained using the method [`crm.item.list`](../../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **IS_PRIMARY**
[`boolean`][1] | Indicates whether the binding is primary

Possible values:
- `Y` - Yes
- `N` - No

* If there is no binding with `IS_PRIMARY = Y`, it will be set for the first binding in `items`
* If multiple bindings are provided with `IS_PRIMARY = Y`, the first binding with `IS_PRIMARY = Y` will be considered primary
||
|| **SORT**
[`integer`][1] | Sort index

By default - `i + 10`, where `i` is the maximum sort index of existing and provided bindings for the current contact or 0 if `SORT` is not provided for any bindings and if the contact has no bindings

If an existing binding is provided without the `SORT` parameter, the default value will not be set, and the value will remain the same ||
|#


## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

Set the following associated companies for the contact with `id = 82`:
* Company with `id = 8`, set it as primary and set `SORT = 100`
* Company with `id = 9` and set `SORT = 200`
* Company with `id = 10` and set `SORT = 400`

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
            'crm.contact.company.items.set',
            {
                id: 82,
                items: [
                    {
                        COMPANY_ID: 8,
                        IS_PRIMARY: "Y",
                        SORT: 100,
                    },
                    {
                        COMPANY_ID: 9,
                        SORT: 200,
                    },
                    {
                        COMPANY_ID: 10,
                        SORT: 400,
                    }
                ],
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
		"start": 1724139480.073569,
		"finish": 1724139481.016709,
		"duration": 0.9431400299072266,
		"processing": 0.4230809211730957,
		"date_start": "2024-08-20T09:38:00+02:00",
		"date_finish": "2024-08-20T09:38:01+02:00",
		"operating": 0
	}
}
```

### Returned Data

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
  "error_description": "The parameter items must be array."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `-`     | The parameter ownerEntityID is invalid or not defined. | The provided `id` is less than 0 or not provided at all ||
|| `-`     | The parameter items must be array. | A non-array value was provided in `items` ||
|| `ACCESS_DENIED` | Access denied! | The user does not have permission to edit contacts ||
|| `-`     | Not found. | Contact with the provided `id` was not found ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}


## Continue Learning

TODO

[1]: ../../../data-types.md