# How to Embed an Application in the CALENDAR_GRIDVIEW

You can embed an application into the calendar. At the top of the calendar, in the view types list, there is a place for embedding `CALENDAR_GRIDVIEW`, where you can add your item.

For more details about the widget, refer to the article [Widget in the Calendar](../widgets/calendar.md).

## How to Bind an Application to the Calendar

Use the method [placement.bind](../widgets/placement-bind.md) to link the application to the calendar. The parameters `PLACEMENT`, `HANDLER`, and `TITLE` define where and how your application will be displayed.

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'placement.bind',
    		{
    			PLACEMENT: 'CALENDAR_GRIDVIEW',
    			HANDLER: 'http://your_site/handler.php',
    			TITLE: 'Custom tab'
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.bind',
                [
                    'PLACEMENT' => 'CALENDAR_GRIDVIEW',
                    'HANDLER'   => 'http://your_site/handler.php',
                    'TITLE'     => 'Custom tab',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error binding placement: ' . $e->getMessage();
    }
    ```

- BX24.js

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

If you output all the parameters that are sent in the request to the application:

{% list tabs %}

- PHP CRest

    ```php
    echo "<pre>";
    print_r($_REQUEST);
    echo "</pre>";
    ```

{% endlist %}

You can see that certain parameters are passed, for example, the date range displayed in the calendar:

{% list tabs %}

- PHP CRest

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
    try
    {
    	const dateFrom = new Date();
    	const dateTo = new Date(dateFrom.getTime() + 86400 * 30 * 1000); // Multiply by 1000 to convert seconds to milliseconds
    	dateFrom.setHours(0, 0, 0, 0);
    	dateTo.setHours(0, 0, 0, 0);
    
    	const response = await $b24.callMethod(
    		'placement.getEvents',
    		{
    			dateFrom: dateFrom,
    			dateTo: dateTo
    		}
    	);
    
    	const events = response.getData().result;
    	console.log('getEvents response:');
    	console.dir(events);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $dateFrom = new DateTime();
        $dateTo = new DateTime($dateFrom->getTimestamp() + 86400 * 30 * 1000);
        $dateFrom->setTime(0, 0, 0, 0);
        $dateTo->setTime(0, 0, 0, 0);
    
        $response = $b24Service
            ->placement
            ->call(
                'getEvents',
                [
                    'dateFrom' => $dateFrom,
                    'dateTo' => $dateTo
                ]
            );
    
        $events = $response
            ->getResponseData()
            ->getResult();
    
        echo 'getEvents response:';
        print_r($events);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting events: ' . $e->getMessage();
    }
    ```

- BX24.js

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
    try
    {
    	const response = await $b24.callMethod(
    		'placement.call',
    		{
    			id: "1431170", // event identifier
    			dateFrom: "07.11.2018" // event date. Optional, but important for recurring events
    		}
    	);
    	
    	const result = response.getData().result;
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->placement
            ->call(
                'viewEvent',
                [
                    'id'      => "1431170", // event identifier
                    'dateFrom' => "07.11.2018" // event date. Optional, but important for recurring events
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        // No data processing logic required
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling viewEvent placement: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.placement.call(
        'viewEvent',
        {
            id: "1431170", // event identifier
            dateFrom: "07.11.2018" // event date. Optional, but important for recurring events
        },
        function(){}
    );
    ```

{% endlist %}

### Add New Event

The `addEvent` method opens a card to add a new event.

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'placement.addEvent',
    		{}
    	);
    	
    	const result = response.getData().result;
    	// Your required data processing logic
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->placement
            ->call('addEvent', []);
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling addEvent: ' . $e->getMessage();
    }
    ```

- BX24.js

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
    try
    {
    	const response = await $b24.callMethod(
    		'placement.call',
    		{
    			placement: 'editEvent',
    			params: {
    				uid: "1431171|19.07.2018"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->placement
            ->call(
                'editEvent',
                [
                    'uid' => "1431171|19.07.2018"
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling editEvent placement: ' . $e->getMessage();
    }
    ```

- BX24.js

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

The `deleteEvent` method deletes an event.

{% list tabs %}

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'placement.deleteEvent',
    		{
    			id: "1431169"
    		}
    	);
    	
    	const result = response.getData().result;
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->placement
            ->call(
                'deleteEvent',
                [
                    'id' => "1431169"
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling deleteEvent: ' . $e->getMessage();
    }
    ```

- BX24.js

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