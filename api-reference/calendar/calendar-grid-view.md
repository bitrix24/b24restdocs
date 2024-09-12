# Embed in CALENDAR_GRIDVIEW

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

The **CALENDAR_GRIDVIEW** placement allows you to embed in the calendar view (at the top - where day/week/month/list is located).

## Example

How to bind the application to the calendar embed:

```javascript
BX24.callMethod('placement.bind', {
    PLACEMENT:'CALENDAR_GRIDVIEW',
    HANDLER: 'http://svd.com/svdapp.php',
    TITLE: 'Custom tab'
}, (result) => {console.log(result)});
```

If you call in the application:

```php
echo "<pre>";
print_r($_REQUEST);
echo "</pre>";
```

you will see that certain parameters are received. In particular:

```php
[PLACEMENT_OPTIONS] => {
    "viewRangeFrom":"2018-09-30",
    "viewRangeTo":"2018-11-04"
}
```

Also, when working in the embed, there is a specific interface: methods and events.

## Methods (js methods)

- **getEvents** – retrieving events.

```javascript
var dateFrom = new Date();
var dateTo = new Date(dateFrom.getTime() + 86400 * 30 * 1000); // Multiply by 1000 to convert seconds to milliseconds
dateFrom.setHours(0, 0, 0, 0);
dateTo.setHours(0, 0, 0, 0);

BX24.placement.call('getEvents',
    {
        dateFrom: dateFrom,
        dateTo: dateTo
    },
    function(events) {
        console.log('getEvents response:');
        console.dir(events);
    }
);
```
  
- **viewEvent** – viewing an event (opening the view card).

```javascript
BX24.placement.call('viewEvent',
	{
		id: "1431170", // event id
		dateFrom: "11.07.2018" // event date (not mandatory, but important for recurring)
	},
	function(){}
);
```

- **addEvent** – adding a new event (opening the card).

```javascript
BX24.placement.call('addEvent', function(){});
```

- **editEvent** – editing an event (opening the card).

```javascript
BX24.placement.call('editEvent', {uid: "1431171|19.07.2018"}, function(){});
```

- **deleteEvent** – deleting an event.

```javascript
BX24.placement.call('deleteEvent',
	{
		id: "1431169"
	},
	function(){}
);
```

## Events that can be tracked in the placement

#|
|| **Event** | **Description** ||
|| Calendar.customView:refreshEntries | Refreshing events. ||
|| Calendar.customView:decreaseViewRangeDate | Clicking the back arrow, i.e., rewinding the calendar to previous dates. ||
|| Calendar.customView:increaseViewRangeDate | Clicking the forward arrow, i.e., rewinding the calendar to upcoming dates. ||
|| Calendar.customView:adjustToDate | Jumping to a specific date. ||
|#

## Continue Learning

- [{#T}](../widgets/index.md).
- [{#T}](../../local-integrations/local-apps.md)
- [{#T}](../widgets/user-field/index.md)