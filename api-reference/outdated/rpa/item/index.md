# Elements: Overview of Methods

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Since the elements of each workflow are stored in a separate table, the identifiers of elements from different workflows will match.

Therefore, it is necessary to pass the workflow identifier `typeId` to all methods.

#| 
|| **Method** | **Description** ||
|| [rpa.item.add](./rpa-item-add.md) | Adds a new element to the workflow with the identifier `typeId` ||
|| [rpa.item.update](./rpa-item-update.md) | Updates the element with the identifier `id` in the workflow with the identifier `typeId` ||
|| [rpa.item.get](./rpa-item-get.md) | Retrieves information about the element with the identifier `id` in the workflow with the identifier `typeId` ||
|| [rpa.item.getTasks](./rpa-item-get-tasks.md) | Retrieves data about the current tasks of the element with the identifier `id` in the workflow with the identifier `typeId` ||
|| [rpa.item.list](./rpa-item-list.md) | Retrieves a list of elements in the workflow with the identifier `typeId` ||
|| [rpa.item.delete](./rpa-item-delete.md) | Deletes the element || 
|#