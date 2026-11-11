# Get Calendar List calendar.section.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user except extranet users

This method `calendar.section.get` retrieves a list of calendars of the specified type and owner to which the user has the right to view event time.

The method has no pagination.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type. Standard values:
- `user` — user calendar
- `group` — group calendar
- `company_calendar` — company calendar
- `location` — meeting room calendar, used for booking time in the meeting room calendar through a third-party application
- `resource` — resource calendar

Bitrix24 may have additional calendar types, including manually created ones. Their string code comes in the `CAL_TYPE` field of the response, and the same value can be passed in `type` ||
|| **ownerId**
[`integer`](../data-types.md) | Identifier of the calendar owner.

Required for all calendar types except `user`. For the `user` type, the parameter can be omitted — then the current user's identifier is used.

What to pass:
- `user` — user identifier
- `group` — group identifier
- `company_calendar` and `location` — `0`

The method does not return resources created by the [calendar.resource.add](./resource/calendar-resource-add.md) method: their owner is not filled in, so the filter by `ownerId` does not find them. Use [calendar.resource.list](./resource/calendar-resource-list.md) for resources ||
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
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each CalendarSection returned in result[]
    type CalendarSection = {
      ID: string
      NAME: string
      GAPI_CALENDAR_ID: string | null
      DESCRIPTION: string
      COLOR: string
      TEXT_COLOR: string | null
      EXPORT: { ALLOW: boolean; PATH: string; LINK: string }
      CAL_TYPE: string
      OWNER_ID: string
      CREATED_BY: string
      DATE_CREATE: string
      TIMESTAMP_X: string
      CAL_DAV_CON: number | string | false | null
      SYNC_TOKEN: string | null
      PAGE_TOKEN: string | null
      EXTERNAL_TYPE: string
      ACCESS?: Record<string, number>
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
      const response = await $b24.actions.v2.call.make<CalendarSection[]>({
        method: 'calendar.section.get',
        params: {
          type: 'user',
          ownerId: 1,
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

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.section.get',
            params: {
              type: 'user',
              ownerId: 1,
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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.calendar.section.get(
            type="user",
            owner_id=1,
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
        // Next, process $result in your code

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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "calendar.section.get", b24.Params{
    	"type":    "user",
    	"ownerId": 1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("calendar.section.get: %w", err)
    }

    var sections []struct {
    	ID             string  `json:"ID"`
    	Name           string  `json:"NAME"`
    	GapiCalendarID *string `json:"GAPI_CALENDAR_ID"`
    	Description    string  `json:"DESCRIPTION"`
    	Color          string  `json:"COLOR"`
    	TextColor      *string `json:"TEXT_COLOR"`
    	Export         struct {
    		Allow bool   `json:"ALLOW"`
    		Path  string `json:"PATH"`
    		Link  string `json:"LINK"`
    	} `json:"EXPORT"`
    	CalType      string          `json:"CAL_TYPE"`
    	OwnerID      string          `json:"OWNER_ID"`
    	CreatedBy    string          `json:"CREATED_BY"`
    	DateCreate   string          `json:"DATE_CREATE"`
    	TimestampX   string          `json:"TIMESTAMP_X"`
    	CalDavCon    json.RawMessage `json:"CAL_DAV_CON"`
    	SyncToken    *string         `json:"SYNC_TOKEN"`
    	PageToken    *string         `json:"PAGE_TOKEN"`
    	ExternalType string          `json:"EXTERNAL_TYPE"`
    	Access       map[string]int  `json:"ACCESS"`
    	IsCollab     bool            `json:"IS_COLLAB"`
    	Perm         struct {
    		ViewTime    bool `json:"view_time"`
    		ViewTitle   bool `json:"view_title"`
    		ViewFull    bool `json:"view_full"`
    		Add         bool `json:"add"`
    		Edit        bool `json:"edit"`
    		EditSection bool `json:"edit_section"`
    		Access      bool `json:"access"`
    	} `json:"PERM"`
    }
    if err := json.Unmarshal(res.Result, &sections); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("sections:", len(sections))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

The first array element is shown in full, the second one is shortened.

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
                "ALLOW": true,
                "PATH": "https://example.bitrix24.com/company/personal/user/1/calendar/",
                "LINK": "&type=user&owner=1&ncc=1&user=1&sec_id=190&sign=704a559a691722ae080fa420dcb9d7f8&bx_hit_hash=e7A0WxIDCc1ZVozSP9S6CsxsqXG5efsL"
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
            "NAME": "Work Section",
            "CAL_TYPE": "user",
            "OWNER_ID": "1",
            "IS_COLLAB": false
        }
    ],
    "time": {
        "start": 1733824626.418185,
        "finish": 1733824626.650208,
        "duration": 0.23202300071716309,
        "processing": 0.0054471492767333984,
        "date_start": "2024-12-10T09:57:06+00:00",
        "date_finish": "2024-12-10T09:57:06+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Array of calendars [(detailed description)](#calendar-section) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. A top-level response field, it does not belong to the `result` array elements ||
|#

#### Calendar Object {#calendar-section}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Calendar identifier ||
|| **NAME**
[`string`](../data-types.md) | Calendar name ||
|| **GAPI_CALENDAR_ID**
[`string`](../data-types.md) | Google calendar identifier for synchronization. Can be `null` if synchronization is not configured ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Calendar description ||
|| **COLOR**
[`string`](../data-types.md) | Calendar color in the `#RRGGBB` format ||
|| **TEXT_COLOR**
[`string`](../data-types.md) | Text color in the calendar in the `#RRGGBB` format. Can be `null` if the color is not set: this is how an automatically created calendar comes ||
|| **EXPORT**
[`object`](../data-types.md) | Object with calendar export parameters [(detailed description)](#export) ||
|| **CAL_TYPE**
[`string`](../data-types.md) | Calendar type ||
|| **OWNER_ID**
[`string`](../data-types.md) | Identifier of the calendar owner.

Rules by type:
- `user` — user identifier
- `group` — group identifier
- `company_calendar` — `0`
- `location` — `0` ||
|| **CREATED_BY**
[`string`](../data-types.md) | Identifier of the calendar creator ||
|| **DATE_CREATE**
[`string`](../data-types.md) | Calendar creation date as a string, for example `2024-12-10 06:36:00` ||
|| **TIMESTAMP_X**
[`string`](../data-types.md) | Calendar modification date as a string, for example `2024-12-10 06:36:00` ||
|| **CAL_DAV_CON**
[```integer|boolean|string```](../data-types.md) | CalDAV connection identifier. The value depends on the calendar type:
- for a user calendar — a number if the connection exists, `false` if the connection is deleted, `null` if synchronization is not configured
- for calendars of other types — always `false` ||
|| **SYNC_TOKEN**
[`string`](../data-types.md) | Calendar synchronization token. Can be `null` if synchronization is not configured ||
|| **PAGE_TOKEN**
[`string`](../data-types.md) | Calendar page-by-page synchronization token. Can be `null` if synchronization is not configured ||
|| **EXTERNAL_TYPE**
[`string`](../data-types.md) | Provider type for synchronization. For a local calendar — `local` ||
|| **ACCESS**
[`object`](../data-types.md) | Object containing access data for the calendar.

The field comes for all calendars returned by the method. The exception is collaboration users without the right to manage calendar access: they do not get the field.

The object key is the code of the object the access is granted to: `U<id>` — user, `G<id>` — user group, `D<id>` — department. For a group calendar, the code denotes the role in the group: `SG<id>_A` — group owner, `SG<id>_E` — moderators, `SG<id>_K` — all members. The value is a numerical identifier of the access permission. You can obtain the name of the access permission using the [access.name](../common/system/access-name.md) method. Determine access rights for the current user using the [user.access](../common/users/user-access.md) method.

Access permission identifiers differ across Bitrix24 accounts. Only the administrator of the on-premise version of Bitrix24 can retrieve all identifiers ||
|| **IS_COLLAB**
[`boolean`](../data-types.md) | Flag indicating whether the calendar belongs to a collaboration ||
|| **PERM**
[`object`](../data-types.md) | Object containing access permissions for the current user to the calendar [(detailed description)](#perm). The field always comes: the method does not return calendars without the right to view event time ||
|#

#### EXPORT Object {#export}

#|
|| **Name**
`type` | **Description** ||
|| **ALLOW**
[`boolean`](../data-types.md) | Calendar export is allowed. Always `true` in the response ||
|| **PATH**
[`string`](../data-types.md) | Full calendar address in Bitrix24, for example `https://example.bitrix24.com/company/personal/user/1/calendar/` ||
|| **LINK**
[`string`](../data-types.md) | Signed parameters of the export link: a query string with `sec_id`, `sign`, and `bx_hit_hash` that is appended to the calendar address. It is not a link by itself ||
|#

The method does not return the `SET` field with the export period: the `export.SET` value passed to [calendar.section.add](./calendar-section-add.md) is not saved, the export period is always `all`.

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

The `PERM` values describe the current user's rights to this calendar, so some of them may be `false`. For example, for another user's calendar without granted rights: `view_time` is `true`, while `add`, `edit`, `edit_section`, and `access` are `false`.

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The mandatory parameter \"type\" is not specified for the method \"calendar.section.get\""
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | The mandatory parameter "type" is not specified for the method "calendar.section.get" | The required parameter `type` was not provided ||
|| `400` | Empty value | The mandatory parameter "ownerId" is not specified for the method "calendar.section.get" | The required parameter `ownerId` was not provided and the `type` parameter is not equal to `user` ||
|| `400` | Empty value | Access denied | The method is unavailable to extranet users ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-section-add.md)
- [{#T}](./calendar-section-update.md)
- [{#T}](./calendar-section-delete.md)
