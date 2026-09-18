# Booking: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A booking links a resource, time, and client in a single record. A resource can be a service, employee, room, or another object available for scheduling. You can create a booking for a selected time interval, link it to a contact or company, and additionally associate it with a CRM deal.

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [How to schedule a client for a service](https://helpdesk.bitrix24.com/open/23851534/)

## Getting Started

1. Retrieve or create a resource using the [booking.v1.resource.*](../resource/index.md) methods. You will need its identifier in the `resourceIds` parameter
2. Create a booking using [booking.v1.booking.add](./booking-v1-booking-add.md) and pass the resource identifiers and time interval. If the entry is already on the waiting list, use [booking.v1.booking.createfromwaitlist](./booking-v1-booking-createfromwaitlist.md)
3. Link a contact or company to the booking using the [booking.v1.booking.client.*](./client/index.md) methods
4. If necessary, associate the booking with a CRM deal using the [booking.v1.booking.externalData.*](./external-data/index.md) methods
5. Retrieve and update the booking using [booking.v1.booking.get](./booking-v1-booking-get.md), [booking.v1.booking.list](./booking-v1-booking-list.md), and [booking.v1.booking.update](./booking-v1-booking-update.md)

## Pagination

The [booking.v1.booking.list](./booking-v1-booking-list.md) method returns up to 50 bookings per call. To retrieve the next page, pass `50`, `100`, `150`, and so on in the `start` parameter.

## Connection of Booking with Other Objects

**Waiting List.** Pass the `id` of the entry from the waiting list to the `waitListId` parameter of the [booking.v1.booking.createfromwaitlist](./booking-v1-booking-createfromwaitlist.md) method to transfer the client's entry to a booking at a specific time.

**Resource.** Pass the `id` of [resources](../resource/index.md) you want to book in the `resourceIds` parameter of the [booking.v1.booking.*](#all-methods) methods. A single booking can include multiple resources with overlapping times. For example, you can book a driver service and a car simultaneously.

**Client.** You can attach a [contact](../../crm/contacts/index.md) or [company](../../crm/companies/index.md) from CRM to the booking. Pass the `id` of the contact or company in the [booking.v1.booking.client.*](./client/index.md) methods.

**Deal.** You can attach a [deal](../../crm/deals/index.md) from CRM to the booking. Pass the `id` of the deal in the [booking.v1.booking.externalData.*](./external-data/index.md) methods.

## Events

Events allow the application to respond when a booking is created, updated, or deleted. Learn how to subscribe to them in the [Booking Events](./events/index.md) section.

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
