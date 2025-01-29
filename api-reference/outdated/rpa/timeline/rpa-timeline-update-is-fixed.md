# Update the Attachment Flag of the Record rpa.timeline.updateIsFixed

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the attachment flag of the record.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the record ||
|| **isFixed** 
[`string`](../../../data-types.md) | Attachment flag of the record. Possible values:
- `Y` — the record will be attached
- `N` — the record will not be attached ||
|#

## Response Handling

HTTP status: **200**

```json
{
    "timeline": {
        "id": 322,
        ...
    }
}
```

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-timeline-add.md)
- [{#T}](./rpa-timeline-update.md)
- [{#T}](./rpa-timeline-list-for-item.md)
- [{#T}](./rpa-timeline-delete.md)