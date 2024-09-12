# Timeline Records

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

A set of methods for working with timeline records.

#|
|| **Method** | **Description** ||
|| [rpa.timeline.listForItem](./rpa-timeline-list-for-item.md) | This method returns an array of timeline records for the entity with the identifier itemId of the process with the identifier itemId, sorted in descending order by creation date (newest on top). ||
|| [rpa.timeline.updateIsFixed](./rpa-timeline-update-is-fixed.md) | This method updates the attachment flag of the record. ||
|| [rpa.timeline.add](./rpa-timeline-add.md) | This method will create a new timeline record for the entity with the identifier itemId of the process with the identifier typeId. ||
|| [rpa.timeline.update](./rpa-timeline-update.md) | This method updates the timeline record with the identifier id. ||
|| [rpa.timeline.delete](./rpa-timeline-delete.md) | This method deletes the timeline record with the identifier id. ||
|#