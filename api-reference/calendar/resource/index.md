# Resource Booking: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

Resource booking lets you schedule clients for a service at a specific time. A resource is something that clients book, such as a car, a specialist's office, or a bowling lane. Calendar meeting rooms are booked differently: through the `location` parameter of the [calendar.event.*](../calendar-event/index.md) methods.

Bitrix24 also has [Online Booking](../../booking/index.md), a separate tool with its own resources, bookings, and `booking.v1.*` methods. The `calendar.resource.*` resources are not related to it.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to Use Resource Booking](https://helpdesk.bitrix24.com/open/15375256/)

## How Resource Booking Works

Resources are booked through a custom CRM field of type `resourcebooking`. This field can be created in the detail forms of leads and deals. The `calendar.resource.*` methods cannot create bookings.

Technically, a resource is a calendar of type `resource`, and a booking is an event in that calendar. This is why the `calendar.resource.booking.list` method returns bookings with the fields of a [calendar event](../calendar-event/index.md).

## Getting Started

1. Create a resource with the [calendar.resource.add](./calendar-resource-add.md) method. A resource can be created by an administrator or by a user with permission to edit resource calendars.
2. Select the resource in the settings of the `resourcebooking` field in the lead or deal detail form. The resource cannot be linked to the field through REST.
3. Once clients are booked through the field, retrieve the bookings with the [calendar.resource.booking.list](./calendar-resource-booking-list.md) method: by resources for a period or by booking IDs from the field.

{% note tip "User Documentation" %}

- [Custom fields in CRM](https://helpdesk.bitrix24.com/open/22067852/)

{% endnote %}

## Linking Resources to Other Entities

A resource is linked to the user who created it, to custom CRM fields, and to REST events.

**User.** The `CREATED_BY` field in the [calendar.resource.list](./calendar-resource-list.md) response contains the ID of the user who created the resource. User data can be retrieved with the [user.get](../../user/user-get.md) method.

**Custom CRM Fields.** Booking IDs are stored in the value of the `resourcebooking` field. Pass them to the `filter.resourceIdList` parameter of the [calendar.resource.booking.list](./calendar-resource-booking-list.md) method. The IDs can be retrieved through:
- universal methods — [crm.item.get](../../crm/universal/crm-item-get.md), [crm.item.list](../../crm/universal/crm-item-list.md)
- methods for leads — [crm.lead.get](../../crm/leads/crm-lead-get.md), [crm.lead.list](../../crm/leads/crm-lead-list.md)
- methods for deals — [crm.deal.get](../../crm/deals/crm-deal-get.md), [crm.deal.list](../../crm/deals/crm-deal-list.md)

To find out which custom fields have the type `resourcebooking`, use the method that retrieves the list of fields:
- for leads — [crm.lead.userfield.list](../../crm/leads/userfield/crm-lead-userfield-list.md)
- for deals — [crm.deal.userfield.list](../../crm/deals/user-defined-fields/crm-deal-userfield-list.md)

**REST Events.** When a resource is added, updated, or deleted, the [OnCalendarSectionAdd](../events/on-calendar-section-add.md), [OnCalendarSectionUpdate](../events/on-calendar-section-update.md), and [OnCalendarSectionDelete](../events/on-calendar-section-delete.md) events are sent. How to distinguish a resource from a regular calendar in a handler is described in the [calendar events overview](../events/index.md#section-or-resource).

## Overview of Methods {#all-methods}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [calendar.resource.add](./calendar-resource-add.md) | Adds a resource ||
|| [calendar.resource.update](./calendar-resource-update.md) | Changes the resource name ||
|| [calendar.resource.list](./calendar-resource-list.md) | Retrieves a list of resources ||
|| [calendar.resource.delete](./calendar-resource-delete.md) | Deletes a resource together with its bookings ||
|| [calendar.resource.booking.list](./calendar-resource-booking-list.md) | Retrieves resource bookings by filter ||
|#
