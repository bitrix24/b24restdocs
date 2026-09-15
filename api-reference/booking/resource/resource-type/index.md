# Resource Types: Overview of Methods and Events

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Resource types are needed to categorize booking objects. For example, one type can group meeting rooms, while another can group company vehicles.

With resource types, you can:

- group similar objects
- configure notification templates for clients
- filter bookings

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Add a resource to Booking section](https://helpdesk.bitrix24.com/open/23848816/)

## How to Start

1. Create a resource type using [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md)
2. Retrieve the type `id` using [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md)
3. Pass the type `id` in the `typeId` parameter of [booking.v1.resource.add](../booking-v1-resource-add.md)

## Relationship with Other Objects

**Resource.** Use the `id` of the resource type in the `typeId` parameter of the [booking.v1.resource.*](../index.md) methods.

## Resource Type Code

Each type has a symbolic code `code` — external integrations use it to find the type without knowing its numeric identifier. The code is unique among the types of the `booking` module.

#|
|| **Code** | **Resource Type** ||
|| `doctor` | Doctor ||
|| `equipment` | Equipment ||
|| `expert` | Specialist ||
|| `car` | Vehicle ||
|| `room` | Room ||
|#

These types are created when the module is installed.

Other Bitrix24 modules also have resource types. To retrieve only your own, pass the `moduleId` filter with the value `booking` to [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md).

## Features of Working with Resource Types

You can create and modify resource types both through the Bitrix24 interface and using the [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) and [booking.v1.resourceType.update](./booking-v1-resourcetype-update.md) methods.

The code is mandatory when updating, and its value must differ from the codes of all existing types, including the code of the type being updated — see the details on the [booking.v1.resourceType.update](./booking-v1-resourcetype-update.md) page.

A type can only be deleted using the [booking.v1.resourceType.delete](./booking-v1-resourcetype-delete.md) method, and only if no resource is linked to it.

Notification configurations of a type are not transferred to resources: a resource receives its own default values when created.

## Overview of Methods and Events {#all-methods}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can perform the method: any user

### Resource Type

#|
|| **Method** | **Description** ||
|| [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) | Adds a new resource type ||
|| [booking.v1.resourceType.update](./booking-v1-resourcetype-update.md) | Updates a resource type ||
|| [booking.v1.resourceType.get](./booking-v1-resourcetype-get.md) | Retrieves a resource type ||
|| [booking.v1.resourceType.list](./booking-v1-resourcetype-list.md) | Retrieves a list of resource types ||
|| [booking.v1.resourceType.delete](./booking-v1-resourcetype-delete.md) | Deletes a resource type ||
|#

### Events

How to subscribe — see the [Resource Type Events](./events/index.md) section.

#|
|| **Event** | **Triggered** ||
|| [onBookingResourceTypeAdd](./events/on-booking-resource-type-add.md) | When a resource type is created manually or by the [booking.v1.resourceType.add](./booking-v1-resourcetype-add.md) method ||
|| [onBookingResourceTypeUpdate](./events/on-booking-resource-type-update.md) | When a resource type is updated manually or by the [booking.v1.resourceType.update](./booking-v1-resourcetype-update.md) method ||
|| [onBookingResourceTypeDelete](./events/on-booking-resource-type-delete.md) | When a resource type is deleted by the [booking.v1.resourceType.delete](./booking-v1-resourcetype-delete.md) method ||
|#
