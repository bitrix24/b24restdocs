# Resources: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Resources are objects that can be reserved: rooms, equipment, or services. The methods of this section create, modify, find, and delete resources, while the available time and types are configured in the child sections.

{% note info "" %}

The methods only work when the Booking tool is enabled in the Bitrix24 settings. Otherwise, any method of this section returns the error `Booking tool is disabled. Please contact your administrator.`

{% endnote %}

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Add a resource to Booking section](https://helpdesk.bitrix24.com/open/23848816/)

## How to Start

1. Create or retrieve a resource type using [booking.v1.resourceType.*](./resource-type/index.md)
2. Create a resource using [booking.v1.resource.add](./booking-v1-resource-add.md)
3. Configure available time using [booking.v1.resource.slots.*](./slots/index.md)
4. Pass the resource `id` in the `resourceIds` parameter of the [booking.v1.booking.*](../booking/index.md) methods so that the resource is included in a booking

## Relationship with Other Objects

**Reservation.** Pass the `id` of the resources in the `resourceIds` parameter of the [booking.v1.booking.*](../booking/index.md) methods. A single reservation can include multiple resources.

**Resource Type.** A type has its own notification configurations, but a resource does not inherit them: a resource receives its own default values when created. Set the notification configurations directly in [booking.v1.resource.add](./booking-v1-resource-add.md).

**Slots.** Slots define the times when a resource is available for booking: for example, only on Tuesdays and Thursdays from 11:00 AM to 3:00 PM. Slots are retained within the resource itself, so changing them triggers the [onBookingResourceUpdate](./events/on-booking-resource-update.md) event, just as updating the resource does.

## Overview of Methods and Events {#all-methods}

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can perform the method: any user

### Resource

#|
|| **Method** | **Description** ||
|| [booking.v1.resource.add](./booking-v1-resource-add.md) | Adds a new resource ||
|| [booking.v1.resource.update](./booking-v1-resource-update.md) | Updates a resource ||
|| [booking.v1.resource.get](./booking-v1-resource-get.md) | Retrieves a resource ||
|| [booking.v1.resource.list](./booking-v1-resource-list.md) | Retrieves a list of resources ||
|| [booking.v1.resource.delete](./booking-v1-resource-delete.md) | Deletes a resource ||
|#

### Resource Type

See the [Resource Types](./resource-type/index.md) section for details.

#|
|| **Method** | **Description** ||
|| [booking.v1.resourceType.add](./resource-type/booking-v1-resourcetype-add.md) | Adds a new resource type ||
|| [booking.v1.resourceType.update](./resource-type/booking-v1-resourcetype-update.md) | Updates a resource type ||
|| [booking.v1.resourceType.get](./resource-type/booking-v1-resourcetype-get.md) | Retrieves a resource type ||
|| [booking.v1.resourceType.list](./resource-type/booking-v1-resourcetype-list.md) | Retrieves a list of resource types ||
|| [booking.v1.resourceType.delete](./resource-type/booking-v1-resourcetype-delete.md) | Deletes a resource type ||
|#

### Slots

See the [Slots](./slots/index.md) section for details.

#|
|| **Method** | **Description** ||
|| [booking.v1.resource.slots.set](./slots/booking-v1-resource-slots-set.md) | Sets slots for the resource ||
|| [booking.v1.resource.slots.list](./slots/booking-v1-resource-slots-list.md) | Retrieves slot settings for the resource ||
|| [booking.v1.resource.slots.unset](./slots/booking-v1-resource-slots-unset.md) | Removes slots for the resource ||
|#

### Events

How to subscribe to events — see the [Resource Events](./events/index.md) and [Resource Type Events](./resource-type/events/index.md) sections.

#|
|| **Event** | **Triggered** ||
|| [onBookingResourceAdd](./events/on-booking-resource-add.md) | When a resource is created manually or by the [booking.v1.resource.add](./booking-v1-resource-add.md) method ||
|| [onBookingResourceUpdate](./events/on-booking-resource-update.md) | When a resource is updated manually or by the [booking.v1.resource.update](./booking-v1-resource-update.md), [booking.v1.resource.slots.set](./slots/booking-v1-resource-slots-set.md), [booking.v1.resource.slots.unset](./slots/booking-v1-resource-slots-unset.md) methods ||
|| [onBookingResourceDelete](./events/on-booking-resource-delete.md) | When a resource is deleted manually or by the [booking.v1.resource.delete](./booking-v1-resource-delete.md) method ||
|| [onBookingResourceTypeAdd](./resource-type/events/on-booking-resource-type-add.md) | When a resource type is created manually or by the [booking.v1.resourceType.add](./resource-type/booking-v1-resourcetype-add.md) method ||
|| [onBookingResourceTypeUpdate](./resource-type/events/on-booking-resource-type-update.md) | When a resource type is updated manually or by the [booking.v1.resourceType.update](./resource-type/booking-v1-resourcetype-update.md) method ||
|| [onBookingResourceTypeDelete](./resource-type/events/on-booking-resource-type-delete.md) | When a resource type is deleted by the [booking.v1.resourceType.delete](./resource-type/booking-v1-resourcetype-delete.md) method ||
|#
