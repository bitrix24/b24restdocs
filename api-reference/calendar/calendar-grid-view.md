# How to Embed an App in the Calendar

> Scope: [`calendar`](../scopes/permissions.md), [`placement`](../scopes/permissions.md), [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md) or [`user_basic`](../scopes/permissions.md) — for the `user.current` method
>
> Who can execute the methods: to complete the whole scenario, the strictest of the listed rights is required — "a user with administrator rights or with the right to create applications, as well as the owner of a personal application"
>
> - [placement.bind](../widgets/placement-bind.md) and [placement.unbind](../widgets/placement-unbind.md) — a user with administrator rights or with the right to create applications, as well as the owner of a personal application
> - [calendar.event.get](./calendar-event/calendar-event-get.md), [user.current](../user/user-current.md), [BX24.placement.call](../widgets/ui-interaction/bx24-placement-call.md), and [BX24.placement.bindEvent](../widgets/ui-interaction/bx24-placement-bind-event.md) — any user

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

You can embed an app into the calendar via the `CALENDAR_GRIDVIEW` placement point. After registering a handler, a new item appears in the list of calendar views. When a user opens this item, Bitrix24 calls the handler and passes the date range currently displayed in the calendar.

The scenario consists of seven steps: six main ones and the final removal of the handler. The steps follow this order because the item appears in the calendar only after the handler is registered, and the date range and calendar commands are available only in an opened item.

1. Register a handler using the [placement.bind](../widgets/placement-bind.md) method
2. Verify that the item appeared in the calendar
3. Retrieve the date range from `PLACEMENT_OPTIONS`
4. Retrieve events for the period using the [calendar.event.get](./calendar-event/calendar-event-get.md) method
5. Manage the calendar from the app iframe via [BX24.placement.call](../widgets/ui-interaction/bx24-placement-call.md)
6. Subscribe to calendar interface events via [BX24.placement.bindEvent](../widgets/ui-interaction/bx24-placement-bind-event.md)
7. Unregister the handler using the [placement.unbind](../widgets/placement-unbind.md) method if the app is no longer needed

A detailed description of the placement point and incoming data can be found in the [Calendar View CALENDAR_GRIDVIEW](../widgets/calendar.md) article.

The JS, PHP, and Python examples are alternatives rather than parts of a single file: choose one way to retrieve data per step. The JS examples are written as ES modules, include them via `<script type="module">` or bundle them with a bundler.

## Prepare the App

The scenario requires a handler that is accessible over the internet via HTTPS. If you are developing the app locally, configure a public address for your local server.

Verify that:

- the app is installed and installation is complete
- the app has the `calendar` and `placement` scopes, and for the `user.current` method — one of the `user`, `user_brief`, or `user_basic` scopes
- the handler accepts POST requests
- the handler does not block embedding in an iframe: the response has no `X-Frame-Options: DENY`, and `Content-Security-Policy` allows the Bitrix24 frame
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
- `LANG_ALL` — optional parameter with item names for different interface languages. An object where the key is the language code (`de`, `en`) and the value is an object with the `TITLE` field. If `LANG_ALL` is passed, the top-level `TITLE` and `DESCRIPTION` values are not used

{% include [Note on examples](../../_includes/examples.md) %}

