# Client on the Waitlist: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Clients of a waitlist entry are CRM contacts and companies. For example, an application can link a service request to the client's contact and to the company the client represents.

> Quick navigation: [all methods](#all-methods)

## Relationships with Other Objects

**Waitlist.** Pass the entry `ID` in the `waitListId` parameter to [Set the List of Clients](./booking-v1-waitlist-client-set.md), [Retrieve It](./booking-v1-waitlist-client-list.md), or [Remove All Links](./booking-v1-waitlist-client-unset.md). You can retrieve the entry `ID` using [booking.v1.waitlist.add](../booking-v1-waitlist-add.md) or [booking.v1.waitlist.list](../booking-v1-waitlist-list.md).

**Contact.** In the `clients` array of the [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md) method, pass an object with the contact `id` and the type `type: {"module": "crm", "code": "CONTACT"}`. You can retrieve the contact `id` using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 3` parameter.

**Company.** In the `clients` array of the [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md) method, pass an object with the company `id` and the type `type: {"module": "crm", "code": "COMPANY"}`. You can retrieve the company `id` using the [crm.item.list](../../../crm/universal/crm-item-list.md) method with the `entityTypeId = 4` parameter.

{% note info "" %}

If the client is new, first add them to the CRM using the [crm.item.add](../../../crm/universal/crm-item-add.md) method with the `entityTypeId = 3` for a contact or `entityTypeId = 4` for a company.

{% endnote %}

## How to Start

1. Create a waitlist entry using the [booking.v1.waitlist.add](../booking-v1-waitlist-add.md) method or find an existing one using the [booking.v1.waitlist.list](../booking-v1-waitlist-list.md) method
2. Find the clients in the CRM using the [crm.item.list](../../../crm/universal/crm-item-list.md) method
3. Pass `waitListId` and the `clients` array to the [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md) method
4. Check the links using the [booking.v1.waitlist.client.list](./booking-v1-waitlist-client-list.md) method

{% note warning "" %}

The [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md) method replaces the entry's entire client list with the passed set. Previously linked clients that are not in the new list lose their link to the entry.

If the list is already empty and a [Deal](../external-data/index.md) is linked to the entry, the [booking.v1.waitlist.client.unset](./booking-v1-waitlist-client-unset.md) method and a [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md#empty-clients) call with `clients: []` link the contacts and company of that deal to the entry.

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can execute the methods: any user

#|
|| **Method** | **Description** ||
|| [booking.v1.waitlist.client.set](./booking-v1-waitlist-client-set.md) | Sets the list of clients for a waitlist entry ||
|| [booking.v1.waitlist.client.list](./booking-v1-waitlist-client-list.md) | Retrieves the list of clients for a waitlist entry ||
|| [booking.v1.waitlist.client.unset](./booking-v1-waitlist-client-unset.md) | Removes all client links from a waitlist entry ||
|#
