# Update Calendar calendar.section.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../scopes/permissions.md)
>
> Who can execute the method: any user

This method `calendar.section.update` updates the calendar: it changes only the parameters that are passed.

The method replaces the `access` object entirely. The calendar is identified only by the `id` parameter: the type and the owner are taken from the saved calendar, and the method checks rights by them. The `type` and `ownerId` parameters are required, but the values from the request do not get into the update. The method does not accept the `export` parameter: the export period cannot be changed through this method, it is always `all`.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **type***
[`string`](../data-types.md) | Calendar type. Possible values:
- `user` — user calendar
- `group` — group calendar

The parameter is required but does not affect the update: the method takes the type from the saved calendar ||
|| **ownerId**
[`integer`](../data-types.md) | Identifier of the calendar owner.

Required for all calendar types except `user`. Does not affect the update: the method takes the owner from the saved calendar ||
|| **id***
[`integer`](../data-types.md) | Identifier of an existing calendar ||
|| **name**
[`string`](../data-types.md) | Calendar name. The method ignores an empty string and keeps the previous name ||
|| **description**
[`string`](../data-types.md) | Calendar description. The method ignores an empty string: the description cannot be cleared with this method ||
|| **color**
[`string`](../data-types.md) | Calendar color in the `#RRGGBB` format. If the value has fewer than six hexadecimal characters, the method saves the color `#9dcf00` ||
|| **text_color**
[`string`](../data-types.md) | Text color in the calendar in the `#RRGGBB` format. If the value has fewer than six hexadecimal characters, the method saves the color `#9dcf00` ||
|| **access**
[`object`](../data-types.md) | Object of calendar access rights [(detailed description)](#access) ||
|#

### Parameter access {#access}

The object format is described in the [Parameter access](./calendar-section-add.md#access) section of the `calendar.section.add` method. The value is the same object that comes in the `ACCESS` field of the [calendar.section.get](./calendar-section-get.md) response.

If the user has no right to manage calendar access, the method silently ignores the passed object and the calendar rights remain the same. If the right is present, an empty `{}` object resets the rights to the default values.

Value example:

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
    -d '{"id":325,"type":"user","ownerId":2,"name":"Changed Section Name","description":"New description for section","color":"#9cbeaa","text_color":"#283099"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.section.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":325,"type":"user","ownerId":2,"name":"Changed Section Name","description":"New description for section","color":"#9cbeaa","text_color":"#283099","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.section.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CalendarSectionUpdateResult = number

    try {
      const response = await $b24.actions.v2.call.make<CalendarSectionUpdateResult>({
        method: 'calendar.section.update',
        params: {
          id: 325,
          type: 'user',
          ownerId: 2,
          name: 'Changed Section Name',
          description: 'New description for section',
          color: '#9cbeaa',
          text_color: '#283099',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated calendar section ID:', result)
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
      async function updateCalendarSection() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.section.update',
            params: {
              id: 325,
              type: 'user',
              ownerId: 2,
              name: 'Changed Section Name',
              description: 'New description for section',
              color: '#9cbeaa',
              text_color: '#283099',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated calendar section ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCalendarSection)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.calendar.section.update(
            type="user",
            owner_id=2,
            bitrix_id=325,
            name="Changed Section Name",
            description="New description for section",
            color="#9cbeaa",
            text_color="#283099",
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
                'calendar.section.update',
                [
                    'id'          => 325,
                    'type'        => 'user',
                    'ownerId'     => 2,
                    'name'        => 'Changed Section Name',
                    'description' => 'New description for section',
                    'color'       => '#9cbeaa',
                    'text_color'  => '#283099',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        // Next, process $result in your code

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating calendar section: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.section.update',
        {
            id: 325,
            type: 'user',
            ownerId: 2,
            name: 'Changed Section Name',
            description: 'New description for section',
            color: '#9cbeaa',
            text_color: '#283099'
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.section.update',
        [
            'id' => 325,
            'type' => 'user',
            'ownerId' => 2,
            'name' => 'Changed Section Name',
            'description' => 'New description for section',
            'color' => '#9cbeaa',
            'text_color' => '#283099'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "calendar.section.update", b24.Params{
    	"id":          325,
    	"type":        "user",
    	"ownerId":     2,
    	"name":        "Changed Section Name",
    	"description": "New description for section",
    	"color":       "#9cbeaa",
    	"text_color":  "#283099",
    })
    if err != nil {
    	return fmt.Errorf("calendar.section.update: %w", err)
    }

    var value b24.ID
    if err := json.Unmarshal(res.Result, &value); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("result:", value)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 325,
    "time": {
        "start": 1733828946.418185,
        "finish": 1733828946.650208,
        "duration": 0.23202300071716309,
        "processing": 0.0054471492767333984,
        "date_start": "2024-12-10T09:09:06+00:00",
        "date_finish": "2024-12-10T09:09:06+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the updated calendar ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The mandatory parameter \"type\" is not specified for the method \"calendar.section.update\""
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | The mandatory parameter "type" is not specified for the method "calendar.section.update" | The required parameter `type` is missing ||
|| `400` | Empty value | The mandatory parameter "ownerId" is not specified for the method "calendar.section.update" | The required parameter `ownerId` is missing and the `type` parameter is not equal to `user` ||
|| `400` | Empty value | Section ID is not specified | The required parameter `id` is missing. An administrator gets this error also when there is no calendar with the specified `id` ||
|| `400` | Empty value | Invalid parameter value for "name" | Incorrect data format in the `name` field ||
|| `400` | Empty value | Invalid parameter value for "description" | Incorrect data format in the `description` field ||
|| `400` | Empty value | Access denied | No rights to edit the calendar. A user who is not an administrator gets this error also when there is no calendar with the specified `id` ||
|| `400` | Empty value | Error while editing section | Another error ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./calendar-section-add.md)
- [{#T}](./calendar-section-get.md)
- [{#T}](./calendar-section-delete.md)
