# Delete Timeline Entry rpa.timeline.delete

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method deletes a timeline entry with the identifier `id`.

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`number`](../../../data-types.md) | Identifier of the entry ||
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}

{% note warning %}

The method only deletes entries that were created by the same user and through the application.

{% endnote %}

## Continue Exploring 

- [{#T}](./index.md)
- [{#T}](./rpa-timeline-add.md)
- [{#T}](./rpa-timeline-update.md)
- [{#T}](./rpa-timeline-update-is-fixed.md)
- [{#T}](./rpa-timeline-list-for-item.md)