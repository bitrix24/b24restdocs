# Resources: Overview of Methods

Resources are objects that can be reserved: rooms, equipment, services, and so on.

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [Add a resource to Booking section](https://helpdesk.bitrix24.com/open/23848816/)

## Connection of Resources with Other Objects

**Reservation.** Pass the `id` of the resources in the `resourceIds` parameter of the [booking.v1.booking.*](../booking/index.md) methods. A single reservation can include multiple resources.

## Additional Settings for Resources

**Resource Type.** To create, modify, delete, or find a resource type, use the methods from the [booking.v1.resourceType.*](./resource-type/index.md) group. Additionally, these methods allow you to configure activity and message templates. These settings are applied by default when creating a resource of the specified type.

**Slots.** To configure the times when a resource is available for booking, use the methods from the [booking.v1.resource.slots.*](./slots/index.md) group. For example, you can set the resource's availability only on Tuesdays and Thursdays from 11:00 AM to 3:00 PM.

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can perform the method: any user

#|
|| **Method** | **Description** ||
|| [booking.v1.resource.add](./booking-v1-resource-add.md) | Adds a new resource ||
|| [booking.v1.resource.delete](./booking-v1-resource-delete.md) | Deletes a resource ||
|| [booking.v1.resource.get](./booking-v1-resource-get.md) | Retrieves a resource ||
|| [booking.v1.resource.list](./booking-v1-resource-list.md) | Retrieves a list of resources ||
|| [booking.v1.resource.update](./booking-v1-resource-update.md) | Updates a resource ||
|#