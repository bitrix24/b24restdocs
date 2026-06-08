# Get Event by ID calendar.event.getbyid

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves information about a calendar event by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the event.

You can obtain the identifier using the [calendar.event.get](./calendar-event-get.md) or [calendar.event.get.nearest](./calendar-event-get-nearest.md) methods ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":324}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.event.getbyid
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":324,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.event.getbyid
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CalendarEventResult = {
      ID: string
      PARENT_ID: string
      DELETED: string
      CAL_TYPE: string
      OWNER_ID: string
      NAME: string
      DATE_FROM: string
      DATE_TO: string
      DESCRIPTION: string
      EVENT_TYPE: string
      TZ_FROM: string
      TZ_TO: string
      SECTION_ID: string
      CREATED_BY: string
      ACCESSIBILITY: string
      IMPORTANCE: string
      IS_MEETING: boolean
      MEETING_STATUS: string
      COLOR: string
      LOCATION: string
    }

    try {
      const response = await $b24.actions.v2.call.make<CalendarEventResult>({
        method: 'calendar.event.getbyid',
        params: {
          id: 324,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Event:', result.ID, result.NAME, result.DATE_FROM, result.DATE_TO)
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
      async function getCalendarEventById() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.event.getbyid',
            params: {
              id: 324,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Event:', result.ID, result.NAME, result.DATE_FROM, result.DATE_TO)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCalendarEventById)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'calendar.event.getbyid',
                [
                    'id' => 324
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your logic for processing data
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting event by ID: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.event.getbyid',
        {
            id: 324
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.event.getbyid',
        [
            'id' => 324
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": "1265",
        "PARENT_ID": "1265",
        "DELETED": "N",
        "CAL_TYPE": "user",
        "OWNER_ID": "1",
        "NAME": "Event Name",
        "DATE_FROM": "12/11/2024 05:59:00 pm",
        "DATE_TO": "12/11/2024 06:59:00 pm",
        "ORIGINAL_DATE_FROM": null,
        "TZ_FROM": "Europe/Riga",
        "TZ_TO": "Europe/Riga",
        "TZ_OFFSET_FROM": "7200",
        "TZ_OFFSET_TO": "7200",
        "DATE_FROM_TS_UTC": "1733932740",
        "DATE_TO_TS_UTC": "1733936340",
        "DT_SKIP_TIME": "N",
        "DT_LENGTH": 3600,
        "EVENT_TYPE": null,
        "CREATED_BY": "1",
        "DATE_CREATE": "12/05/2024 01:48:41 pm",
        "TIMESTAMP_X": "12/05/2024 01:48:41 pm",
        "DESCRIPTION": "Description for event",
        "PRIVATE_EVENT": "",
        "ACCESSIBILITY": "free",
        "IMPORTANCE": "normal",
        "IS_MEETING": true,
        "MEETING_STATUS": "H",
        "MEETING_HOST": "1",
        "MEETING": {
            "HOST_NAME": "User Name",
            "NOTIFY": false,
            "REINVITE": false,
            "ALLOW_INVITE": false,
            "HIDE_GUESTS": false,
            "MEETING_CREATOR": 1,
            "LANGUAGE_ID": "de",
            "MAIL_FROM": ""
        },
        "LOCATION": "test location",
        "REMIND": [
            {
                "type": "min",
                "count": 50
            }
        ],
        "COLOR": "#9dcf00",
        "RRULE": {
            "FREQ": "WEEKLY",
            "BYDAY": {
                "MO": "MO",
                "WE": "WE"
            },
            "INTERVAL": 1,
            "UNTIL": "12/24/2024",
            "~UNTIL": "12/24/2024",
            "UNTIL_TS": 1734998400
        },
        "EXDATE": "11/28/2024;12/05/2024;12/12/2024;12/19/2024;12/26/2024",
        "DAV_XML_ID": "20241211T155900Z-534185204b362e9be7e261e92ccd9078@b24evo.lan",
        "G_EVENT_ID": "",
        "DAV_EXCH_LABEL": "",
        "CAL_DAV_LABEL": "",
        "VERSION": "1",
        "ATTENDEES_CODES": [
            "U1"
        ],
        "RECURRENCE_ID": 1272,
        "RELATIONS": {
            "ORIGINAL_RECURSION_ID": 1271,
            "COMMENT_XML_ID": "EVENT_1271_12/23/2024"
        },
        "SECTION_ID": "4",
        "SYNC_STATUS": null,
        "UF_CRM_CAL_EVENT": [
            "CO_1",
            "L_5"
        ],
        "UF_WEBDAV_CAL_EVENT": false,
        "SECTION_DAV_XML_ID": null,
        "DATE_FROM_FORMATTED": "Wed Dec 11 2024 17:59:00",
        "DATE_TO_FORMATTED": "Wed Dec 11 2024 18:59:00",
        "SECT_ID": "4",
        "ATTENDEE_LIST": [
            {
                "id": 1,
                "entryId": "1265",
                "status": "H"
            }
        ],
        "COLLAB_ID": null,
        "~RRULE_DESCRIPTION": "every week on: Mon, Wed, from 12/11/2024 to 12/24/2024",
        "attendeesEntityList": [
            {
                "entityId": "user",
                "id": 1
            }
        ],
        "~DESCRIPTION": "Description for event",
        "~USER_OFFSET_FROM": 7200,
        "~USER_OFFSET_TO": 7200
    },
    "time": {
        "start": 1733406529.495513,
        "finish": 1733406529.744687,
        "duration": 0.2491741180419922,
        "processing": 0.0327458381652832,
        "date_start": "2024-12-05T13:48:49+00:00",
        "date_finish": "2024-12-05T13:48:49+00:00"
    }
}
```

### Returned Data

{% include notitle [calendar event fields](.././_includes/calendar_event_fields.md) %}

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "id" for the method "calendar.event.getbyid" is not set"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "id" for the method "calendar.event.getbyid" is not set | The required parameter `id` was not provided ||
|| Empty string | Access denied | Access to the method is prohibited for external users ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}