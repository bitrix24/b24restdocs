# Overview of Methods and Events

## Methods

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute methods: administrator

#|
|| **Method** | **Description** ||
|| [catalog.measure.add](./catalog-measure-add.md) | Adds a unit of measurement ||
|| [catalog.measure.update](./catalog-measure-update.md) | Updates a unit of measurement ||
|| [catalog.measure.get](./catalog-measure-get.md) | Returns information about a unit of measurement by its identifier ||
|| [catalog.measure.list](./catalog-measure-list.md) | Returns a list of units of measurement ||
|| [catalog.measure.delete](./catalog-measure-delete.md) | Deletes a unit of measurement ||
|| [catalog.measure.getFields](./catalog-measure-get-fields.md) | Returns available fields for units of measurement ||
|#

## Events

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can subscribe: any user

#|
|| **Event** | **Triggered** ||
|| [CATALOG.MEASURE.ON.ADD](../events/catalog-measure-on-add.md) | When a unit of measurement is added ||
|| [CATALOG.MEASURE.ON.UPDATE](../events/catalog-measure-on-update.md)| When a unit of measurement is updated ||
|| [CATALOG.MEASURE.ON.DELETE](../events/catalog-measure-on-delete.md)| When a unit of measurement is deleted ||
|#