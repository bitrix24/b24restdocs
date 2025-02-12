# Widget in the CALENDAR_GRIDVIEW

> Scope: [`calendar`](../scopes/permissions.md)

You can add your item to the list of calendar view types.

The specific placement code for the widget is specified in the `PLACEMENT` parameter of the [placement.bind](./placement-bind.md) method.

## Where the widget is embedded

#|
|| **Widget Code** | **Location** ||
|| `CALENDAR_GRIDVIEW` | Item in the list of calendar view types ||
|#

## What the handler receives

Data is sent as a POST request {.b24-info}

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => b4f4b92b5178b5a5276e181ca09d25a7
    [AUTH_ID] => be56ba6600705a0700005a4b00000001f0f107e5806d5fe9a98e02021a72e57645f86a
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => aed5e16600705a0700005a4b00000001f0f107a80816604b24a8719792ac2a21d629b5
    [member_id] => da45a03b265edd8787f8a258d793cc5d
    [status] => L
    [PLACEMENT] => CALENDAR_GRIDVIEW
    [PLACEMENT_OPTIONS] => {"viewRangeFrom":"2024-08-12","viewRangeTo":"2024-08-18"}
)

```

{% include [Note on required parameters](../../_includes/required.md) %}

{% include notitle [description of standard data](./_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is a JSON string containing an array of one or more keys.

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **viewRangeFrom*** 
[`date`](../data-types.md) | The start of the date range currently displayed in the calendar.

This can be used to retrieve a list of events to display in the widget using the [calendar.event.get](../calendar/calendar-event/calendar-event-get.md) method.

||
|| **viewRangeTo*** 
[`date`](../data-types.md) | The end of the date range currently displayed in the calendar.

This can be used to retrieve a list of events to display in the widget using the [calendar.event.get](../calendar/calendar-event//calendar-event-get.md) method.

||
|#

## Continue your exploration

- [{#T}](../calendar/calendar-grid-veiw.md)
- [{#T}](./placement-bind.md)
- [{#T}](./ui-interaction/index.md)
- [{#T}](./ui-interaction/crm-card.md)
- [{#T}](../interactivity/index.md)
- [{#T}](./open-application.md)
- [{#T}](./open-path.md)