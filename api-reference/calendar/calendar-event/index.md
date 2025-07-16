# Calendar Events: Overview of Methods

Calendar events are scheduled activities or meetings. They contain information about the date, time, location, and participants of the event. Events help users manage their schedules and remind them of upcoming activities and meetings.

> Quick navigation: [all methods and events](#all-methods) 
> 
> User documentation: [how to create an event in the calendar](https://helpdesk.bitrix24.com/open/21307284/)

## Linking Calendar Events with Other Objects

**User.** An event is linked to a user by the calendar owner's ID `ownerId` for the `user` calendar type. If the event is a meeting with participants `is_meeting = 'Y'`, it is additionally linked to the event organizer `host` and the participants `attendees`. You can obtain the user ID using the [user.get](../../user/user-get.md) method.

**Group.** An event is linked to a group by the calendar owner's ID `ownerId` for the `group` calendar type. The ID can be obtained using the [create new group](../../sonet-group/sonet-group-create.md) method or the [get list of groups](../../sonet-group/socialnetwork-api-workgroup-list.md) method.

**CRM Objects.** You can link CRM objects to an event: companies, contacts, leads, and deals. To link objects, list their IDs with [prefixes](../../crm/data-types.md#object_type) in the `crm_fields` parameter. For example, `C_3` for a contact with `id = 3`. You can obtain the ID using the [create new CRM item](../../crm/universal/crm-item-add.md) method or the [get list of items](../../crm/universal/crm-item-list.md) method.

{% note tip "User Documentation" %}

- [Bitrix24 Calendar](https://helpdesk.bitrix24.com/open/15144548/)
- [How to create a group and project](https://helpdesk.bitrix24.com/open/22796428/)

{% endnote %}

## List of Events

You can obtain a list of calendar events using two methods:
- [calendar.event.get](./calendar-event-get.md) — returns a list of any events, past and future, for the specified period
- [calendar.event.get.nearest](./calendar-event-get-nearest.md) — returns a list of only future events for the specified number of days

## User Participation in an Event
The user decides whether to participate in the event or not. The decision is recorded in the participation status and can have the following values:
- `Y` — accepted
- `N` — declined
- `Q` — invited but not yet responded

You can find out the current user's participation status in the event using the [calendar.meeting.status.get](./calendar-meeting-status-get.md) method. To set the status, use the [calendar.meeting.status.set](./calendar-meeting-status-set.md) method.

The [calendar.accessibility.get](./calendar-accessibility-get.md) method retrieves the availability of users from the list.

## Overview of Methods and Events {#all-methods}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [calendar.event.add](./calendar-event-add.md) | Add an event ||
    || [calendar.event.update](./calendar-event-update.md) | Update an event ||
    || [calendar.event.getById](./calendar-event-get-by-id.md) | Get an event by `id` ||
    || [calendar.event.get](./calendar-event-get.md) | Get a list of calendar events ||
    || [calendar.event.getNearest](./calendar-event-get-nearest.md) | Get a list of future events ||
    || [calendar.event.delete](./calendar-event-delete.md) | Delete an event ||
    || [calendar.meeting.status.get](./calendar-meeting-status-get.md) | Get the current user's participation status in the event ||
    || [calendar.meeting.status.set](./calendar-meeting-status-set.md) | Set the participation status in the event for the current user ||
    || [calendar.accessibility.get](./calendar-accessibility-get.md) | Get the availability of users from the list ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnCalendarEntryAdd](./events/on-calendar-entry-add.md) | When an event is added ||
    || [OnCalendarEntryUpdate](./events/on-calendar-entry-update.md) | When an event is updated ||
    || [OnCalendarEntryDelete](./events/on-calendar-entry-delete.md) | When an event is deleted ||
    |#

{% endlist %}