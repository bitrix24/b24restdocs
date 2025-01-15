# Embed in CALENDAR_GRIDVIEW

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

The **CALENDAR_GRIDVIEW** placement allows you to embed into the calendar view (at the top - where day/week/month/list options are located).

## Example

How to bind the application to the calendar embed:

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod('placement.bind', {
        PLACEMENT:'CALENDAR_GRIDVIEW',
        HANDLER: 'http://svd.com/svdapp.php',
        TITLE: 'Custom tab'
    }, (result) => {console.log(result)});
    ```

{% endlist %}

If you call the following in the application:

{% list tabs %}

- PHP

    ```php
    echo "<pre>";
    print_r($_REQUEST);
    echo "</pre>";
    ```

{% endlist %}

you will see that certain parameters are received. In particular:

{% list tabs %}

- PHP

    ```php
    [PLACEMENT_OPTIONS] => {
        "viewRangeFrom":"2018-09-30",
        "viewRangeTo":"2018-11-04"
    }
    ```

{% endlist %}

Additionally, there is a specific interface when working with the embed: methods and events.

## Methods (JS methods)

- **getEvents** – retrieve events.

{% list tabs %}

- JS

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

{% endlist %}

- **viewEvent** – view an event (open the view card).

{% list tabs %}

- JS

    ```javascript
    BX24.placement.call('viewEvent',
        {
            id: "1431170", // event id
            dateFrom: "11.07.2018" // event date (not mandatory, but important for recurring events)
        },
        function(){}
    );
    ```

{% endlist %}

- **addEvent** – add a new event (open the card).

{% list tabs %}

- JS

    ```javascript
    BX24.placement.call('addEvent', function(){});
    ```

{% endlist %}

- **editEvent** – edit an event (open the card).

{% list tabs %}

- JS

    ```javascript
    BX24.placement.call('editEvent', {uid: "1431171|19.07.2018"}, function(){});
    ```

{% endlist %}

- **deleteEvent** – delete an event.

{% list tabs %}

- JS

    ```javascript
    BX24.placement.call('deleteEvent',
        {
            id: "1431169"
        },
        function(){}
    );
    ```

{% endlist %}

## Events that can be tracked in the placement

#| 
|| **Event** | **Description** ||
|| Calendar.customView:refreshEntries | Refreshing events. ||
|| Calendar.customView:decreaseViewRangeDate | Clicking the back arrow, i.e., rewinding the calendar to previous dates. ||
|| Calendar.customView:increaseViewRangeDate | Clicking the forward arrow, i.e., rewinding the calendar to upcoming dates. ||
|| Calendar.customView:adjustToDate | Jumping to a specific date. ||
|#

## See also

- [{#T}](../widgets/index.md)
- [{#T}](../../local-integrations/local-apps.md)
- [{#T}](../widgets/user-field/index.md)