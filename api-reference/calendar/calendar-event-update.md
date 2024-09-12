# Edit an Existing Event calendar.event.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.event.update` edits an existing event.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Event identifier. ||
|| **type**^*^ | Calendar type. ||
|| **ownerId**^*^ | Calendar owner identifier. ||
|| **from** | Start date of the selection. ||
|| **to** | End date of the selection. ||
|| **from_ts** | Can be set instead of **from**. ||
|| **to_ts** | Can be set instead of **to**. ||
|| **section**^*^ | Section identifier. ||
|| **name**^*^ | Event name. ||
|| **skip_time** | **[Y\|N]** Indicates that the date value is passed without time. ||
|| **timezone_from** | Time zone of the event start date and time. Default value is the current user's time zone. ||
|| **timezone_to** | Time zone of the event end date and time. Default value is the current user's time zone. ||
|| **description** | Event description. ||
|| **color** | Event background color. ||
|| **text_color** | Event text color. ||
|| **accessibility** | Availability during the event time: 
- busy; 
- absent; 
- quest; 
- free. ||
|| **importance** | Event importance: 
- high; 
- normal; 
- low. ||
|| **private_event** | **[Y\|N]** Mark for private event. ||
|| **rrule** | Event recurrence. ||
|| **is_meeting** | **[Y\|N]** Indicator of a meeting with event participants. ||
|| **location** | Venue. ||
|| **remind** | Event reminder: 
- type - time type of reminder (min, hour, day); 
- count - numerical value of the time interval. ||
|| **attendees** | List of event participants (if **is_meeting** == "Y"). ||
|| **host** | Event organizer (if **is_meeting** == "Y"). ||
|| **meeting** | Array of parameters including: 
- text - invitation text; 
- open - indicator of an open meeting; 
- notify - flag for notifying about participant confirmation/decline; 
- reinvite - flag for requesting re-confirmation of participation (when editing the event). ||
|#

{% include [Parameter Notes](../../_includes/required.md) %}

## Example

```js
BX24.callMethod("calendar.event.update",
    {
        id: 699,
        type: 'user',
        ownerId: '2',
        name: 'Changed Event Name',
        description: 'New description for event',
        from: '2013-06-17',
        to: '2013-06-17',
        skipTime: 'Y',
        section: 5,
        color: '#9cbe1c',
        text_color: '#283033',
        accessibility: 'free',
        importance: 'normal',
        is_meeting: 'N',
        private_event: 'Y',
        remind: [{type: 'min', count: 10}]
    }
);
```

{% include [Example Notes](../../_includes/examples.md) %}