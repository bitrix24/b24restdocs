# Get a List of Custom Activity Types crm.activity.type.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: administrator, only in the context of an [application](../../../../../settings/app-installation/index.md)

The `crm.activity.type.list` method retrieves a list of custom activity types registered by the current application.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.type.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each ActivityTypeItem returned in result[]
    type ActivityTypeItem = {
      TYPE_ID: string,
      NAME: string,
      IS_CONFIGURABLE_TYPE: string,
      ICON_ID: string,
    }

    try {
      // crm.activity.type.list returns all types of the current application at once, without pagination
      const response = await $b24.actions.v2.call.make<ActivityTypeItem[]>({
        method: 'crm.activity.type.list',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activity types:', result.length, result)
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
      async function fetchActivityTypeList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.activity.type.list returns all types of the current application at once, without pagination
          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.type.list',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Activity types:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchActivityTypeList)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.type.list().response
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
                'crm.activity.type.list',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.type.list",
        {
        }, result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.type.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.type.list", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.type.list: %w", err)
    }

    var items []struct {
    	TypeID             string `json:"TYPE_ID"`
    	Name               string `json:"NAME"`
    	IsConfigurableType string `json:"IS_CONFIGURABLE_TYPE"`
    	IconID             b24.ID `json:"ICON_ID"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.TypeID, it.Name)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "TYPE_ID": "ERP",
            "NAME": "ERP activity",
            "IS_CONFIGURABLE_TYPE": "Y",
            "ICON_ID": "0"
        }
    ],
    "time": {
        "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2025-01-21T13:47:08+02:00",
        "date_finish": "2025-01-21T13:47:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../../data-types.md) | Array of the current application's activity types [(detailed description)](#type). If the application has not registered any types, an empty array `[]` is returned ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

#### Element of the result Array {#type}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID**
[`string`](../../../../data-types.md) | Activity type code. It is passed in `PROVIDER_TYPE_ID` of the [crm.activity.add](../activity-base/crm-activity-add.md) method and in `typeId` of the [crm.activity.configurable.add](../configurable/crm-activity-configurable-add.md) method ||
|| **NAME**
[`string`](../../../../data-types.md) | Activity type name ||
|| **IS_CONFIGURABLE_TYPE**
[`string`](../../../../data-types.md) | Indicates whether the type is used for configurable activities:

- `Y` — the type is used in [crm.activity.configurable.add](../configurable/crm-activity-configurable-add.md)
- `N` — the type is used in [crm.activity.add](../activity-base/crm-activity-add.md) ||
|| **ICON_ID**
[`string`](../../../../data-types.md) | Identifier of the icon file, for example `"39755"`. The value `"0"` means that no icon is uploaded ||
|#

## Error Handling

HTTP status: **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied! Application context required"
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! Admin permissions required | The method is called by a user without administrator permissions ||
|| `403` | `ACCESS_DENIED` | Access denied! Application context required | The method is called via an inbound webhook or without application authorization ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-type-add.md)
- [{#T}](./crm-activity-type-delete.md)
- [{#T}](../activity-base/crm-activity-add.md)
- [{#T}](../configurable/crm-activity-configurable-add.md)
