# Add Calendar calendar.section.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user except collab users

The `calendar.section.add` method adds a new calendar.

The method does not check whether the user from `ownerId` exists or whether the caller has rights to their calendar.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type. Possible values: 
- `user` — user calendar 
- `group` — group calendar ||
|| **ownerId**
[`integer`](../data-types.md) | Identifier of the calendar owner.

Required for all calendar types except `user`. For the `user` type, the parameter can be omitted — the current user's ID is used then ||
|| **name***
[`string`](../data-types.md) | Calendar name ||
|| **description**
[`string`](../data-types.md) | Calendar description ||
|| **color**
[`string`](../data-types.md) | Calendar color in `#RRGGBB` format. The default value is `#E6A469` ||
|| **text_color**
[`string`](../data-types.md) | Text color in the calendar in `#RRGGBB` format. The default value is `#000000` ||
|| **export**
[`object`](../data-types.md) | Object of the calendar export parameters [(detailed description)](#export) ||
|| **access**
[`object`](../data-types.md) | Object of the calendar access rights [(detailed description)](#access) ||
|#

### Export Parameter {#export}

Export settings are saved only if both fields are passed — `ALLOW` and `SET`. If only one field is passed, the method creates a calendar with the default values.

#|
|| **Name**
`type` | **Description** ||
|| **ALLOW**
[`boolean`](../data-types.md) | Allow calendar export. Possible values:
- `true` — allow
- `false` — disallow ||
|| **SET**
[`string`](../data-types.md) | Period for export. Possible values:
- `all` — for the entire period
- `3_9` — 3 months before and 9 after
- `6_12` — 6 months before and 12 after

The method does not save the parameter value: the export period is always `all` ||
|#

### Access Parameter {#access}

The object key is the access code, the value is the numeric permission identifier. The format matches the `ACCESS` field in the [calendar.section.get](./calendar-section-get.md) response.

If the user has no right to manage calendar access, the method silently ignores the passed object and sets the default rights. An empty `{}` object also sets the default rights.

#|
|| **Name**
`type` | **Description** ||
|| **Access Code**
[`string`](../data-types.md) | Code of the object the right is granted to: `U<id>` — user, `G<id>` — user group, `D<id>` — department. For a group calendar, the code denotes the role in the group: `SG<id>_A` — group owner, `SG<id>_E` — moderators, `SG<id>_K` — all members. The right itself is set by the value under the key. For example, `U1` is the user with ID `1` ||
|| **Permission Identifier**
[`integer`](../data-types.md) | Numeric code of the access right permission. The identifiers differ between Bitrix24 instances: an administrator of an on-premise Bitrix24 can find them out ||
|#

Example value:

```json
{
    "access": {
        "U1": 19,
        "G2": 13
    }
}
```

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":2,"name":"New Section","description":"Description for section","color":"#9cbeee","text_color":"#283000","export":{"ALLOW":false,"SET":"3_9"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.section.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"type":"user","ownerId":2,"name":"New Section","description":"Description for section","color":"#9cbeee","text_color":"#283000","export":{"ALLOW":false,"SET":"3_9"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.section.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CalendarSectionAddResult = number

    try {
      const response = await $b24.actions.v2.call.make<CalendarSectionAddResult>({
        method: 'calendar.section.add',
        params: {
          type: 'user',
          ownerId: 2,
          name: 'New Section',
          description: 'Description for section',
          color: '#9cbeee',
          text_color: '#283000',
          export: {
            ALLOW: false,
            SET: '3_9',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created calendar with ID:', result)
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
      async function addCalendarSection() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.section.add',
            params: {
              type: 'user',
              ownerId: 2,
              name: 'New Section',
              description: 'Description for section',
              color: '#9cbeee',
              text_color: '#283000',
              export: {
                ALLOW: false,
                SET: '3_9',
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
          console.info('Created calendar with ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCalendarSection)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.calendar.section.add(
            type="user",
            owner_id=2,
            name="New Section",
            description="Description for section",
            color="#9cbeee",
            text_color="#283000",
            export={
                "ALLOW": False,
                "SET": "3_9",
            },
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
                'calendar.section.add',
                [
                    'type'        => 'user',
                    'ownerId'     => 2,
                    'name'        => 'New Section',
                    'description' => 'Description for section',
                    'color'       => '#9cbeee',
                    'text_color'  => '#283000',
                    'export'      => [
                        'ALLOW' => false,
                        'SET'   => '3_9',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Next, process $result in your code

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding calendar section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.section.add',
        {
            type: 'user',
            ownerId: 2,
            name: 'New Section',
            description: 'Description for section',
            color: '#9cbeee',
            text_color: '#283000',
            export: {
                ALLOW: false,
                SET: '3_9'
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.section.add',
        [
            'type' => 'user',
            'ownerId' => 2,
            'name' => 'New Section',
            'description' => 'Description for section',
            'color' => '#9cbeee',
            'text_color' => '#283000',
            'export' => [
                'ALLOW' => false,
                'SET' => '3_9'
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "calendar.section.add", b24.Params{
    	"type":        "user",
    	"ownerId":     2,
    	"name":        "New Section",
    	"description": "Description for section",
    	"color":       "#9cbeee",
    	"text_color":  "#283000",
    	"export": b24.Params{
    		"ALLOW": false,
    		"SET":   "3_9",
    	},
    })
    if err != nil {
    	return fmt.Errorf("calendar.section.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 190,
    "time": {
        "start": 1733812564.64201,
        "finish": 1733812565.71673,
        "duration": 1.0747201442718506,
        "processing": 0.05963897705078125,
        "date_start": "2024-12-08T06:36:04+00:00",
        "date_finish": "2024-12-08T06:36:05+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the created calendar ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The mandatory parameter \"type\" is not specified for the method \"calendar.section.add\""
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | The mandatory parameter "type" is not specified for the method "calendar.section.add" | The required parameter `type` is not provided ||
|| `400` | Empty value | The mandatory parameter "ownerId" is not specified for the method "calendar.section.add" | The required parameter `ownerId` is not provided and the `type` parameter is not equal to `user` ||
|| `400` | Empty value | Invalid parameter value for "name" | The `name` parameter is not provided or is not a string ||
|| `400` | Empty value | Invalid parameter value for "description" | Incorrect data format in the `description` field ||
|| `400` | Empty value | Access denied | For `type` with the value `group`, the user has no right to modify group calendars, or the method is called by a collab user ||
|| `400` | Empty value | Error while creating section | Another error ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-section-update.md)
- [{#T}](./calendar-section-get.md)
- [{#T}](./calendar-section-delete.md)
