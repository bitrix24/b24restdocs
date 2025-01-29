# Update Timeline Record rpa.timeline.update

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the timeline record with the identifier `id`.

The method only updates the `title` and `description` fields, and only those records that were created by the same user and through the application.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the record ||
|| **fields** 
[`object`](../../../data-types.md) | Object with [fields](#fields) of the record ||
|#

### Fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title** 
[`string`](../../../data-types.md) | Title of the record ||
|| **description** 
[`string`](../../../data-types.md) | Description of the record. HTML tags can be used ||
|#

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-timeline-add.md)
- [{#T}](./rpa-timeline-update-is-fixed.md)
- [{#T}](./rpa-timeline-list-for-item.md)
- [{#T}](./rpa-timeline-delete.md)