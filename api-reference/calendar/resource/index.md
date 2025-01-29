# Resource Booking: Overview of Methods

Resource booking is the ability to provide services for a specific time. A resource is something that clients book, such as a car, a meeting room, or a bowling lane.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [How to use resource booking option](https://helpdesk.bitrix24.com/open/15375256/)

## How Resource Booking Works

In Bitrix24, resource booking is done through a custom CRM field of type `resourcebooking`. This field can be created in the detail forms of leads and deals.

Resource availability can be tracked in the CRM Calendar. Technically, a resource is a section of the calendar, and booking is a calendar event.

The method [calendar.resource.add](./calendar-resource-add.md) adds a new resource. The resource will only appear in the system. It can be linked to a custom field of a CRM entity only through the detail form of the entity in edit mode.

Resource bookings can be retrieved using the method [calendar.resource.booking.list](./calendar-resource-booking-list.md). A booking has a set of fields similar to the fields of a calendar event. Some fields remain empty as they are not relevant for booking.

{% note tip "User Documentation" %}

- [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

{% endnote %}

## Linking Resources to Other Entities

**User.** A resource is linked to a user through the identifier in the `CREATED_BY` field. The system automatically sets the identifier of the user who created the resource.

**Custom CRM Fields.** Resource bookings in deals and leads are linked to custom fields of type `resourcebooking`. Identifiers of bookings can be obtained through:
- universal methods — [crm.item.get](../../crm/universal/crm-item-get.md), [crm.item.list](../../crm/universal/crm-item-list.md)
- methods for leads — [crm.lead.get](../../crm/leads/crm-lead-get.md), [crm.lead.list](../../crm/leads/crm-lead-list.md)
- methods for deals — [crm.deal.get](../../crm/deals/crm-deal-get.md), [crm.deal.list](../../crm/deals/crm-deal-list.md) 

To find out which custom fields have the type resourcebooking, you can use the method to get the list of fields:
- for leads — [crm.lead.userfield.list](../../crm/leads/userfield/crm-lead-userfield-list.md) 
- for deals — [crm.deal.userfield.list](../../crm/deals/user-defined-fields/crm-deal-userfield-list.md)

## Overview of Methods {#all-methods}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#| 
|| **Method** | **Description** ||
|| [calendar.resource.add](./calendar-resource-add.md) | Add a resource ||
|| [calendar.resource.update](./calendar-resource-update.md) | Update a resource ||
|| [calendar.resource.list](./calendar-resource-list.md) | Get a list of resources ||
|| [calendar.resource.booking.list](./calendar-resource-booking-list.md) | Get resource bookings by filter ||
|| [calendar.resource.delete](./calendar-resource-delete.md) | Delete a resource || 
|#