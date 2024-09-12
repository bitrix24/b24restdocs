# Get an array of processes with their fields rpa.type.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.type.list` will return an array of processes with their fields.

#|
|| **Parameter** | **Description** ||
|| **select** | Array of fields to output. By default, all fields are output. ||
|| **order** | List for sorting, where the key is the field and the value is ASC or DESC. ||
|| **filter** | List for filtering. ||
|| **start** | Offset for pagination. ||
|#

## Response in case of success

> 200 OK

```json
{
    "types": [
        {},
        {}
    ]
}
```