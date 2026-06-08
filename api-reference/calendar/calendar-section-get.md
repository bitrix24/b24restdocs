# Get Calendar List calendar.section.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves a list of calendars.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type: 
- `user` — user calendar
- `group` — group calendar
- `company_calendar` — company calendar 
- `location` — meeting room calendar. Used for booking time in the meeting room calendar through a third-party application
- other types, including custom ||
|| **ownerId***
[`integer`](../data-types.md) | Identifier of the calendar owner.

This parameter can be omitted if the calendar type is `user`. In this case, the current user's identifier is used.

For the `location` calendar type, the `ownerId` parameter must be set to `0` ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":1}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.section.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":1,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.section.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each CalendarSection returned in result[]
    type CalendarSection = {
      ID: string
      NAME: string
      GAPI_CALENDAR_ID: string | null
      DESCRIPTION: string
      COLOR: string
      TEXT_COLOR: string
      EXPORT: { ALLOW: boolean }
      CAL_TYPE: string
      OWNER_ID: string
      CREATED_BY: string
      DATE_CREATE: ISODate | null
      TIMESTAMP_X: ISODate | null
      CAL_DAV_CON: string | null
      SYNC_TOKEN: string | null
      PAGE_TOKEN: string | null
      EXTERNAL_TYPE: string
      ACCESS: Record<string, number>
      IS_COLLAB: boolean
      PERM: {
        view_time: boolean
        view_title: boolean
        view_full: boolean
        add: boolean
        edit: boolean
        edit_section: boolean
        access: boolean
      }
    }

    try {
      // calendar.section.get returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<CalendarSection[]>({
        method: 'calendar.section.get',
        params: {
          type: 'user',
          ownerId: 1,
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Sections count:', result.length, 'first section:', result[0])
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
      async function getCalendarSections() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // calendar.section.get returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'calendar.section.get',
            params: {
              type: 'user',
              ownerId: 1,
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Sections count:', result.length, 'first section:', result[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCalendarSections)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'calendar.section.get',
                [
                    'type'    => 'user',
                    'ownerId' => 1
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
        echo 'Error getting calendar section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.section.get',
        {
            type: 'user',
            ownerId: 1
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.section.get',
        [
            'type' => 'user',
            'ownerId' => 1
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "190",
            "NAME": "New Section",
            "GAPI_CALENDAR_ID": null,
            "DESCRIPTION": "Description for section",
            "COLOR": "#9cbeee",
            "TEXT_COLOR": "#283000",
            "EXPORT": {
                "ALLOW": true
            },
            "CAL_TYPE": "user",
            "OWNER_ID": "1",
            "CREATED_BY": "1",
            "DATE_CREATE": "2024-12-10 06:36:00",
            "TIMESTAMP_X": "2024-12-10 06:36:00",
            "CAL_DAV_CON": null,
            "SYNC_TOKEN": null,
            "PAGE_TOKEN": null,
            "EXTERNAL_TYPE": "local",
            "ACCESS": {
                "D114": 17,
                "G2": 13,
                "U2": 15,
                "U1": 19
            },
            "IS_COLLAB": false,
            "PERM": {
                "view_time": true,
                "view_title": true,
                "view_full": true,
                "add": true,
                "edit": true,
                "edit_section": true,
                "access": true
            }
        },
        {
            "ID": "191",
            ...
        }
        {
            "ID": "192",
            ...
        }
    ],
    "time": {
        "start": 1733828946.418185,
        "finish": 1733828946.650208,
        "duration": 0.23202300071716309,
        "processing": 0.0054471492767333984,
        "date_start": "2024-12-08T11:09:06+00:00",
        "date_finish": "2024-12-08T11:09:06+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Array of calendars ||
|| **ID**
[`string`](../data-types.md) | Calendar identifier ||
|| **NAME**
[`string`](../data-types.md) | Calendar name ||
|| **GAPI_CALENDAR_ID**
[`string`](../data-types.md) | Synchronization identifier ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Calendar description ||
|| **COLOR**
[`string`](../data-types.md) | Calendar color ||
|| **TEXT_COLOR**
[`string`](../data-types.md) | Text color in the calendar ||
|| **EXPORT**
[`object`](../data-types.md) | Object with [calendar export parameters](#export)
 ||
|| **CAL_TYPE**
[`string`](../data-types.md) | Calendar type ||
|| **OWNER_ID**
[`string`](../data-types.md) | Identifier of the calendar owner. 

For the user calendar type `user`, this field contains the user identifier. For the group calendar `group`, it contains the group identifier ||
|| **CREATED_BY**
[`string`](../data-types.md) | Identifier of the calendar creator ||
|| **DATE_CREATE**
[`datetime`](../data-types.md) | Calendar creation date ||
|| **TIMESTAMP_X**
[`datetime`](../data-types.md) | Calendar modification date ||
|| **CAL_DAV_CON**
[`string`](../data-types.md) | Synchronization identifier ||
|| **SYNC_TOKEN**
[`string`](../data-types.md) | Synchronization identifier ||
|| **PAGE_TOKEN**
[`string`](../data-types.md) | Synchronization identifier ||
|| **EXTERNAL_TYPE**
[`string`](../data-types.md) | Provider type for synchronization ||
|| **ACCESS**
[`object`](../data-types.md) | Object containing access data for the calendar. 

The object key is the access permission identifier. You can obtain the name of the access permission using the [access.name](../common/system/access-name.md) method. Determine access rights for the current user using the [user.access](../common/users/user-access.md) method.

The object value contains a numerical identifier of the access permission. Access permission identifiers differ across different accounts. Currently, only the portal administrator in the on-premise version of Bitrix24 can retrieve all identifiers. ||
|| **IS_COLLAB**
[`boolean`](../data-types.md) | Flag indicating whether the calendar belongs to a collaboration ||
|| **PERM**
[`object`](../data-types.md) | Object containing [access permissions](#perm) for the current user to the calendar ||
|#

#### EXPORT Object {#export}

#|
|| **Name**
`type` | **Description** ||
|| **ALLOW**
[`boolean`](../data-types.md) | Calendar export is allowed ||
|#

#### PERM Object {#perm}

#|
|| **Name**
`type` | **Description** ||
|| **view_time**
[`boolean`](../data-types.md) | View calendar event times ||
|| **view_title**
[`boolean`](../data-types.md) | View calendar event titles ||
|| **view_full**
[`boolean`](../data-types.md) | Full access to event information in the calendar ||
|| **add**
[`boolean`](../data-types.md) | Add events to the calendar ||
|| **edit**
[`boolean`](../data-types.md) | Edit events in the calendar ||
|| **edit_section**
[`boolean`](../data-types.md) | Edit the calendar ||
|| **access**
[`boolean`](../data-types.md) | Full access to the calendar ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The required parameter "type" for the method "calendar.section.get" is not set"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| Empty string | The required parameter "type" for the method "calendar.section.get" is not set | The required parameter `type` was not provided ||
|| Empty string | The required parameter "ownerId" for the method "calendar.section.get" is not set | The required parameter `ownerId` was not provided and the `type` parameter is not equal to `user` ||
|| Empty string | Access denied | Access to the method is prohibited for external users ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-section-add.md)
- [{#T}](./calendar-section-update.md)
- [{#T}](./calendar-section-delete.md)