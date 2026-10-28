# Get a List of Upcoming Events calendar.event.get.nearest

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `calendar.event.get.nearest` retrieves a list of upcoming events.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../data-types.md) | Calendar type: 
- `user` — user calendar
- `group` — group calendar
- `company_calendar` — company calendar ||
|| **ownerId**
[`integer`](../../data-types.md) | Identifier of the calendar owner.

Pass `ownerId` together with `type`. The parameter value does not affect the list of returned events. For the company calendar, pass the value `0` ||
|| **days**
[`integer`](../../data-types.md) | Number of days to retrieve, starting from the current date. Default is `60` ||
|| **forCurrentUser**
[`boolean`](../../data-types.md) | Output the list of events for the current user. Default is `true` ||
|| **maxEventsCount**
[`integer`](../../data-types.md) | Maximum number of events to display. No limit by default ||
|| **detailUrl**
[`string`](../../data-types.md) | Calendar link template, for example `/company/personal/user/#user_id#/calendar/`. The method replaces `#user_id#` with the current user's ID, discards the part of the link after `?`, and appends the event parameters.

If the parameter is passed, each event in the response has a `~detailUrl` field with a link to the event ||
|#

{% note info %}

To select a calendar type, pass `type` and `ownerId` together. The `ownerId` value does not affect the list of returned events. For the company calendar, pass `type: company_calendar` and `ownerId: 0`.

If `forCurrentUser: true` is passed, or `type` and `ownerId` are not specified, the method forcibly switches to the current user's calendar: `type: user` and `forCurrentUser: true`.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

