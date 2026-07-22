# How to Embed an App in the Calendar

> Scope: [`calendar`](../scopes/permissions.md), [`placement`](../scopes/permissions.md)
>
> Who can execute:
> - [placement.bind](../widgets/placement-bind.md) — administrator
> - [calendar.event.get](./calendar-event/calendar-event-get.md) — user with access to the calendar
> - `BX24.placement.call` and `BX24.placement.bindEvent` — app opened at the `CALENDAR_GRIDVIEW` placement point

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can embed an app into the calendar via the `CALENDAR_GRIDVIEW` placement point. After registering a handler, a new item will appear in the list of calendar views. When a user opens this item, Bitrix24 calls the handler and passes the date range currently displayed in the calendar.

The scenario consists of five steps.

1. Register a handler using the [placement.bind](../widgets/placement-bind.md) method
2. Verify that the item appeared in the calendar
3. Retrieve the date range from `PLACEMENT_OPTIONS`
4. Retrieve events for the period using the [calendar.event.get](./calendar-event/calendar-event-get.md) method
5. Manage the calendar from the app iframe via [BX24.placement.call](../widgets/ui-interaction/bx24-placement-call.md)

A detailed description of the placement point and incoming data can be found in the [Calendar Widget](../widgets/calendar.md) article. This page demonstrates a practical scenario for working with the placement point.

## Prepare the App

The scenario requires a handler that is accessible over the internet via HTTPS. If you are developing the app locally, configure a public address for your local server.

Verify that:

- the app is installed and installation is complete
- the app has `calendar` and `placement` permissions
- the handler accepts POST requests
- the user has access to the calendar whose events need to be shown
- the `type` and `ownerId` of the calendar are known for the `calendar.event.get` method

For a user calendar, pass `type: user` and the user ID in `ownerId`. For the company calendar, pass `type: company_calendar` and `ownerId: 0`.

If the app installation is not complete, the placement point may not be displayed. Complete the installation using a method or action from the [Complete App Installation](../../settings/app-installation/installation-finish.md) article.

## 1. Register the Item in the Calendar

Register a handler using the [placement.bind](../widgets/placement-bind.md) method. Pass the `CALENDAR_GRIDVIEW` placement point code in the `PLACEMENT` parameter.

Registration parameters:

