# Get Fields for Contact-Company crm.contact.company.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.contact.company.fields` returns the description of fields for the contact-company relationship.


## Method Parameters

No parameters


## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Retrieving the list of fields for the contact-company relationship

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
            'crm.contact.company.fields',
            {},
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
	"result": {
		"SORT": {
			"type": "integer",
			"isRequired": false,
			"isReadOnly": false,
			"isImmutable": false,
			"isMultiple": false,
			"isDynamic": false,
			"title": "Sorting"
		},
		"IS_PRIMARY": {
			"type": "char",
			"isRequired": false,
			"isReadOnly": false,
			"isImmutable": false,
			"isMultiple": false,
			"isDynamic": false,
			"title": "Primary"
		},
		"COMPANY_ID": {
			"type": "integer",
			"isRequired": true,
			"isReadOnly": false,
			"isImmutable": false,
			"isMultiple": false,
			"isDynamic": false,
			"title": "Company"
		}
	},
	"time": {
		"start": 1724065480.986461,
		"finish": 1724065481.321185,
		"duration": 0.33472418785095215,
		"processing": 0.01616501808166504,
		"date_start": "2024-08-19T13:04:40+02:00",
		"date_finish": "2024-08-19T13:04:41+02:00"
	}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`][1] | An object in the format:
```
{
    field_1: value_1,
    field_2: value_2,
    ...
    field_n: value_n,
}
```

where:
- `field_n` — field of the entity
- `value_n` — information about the field in the format [`crm_rest_field_description`](../../data-types.md#crm_rest_field_description) ||
|| **time**
[`time`][1]   | Information about the request execution time ||
|#


## Error Handling

The method does not return errors

{% include [system errors](../../../../_includes/system-errors.md) %}


## Continue Learning

TODO

[1]: ../../data-types.md