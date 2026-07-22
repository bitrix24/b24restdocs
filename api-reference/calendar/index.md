# Calendar: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The calendar helps users plan meetings, tasks, and events. Calendars can be managed using a group of methods [calendar.section.*](#base).

Calendar events are scheduled activities or meetings. A group of methods [calendar.event.*](./calendar-event/index.md) is used to create, modify, retrieve, or delete events.

> Quick navigation: [all methods and events](#all-methods)
> 
> User documentation: [Bitrix24 calendar](https://helpdesk.bitrix24.com/section/47483/)

## Connection with Other Objects

**User.** The calendar is linked to a user by the calendar owner's ID `ownerId` for the calendar type `user`. The user ID can be obtained using the method [user.get](../user/user-get.md).

**Group.** The calendar is linked to a group by the calendar owner's ID `ownerId` for the calendar type `group`. The ID can be obtained by [creating a new group](../sonet-group/sonet-group-create.md) or by [getting the list of groups](../sonet-group/socialnetwork-api-workgroup-list.md).

{% note tip "User documentation" %}

- [How to create a group or project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## Calendar Settings

In the main calendar settings, the company's working hours, weekends, and holidays are specified. The settings can be retrieved using the method [calendar.settings.get](./calendar-settings-get.md).

In user settings, an employee can specify personal preferences, such as a time zone or week number display. User settings can be retrieved using the method [calendar.user.settings.get](./calendar-user-settings-get.md) and set using the method [calendar.user.settings.set](./calendar-user-settings-set.md).

{% note tip "User documentation" %}

- [Calendar settings](https://helpdesk.bitrix24.com/open/13981546/)

{% endnote %}

## Resource Booking

In Bitrix24, resource booking is done through a custom CRM field of type `resourcebooking`. Such a field can be created in the [lead](../crm/leads/userfield/index.md) and [deal](../crm/deals/user-defined-fields/index.md) forms.

Resource availability can be tracked in the CRM Calendar. Technically, a resource is a section of the calendar, and booking is a calendar event.

The management of resources is handled by a group of methods [calendar.resource.*](./resource/index.md).

{% note tip "User documentation" %}

- [How to use resource booking](https://helpdesk.bitrix24.com/open/15375256/)

{% endnote %}

## **Widgets**

An application can be embedded into the calendar. In the list of calendar view types, there is a place for embedding `CALENDAR_GRIDVIEW`, where you can add [your item](../widgets/calendar.md).

{% note tip "Typical use-cases and scenarios" %}

-  [{#T}](../widgets/index.md)
-  [{#T}](./calendar-grid-view.md)

{% endnote %}

## Overview of Methods and Events {#all-methods}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

## Main {#base}

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [calendar.section.add](./calendar-section-add.md) | Add a new calendar ||
    || [calendar.section.update](./calendar-section-update.md) | Update calendar ||
    || [calendar.section.get](./calendar-section-get.md) | Get list of calendars ||
    || [calendar.section.delete](./calendar-section-delete.md) | Delete calendar ||
    || [calendar.settings.get](./calendar-settings-get.md) | Get basic calendar settings ||
    || [calendar.user.settings.get](./calendar-user-settings-get.md) | Get user calendar settings ||
    || [calendar.user.settings.set](./calendar-user-settings-set.md) | Set user calendar settings ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnCalendarSectionAdd](./events/on-calendar-section-add.md) | When adding a calendar section or resource ||
    || [OnCalendarSectionUpdate](./events/on-calendar-section-update.md) | When changing a calendar section or resource ||
    || [OnCalendarSectionDelete](./events/on-calendar-section-delete.md) | When deleting a calendar section or resource ||
    |#

{% endlist %}

## Calendar Events

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [calendar.event.add](./calendar-event/calendar-event-add.md) | Add event ||
    || [calendar.event.update](./calendar-event/calendar-event-update.md) | Update event ||
    || [calendar.event.getById](./calendar-event/calendar-event-get-by-id.md) | Get event by `id` ||
    || [calendar.event.get](./calendar-event/calendar-event-get.md) | Get list of calendar events ||
    || [calendar.event.getNearest](./calendar-event/calendar-event-get-nearest.md) | Get list of future events ||
    || [calendar.event.delete](./calendar-event/calendar-event-delete.md) | Delete event ||
    || [calendar.meeting.status.get](./calendar-event/calendar-meeting-status-get.md) | Get current user's participation status in an event ||
    || [calendar.meeting.status.set](./calendar-event/calendar-meeting-status-set.md) | Set event participation status for the current user ||
    || [calendar.accessibility.get](./calendar-event/calendar-accessibility-get.md) | Get availability of users from a list ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnCalendarEntryAdd](./calendar-event/events/on-calendar-entry-add.md) | When adding an event ||
    || [OnCalendarEntryUpdate](./calendar-event/events/on-calendar-entry-update.md) | When changing an event ||
    || [OnCalendarEntryDelete](./calendar-event/events/on-calendar-entry-delete.md) | When deleting an event ||
    |#

{% endlist %}

## Resource Booking

#|
|| **Method** | **Description** ||
|| [calendar.resource.add](./resource/calendar-resource-add.md) | Add a resource ||
|| [calendar.resource.update](./resource/calendar-resource-update.md) | Update a resource ||
|| [calendar.resource.list](./resource/calendar-resource-list.md) | Get a list of resources ||
|| [calendar.resource.booking.list](./resource/calendar-resource-booking-list.md) | Get resource bookings by filter ||
|| [calendar.resource.delete](./resource/calendar-resource-delete.md) | Delete a resource ||
|#
