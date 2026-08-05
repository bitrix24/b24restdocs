# Slots: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Slots are time intervals during which a resource can be reserved.

> Quick navigation: [all methods](#all-methods)

## How to Start

1. Retrieve the resource `id` using [booking.v1.resource.list](../booking-v1-resource-list.md)
2. Set the resource slots using [booking.v1.resource.slots.set](./booking-v1-resource-slots-set.md)
3. Check the slot settings using [booking.v1.resource.slots.list](./booking-v1-resource-slots-list.md)
4. Delete the slot settings using [booking.v1.resource.slots.unset](./booking-v1-resource-slots-unset.md) if the resource no longer needs to be bookable

## Relationship with Other Objects

**Resource.** To specify time intervals for booking, provide the `id` of the resource in the `resourceId` parameter of the [booking.v1.resource.slots.set](./booking-v1-resource-slots-set.md) method.

## Features of Slots

Slots have three time parameters: the resource availability period `from` and `to`, and the booking duration `slotSize`. The time parameters accept and return values in minutes.

Calculate the start of the day from 0:00. To convert hours to minutes, use the formula `hours × 60 = minutes`. For example, 14:00 = 14 × 60 = 840 minutes.

- `from: 540` — time available for booking from 9:00

- `to: 1080` — time available for booking until 18:00

- `slotSize: 60` — booking duration is 1 hour

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
