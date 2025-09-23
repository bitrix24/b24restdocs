# Online Booking: Overview of Methods

Online booking is a tool for automating the reservation of resources: rooms, equipment, specialist services, and more. You can move clients between the waitlist and bookings, manage occupancy, and synchronize resources with external systems.

> Quick navigation: [all methods](#all-methods) 
> 
> User documentation: [Booking: Getting Started](https://helpdesk.bitrix24.com/open/23873394/)

Online booking has three groups of methods: resources, waitlist, and booking. The methods are interconnected:

- resources define what can be booked
- the waitlist handles overload
- booking methods record occupancy

{% note info "" %}

In the on-premise Bitrix24, methods can be used starting from the [module version](../../settings/cloud-and-on-premise/on-premise/versions.md) `booking 25.300.0`.

{% endnote %}

## Setting Up Resources

Resources are objects that can be booked: rooms, equipment, services. Methods in this group allow you to:

- create, modify, and delete resources — [booking.v1.resource.*](./resource/index.md)
- configure resource types, such as "room," "car," "specialist" — [booking.v1.resourceType.*](./resource/resource-type/index.md)
- set resource availability by time — [booking.v1.resource.slots.*](./resource/slots/index.md)

## Working with the Waitlist

Use the waitlist when the desired resource is temporarily unavailable. Methods in this group allow you to:

- add, modify, and delete entries in the waitlist — [booking.v1.waitlist.*](./waitlist/index.md)
- move an entry from booking to the waitlist — [booking.v1.waitlist.createfrombooking](./waitlist/booking-v1-waitlist-createfrombooking.md)
- manage relationships between waitlist entries and CRM clients — [booking.v1.waitlist.client.*](./waitlist/client/index.md) and other objects — [booking.v1.waitList.externalData.*](./waitlist/external-data/index.md)

## Booking Resources

Booking is a confirmed reservation for a resource. Methods in this group allow you to:

- create, modify, and cancel bookings — [booking.v1.booking.*](./booking/index.md)
- create a booking from an entry in the waitlist — [booking.v1.booking.createfromwaitlist](./booking/booking-v1-booking-createfromwaitlist.md)
- manage relationships between bookings and CRM clients — [booking.v1.booking.client.*](./booking/client/index.md) and other objects — [booking.v1.booking.externalData.*](./booking/external-data/index.md)

## Overview of Methods {#all-methods}

> Scope: [`booking`](../scopes/permissions.md)
>
> Who can execute the method: any user

### Resource

#|
|| **Method** | **Description** ||
|| [booking.v1.resource.add](./resource/booking-v1-resource-add.md) | Adds a new resource ||
|| [booking.v1.resource.delete](./resource/booking-v1-resource-delete.md) | Deletes a resource ||
|| [booking.v1.resource.get](./resource/booking-v1-resource-get.md) | Retrieves a resource ||
|| [booking.v1.resource.list](./resource/booking-v1-resource-list.md) | Retrieves a list of resources ||
|| [booking.v1.resource.update](./resource/booking-v1-resource-update.md) | Updates a resource ||
|#

#### Resource Type

#|
|| **Method** | **Description** ||
|| [booking.v1.resourceType.add](./resource/resource-type/booking-v1-resourcetype-add.md) | Adds a new resource type ||
|| [booking.v1.resourceType.delete](./resource/resource-type/booking-v1-resourcetype-delete.md) | Deletes a resource type ||
|| [booking.v1.resourceType.get](./resource/resource-type/booking-v1-resourcetype-get.md) | Retrieves a resource type ||
|| [booking.v1.resourceType.list](./resource/resource-type/booking-v1-resourcetype-list.md) | Retrieves a list of resource types ||
|| [booking.v1.resourceType.update](./resource/resource-type/booking-v1-resourcetype-update.md) | Updates a resource type ||
|#

#### Time Slots for Resources

#|
|| **Method** | **Description** ||
|| [booking.v1.resource.slots.list](./resource/slots/booking-v1-resource-slots-list.md) | Retrieves the slot settings for a resource ||
|| [booking.v1.resource.slots.set](./resource/slots/booking-v1-resource-slots-set.md) | Sets slots for a resource ||
|| [booking.v1.resource.slots.unset](./resource/slots/booking-v1-resource-slots-unset.md) | Removes slots for a resource ||
|#

### Waitlist

#|
|| **Method** | **Description** ||
|| [booking.v1.waitlist.add](./waitlist/booking-v1-waitlist-add.md) | Adds to the waitlist ||
|| [booking.v1.waitlist.createfrombooking](./waitlist/booking-v1-waitlist-createfrombooking.md) | Creates an entry in the waitlist from a booking ||
|| [booking.v1.waitlist.delete](./waitlist/booking-v1-waitlist-delete.md) | Deletes an entry from the waitlist ||
|| [booking.v1.waitlist.get](./waitlist/booking-v1-waitlist-get.md) | Retrieves an entry from the waitlist ||
|| [booking.v1.waitlist.list](./waitlist/booking-v1-waitlist-list.md) | Retrieves a list of entries from the waitlist ||
|| [booking.v1.waitlist.update](./waitlist/booking-v1-waitlist-update.md) | Updates an entry in the waitlist ||
|#

#### Client in the Waitlist

#|
|| **Method** | **Description** ||
|| [booking.v1.waitlist.client.list](./waitlist/client/booking-v1-waitlist-client-list.md) | Retrieves a list of clients in the waitlist entry ||
|| [booking.v1.waitlist.client.set](./waitlist/client/booking-v1-waitlist-client-set.md) | Adds clients to the waitlist entry ||
|| [booking.v1.waitlist.client.unset](./waitlist/client/booking-v1-waitlist-client-unset.md) | Removes clients from the waitlist entry ||
|#

#### Waitlist Relationships with Additional Objects

#|
|| **Method** | **Description** ||
|| [booking.v1.waitList.externalData.list](./waitlist/external-data/booking-v1-waitlist-externaldata-list.md) | Retrieves relationships for the waitlist entry ||
|| [booking.v1.waitlist.externalData.set](./waitlist/external-data/booking-v1-waitlist-externaldata-set.md) | Sets relationships for the waitlist entry ||
|| [booking.v1.waitlist.externalData.unset](./waitlist/external-data/booking-v1-waitlist-externaldata-unset.md) | Removes relationships for the waitlist entry ||
|#

### Booking

#|
|| **Method** | **Description** ||
|| [booking.v1.booking.add](./booking/booking-v1-booking-add.md) | Adds a booking ||
|| [booking.v1.booking.createfromwaitlist](./booking/booking-v1-booking-createfromwaitlist.md) | Creates a booking from the waitlist ||
|| [booking.v1.booking.delete](./booking/booking-v1-booking-delete.md) | Deletes a booking ||
|| [booking.v1.booking.get](./booking/booking-v1-booking-get.md) | Retrieves booking information ||
|| [booking.v1.booking.list](./booking/booking-v1-booking-list.md) | Retrieves a list of bookings ||
|| [booking.v1.booking.update](./booking/booking-v1-booking-update.md) | Updates a booking ||
|#

#### Client in Booking

#|
|| **Method** | **Description** ||
|| [booking.v1.booking.client.list](./booking/client/booking-v1-booking-client-list.md) | Retrieves a list of clients in the booking ||
|| [booking.v1.booking.client.set](./booking/client/booking-v1-booking-client-set.md) | Adds a client to the booking ||
|| [booking.v1.booking.client.unset](./booking/client/booking-v1-booking-client-unset.md) | Removes clients from the booking ||
|#

#### Booking Relationships with Additional Objects

#|
|| **Method** | **Description** ||
|| [booking.v1.booking.externalData.list](./booking/external-data/booking-v1-booking-externaldata-list.md) | Retrieves relationships for the booking ||
|| [booking.v1.booking.externalData.set](./booking/external-data/booking-v1-booking-externaldata-set.md) | Sets relationships for the booking ||
|| [booking.v1.booking.externalData.unset](./booking/external-data/booking-v1-booking-externaldata-unset.md) | Removes relationships for the booking ||
|#

#### Auxiliary Methods

#|
|| **Method** | **Description** ||
|| [booking.v1.clienttype.list](./booking-v1-clienttype-list.md) | Retrieves a list of client types ||
|#