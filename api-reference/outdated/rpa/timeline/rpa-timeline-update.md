# Update Timeline Record rpa.timeline.update

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the timeline record with the identifier `id`.

The method only updates the `title` and `description` fields.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the record ||
|| **fields** 
[`array`](../../../data-types.md) | Fields of the record ||
|#

### Fields Parameter

#|
|| **Parameter** | **Description** ||
|| **title** | Title of the record ||
|| **description** | Description of the record. HTML tags can be used ||
|#

{% note warning %}

The method only updates records that were created by the same user and through the application.

{% endnote %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-timeline-add.md)
- [{#T}](./rpa-timeline-update-is-fixed.md)
- [{#T}](./rpa-timeline-list-for-item.md)
- [{#T}](./rpa-timeline-delete.md)