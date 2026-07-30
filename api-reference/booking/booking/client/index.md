# Client in Booking: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can add a customer to a resource booking: either a contact or a company. Messages regarding the appointment, such as confirmations, reminders, and feedback requests, are sent to the customer's phone number.

> Quick navigation: [All Methods](#all-methods)
>
> User documentation: [Booking in CRM](https://helpdesk.bitrix24.com/open/24948162/)

## Connection with Other Objects

**Booking.** Use the `ID` of the booking in the `bookingId` parameter of the [booking.v1.booking.client.*](./index.md) methods to add or replace a customer. You can retrieve the `ID` of the booking using the [creation](../booking-v1-booking-add.md) or [filtering](../booking-v1-booking-list.md) methods.

**Contact.** To attach a contact to the booking, pass the `ID` of the contact in the [booking.v1.booking.client.set](./booking-v1-booking-client-set.md) method. You can retrieve the `ID` of the contact using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 3` parameter.

**Company.** To attach a company to the booking, pass the `ID` of the company in the [booking.v1.booking.client.set](./booking-v1-booking-client-set.md) method. You can retrieve the `ID` of the company using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 4` parameter.

{% note info "" %}

If the client is new, first add them to the CRM using the [crm.item.add](../../../crm/universal/crm-item-add.md) method with the `entityTypeId = 3` for a contact or `entityTypeId = 4` for a company.

{% endnote %}

## Getting Started

1. Retrieve the `ID` of the booking using the [booking.v1.booking.add](../booking-v1-booking-add.md) or [booking.v1.booking.list](../booking-v1-booking-list.md) method
2. Find a contact or company in the CRM using the [crm.item.list](../../../crm/universal/crm-item-list.md) method
3. If the customer does not exist in the CRM, create a contact or company using the [crm.item.add](../../../crm/universal/crm-item-add.md) method
4. Pass the `ID` of the booking and the customer to the [booking.v1.booking.client.set](./booking-v1-booking-client-set.md) method
5. Verify the link using the [booking.v1.booking.client.list](./booking-v1-booking-client-list.md) method
6. If necessary, remove the customer from the booking using the [booking.v1.booking.client.unset](./booking-v1-booking-client-unset.md) method

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| [booking.v1.booking.client.list](./booking-v1-booking-client-list.md) | Returns the contact and company associated with the booking ||
|| [booking.v1.booking.client.set](./booking-v1-booking-client-set.md) | Adds a contact or company to the booking ||
|| [booking.v1.booking.client.unset](./booking-v1-booking-client-unset.md) | Removes a contact or company from the booking ||
|#
