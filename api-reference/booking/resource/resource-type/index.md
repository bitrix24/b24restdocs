# Resource Types: Overview of Methods

Resource types are needed for categorizing booking objects. For example, the type Meeting Rooms encompasses rooms for meetings, while the type Chery includes cars of a specific brand.

With resource types, you can:
- group similar objects,
- configure notification templates for clients,
- filter bookings.

> Quick navigation: [all methods](#all-methods)

## Connection of Resource Types with Other Objects

**Resource.** Use the `id` of the resource type in the `typeId` parameter of the [booking.v1.resource.*](../index.md) methods.

## Features of Working with Resource Types

You can create resource types through the Bitrix24 interface or by using the [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) method. To modify or delete, use only the [booking.v1.resourceType.update](./booking-v1-resourcetype-update.md) and [booking.v1.resourceType.delete](./booking-v1-resourcetype-delete.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can perform the method: any user

#|
|| [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) | Adds a new resource type ||
|| [booking.v1.resourceType.update](./booking-v1-resourcetype-update.md) | Updates a resource type ||
|| [booking.v1.resourceType.get](./booking-v1-resourcetype-get.md) | Retrieves a resource type ||
|| [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md) | Retrieves a list of resource types ||
|| [booking.v1.resourceType.delete](./booking-v1-resourcetype-delete.md) | Deletes a resource type ||
|#