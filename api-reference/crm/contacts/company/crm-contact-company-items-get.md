# Get a set of companies associated with the specified contact crm.contact.company.items.get

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user with "read" access permission for contacts

The method `crm.contact.company.items.get` returns a set of companies associated with the specified contact.


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

Example of retrieving all linked companies for a contact with `id = 54`

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
            'crm.contact.company.items.get',
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
  "result": [
    {
      "COMPANY_ID": 7,
      "SORT": 100,
      "ROLE_ID": 0,
      "IS_PRIMARY": "Y"
    },
    {
      "COMPANY_ID": 8,
      "SORT": 110,
      "ROLE_ID": 0,
      "IS_PRIMARY": "N"
    },
    {
      "COMPANY_ID": 9,
      "SORT": 120,
      "ROLE_ID": 0,
      "IS_PRIMARY": "N"
    }
  ],
  "time": {
    "start": 1724078791.470108,
    "finish": 1724078791.969407,
    "duration": 0.4992990493774414,
    "processing": 0.19150400161743164,
    "date_start": "2024-08-19T16:46:31+02:00",
    "date_finish": "2024-08-19T16:46:31+02:00"
  }
}
```

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`contact_company_binding[]`](#parametr-contact_company_binding) | Root element of the response. Contains an array with information about the companies linked to the contact ||
|| **time**
[`time`][1] | Information about the execution time of the request ||
|#

### Parameter contact_company_binding

#|
|| **Name**
`type` | **Description** ||
|| **COMPANY_ID**
[`integer`][1] | Identifier of the company ||
|| **SORT**
[`integer`][1] | Sort index ||
|| **ROLE_ID**
[`integer`][1] | Identifier of the role (reserved) ||
|| **IS_PRIMARY**
[`boolean`][1] | Indicates whether the binding is primary
- `Y` - Yes
- `N` - No ||
|#


## Error Handling

HTTP status: **200**

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
|| `ACCESS_DENIED` | Access denied! | The user does not have permission to read contacts ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}


## Continue Learning

TODO

[1]: ../../../data-types.md