- `PLACEMENT` — `CALENDAR_GRIDVIEW` placement point code
- `HANDLER` — public HTTPS address of the app handler
- `TITLE` — name of the item in the calendar view list

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    // App settings page opened in a Bitrix24 iframe
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    const response = await $b24.actions.v2.call.make({
        method: 'placement.bind',
        params: {
            PLACEMENT: 'CALENDAR_GRIDVIEW',
            HANDLER: 'https://example.com/calendar-handler.php',
            TITLE: 'Work plan',
        },
        requestId: 'calendar-grid-view-bind',
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    console.info(response.getData().result)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Exceptions\BaseException;

    // $b24 is built on an app token
    try
    {
        $b24->getPlacementScope()->placement()->bind(
            'CALENDAR_GRIDVIEW',
            'https://example.com/calendar-handler.php',
            [
                'de' => ['TITLE' => 'Work plan'],
                'en' => ['TITLE' => 'Work plan'],
            ]
        );

        echo 'Calendar item is registered';
    }
    catch (BaseException $exception)
    {
        echo $exception->getMessage();
    }
    ```

{% endlist %}

If the handler is registered, the method returns `true`.

```json
{
    "result": true
}
```

## 2. Verify the Item in the Calendar

Open the Bitrix24 calendar. An item with the name from the `TITLE` parameter should appear in the list of calendar views.

If the item does not appear, verify that:

- the app installation is fully completed
- the `CALENDAR_GRIDVIEW` code was passed in `PLACEMENT`
- the handler is accessible via HTTPS
- the user is working in Bitrix24 with the installed app

## 3. Retrieve the Date Range in the Handler

When a user opens an item in the calendar, Bitrix24 calls the handler specified in the `HANDLER` parameter and passes the widget data in a POST request. The date range of the current calendar view is located in the `PLACEMENT_OPTIONS` parameter.

Example of incoming data:

```php
Array
(
    [DOMAIN] => example.bitrix24.com
    [AUTH_ID] => be56ba6600705a0700005a4b00000001f0f107e5806d5fe9a98e02021a72e57645f86a
    [PLACEMENT] => CALENDAR_GRIDVIEW
    [PLACEMENT_OPTIONS] => {"viewRangeFrom":"2024-08-12","viewRangeTo":"2024-08-18"}
)
```

In the handler, convert `PLACEMENT_OPTIONS` from a JSON string into an array to retrieve the dates.

```php
<?php

$placementOptions = json_decode($_REQUEST['PLACEMENT_OPTIONS'] ?? '{}', true);

$dateFrom = $placementOptions['viewRangeFrom'] ?? null;
$dateTo = $placementOptions['viewRangeTo'] ?? null;

if ($dateFrom === null || $dateTo === null)
{
    http_response_code(400);
    echo 'Date range is required';
    exit;
}
```

For a full list of the data received by the handler, see the [Widget in Calendar](../widgets/calendar.md) article.

## 4. Retrieve Events for the Selected Period

Pass the dates from `PLACEMENT_OPTIONS` to the [calendar.event.get](./calendar-event/calendar-event-get.md) method. For the user's calendar, specify:

- `type` — `user`
- `ownerId` — user identifier
- `from` — the start date of the period from `viewRangeFrom`
- `to` — the end date of the period from `viewRangeTo`

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    // Example for an iframe app
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    // For calendar.event.get, a direct method call by name is used
    const response = await $b24.actions.v2.call.make({
        method: 'calendar.event.get',
        params: {
            type: 'user',
            ownerId: 1,
            from: '2024-08-12',
            to: '2024-08-18',
        },
        requestId: 'calendar-event-get',
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    const events = response.getData().result
    console.info(events)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Exceptions\BaseException;

    // $b24Service is built on an app token or an OAuth token
    try
    {
        // For calendar.event.get, a direct method call by name is used
        $response = $b24Service->core->call(
            'calendar.event.get',
            [
                'type' => 'user',
                'ownerId' => 1,
                'from' => '2024-08-12',
                'to' => '2024-08-18',
            ]
        );

        $events = $response->getResponseData()->getResult();
        print_r($events);
    }
    catch (BaseException $exception)
    {
        echo $exception->getMessage();
    }
    ```

{% endlist %}

If you need to retrieve events only from specific calendars, add the `section` parameter with an array of calendar identifiers.

```json
{
    "type": "user",
    "ownerId": 1,
    "from": "2024-08-12",
    "to": "2024-08-18",
    "section": [21, 44]
}
```

The method returns an array of events for the period. If there are no events, the `result` array will be empty.

```json
{
    "result": []
}
```

In the server-side handler, you can use the `AUTH_ID` authorization token passed by Bitrix24 in the widget's POST request.

```php
<?php

$domain = $_REQUEST['DOMAIN'] ?? null;
$authId = $_REQUEST['AUTH_ID'] ?? null;
$placementOptions = json_decode($_REQUEST['PLACEMENT_OPTIONS'] ?? '{}', true);

$dateFrom = $placementOptions['viewRangeFrom'] ?? null;
$dateTo = $placementOptions['viewRangeTo'] ?? null;

if ($domain === null || $authId === null || $dateFrom === null || $dateTo === null)
{
    http_response_code(400);
    echo 'Required data is missing';
    exit;
}

$payload = http_build_query([
    'type' => 'user',
    'ownerId' => 1,
    'from' => $dateFrom,
    'to' => $dateTo,
    'auth' => $authId,
]);

$context = stream_context_create([
    'http' => [
        'method' => 'POST',
        'header' => 'Content-Type: application/x-www-form-urlencoded',
        'content' => $payload,
    ],
]);

$response = file_get_contents('https://' . $domain . '/rest/calendar.event.get', false, $context);
$result = json_decode($response, true);

$events = $result['result'] ?? [];
?>
```

After the request, generate the handler's HTML page and display either the list of events or a message stating that no events exist for the period.

## 5. Manage the Calendar from an Iframe Application

Inside the opened placement, the application can call calendar commands using the [BX24.placement.call](../widgets/ui-interaction/bx24-placement-call.md) method. These commands are not REST methods and only work within the application iframe opened from `CALENDAR_GRIDVIEW`.

Calendar commands:

#|
|| **Command** | **What it does** | **Parameters** ||

|| `getEvents`
| A service command for working with the list of events for a period within the calendar interface. To get events in the application, use method `calendar.event.get` from step 4| `dateFrom`, `dateTo`, `entries` ||

|| `viewEvent`
| Opens the event card| `id` or `uid`. For a recurring event, you can pass `dateFrom` ||

|| `addEvent`
| Opens the create event card| No parameters required ||

|| `editEvent`
| Opens the edit event card| `id` or `uid`. For a recurring event, you can pass `dateFrom` ||

|| `deleteEvent`
| Starts event deletion| `id` or `uid`. For a recurring event, you can pass `dateFrom` ||
|#

Example of opening an event card:

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.call(
            'viewEvent',
            {
                id: '1265',
                dateFrom: '2024-08-12'
            },
            function(response) {
                console.log(response);
            }
        );
    });
});
```

To edit or delete a recurring event, pass `uid`, if it is present in the event data. If `uid` is unknown, pass `id` and the event instance date in `dateFrom`.

## 6. Subscribe to Interface Events

The calendar sends interface events when a user changes the range or refreshes the view. In the application iframe, you can subscribe to them using the [BX24.placement.bindEvent](../widgets/ui-interaction/bx24-placement-bind-event.md) method.

#|
|| **Event** | **When sent** | **What the handler receives** ||

|| `Calendar.customView:refreshEntries`
| When calendar events are updated| An empty object `{}` ||

|| `Calendar.customView:decreaseViewRangeDate`
| When navigating to the previous date range| An empty object `{}` ||

|| `Calendar.customView:increaseViewRangeDate`
| When navigating to the next date range| An empty object `{}` ||

|| `Calendar.customView:adjustToDate`
| When navigating to a specific date| The date in `Y-m-d` format ||
|#

```js
BX24.ready(function () {
    BX24.init(function () {
        BX24.placement.bindEvent('Calendar.customView:refreshEntries', function () {
            console.log('Calendar entries should be refreshed');
        });

        BX24.placement.bindEvent('Calendar.customView:adjustToDate', function (date) {
            console.log(date);
        });
    });
});
```

## Verify the Result

The scenario works correctly if:

- a new item appears in the calendar
- the application handler is called when an item is opened
- the handler receives `PLACEMENT_OPTIONS.viewRangeFrom` and `PLACEMENT_OPTIONS.viewRangeTo`
- the `calendar.event.get` method returns the `result` array
- the application shows the user the events or a message stating that no events exist for the period
- `BX24.placement.call` commands are executed only within the `CALENDAR_GRIDVIEW` placement iframe
- `BX24.placement.bindEvent` handlers respond to changes in the calendar view

## Common Issues

#|
|| **Problem**| **What to check** ||

|| Item did not appear in the calendar| The application is fully installed, `PLACEMENT` is equal to `CALENDAR_GRIDVIEW`, the user opened Bitrix24 with the installed application ||

|| Handler is not called| The URL from `HANDLER` is accessible from the internet via HTTPS and returns a page without errors ||

|| There is no date range in the handler| The request came specifically from point `CALENDAR_GRIDVIEW`, the parameter `PLACEMENT_OPTIONS` is parsed as JSON ||

|| Method `calendar.event.get` returns an empty array| There are events in the period, `type`, `ownerId`, and `section` are specified correctly, the user has access to the calendar ||

|| `BX24.placement.call` does not execute the command| The code is running inside the iframe of point `CALENDAR_GRIDVIEW`, library `BX24` is initialized, the command is supported by the calendar ||

|| `viewEvent`, `editEvent`, or `deleteEvent` cannot find the event| A correct `id` or `uid` was passed. For a recurring event, the instance date was passed in `dateFrom` ||
|#

## Key Considerations

- `PLACEMENT_OPTIONS` is passed as a JSON string and must be parsed before use
- `viewRangeFrom` and `viewRangeTo` show the range that is open in the calendar at the moment the handler is called
- `calendar.event.get` is a Bitrix24 REST API method for retrieving calendar events
- `BX24.placement.call` is an interface command mechanism within an iframe application, not a REST method
- Interface commands `getEvents`, `viewEvent`, `addEvent`, `editEvent`, and `deleteEvent` are only available at the `CALENDAR_GRIDVIEW` integration point
- Events `Calendar.customView:refreshEntries`, `Calendar.customView:decreaseViewRangeDate`, and `Calendar.customView:increaseViewRangeDate` pass an empty object `{}`
- Event `Calendar.customView:adjustToDate` passes the date in `Y-m-d` format

## Continue Learning

- [{#T}](../widgets/calendar.md)
- [{#T}](../widgets/placement-bind.md)
- [{#T}](../widgets/ui-interaction/bx24-placement-call.md)
- [{#T}](../widgets/ui-interaction/bx24-placement-bind-event.md)
- [{#T}](./calendar-event/calendar-event-get.md)
- [{#T}](./calendar-event/index.md)
- [{#T}](../../settings/app-installation/installation-finish.md)
- [{#T}](../../local-integrations/local-apps.md)
