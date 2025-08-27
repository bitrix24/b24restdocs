# Client in Booking: Overview of Methods

You can add a client to a resource booking: either a contact or a company. Messages regarding the appointment, such as confirmations, reminders, and feedback requests, are sent to the client's phone number.

> Quick navigation: [all methods](#all-methods)

## Connection with Other Objects

**Booking.** Use the `ID` of the booking in the `bookingId` parameter of the [booking.v1.booking.client.*](./index.md) methods to add or replace a client. You can obtain the `ID` of the booking using the [creation](../booking-v1-booking-add.md) or [filtering](../booking-v1-booking-list.md) methods.

**Contact.** To attach a contact to the booking, pass the `ID` of the contact in the [booking.v1.booking.client.set](./booking-v1-booking-client-set) method. You can obtain the `ID` of the contact using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 3` parameter.

**Company.** To attach a company to the booking, pass the `ID` of the company in the [booking.v1.booking.client.set](./booking-v1-booking-client-set) method. You can obtain the `ID` of the company using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 4` parameter.

{% note info "" %}

If the client is new, first add them to the CRM using the [crm.item.add](../../../crm/universal/crm-item-add.md) method with the `entityTypeId = 3` for a contact or `entityTypeId = 4` for a company.

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can perform the method: any user

#|
|| [booking.v1.booking.client.list](./booking-v1-booking-client-list.md) | Returns the contact and company associated with the booking ||
|| [booking.v1.booking.client.set](./booking-v1-booking-client-set.md) | Adds a contact or company to the booking ||
|| [booking.v1.booking.client.unset](./booking-v1-booking-client-unset.md) | Removes a contact or company from the booking ||
|#