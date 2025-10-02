# Linking Objects to Booking: Overview of Methods

Additional objects can be linked to bookings. This helps synchronize sales and utilize automation, such as Automation rules in a deal.

> Quick navigation: [all methods](#all-methods)

## Connection with Objects

**Booking.** To create a new link for a booking, specify the `ID` of the booking in the `bookingId` parameter. You can obtain the `ID` using the [creation](../booking-v1-booking-add.md) or [filtering](../booking-v1-booking-list.md) methods.

{% note info "" %}

Currently, only CRM deals can be linked to bookings. Other modules and types of objects are not supported yet.

{% endnote %}

**Deal.** To create a link with a deal, pass the `ID` of the deal in the `value` parameter. You can obtain the `ID` using the [creation](../../../crm/deals/crm-deal-add.md) or [filtering](../../../crm/deals/crm-deal-list.md) methods. Use fixed values for the parameters `moduleId = crm` and `entityTypeId = DEAL`.

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions)
>
> Who can perform the methods: any user

#|
|| **Method** | **Description** ||
|| [booking.v1.booking.externalData.list](./booking-v1-booking-externaldata-list.md) | Retrieves booking links ||
|| [booking.v1.booking.externalData.set](./booking-v1-booking-externaldata-set.md) | Sets links for booking ||
|| [booking.v1.booking.externalData.unset](./booking-v1-booking-externaldata-unset.md) | Removes links for booking ||
|#