1. Get events for the current user for the next 10 days.

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"type":"user","ownerId":2,"days":10,"forCurrentUser":true,"maxEventsCount":100,"detailUrl":"/company/personal/user/#user_id#/calendar/"}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.event.get.nearest
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"type":"user","ownerId":2,"days":10,"forCurrentUser":true,"maxEventsCount":100,"detailUrl":"/company/personal/user/#user_id#/calendar/","auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/calendar.event.get.nearest
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type CalendarEventItem = {
          ID: string
          NAME: string
          DATE_FROM: string
          DATE_TO: string
          TIMESTAMP_X_ISO?: string
          DELETED: string
        }

        try {
          const response = await $b24.actions.v2.call.make<CalendarEventItem[]>({
            method: 'calendar.event.get.nearest',
            params: {
            type: 'user',
            ownerId: 2,
            days: 10,
            forCurrentUser: true,
            maxEventsCount: 100,
            detailUrl: '/company/personal/user/#user_id#/calendar/',
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const events = response.getData()!.result
            events.forEach((event) => console.info('Event:', event.ID, event.NAME, event.DATE_FROM, event.DATE_TO))
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
          async function getNearestCalendarEvents() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'calendar.event.get.nearest',
                params: {
                  type: 'user',
                  ownerId: 2,
                  days: 10,
                  forCurrentUser: true,
                  maxEventsCount: 100,
                  detailUrl: '/company/personal/user/#user_id#/calendar/',
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const events = response.getData().result
              events.forEach((event) => console.info('Event:', event.ID, event.NAME, event.DATE_FROM, event.DATE_TO))
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getNearestCalendarEvents)
        </script>
        ```

    - Python

        ```python
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.calendar.event.get.nearest(
                type="user",
                owner_id=2,
                days=10,
                for_current_user=True,
                max_events_count=100,
                detail_url="/company/personal/user/#user_id#/calendar/",
            ).response
            result = bitrix_response.result
            print(result)
        except BitrixAPIError as error:
            print(
                "Bitrix API error",
                f"error: {error.error}",
                f"error_description: {error.error_description}",
                sep="\n",
            )
        except BitrixSDKException as error:
            print(f"Bitrix SDK error: {error.message}")
        except Exception as error:
            print(f"Unexpected error: {error}")
        ```

    - PHP

        ```php
        try {
            $response = $b24Service
                ->core
                ->call(
                    'calendar.event.get.nearest',
                    [
                        'type'           => 'user',
                        'ownerId'        => 2,
                        'days'           => 10,
                        'forCurrentUser' => true,
                        'maxEventsCount' => 100,
                        'detailUrl'      => '/company/personal/user/#user_id#/calendar/',
                    ]
                );

            $result = $response
                ->getResponseData()
                ->getResult();

            echo 'Success: ' . print_r($result, true);
            // Your required data processing logic
            processData($result);

        } catch (Throwable $e) {
            error_log($e->getMessage());
            echo 'Error getting calendar events: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            'calendar.event.get.nearest',
            {
                type: 'user',
                ownerId: 2,
                days: 10,
                forCurrentUser: true,
                maxEventsCount: 100,
                detailUrl: '/company/personal/user/#user_id#/calendar/'
            }
        );
        ```

    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'calendar.event.get.nearest',
            [
                'type' => 'user',
                'ownerId' => 2,
                'days' => 10,
                'forCurrentUser' => true,
                'maxEventsCount' => 100,
                'detailUrl' => '/company/personal/user/#user_id#/calendar/'
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "calendar.event.get.nearest", b24.Params{
        	"type":           "user",
        	"ownerId":        2,
        	"days":           10,
        	"forCurrentUser": true,
        	"maxEventsCount": 100,
        	"detailUrl":      "/company/personal/user/#user_id#/calendar/",
        })
        if err != nil {
        	return fmt.Errorf("calendar.event.get.nearest: %w", err)
        }

        // The response arrives as json.RawMessage — unmarshal it
        // into a struct matching the response shape shown below on this page.
        fmt.Printf("%s\n", res.Result)
        ```

    {% endlist %}

2. Get events from the Company Calendar.

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"type":"company_calendar","ownerId":0,"forCurrentUser":false}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.event.get.nearest
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"type":"company_calendar","ownerId":0,"forCurrentUser":false,"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/calendar.event.get.nearest
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type CalendarEventItem = {
          ID: string
          NAME: string
          DATE_FROM: string
          DATE_TO: string
          TIMESTAMP_X_ISO?: string
          DELETED: string
        }

        try {
          const response = await $b24.actions.v2.call.make<CalendarEventItem[]>({
            method: 'calendar.event.get.nearest',
            params: {
            type: 'company_calendar',
            ownerId: 0,
            forCurrentUser: false,
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const events = response.getData()!.result
            events.forEach((event) => console.info('Event:', event.ID, event.NAME, event.DATE_FROM, event.DATE_TO))
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
          async function getNearestCalendarEvents() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'calendar.event.get.nearest',
                params: {
                  type: 'company_calendar',
                  ownerId: 0,
                  forCurrentUser: false,
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const events = response.getData().result
              events.forEach((event) => console.info('Event:', event.ID, event.NAME, event.DATE_FROM, event.DATE_TO))
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', getNearestCalendarEvents)
        </script>
        ```

    - Python

        ```python
        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.calendar.event.get.nearest(
                type="company_calendar",
                owner_id=0,
                for_current_user=False,
            ).response
            result = bitrix_response.result
            print(result)
        except BitrixAPIError as error:
            print(
                "Bitrix API error",
                f"error: {error.error}",
                f"error_description: {error.error_description}",
                sep="\n",
            )
        except BitrixSDKException as error:
            print(f"Bitrix SDK error: {error.message}")
        except Exception as error:
            print(f"Unexpected error: {error}")
        ```

    - PHP

        ```php
        try {
            $response = $b24Service
                ->core
                ->call(
                    'calendar.event.get.nearest',
                    [
                        'type'           => 'company_calendar',
                        'ownerId'        => 0,
                        'forCurrentUser' => false,
                    ]
                );

            $result = $response
                ->getResponseData()
                ->getResult();

            echo 'Success: ' . print_r($result, true);
            // Your required data processing logic
            processData($result);

        } catch (Throwable $e) {
            error_log($e->getMessage());
            echo 'Error getting calendar events: ' . $e->getMessage();
        }
        ```

    - BX24.js

        ```js
        BX24.callMethod(
            'calendar.event.get.nearest',
            {
                type: 'company_calendar',
                ownerId: 0,
                forCurrentUser: false
            }
        );
        ```

    - PHP CRest

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'calendar.event.get.nearest',
            [
                'type' => 'company_calendar',
                'ownerId' => 0,
                'forCurrentUser' => false
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "calendar.event.get.nearest", b24.Params{
        	"type":           "company_calendar",
        	"ownerId":        0,
        	"forCurrentUser": false,
        }, b24.WithIdempotent())
        if err != nil {
        	return fmt.Errorf("calendar.event.get.nearest: %w", err)
        }

        // The response arrives as json.RawMessage — unmarshal it
        // into a struct matching the response shape shown below on this page.
        fmt.Printf("%s\n", res.Result)
        ```

    {% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "ID": "8373",
            "PARENT_ID": "8373",
            "ACTIVE": "Y",
            "DELETED": "N",
            "CAL_TYPE": "user",
            "OWNER_ID": "1269",
            "NAME": "Sales team meeting",
            "DATE_FROM": "11/20/2026 10:00:00 am",
            "DATE_TO": "11/20/2026 11:00:00 am",
            "ORIGINAL_DATE_FROM": null,
            "DATE_FROM_TS_UTC": "1795147200",
            "DATE_TO_TS_UTC": "1795150800",
            "DT_SKIP_TIME": "N",
            "DT_LENGTH": 3600,
            "EVENT_TYPE": null,
            "CREATED_BY": "1269",
            "DATE_CREATE": "10/27/2026 07:14:36 pm",
            "TIMESTAMP_X": "10/27/2026 07:14:36 pm",
            "DESCRIPTION": "Discussing the plan for the week",
            "DT_FROM": null,
            "DT_TO": null,
            "PRIVATE_EVENT": "",
            "ACCESSIBILITY": "busy",
            "IMPORTANCE": "normal",
            "IS_MEETING": true,
            "MEETING_STATUS": "H",
            "MEETING_HOST": "1269",
            "MEETING": {
                "HOST_NAME": "Klaus Weber",
                "NOTIFY": true,
                "REINVITE": false,
                "ALLOW_INVITE": false,
                "HIDE_GUESTS": false,
                "MEETING_CREATOR": 1269,
                "LANGUAGE_ID": "de",
                "MAIL_FROM": ""
            },
            "LOCATION": "Meeting room 1",
            "REMIND": [
                {
                    "type": "min",
                    "count": 15
                }
            ],
            "COLOR": "#9cbe1c",
            "TEXT_COLOR": "",
            "RRULE": "",
            "EXDATE": "",
            "DAV_XML_ID": "20261120T070000Z-1de13363db84ad6757cacd3954782b9f@your-domain.bitrix24.com",
            "G_EVENT_ID": "",
            "DAV_EXCH_LABEL": "",
            "CAL_DAV_LABEL": "",
            "VERSION": "1",
            "ATTENDEES_CODES": [
                "U1269",
                "U1271"
            ],
            "RECURRENCE_ID": null,
            "RELATIONS": "",
            "SECTION_ID": "157",
            "SYNC_STATUS": null,
            "UF_CRM_CAL_EVENT": null,
            "UF_WEBDAV_CAL_EVENT": null,
            "SECTION_DAV_XML_ID": null,
            "DATE_FROM_FORMATTED": "Fri Nov 20 2026 10:00:00",
            "DATE_TO_FORMATTED": "Fri Nov 20 2026 11:00:00",
            "IS_DAYLIGHT_SAVING_TZ": "N",
            "SECT_ID": "157",
            "OPTIONS": null,
            "ATTENDEE_LIST": [
                {
                    "id": 1269,
                    "entryId": "8373",
                    "status": "H"
                },
                {
                    "id": 1271,
                    "entryId": "8375",
                    "status": "Q"
                }
            ],
            "COLLAB_ID": null,
            "attendeesEntityList": [
                {
                    "entityId": "user",
                    "id": 1269
                },
                {
                    "entityId": "user",
                    "id": 1271
                }
            ],
            "~DESCRIPTION": "Discussing the plan for the week",
            "~USER_OFFSET_FROM": 0,
            "~USER_OFFSET_TO": 0,
            "DT_FROM_TS": 1795158000,
            "DT_TO_TS": 1795161600,
            "~URL": "https://your-domain.bitrix24.com/company/personal/user/1269/calendar/?EVENT_ID=8373&EVENT_DATE=11%2F20%2F2026",
            "~WEEK_DAY": "Fri",
            "~FROM_TO_HTML": "11/20/2026,  10:00 am -  11:00 am",
            "~detailUrl": "/company/personal/user/1269/calendar/?EVENT_ID=8373&EVENT_DATE=11%2F20%2F2026%2010%3A00%3A00%20am",
            "TIMESTAMP_X_ISO": "2026-10-27T19:14:36+03:00",
            "DATE_CREATE_ISO": "2026-10-27T19:14:36+03:00"
        }
    ],
    "time": {
        "start": 1793117687,
        "finish": 1793117687.886115,
        "duration": 0.8861150741577148,
        "processing": 0,
        "date_start": "2026-10-27T19:14:47+03:00",
        "date_finish": "2026-10-27T19:14:47+03:00",
        "operating_reset_at": 1793118240,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of upcoming calendar events [(detailed description)](#event). If there are no events in the selected period, the array is empty ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Event Object {#event}

{% include notitle [calendar event fields](.././_includes/calendar_event_fields.md) %}

## Error Handling

HTTP status: **400**

```json
{
  "error": "",
  "error_description": "Access denied"
}
```
{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | Access denied | Access to the calendar type is restricted or the user has not activated the calendar functionality ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./calendar-event-add.md)
- [{#T}](./calendar-event-get-by-id.md)
- [{#T}](./calendar-event-get.md)
- [{#T}](./calendar-meeting-status-get.md)
