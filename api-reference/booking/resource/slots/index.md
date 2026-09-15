# Slots: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Slots are time intervals during which a resource can be reserved.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Add a resource to Booking section](https://helpdesk.bitrix24.com/open/23848816/)

## How to Start

1. Retrieve the resource `id` using [booking.v1.resource.list](../booking-v1-resource-list.md)
2. Set the resource slots using [booking.v1.resource.slots.set](./booking-v1-resource-slots-set.md)
3. Check the slot settings using [booking.v1.resource.slots.list](./booking-v1-resource-slots-list.md)
4. Delete the slot settings using [booking.v1.resource.slots.unset](./booking-v1-resource-slots-unset.md) if the resource no longer needs to be bookable

## Relationship with Other Objects

**Resource.** To specify time intervals for booking, provide the `id` of the resource in the `resourceId` parameter of the [booking.v1.resource.slots.set](./booking-v1-resource-slots-set.md) method. Slots are retained within the resource itself, so changing them triggers the [onBookingResourceUpdate](../events/on-booking-resource-update.md) event.

**Booking.** Slots define the available time in the Bitrix24 interface and in the CRM booking form. The [booking.v1.booking.add](../../booking/booking-v1-booking-add.md) method does not validate the time against slots — a booking can be created outside a slot.

## Features of Slots

A slot has five mandatory settings: the availability period `from` and `to`, the booking duration `slotSize`, the time zone `timezone`, and the days of the week `weekDays`. The first three accept and return values in minutes, counted in the specified time zone.

Count the time from the start of the day, 0:00. To convert hours to minutes, use the formula `hours × 60 = minutes`. For example, 14:00 = 14 × 60 = 840 minutes.

- `from: 540` — time available for booking from 9:00

- `to: 1080` — time available for booking until 18:00

- `slotSize: 60` — booking duration is one hour

Slots repeat weekly on the days of the week listed in the `weekDays` field. A slot has no start or end date and no exceptions.

The [booking.v1.resource.slots.set](./booking-v1-resource-slots-set.md) method replaces the entire set of resource slots rather than adding a new slot to the existing ones.

A resource can have one or several intervals per day: morning and evening, for example. Configure each interval separately so that the client can choose only an available time slot.

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [booking.v1.resource.slots.set](./booking-v1-resource-slots-set.md) | Sets slots for the resource ||
|| [booking.v1.resource.slots.list](./booking-v1-resource-slots-list.md) | Retrieves slot settings for the resource ||
|| [booking.v1.resource.slots.unset](./booking-v1-resource-slots-unset.md) | Removes slots for the resource ||
|#
