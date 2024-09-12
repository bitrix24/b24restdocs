# Add a New Event calendar.event.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.event.add` adds a new event.

#|
|| **Parameter** | **Description** ||
|| **type**^*^ | Calendar type: 
- user; 
- group. ||
|| **ownerId**^*^ | Identifier of the calendar owner. ||
|| **from**^*^ | Start date of the selection. ||
|| **to**^*^ | End date of the selection. ||
|| **from_ts** | Can be set instead of **from**. ||
|| **to_ts** | Can be set instead of **to**. ||
|| **section**^*^ | Identifier of the section. ||
|| **name**^*^ | Name of the event. ||
|| **skip_time** | **[Y\|N]** Indicates that the date value is passed without time. Date format according to ISO-8601 standard. ||
|| **timezone_from** | Time zone of the event's start date and time. Default value - the current user's time zone. Specified as a string, for example: Europe/Riga. ||
|| **timezone_to** | Time zone of the event's end date and time. Default value - the current user's time zone. Specified as a string, for example: Europe/Riga. ||
|| **description** | Description of the event. ||
|| **color** | Background color of the event. When passing the color of the added event, the `#` symbol must be passed in unicode as `%23`. ||
|| **text_color** | Text color of the event. When passing the color of the added event, the `#` symbol must be passed in unicode as `%23`. ||
|| **accessibility** | Availability during the event time: 
- busy; 
- absent; 
- quest; 
- free. ||
|| **importance** | Importance of the event: 
- high; 
- normal; 
- low. ||
|| **private_event** | **[Y\|N]** Mark for a private event. ||
|| **rrule** | Recurrence of the event. ||
|| **is_meeting** | **[Y\|N]** Indicator of a meeting with event participants. ||
|| **location** | Venue. ||
|| **remind** | Reminder for the event: 
- type - time type of reminder (min, hour, day); 
- count - numerical value of the time interval. ||
|| **attendees** | List of event participants (if **is_meeting** == "Y"). ||
|| **host** | Organizer of the event (if **is_meeting** == "Y"). ||
|| **meeting** | Array of parameters including: 
- text - invitation text; 
- open - indicator of an open meeting; 
- notify - flag for notifying about participant confirmation/decline; 
- reinvite - flag for requesting re-confirmation of participation (when editing the event). ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.event.add",
    {
        type: 'user',
        ownerId: '2',
        name: 'New Event Name',
        description: 'Description for event',
        from: '2013-06-14',
        to: '2013-06-14',
        skipTime: 'Y',
        section: 5,
        color: '#9cbe1c',
        text_color: '#283033',
        accessibility: 'absent',
        importance: 'normal',
        is_meeting: 'Y',
        private_event: 'N',
        remind: [{type: 'min', count: 20}],
        location: 'Kaliningrad',
        attendees: [1, 2, 3],
        host: 2,
        meeting: {
            text: 'inviting text',
            open: true,
            notify: true,
            reinvite: false
        }
    }
);
```

{% include [Example Notes](../../_includes/examples.md) %}

## Response on Success

Returns the id of the new event.