# Elements

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Since the elements of each process are stored in a separate table, the identifiers of elements from different processes will match.

Therefore, it is necessary to pass the process identifier `typeId` to all methods.

#|
|| **Method** | **Description** ||
|| [rpa.item.get](./rpa-item-get.md) | Returns information about the element with identifier id of the process with identifier typeId. ||
|| [rpa.item.list](./rpa-item-list.md) | The method will return an array of elements for the process with identifier typeId. ||
|| [rpa.item.add](./rpa-item-add.md) | The method creates a new element for the process with identifier typeId. ||
|| [rpa.item.update](./rpa-item-update.md) | The method updates the element with identifier id of the process with identifier typeId. ||
|| [rpa.item.delete](./rpa-item-delete.md) | The method will delete the element. ||
|| [rpa.item.getTasks](./rpa-item-get-tasks.md) | The method will return data about the current tasks of the element with identifier id of the process with identifier typeId. ||
|#