# Get a List of Resources calendar.resource.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: any user except extranet users

The `calendar.resource.list` method retrieves a list of all resources in Bitrix24. Resources deleted with the [calendar.resource.delete](./calendar-resource-delete.md) method are not included in the list. How resource booking works is described in the [methods overview](./index.md).

The method has no filtering, sorting, or pagination: it returns all resources in a single response and ignores the `start` parameter.

Resource IDs from the response are needed for the [calendar.resource.update](./calendar-resource-update.md) and [calendar.resource.delete](./calendar-resource-delete.md) methods and for the `filter.resourceTypeIdList` parameter of the [calendar.resource.booking.list](./calendar-resource-booking-list.md) method.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.resource.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.resource.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each resource returned in result[]
    type CalendarResourceItem = {
      ID: string
      NAME: string
      CREATED_BY: string
    }

    try {
      // calendar.resource.list has no pagination: it returns all active resources in one response
      const response = await $b24.actions.v2.call.make<CalendarResourceItem[]>({
        method: 'calendar.resource.list',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Resources:', result, 'Count:', result.length)
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
      async function fetchCalendarResources() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // calendar.resource.list has no pagination: it returns all active resources in one response
          const response = await $b24.actions.v2.call.make({
            method: 'calendar.resource.list',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Resources:', result, 'Count:', result.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchCalendarResources)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.calendar.resource.list().response
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
                'calendar.resource.list',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching calendar resources: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.resource.list'
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.resource.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "calendar.resource.list", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("calendar.resource.list: %w", err)
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
            "ID": "198",
            "NAME": "Car",
            "CREATED_BY": "1"
        },
        {
            "ID": "199",
            "NAME": "Bowling lane 1",
            "CREATED_BY": "1"
        }
    ],
    "time": {
        "start": 1733318565.183275,
        "finish": 1733318565.695058,
        "duration": 0.5117831230163574,
        "processing": 0.29406094551086426,
        "date_start": "2024-12-04T13:22:45+00:00",
        "date_finish": "2024-12-04T13:22:45+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Array of objects. Each object describes a [resource](#resource). If there are no resources, an empty array is returned ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Resource Object {#resource}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Resource identifier ||
|| **NAME**
[`string`](../../data-types.md) | Resource name ||
|| **CREATED_BY**
[`string`](../../data-types.md) | Identifier of the user who created the resource ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Access denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | Access denied | The method is called by an extranet user ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./calendar-resource-add.md)
- [{#T}](./calendar-resource-update.md)
- [{#T}](./calendar-resource-delete.md)
- [{#T}](./calendar-resource-booking-list.md)
