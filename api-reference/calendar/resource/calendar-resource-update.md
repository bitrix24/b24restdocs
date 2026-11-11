# Update resource calendar.resource.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`calendar`](../../scopes/permissions.md)
>
> Who can execute the method: an administrator or a user with permission to edit resource calendars

The `calendar.resource.update` method changes the resource name. How resource booking works is described in the [methods overview](./index.md).

{% note warning "" %}

- The method does not check that `resourceId` belongs to a resource. If you pass the ID of another calendar, such as a personal one, the method renames it and turns it into a resource: the calendar disappears from the user's calendars and appears in the [calendar.resource.list](./calendar-resource-list.md) list. Before calling the method, make sure the ID is in the response of that method.
- On every call, the method resets the resource access permissions to the default values: all employees can edit the resource, and the user who called the method can manage access. Permissions configured in the interface are not retained.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **resourceId***
[`integer`](../../data-types.md) | Resource identifier. The value must be greater than `0`.

You can obtain the identifier using the resource creation method [calendar.resource.add](./calendar-resource-add.md) or the resource listing method [calendar.resource.list](./calendar-resource-list.md) ||
|| **name***
[`string`](../../data-types.md) | New name of the resource. An empty string is treated as a missing parameter ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"resourceId":197,"name":"Changed Resource Name"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/calendar.resource.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"resourceId":197,"name":"Changed Resource Name","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/calendar.resource.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ResourceUpdateResult = number

    try {
      const response = await $b24.actions.v2.call.make<ResourceUpdateResult>({
        method: 'calendar.resource.update',
        params: {
          resourceId: 197,
          name: 'Changed Resource Name',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated resource ID:', result)
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
      async function updateCalendarResource() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'calendar.resource.update',
            params: {
              resourceId: 197,
              name: 'Changed Resource Name',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated resource ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateCalendarResource)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.calendar.resource.update(
            resource_id=197,
            name="Changed Resource Name",
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
                'calendar.resource.update',
                [
                    'resourceId' => 197,
                    'name'       => 'Changed Resource Name',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating resource: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'calendar.resource.update',
        {
            resourceId: 197,
            name: 'Changed Resource Name'
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'calendar.resource.update',
        [
            'resourceId' => 197,
            'name' => 'Changed Resource Name'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "calendar.resource.update", b24.Params{
    	"resourceId": 197,
    	"name":       "Changed Resource Name",
    })
    if err != nil {
    	return fmt.Errorf("calendar.resource.update: %w", err)
    }

    var value b24.ID
    if err := json.Unmarshal(res.Result, &value); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("result:", value)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 197,
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
[`integer`](../../data-types.md) | The ID from the `resourceId` parameter. The method returns it even when no calendar with this ID exists: there is no error, although nothing changes ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The mandatory parameter \"id\" is not specified for the method \"calendar.resource.update\""
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | The mandatory parameter "id" is not specified for the method "calendar.resource.update" | The required parameter `resourceId` is missing or its value is not greater than `0`. The error text names the parameter `id` ||
|| `400` | Empty value | The mandatory parameter "name" is not specified for the method "calendar.resource.update" | The required parameter `name` is missing ||
|| `400` | Empty value | Access denied | The method is called by an extranet user, or the user has no permission to edit resource calendars ||
|| `400` | Empty value | Error updating resource | Another error ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./calendar-resource-add.md)
- [{#T}](./calendar-resource-list.md)
- [{#T}](./calendar-resource-delete.md)
- [{#T}](./calendar-resource-booking-list.md)
