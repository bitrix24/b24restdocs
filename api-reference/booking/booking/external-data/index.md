# Linking Objects to Booking: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Additional objects can be linked to bookings. This helps synchronize sales and utilize automation, such as Automation rules in a deal.

> Quick navigation: [all methods](#all-methods)

## How to Start

1. Obtain the booking `ID` using [booking.v1.booking.add](../booking-v1-booking-add.md) or [booking.v1.booking.list](../booking-v1-booking-list.md)
2. Obtain the deal `ID` using [crm.deal.add](../../../crm/deals/crm-deal-add.md) or [crm.deal.list](../../../crm/deals/crm-deal-list.md)
3. Set the connection using [booking.v1.booking.externalData.set](./booking-v1-booking-externaldata-set.md). Pass the complete set of required connections: the booking `ID` in `bookingId`, the deal `ID` in `value`, and the fixed values `moduleId = crm` and `entityTypeId = DEAL`
4. Check the connection using [booking.v1.booking.externalData.list](./booking-v1-booking-externaldata-list.md)

## Connection with Other Objects

**Booking.** To create a new link for a booking, specify the `ID` of the booking in the `bookingId` parameter. You can obtain the `ID` using the [creation](../booking-v1-booking-add.md) or [filtering](../booking-v1-booking-list.md) methods.

**Deal.** To create a connection with a deal, pass the deal `ID` in the `value` parameter. You can obtain the `ID` using the [creation](../../../crm/deals/crm-deal-add.md) or [filtering](../../../crm/deals/crm-deal-list.md) methods. The linked object type is specified by the `moduleId` and `entityTypeId` parameters.

{% note warning "" %}

The method `booking.v1.booking.externalData.set` replaces the entire current set of connections. To retain existing connections, first retrieve them using `booking.v1.booking.externalData.list` and pass them together with the new ones.

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`booking`](../../../scopes/permissions.md)
>
> Who can perform the methods: any user

#|
|| **Method** | **Description** ||
|| [booking.v1.booking.externalData.list](./booking-v1-booking-externaldata-list.md) | Retrieves all booking connections ||
|| [booking.v1.booking.externalData.set](./booking-v1-booking-externaldata-set.md) | Replaces the set of booking connections ||
|| [booking.v1.booking.externalData.unset](./booking-v1-booking-externaldata-unset.md) | Removes all booking connections ||
|#
