# Retrieve a List of Processes with Their Fields rpa.type.list

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative to this functionality.

{% endnote %}

The method will return an array of processes with their fields.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../../data-types.md) | Array of fields to output. By default, all fields are returned. ||
|| **order**
[`object`](../../../data-types.md) | List for sorting, where the key is the field and the value is `ASC` or `DESC`. ||
|| **filter**
[`object`](../../../data-types.md) | List for filtering. ||
|| **start**
[`integer`](../../../data-types.md) | This parameter is used to manage pagination.

The page size for results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` is the desired page number.    ||
|#

## Response Handling

HTTP Status: **200**

```json
{
    "types": [
        {},
        {}
    ]
}
```

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-type-add.md)
- [{#T}](./rpa-type-update.md)
- [{#T}](./rpa-type-get.md)
- [{#T}](./rpa-type-delete.md)