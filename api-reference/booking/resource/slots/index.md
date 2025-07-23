# Slots: Overview of Methods

Slots are time intervals during which a resource can be reserved.

> Quick navigation: [all methods](#all-methods) 

## Connection of Slots with Other Objects

**Resource.** To specify time intervals for booking, provide the `id` of the resource in the `resourceId` parameter of the [booking.v1.resource.slots.set](./booking-v1-resource-slots-set.md) method.

## Features of Slots

Slots have three time parameters: the availability period of the resource `from` and `to`, and the duration of the booking `slotSize`. The time parameters accept and return values in minutes. Calculate time from the start of the day at 0:00. To convert hours to minutes, use the formula `hours × 60 = minutes`, for example, 14:00 = 14 × 60 = 840 minutes:

- `from: 540` — booking time is available from 9:00,

- `to: 1080` — booking time is available until 18:00,

- `slotSize: 60` — duration of the booking is 1 hour.

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