# Client on the Waitlist: Overview of Methods

You can add a client to the waitlist: either a contact or a company. The client will receive a notification when their entry is moved to a specific time.

> Quick navigation: [all methods](#all-methods) 

## Connection with Other Objects

**Waitlist.** To add or replace a client, use the `ID` of the waitlist entry in the `waitListId` parameter of the [booking.v1.waitlist.client.*](./index.md) methods. You can obtain the `ID` of the entry using the [creation](../booking-v1-waitlist-add.md) or [filtering](../booking-v1-waitlist-list.md) methods.

**Contact.** To attach a contact to the waitlist entry, pass the `ID` of the contact in the [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set) method. You can obtain the `ID` of the contact using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 3` parameter.

**Company.** To attach a company to the waitlist entry, pass the `ID` of the company in the [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set) method. You can obtain the `ID` of the company using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 4` parameter.

{% note info "" %}

If the client is new, first add them to the CRM using the [crm.item.add](../../../crm/universal/crm-item-add.md) method with the `entityTypeId = 3` for a contact or `entityTypeId = 4` for a company.

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can perform the method: any user

#|
|| [booking.v1.waitlist.client.list](./booking-v1-waitlist-client-list.md) | Returns the contact and company linked to the waitlist entry ||
|| [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md) | Adds a contact or company to the waitlist entry ||
|| [booking.v1.waitlist.client.unset](./booking-v1-waitlist-client-unset.md) | Removes a contact or company from the waitlist entry ||
|#