# How to Embed an Application in the CALENDAR_GRIDVIEW

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can embed an application into the calendar. At the top of the calendar, in the view types list, there is a place for embedding `CALENDAR_GRIDVIEW`, where you can add your item.

For more details about the widget, refer to the article [Widget in the Calendar](../widgets/calendar.md).

## How to Bind an Application to the Calendar

Use the method [placement.bind](../widgets/placement-bind.md) to link the application to the calendar. The parameters `PLACEMENT`, `HANDLER`, and `TITLE` define where and how your application will be displayed.

{% list tabs %}

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PlacementBindResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<PlacementBindResult>({
        method: 'placement.bind',
        params: {
          PLACEMENT: 'CALENDAR_GRIDVIEW',
          HANDLER: 'http://your_site/handler.php',
          TITLE: 'Custom tab',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('placement.bind result:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function bindCalendarPlacement() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'CALENDAR_GRIDVIEW',
              HANDLER: 'http://your_site/handler.php',
              TITLE: 'Custom tab',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('placement.bind result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindCalendarPlacement)
    </script>
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each event returned in result[]
    type PlacementEvent = {
      id: string
    }

    try {
      const dateFrom = new Date()
      const dateTo = new Date(dateFrom.getTime() + 86400 * 30 * 1000) // Multiply by 1000 to convert seconds to milliseconds
      dateFrom.setHours(0, 0, 0, 0)
      dateTo.setHours(0, 0, 0, 0)

      const response = await $b24.actions.v2.call.make<PlacementEvent[]>({
        method: 'placement.getEvents',
        params: {
          dateFrom: dateFrom,
          dateTo: dateTo,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('getEvents response:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getCalendarEvents() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const dateFrom = new Date()
          const dateTo = new Date(dateFrom.getTime() + 86400 * 30 * 1000) // Multiply by 1000 to convert seconds to milliseconds
          dateFrom.setHours(0, 0, 0, 0)
          dateTo.setHours(0, 0, 0, 0)

          const response = await $b24.actions.v2.call.make({
            method: 'placement.getEvents',
            params: {
              dateFrom: dateFrom,
              dateTo: dateTo,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('getEvents response:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCalendarEvents)
    </script>
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ViewEventResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<ViewEventResult>({
        method: 'placement.call',
        params: {
          id: '1431170', // event identifier
          dateFrom: '11.07.2018', // event date; optional but important for recurring events
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('placement.call viewEvent result:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function viewCalendarEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.call',
            params: {
              id: '1431170', // event identifier
              dateFrom: '11.07.2018', // event date; optional but important for recurring events
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('placement.call viewEvent result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', viewCalendarEvent)
    </script>
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AddEventResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<AddEventResult>({
        method: 'placement.addEvent',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('placement.addEvent result:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function addCalendarEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.addEvent',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('placement.addEvent result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCalendarEvent)
    </script>
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type EditEventResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<EditEventResult>({
        method: 'placement.call',
        params: {
          placement: 'editEvent',
          params: {
            uid: '1431171|19.07.2018',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('placement.call editEvent result:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function editCalendarEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.call',
            params: {
              placement: 'editEvent',
              params: {
                uid: '1431171|19.07.2018',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('placement.call editEvent result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', editCalendarEvent)
    </script>
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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type DeleteEventResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<DeleteEventResult>({
        method: 'placement.deleteEvent',
        params: {
          id: '1431169',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('placement.deleteEvent result:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function deleteCalendarEvent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.deleteEvent',
            params: {
              id: '1431169',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('placement.deleteEvent result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteCalendarEvent)
    </script>
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