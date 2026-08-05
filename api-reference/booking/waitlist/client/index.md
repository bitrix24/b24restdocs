# Client on the Waitlist: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can add a client to the waitlist: either a contact or a company. The client will receive a notification when their entry is moved to a specific time.

> Quick navigation: [all methods](#all-methods)

## How to Start

1. Create a waitlist entry using the [booking.v1.waitlist.*](../index.md) methods
2. Retrieve the CRM contact or company `ID`
3. Add a client using [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md)
4. Check the linked client information using [booking.v1.waitlist.client.list](./booking-v1-waitlist-client-list.md)

## Relationships with Other Objects

**Waitlist.** To add or replace a client, use the `ID` of the waitlist entry in the `waitListId` parameter of the [booking.v1.waitlist.client.*](./index.md) methods. You can obtain the entry `ID` using [booking.v1.waitlist.add](../booking-v1-waitlist-add.md) or [booking.v1.waitlist.list](../booking-v1-waitlist-list.md).

**Contact.** To attach a contact to the waitlist entry, pass the contact `ID` to the [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md) method. You can obtain the contact `ID` using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 3` parameter.

**Company.** To attach a company to the waitlist entry, pass the company `ID` to the [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md) method. You can obtain the company `ID` using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 4` parameter.

{% note info "" %}

If the client is new, first add them to the CRM using the [crm.item.add](../../../crm/universal/crm-item-add.md) method with the `entityTypeId = 3` for a contact or `entityTypeId = 4` for a company.

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md) | Adds a contact or company to the waitlist entry ||
|| [booking.v1.waitlist.client.list](./booking-v1-waitlist-client-list.md) | Returns the contact and company linked to the waitlist entry ||
|| [booking.v1.waitlist.client.unset](./booking-v1-waitlist-client-unset.md) | Removes a contact or company from the waitlist entry ||
|#
