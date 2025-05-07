# Booking: Overview of Methods

Booking methods allow you to:

- efficiently utilize resources by transferring clients between the waiting list and bookings
- synchronize schedules with external systems

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [How to schedule a client for a service](https://helpdesk.bitrix24.com/open/23851534/)

## Connection of Booking with Other Objects

**Waiting List.** Pass the `id` of the entry from the waiting list to the `waitListId` parameter of the [booking.v1.booking.createfromwaitlist](./booking-v1-booking-createfromwaitlist.md) method to transfer the client's entry to a booking at a specific time.

**Resource.** Pass the `id` of [resources](../resource/index.md) you want to book in the `resourceIds` parameter of the [booking.v1.booking.*](./index.md) methods. A single booking can include multiple resources with overlapping times. For example, you can book a driver service and a car simultaneously.

**Client.** You can attach a [contact](../../crm/contacts/index.md) or [company](../../crm/companies/index.md) from CRM to the booking. Pass the `id` of the contact or company in the [booking.v1.booking.client.*](./client/index.md) methods.

**Deal.** You can attach a [deal](../../crm/deals/index.md) from CRM to the booking. Pass the `id` of the deal in the [booking.v1.booking.externalData.*](./external-data/index.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [booking.v1.booking.add](./booking-v1-booking-add.md) | Adds a booking ||
|| [booking.v1.booking.createfromwaitlist](./booking-v1-booking-createfromwaitlist.md) | Creates a booking from the waiting list ||
|| [booking.v1.booking.delete](./booking-v1-booking-delete.md) | Deletes a booking ||
|| [booking.v1.booking.get](./booking-v1-booking-get.md) | Retrieves information about a booking ||
|| [booking.v1.booking.list](./booking-v1-booking-list.md) | Retrieves a list of bookings ||
|| [booking.v1.booking.update](./booking-v1-booking-update.md) | Updates a booking ||
|#