In all examples the item is named by language via `LANG_ALL`: the `placement.bind` wrapper in B24PhpSDK accepts names only in this parameter. In the PHP and Python examples, `$b24Service` and `client` are pre-created SDK clients on the app token. How to create them is described in the [B24PhpSDK](../../sdk/b24phpsdk/index.md) and [B24PySDK](../../sdk/b24pysdk/index.md#local-token) articles.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    // App settings page opened in a Bitrix24 iframe
    import { initializeB24Frame, Text } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    // B24JsSDK has no typed wrappers for REST methods, so the method is called by name
    const response = await $b24.actions.v2.call.make({
        method: 'placement.bind',
        params: {
            PLACEMENT: 'CALENDAR_GRIDVIEW',
            HANDLER: 'https://example.com/calendar-handler.php',
            LANG_ALL: {
                de: { TITLE: 'Work plan' },
                en: { TITLE: 'Work plan' },
            },
        },
        requestId: Text.getUuidRfc4122(),
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

    // $b24Service is a pre-created SDK client on the app token
    try
    {
        // The third argument is the item names by language, the SDK passes them in LANG_ALL
        $result = $b24Service->getPlacementScope()->placement()->bind(
            'CALENDAR_GRIDVIEW',
            'https://example.com/calendar-handler.php',
            [
                'de' => ['TITLE' => 'Work plan'],
                'en' => ['TITLE' => 'Work plan'],
            ]
        );

        var_dump($result->isSuccess());
    }
    catch (Throwable $exception)
    {
        echo $exception->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client is a pre-created b24pysdk client on the app token
    try:
        bitrix_response = client.placement.bind(
            placement="CALENDAR_GRIDVIEW",
            handler="https://example.com/calendar-handler.php",
            lang_all={
                "de": {"TITLE": "Work plan"},
                "en": {"TITLE": "Work plan"},
            },
        ).response
        print(bitrix_response.result)
    except BitrixAPIError as error:
        print(f"error: {error.error}", f"error_description: {error.error_description}", sep="\n")
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    ```

{% endlist %}

If the handler is registered, the method returns `true`.

```json
{
    "result": true
}
```

If the method returned an error, the causes and actions are collected in the [Errors and Diagnostics](#errors) section.

## 2. Verify the Item in the Calendar

Open the Bitrix24 calendar. An item with the name from the `TITLE` parameter should appear in the list of calendar views. If you passed `LANG_ALL`, the name is taken from the localized `TITLE` for the user's language.

If the item does not appear, the causes and checks are collected in the [Errors and Diagnostics](#errors) section.

## 3. Retrieve the Date Range in the Handler

When a user opens an item in the calendar, Bitrix24 calls the handler from the `HANDLER` parameter and passes the placement data in a POST request. The date range of the current calendar view is located in the `PLACEMENT_OPTIONS` parameter.

Example of incoming data (a part of the fields is shown):

```php
Array
(
    [DOMAIN] => example.bitrix24.com
    [AUTH_ID] => be56ba6600705a0700005a4b00000001f0f107e5806d5fe9a98e02021a72e57645f86a
    [PLACEMENT] => CALENDAR_GRIDVIEW
    [PLACEMENT_OPTIONS] => {"viewRangeFrom":"2024-12-10","viewRangeTo":"2024-12-16","URI":"\/calendar\/"}
)
```

In a server-side handler, `PLACEMENT_OPTIONS` comes as a JSON string and must be parsed. In the iframe, the SDK provides a ready object in `$b24.placement.options`.

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

For a full list of the data received by the handler, see the [Calendar View CALENDAR_GRIDVIEW](../widgets/calendar.md) article.

## 4. Retrieve Events for the Selected Period

Pass the dates from `PLACEMENT_OPTIONS` to the [calendar.event.get](./calendar-event/calendar-event-get.md) method. For the user's calendar, specify:

- `type` — `user`
- `ownerId` — identifier of the user whose calendar the app shows
- `from` — the start date of the period from `viewRangeFrom`
- `to` — the end date of the period from `viewRangeTo`

The examples show the current user's calendar: the app retrieves its identifier with the [user.current](../user/user-current.md) method and passes it in `ownerId`. The dates are taken from `PLACEMENT_OPTIONS`: in JS — from `$b24.placement.options`, in PHP and Python — from the handler's POST request, as in step 3.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    // Example for an app iframe
    import { initializeB24Frame, Text } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    const { viewRangeFrom: dateFrom, viewRangeTo: dateTo } = $b24.placement.options

    // B24JsSDK has no typed wrappers for REST methods, so the methods are called by name
    const me = await $b24.actions.v2.call.make({
        method: 'user.current',
        params: {},
        requestId: Text.getUuidRfc4122(),
    })

    if (!me.isSuccess) {
        throw new Error(me.getErrorMessages().join('; '))
    }

    // Current user identifier for ownerId
    const ownerId = Number(me.getData().result.ID)

    const response = await $b24.actions.v2.call.make({
        method: 'calendar.event.get',
        params: {
            type: 'user',
            ownerId,
            from: dateFrom,
            to: dateTo,
        },
        requestId: Text.getUuidRfc4122(),
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

    // $b24Service is a pre-created SDK client on the app token
    // $dateFrom and $dateTo are retrieved from PLACEMENT_OPTIONS in step 3
    try
    {
        // Current user identifier for ownerId
        $ownerId = $b24Service->getUserScope()->user()->current()->user()->ID;

        $events = $b24Service->getCalendarScope()->event()->get(
            'user',
            $ownerId,
            [
                'from' => $dateFrom,
                'to' => $dateTo,
            ]
        )->getEvents();

        foreach ($events as $event)
        {
            echo $event->NAME . PHP_EOL;
        }
    }
    catch (Throwable $exception)
    {
        echo $exception->getMessage();
    }
    ```

- Python

    ```python
    import json

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client is a pre-created b24pysdk client on the app token
    # post_data are the fields of the POST request Bitrix24 sent to the handler (step 3)
    placement_options = json.loads(post_data["PLACEMENT_OPTIONS"])
    date_from = placement_options["viewRangeFrom"]
    date_to = placement_options["viewRangeTo"]

    try:
        # Current user identifier for owner_id
        owner_id = int(client.user.current().response.result["ID"])

        bitrix_response = client.calendar.event.get(
            type="user",
            owner_id=owner_id,
            from_date=date_from,
            to=date_to,
        ).response
        print(bitrix_response.result)
    except BitrixAPIError as error:
        print(f"error: {error.error}", f"error_description: {error.error_description}", sep="\n")
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    ```

{% endlist %}

If you need to retrieve events only from specific calendars, add the `section` parameter with an array of calendar identifiers.

Retrieve the calendar identifiers for the `section` parameter with the [calendar.section.get](./calendar-section-get.md) method. In the example, `4` and `191` are demonstration values.

```json
{
    "type": "user",
    "ownerId": 1,
    "from": "2024-12-10",
    "to": "2024-12-16",
    "section": [4, 191]
}
```

The method returns an array of events for the period. The example below is a shortened response to the request from the example above: one event of calendar `4` is shown, the events of calendar `191` and a part of the service fields are omitted.

```json
{
    "result": [
        {
            "ID": "1265",
            "NAME": "Meeting with the client",
            "DESCRIPTION": "Discuss the contract terms",
            "CAL_TYPE": "user",
            "OWNER_ID": "1",
            "CREATED_BY": "1",
            "DATE_FROM": "12/11/2024 05:59:00 pm",
            "DATE_TO": "12/11/2024 06:59:00 pm",
            "DATE_FROM_TS_UTC": "1733932740",
            "DATE_TO_TS_UTC": "1733936340",
            "TZ_FROM": "Europe/Riga",
            "TZ_TO": "Europe/Riga",
            "SECTION_ID": "4",
            "IS_MEETING": true,
            "MEETING_STATUS": "H",
            "ACCESSIBILITY": "busy",
            "IMPORTANCE": "normal"
        }
    ]
}
```

Key event fields:

- `ID` — event identifier, needed for the `viewEvent`, `editEvent`, and `deleteEvent` commands
- `NAME` and `DESCRIPTION` — name and description that the app shows to the user
- `CREATED_BY` — identifier of the user who created the event
- `DATE_FROM` and `DATE_TO` — local start and end time in the `TZ_FROM` and `TZ_TO` timezones
- `DATE_FROM_TS_UTC` and `DATE_TO_TS_UTC` — start and end time as a timestamp. Despite the name, the value is shifted by the server timezone offset, so it cannot be read as UTC time. The exact time is set by `DATE_FROM` together with `TZ_FROM` and `DATE_TO` together with `TZ_TO`. In the example the server offset is zero, so the value matched UTC
- `TZ_FROM` and `TZ_TO` — timezones of the event start and end
- `SECTION_ID` — identifier of the calendar where the event was created
- `CAL_TYPE` and `OWNER_ID` — type and owner of the event calendar
- `IS_MEETING` and `MEETING_STATUS` — meeting with participants flag and the current user's participation status
- `ACCESSIBILITY` and `IMPORTANCE` — event availability and importance

If there are no events, the `result` array will be empty.

```json
{
    "result": []
}
```

In the server-side handler, you can use the `AUTH_ID` authorization token that Bitrix24 passes in the POST request to the handler.

```php
<?php

$domain = $_REQUEST['DOMAIN'] ?? null;
$authId = $_REQUEST['AUTH_ID'] ?? null;
$placementOptions = json_decode($_REQUEST['PLACEMENT_OPTIONS'] ?? '{}', true);

$dateFrom = $placementOptions['viewRangeFrom'] ?? null;
$dateTo = $placementOptions['viewRangeTo'] ?? null;

// Identifier of the user whose calendar is shown: 1 is a demonstration value
$ownerId = 1;

// The Bitrix24 domain is checked against a list rather than trusted from the request.
// Replace the list with the domains of your Bitrix24
$allowedDomains = ['example.bitrix24.com', 'example.bitrix24.de'];

if (
    $domain === null || !in_array($domain, $allowedDomains, true)
    || $authId === null || !preg_match('/^[a-zA-Z0-9]{32,}$/', $authId)
    || $dateFrom === null || $dateTo === null
)
{
    http_response_code(400);
    echo 'Required data is missing';
    exit;
}

$payload = http_build_query([
    'type' => 'user',
    'ownerId' => $ownerId,
    'from' => $dateFrom,
    'to' => $dateTo,
    'auth' => $authId,
]);

$curl = curl_init();
curl_setopt_array($curl, [
    CURLOPT_URL => 'https://' . $domain . '/rest/calendar.event.get',
    CURLOPT_POST => true,
    CURLOPT_POSTFIELDS => $payload,
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_TIMEOUT => 10,
]);

$response = curl_exec($curl);
$httpCode = curl_getinfo($curl, CURLINFO_HTTP_CODE);
$curlError = curl_error($curl);
curl_close($curl);

if ($response === false)
{
    error_log('Calendar request failed: ' . $curlError);
    http_response_code(502);
    echo 'Calendar request failed';
    exit;
}

$result = json_decode($response, true);

if ($httpCode >= 400 || !is_array($result) || isset($result['error']))
{
    http_response_code(502);
    echo 'Calendar request returned an error: '
        . 'HTTP ' . $httpCode . ' '
        . htmlspecialchars((string)($result['error'] ?? 'unknown'), ENT_QUOTES, 'UTF-8')
        . ' '
        . htmlspecialchars((string)($result['error_description'] ?? ''), ENT_QUOTES, 'UTF-8');
    exit;
}

$events = $result['result'] ?? [];
?>
```

The example requires the PHP `curl` extension. Retrieve `$ownerId` with a separate `user.current` call using the same `AUTH_ID`.

`AUTH_ID` is a temporary user token. Pass it only to the server-side handler: do not output it in HTML, do not write it to logs, and do not send it to client-side code. Take the domain for the REST call from the `DOMAIN` parameter of the same request and check it against the list of your Bitrix24 domains, as in the example above: the value from the request cannot be substituted into the call address without validation.

After the request, build the handler's HTML page and display either the list of events or a message stating that there are no events for the period. Event names come from Bitrix24, so escape them before output.

```php
<?php
if ($events === [])
{
    echo '<p>No events for the period</p>';
}
else
{
    echo '<ul>';
    foreach ($events as $event)
    {
        echo '<li>'
            . htmlspecialchars((string)($event['NAME'] ?? ''), ENT_QUOTES, 'UTF-8')
            . ' — '
            . htmlspecialchars((string)($event['DATE_FROM'] ?? ''), ENT_QUOTES, 'UTF-8')
            . '</li>';
    }
    echo '</ul>';
}
?>
```

## 5. Manage the Calendar from the App Iframe

Inside the opened placement point, the app can call calendar commands with the [BX24.placement.call](../widgets/ui-interaction/bx24-placement-call.md) method. These commands are not REST API methods and work only in the app iframe opened from `CALENDAR_GRIDVIEW`.

Calendar commands:

#|
|| **Command** | **What it does** | **Parameters** ||

|| `getEvents`
| Loads events for a range into the calendar: after it, `viewEvent`, `editEvent`, and `deleteEvent` find these events. It can answer the app twice. The first answer is sent immediately, before loading: it contains the records the calendar loaded earlier, and on the first call — an empty array. The second answer comes if the loading finished successfully: it is the passed `entries` array with the `UID` field added by the calendar to each record
| `dateFrom` and `dateTo` — range boundaries. `entries` — required array of events from the app, for example from the `calendar.event.get` response ||

|| `viewEvent`
| Opens the event card
| `id` or `uid`. For a recurring event, you can pass `dateFrom` ||

|| `addEvent`
| Opens the event creation card
| No parameters required ||

|| `editEvent`
| Opens the event editing card
| `id` or `uid`. For a recurring event, you can pass `dateFrom` ||

|| `deleteEvent`
| Starts event deletion
| `id` or `uid`. For a recurring event, you can pass `dateFrom` ||
|#

First call `getEvents` for the required range — without it, the commands below do not find events. Then open the card. The app page markup needs a button that opens the card on click:

```html
<button id="open-event" type="button">Open event</button>
```

```js
import { initializeB24Frame } from '@bitrix24/b24jssdk'

const $b24 = await initializeB24Frame()

const { viewRangeFrom, viewRangeTo } = $b24.placement.options

await $b24.placement.call('getEvents', {
    dateFrom: viewRangeFrom,
    dateTo: viewRangeTo,
    entries: [],
})

// The card is opened by a user action, when the events are already loaded
async function openEvent(id) {
    const result = await $b24.placement.call('viewEvent', { id })
    console.log(result)
}

document.querySelector('#open-event').addEventListener('click', () => openEvent('1265'))
```

The calendar sends the first `getEvents` answer before loading finishes. If `viewEvent` is called right after it, the event may not be loaded yet, so in the example the card is opened by pressing the `#open-event` button.

The `viewEvent`, `editEvent`, and `deleteEvent` commands accept the `id` of a regular event. For a meeting in which the user only participates, the event is stored under the identifier of the parent record, so pass `PARENT_ID` if it is present in the `calendar.event.get` response, otherwise `ID`.

For a recurring event instance, additionally pass the date of this instance in `dateFrom` — the date only, in the Bitrix24 date format, without time. For example, for the English interface it is `12/11/2024`.

The calendar calculates the `UID` field itself: it is `PARENT_ID || ID`, and for a recurring event — with the instance date. The `calendar.event.get` method does not return such a field. To get `UID`, pass the events from the `calendar.event.get` response to the `entries` of the `getEvents` command: the calendar adds `UID` to each record. Without it, build `id` and `dateFrom` from the response data, as in the example below.

```js
// event is an element of the array from the calendar.event.get response
const id = event.PARENT_ID || event.ID
// DATE_FROM: "12/11/2024 05:59:00 pm" — take only the instance date,
// it is already written in the Bitrix24 date format
const dateFrom = event.DATE_FROM.split(' ')[0]

await $b24.placement.call('viewEvent', { id, dateFrom })
```

## 6. Subscribe to Interface Events

The calendar sends interface events when the user changes the range or refreshes the view. You can subscribe to them with the [BX24.placement.bindEvent](../widgets/ui-interaction/bx24-placement-bind-event.md) method — in the examples below via `$b24.placement.bindEvent`. The subscription works only in the app iframe opened at the `CALENDAR_GRIDVIEW` placement point.

#|
|| **Event** | **When sent** | **What the handler receives** ||

|| `Calendar.customView:refreshEntries`
| When calendar events are refreshed
| An empty object `{}` ||

|| `Calendar.customView:decreaseViewRangeDate`
| When navigating to the previous date range
| An empty object `{}` ||

|| `Calendar.customView:increaseViewRangeDate`
| When navigating to the next date range
| An empty object `{}` ||

|| `Calendar.customView:adjustToDate`
| When navigating to a specific date
| The date in `Y-m-d` format ||
|#

```js
import { initializeB24Frame, Text } from '@bitrix24/b24jssdk'

const $b24 = await initializeB24Frame()

// Stub: replace with rendering events in the app interface
function renderEvents(events) {
    console.log(events)
}

// Loads events for the range and re-renders the app interface
async function refreshEvents(dateFrom, dateTo) {
    const ownerId = 1 // identifier from user.current

    // B24JsSDK has no typed wrappers for REST methods, so the method is called by name
    const response = await $b24.actions.v2.call.make({
        method: 'calendar.event.get',
        params: {
            type: 'user',
            ownerId,
            from: dateFrom,
            to: dateTo,
        },
        requestId: Text.getUuidRfc4122(),
    })

    if (response.isSuccess) {
        renderEvents(response.getData().result)
    }
}

// Range the app is showing now
let viewRangeFrom = $b24.placement.options.viewRangeFrom
let viewRangeTo = $b24.placement.options.viewRangeTo

// Shifts the range by its length: -1 — back, 1 — forward
function shiftRange(dateFrom, dateTo, direction) {
    const dayMs = 24 * 60 * 60 * 1000
    const length = Date.parse(dateTo) - Date.parse(dateFrom) + dayMs
    const shift = length * direction

    return [
        new Date(Date.parse(dateFrom) + shift).toISOString().slice(0, 10),
        new Date(Date.parse(dateTo) + shift).toISOString().slice(0, 10),
    ]
}

await $b24.placement.bindEvent('Calendar.customView:refreshEntries', () => {
    refreshEvents(viewRangeFrom, viewRangeTo)
})

await $b24.placement.bindEvent('Calendar.customView:adjustToDate', (date) => {
    // Take the date from the event argument: in placement.options the new range
    // appears only after the iframe is reloaded
    const day = String(date).slice(0, 10)

    viewRangeFrom = day
    viewRangeTo = day
    refreshEvents(viewRangeFrom, viewRangeTo)
})

// Navigating to the previous and the next range
for (const [eventName, direction] of [
    ['Calendar.customView:decreaseViewRangeDate', -1],
    ['Calendar.customView:increaseViewRangeDate', 1],
]) {
    await $b24.placement.bindEvent(eventName, () => {
        [viewRangeFrom, viewRangeTo] = shiftRange(viewRangeFrom, viewRangeTo, direction)
        refreshEvents(viewRangeFrom, viewRangeTo)
    })
}
```

`renderEvents` is a stub that is replaced with rendering events in the app interface.

The `refreshEntries`, `decreaseViewRangeDate`, and `increaseViewRangeDate` events come with an empty `{}` object. The calendar does not pass the new range in them, and `$b24.placement.options` stores the range at the moment the iframe was loaded. Therefore the app shifts the range by its length itself and requests events for the new period.

The `adjustToDate` event passes the date in `Y-m-d` format. If the user switches to the app view from another calendar view, the iframe reloads and the handler receives fresh `placement.options`. There is no reload when navigating inside the view, so take the date from the event argument.

If the app is built on `BX24.js`, the same commands and events are called via `BX24.placement.call` and `BX24.placement.bindEvent`.

## 7. Remove the Handler if the App Is No Longer Needed

If the app is deleted or the handler address changes, unregister it with the [placement.unbind](../widgets/placement-unbind.md) method. Pass `PLACEMENT` and the previous `HANDLER`: without `HANDLER`, the method removes all handlers of this app at the placement point.

The code below runs not in the app opened at the `CALENDAR_GRIDVIEW` placement point, but on the app settings or deletion page opened in a Bitrix24 iframe, or on the app server. It can be executed by an administrator, an employee with the right to create applications, or the owner of a personal application.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    // App settings page opened in a Bitrix24 iframe
    import { initializeB24Frame, Text } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    // B24JsSDK has no typed wrappers for REST methods, so the method is called by name
    const response = await $b24.actions.v2.call.make({
        method: 'placement.unbind',
        params: {
            PLACEMENT: 'CALENDAR_GRIDVIEW',
            HANDLER: 'https://example.com/calendar-handler.php',
        },
        requestId: Text.getUuidRfc4122(),
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    console.info('count:', response.getData().result.count)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    // $b24Service is a pre-created SDK client on the app token
    try
    {
        $result = $b24Service->getPlacementScope()->placement()->unbind(
            'CALENDAR_GRIDVIEW',
            'https://example.com/calendar-handler.php'
        );

        var_dump($result->getDeletedPlacementHandlersCount());
    }
    catch (Throwable $exception)
    {
        echo $exception->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # client is a pre-created b24pysdk client on the app token
    try:
        bitrix_response = client.placement.unbind(
            placement="CALENDAR_GRIDVIEW",
            handler="https://example.com/calendar-handler.php",
        ).response
        print(bitrix_response.result)
    except BitrixAPIError as error:
        print(f"error: {error.error}", f"error_description: {error.error_description}", sep="\n")
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    ```

{% endlist %}

The method returns an object with the `count` field — the number of removed handlers. If `count` is `0`, no handler with this `HANDLER` was found.

```json
{
    "result": {
        "count": 1
    }
}
```

## Verify the Result

The scenario works correctly if:

- a new item appeared in the calendar
- the app handler is called when the item is opened
- the handler receives `PLACEMENT_OPTIONS.viewRangeFrom` and `PLACEMENT_OPTIONS.viewRangeTo`
- the `calendar.event.get` method returns the `result` array
- the app shows the user the events or a message stating that there are no events for the period
- after the `getEvents` call, the `viewEvent` command opens the event card and returns `{"result": true}`
- when navigating to the previous or the next range, the `$b24.placement.bindEvent` handler fires and the app shows events for the new period
- after `placement.unbind` the method returned a `count` greater than zero, and the item disappeared from the list of calendar views

## Errors and Diagnostics {#errors}

If the method returned an error, check the request data.

- `ERROR_PLACEMENT_NOT_FOUND` — `placement.bind` did not find the placement point: the app has no `calendar` scope, in which `CALENDAR_GRIDVIEW` is registered. Add the `calendar` scope to the app
- `ERROR_ARGUMENT` with the text `Argument 'The placement code is not specified' is null or empty` — an empty `PLACEMENT` was passed to `placement.bind`. Pass `CALENDAR_GRIDVIEW`
- `WRONG_AUTH_TYPE` — `placement.bind` or `placement.unbind` was called outside the app, for example via a webhook. Repeat the call from the app
- `ACCESS_DENIED` — `placement.bind` was called by a user without the required right. Call the method as an administrator, an employee with the right to create applications, or the owner of a personal application
- `insufficient_scope` — the app has no `calendar` scope for `calendar.event.get`. Add the `calendar` scope to the app
- `expired_token` — the `AUTH_ID` token with which `calendar.event.get` was called has expired. Reopen the item in the calendar: the handler receives a new `AUTH_ID`

The error comes in the following format:

```json
{
    "error": "ERROR_PLACEMENT_NOT_FOUND",
    "error_description": "Placement not found"
}
```

If there is no error but the scenario does not work, check the signs from the table.

#|
|| **Problem**
| **What to check** ||

|| The item did not appear in the calendar
| The app is installed completely, `PLACEMENT` is equal to `CALENDAR_GRIDVIEW`, the handler is accessible via HTTPS, the user opened Bitrix24 with the installed app ||

|| The handler is not called
| The URL from `HANDLER` is accessible from the internet via HTTPS and returns a page without errors ||

|| The item appeared, but the app area is empty
| The handler does not block the iframe, as described in the "Prepare the App" section ||

|| There is no date range in the handler
| The request came exactly from the `CALENDAR_GRIDVIEW` placement point, the `PLACEMENT_OPTIONS` parameter is parsed as JSON ||

|| The `calendar.event.get` method returns an empty array
| There are events in the period, `type`, `ownerId`, and `section` are specified correctly, the user has access to the calendar ||

|| `$b24.placement.call` does not execute the command
| The code runs inside the app iframe opened at the `CALENDAR_GRIDVIEW` placement point, the SDK is initialized via `initializeB24Frame()`, the command is supported by the calendar ||

|| The `$b24.placement.bindEvent` handler does not fire
| The event name matches the table of step 6 character by character, the app is opened at the `CALENDAR_GRIDVIEW` placement point, the subscription is made after `initializeB24Frame()`. The SDK does not return an error for an unknown event name — check the name and the placement point ||

|| `viewEvent`, `editEvent`, or `deleteEvent` does not find the event
| The `getEvents` command was called before it for the range that includes the event. A correct `id` or `uid` was passed, and for a meeting in which the user only participates — `PARENT_ID`. For a recurring event, the instance date was passed in `dateFrom` ||
|#

## Key Considerations

- the `getEvents`, `viewEvent`, `addEvent`, `editEvent`, and `deleteEvent` commands are not REST API methods: they cannot be called via a webhook or from a server, they work only in the app iframe opened at the `CALENDAR_GRIDVIEW` placement point
- the `viewRangeFrom` and `viewRangeTo` range in `$b24.placement.options` is fixed when the iframe is loaded and does not change when the user browses the calendar inside the view
- the `DATE_FROM_TS_UTC` and `DATE_TO_TS_UTC` fields are shifted by the server timezone offset, so they cannot be read as UTC time
- the `placement.unbind` method without the `HANDLER` parameter removes all handlers of the app at the `CALENDAR_GRIDVIEW` placement point

## Continue Learning

- [{#T}](../widgets/calendar.md)
- [{#T}](../widgets/placement-bind.md)
- [{#T}](../widgets/ui-interaction/bx24-placement-call.md)
- [{#T}](../widgets/ui-interaction/bx24-placement-bind-event.md)
- [{#T}](./calendar-event/calendar-event-get.md)
- [{#T}](./calendar-event/index.md)
- [{#T}](../../settings/app-installation/installation-finish.md)
- [{#T}](../../local-integrations/local-apps.md)
