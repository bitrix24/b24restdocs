# Get a List of Processes with Their Fields rpa.type.list

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method will return an array of processes with their fields.

#|
|| **Name**
`type` | **Description** ||
|| **select** | Array of fields to output. By default, all fields are outputted ||
|| **order** | List for sorting, where the key is the field and the value is `ASC` or `DESC` ||
|| **filter** | List for filtering ||
|| **start** | Offset for pagination ||
|#

## Handling the Response

HTTP status: **200**

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