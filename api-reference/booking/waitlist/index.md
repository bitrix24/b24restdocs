# Waitlist: Overview of Methods

Add clients to the waitlist when all booking slots are full. This helps:

- retain client contact information and their preferences
- promptly offer available times to clients when a booking is canceled

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [Bitrix24 Booking: put a client on the waiting list](https://helpdesk.bitrix24.com/open/24948238/)

## Connection of the Waitlist to Other Objects

**Booking.** Pass the `id` of the [booking](../booking/booking-v1-booking-list.md) to the `bookingId` parameter of the [booking.v1.waitlist.createfrombooking](./booking-v1-waitlist-createfrombooking.md) method to move a client from a booking to the waitlist.

**Client.** You can attach a [contact](../../crm/contacts/index.md) or [company](../../crm/companies/index.md) from CRM to a waitlist entry. Pass the `id` of the contact or company in the [booking.v1.waitlist.client.*](./client/index.md) methods.

**Deal.** You can attach a CRM deal to a waitlist entry. Pass the `id` of the deal in the [booking.v1.waitlist.externalData.*](./external-data/index.md) methods.

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [booking.v1.waitlist.add](./booking-v1-waitlist-add.md) | Adds an entry to the waitlist ||
|| [booking.v1.waitlist.createfrombooking](./booking-v1-waitlist-createfrombooking.md) | Creates a waitlist entry from a booking ||
|| [booking.v1.waitlist.delete](./booking-v1-waitlist-delete.md) | Deletes an entry from the waitlist ||
|| [booking.v1.waitlist.get](./booking-v1-waitlist-get.md) | Retrieves an entry from the waitlist ||
|| [booking.v1.waitlist.list](./booking-v1-waitlist-list.md) | Retrieves a list of entries from the waitlist ||
|| [booking.v1.waitlist.update](./booking-v1-waitlist-update.md) | Updates an entry in the waitlist ||
|#