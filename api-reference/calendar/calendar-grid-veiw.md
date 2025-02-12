# How to Embed an Application in the CALENDAR_GRIDVIEW

You can embed an application in the calendar. At the top of the calendar, in the view types list, there is a place for embedding `CALENDAR_GRIDVIEW`, where you can add your item.

For more details about the widget, see the article [Widget in the Calendar](../widgets/calendar.md).

## How to Bind an Application to the Calendar

Use the method [placement.bind](../widgets/placement-bind.md) to link the application to the calendar. The parameters `PLACEMENT`, `HANDLER`, and `TITLE` determine where and how your application will be displayed.

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'placement.bind',
        {
            PLACEMENT:'CALENDAR_GRIDVIEW',
            HANDLER: 'http://your_site/handler.php',
            TITLE: 'Custom tab'
        },
        (result) => {console.log(result)}
    );
    ```

{% endlist %}

If you output all the parameters that are passed in the request to the application:

{% list tabs %}

- PHP

    ```php
    echo "<pre>";
    print_r($_REQUEST);
    echo "</pre>";
    ```

{% endlist %}

You can see that certain parameters are passed, such as the date range displayed in the calendar:

{% list tabs %}

- PHP

    ```php
    [PLACEMENT_OPTIONS] => {
        "viewRangeFrom":"2018-09-30",
        "viewRangeTo":"2018-11-04"
    }
    ```

{% endlist %}

These parameters can be used to customize the display of your application.

## JS Methods

### Get Events

The `getEvents` method retrieves calendar events.

{% list tabs %}

- JS

    ```js
    var dateFrom = new Date();
    var dateTo = new Date(dateFrom.getTime() + 86400 * 30 * 1000); // Multiply by 1000 to convert seconds to milliseconds
    dateFrom.setHours(0, 0, 0, 0);
    dateTo.setHours(0, 0, 0, 0);

    BX24.placement.call(
        'getEvents',
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

### Open Card and View Event

The `viewEvent` method opens a card to view the event.

{% list tabs %}

- JS

    ```js
    BX24.placement.call(
        'viewEvent',
        {
            id: "1431170", // event identifier
            dateFrom: "11.07.2018" // event date. Not required, but important for recurring events
        },
        function(){}
    );
    ```

{% endlist %}

### Add a New Event

The `addEvent` method opens a card to add a new event.

{% list tabs %}

- JS

    ```js
    BX24.placement.call(
        'addEvent',
        function(){}
    );
    ```

{% endlist %}

### Update Event

The `editEvent` method opens a card to edit the event.

{% list tabs %}

- JS

    ```js
    BX24.placement.call(
        'editEvent',
        {
            uid: "1431171|19.07.2018"
        },
        function(){}
    );
    ```

{% endlist %}

### Delete Event

The `deleteEvent` method deletes the event.

{% list tabs %}

- JS

    ```js
    BX24.placement.call(
        'deleteEvent',
        {
            id: "1431169"
        },
        function(){}
    );
    ```

{% endlist %}

## Events

Events that can be tracked in the embedding location:

#|
|| **Event** | **Description** ||
|| `Calendar.customView:refreshEntries` | Refreshing events ||
|| `Calendar.customView:decreaseViewRangeDate` | Clicking the back arrow, i.e., opening the calendar for previous dates ||
|| `Calendar.customView:increaseViewRangeDate` | Clicking the forward arrow, i.e., opening the calendar for upcoming dates ||
|| `Calendar.customView:adjustToDate` | Navigating to a specific date ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](../widgets/index.md)
- [{#T}](../widgets/calendar.md)
- [{#T}](../widgets/user-field/index.md)
- [{#T}](../../local-integrations/local-apps.md)