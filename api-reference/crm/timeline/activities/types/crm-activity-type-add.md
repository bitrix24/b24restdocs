# Add Custom Activity Type crm.activity.type.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: administrator, only in the context of an [application](../../../../../settings/app-installation/index.md)

The `crm.activity.type.add` method registers a custom activity type with a name and an icon.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../../data-types.md) | Object with the fields of the new activity type. Pass field keys in uppercase, for example `TYPE_ID`: the method does not recognize the `type_id` key. [Field description](#parametr-fields) ||
|#

### Parameter fields {#parametr-fields}

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID***
[`string`](../../../../data-types.md) | Activity type code, for example `ERP`. No longer than 100 characters, unique among the current application's types.

The `CONFIGURABLE` code cannot be used together with `IS_CONFIGURABLE_TYPE` = `Y`: it is reserved.

After registration, pass the code in the `PROVIDER_TYPE_ID` field of the [crm.activity.add](../activity-base/crm-activity-add.md) method together with `PROVIDER_ID` = `REST_APP`, or in the `typeId` field of the [crm.activity.configurable.add](../configurable/crm-activity-configurable-add.md) method ||
|| **NAME***
[`string`](../../../../data-types.md) | Activity type name, for example `ERP activity`. No longer than 255 characters ||
|| **ICON_FILE**
[`array`](../../../../data-types.md) | Activity type icon — an [array of the file name and its Base64 content](../../../../files/how-to-upload-files.md#array), for example `["icon.png", "iVBORw0KGgo..."]`.

If the parameter is not passed or the file cannot be read, the type is created without an icon ||
|| **IS_CONFIGURABLE_TYPE**
[`string`](../../../../data-types.md) | Indicates whether the type is used for configurable activities:

- `Y` — the type for the [crm.activity.configurable.add](../configurable/crm-activity-configurable-add.md) method
- `N` — the type for the [crm.activity.add](../activity-base/crm-activity-add.md) method, the default value

Any value other than `Y` is treated as `N` ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":"ERP","NAME":"ERP activity","ICON_FILE":["icon.png","iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYPhfDwAChwGA60e6kgAAAABJRU5ErkJggg=="],"IS_CONFIGURABLE_TYPE":"N"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.type.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.activity.type.add',
        params: {
          fields: {
            TYPE_ID: 'ERP',
            NAME: 'ERP activity',
            ICON_FILE: ['icon.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYPhfDwAChwGA60e6kgAAAABJRU5ErkJggg=='], // file name and Base64 content
            IS_CONFIGURABLE_TYPE: 'N',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Activity type added:', result)
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
      async function addActivityType() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.type.add',
            params: {
              fields: {
                TYPE_ID: 'ERP',
                NAME: 'ERP activity',
                ICON_FILE: ['icon.png', 'iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYPhfDwAChwGA60e6kgAAAABJRU5ErkJggg=='], // file name and Base64 content
                IS_CONFIGURABLE_TYPE: 'N',
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
          console.info('Activity type added:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addActivityType)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.type.add(
            fields={
                "TYPE_ID": "ERP",
                "NAME": "ERP activity",
                "ICON_FILE": [
                    "icon.png",
                    "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mNkYPhfDwAChwGA60e6kgAAAABJRU5ErkJggg==",
                ],
                "IS_CONFIGURABLE_TYPE": "N",
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
                'crm.activity.type.add',
                [
                    'fields' => [
                        'TYPE_ID'            => 'ERP',
                        'NAME'               => 'ERP activity',
                        'ICON_FILE'          => ['icon.png', base64_encode(file_get_contents('icon.png'))],
                        'IS_CONFIGURABLE_TYPE' => 'N',
                    ],
                ]
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
        "crm.activity.type.add",
        {
            fields:
            {
                "TYPE_ID": 'ERP',
                "NAME": "ERP activity",
                'ICON_FILE': document.getElementById('type-icon'), // file input node
                "IS_CONFIGURABLE_TYPE": "N"
            }
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
        'crm.activity.type.add',
        [
            'fields' => [
                'TYPE_ID' => 'ERP',
                'NAME' => 'ERP activity',
                'ICON_FILE' => ['icon.png', base64_encode(file_get_contents('icon.png'))],
                'IS_CONFIGURABLE_TYPE' => 'N'
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
    // iconBytes is the content of the icon file, for example from os.ReadFile("icon.png")
    res, err := client.Core().Call(ctx, "crm.activity.type.add", b24.Params{
    	"fields": b24.Params{
    		"TYPE_ID":              "ERP",
    		"NAME":                 "ERP activity",
    		"ICON_FILE":            []string{"icon.png", base64.StdEncoding.EncodeToString(iconBytes)},
    		"IS_CONFIGURABLE_TYPE": "N",
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.type.add: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
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
[`boolean`](../../../../data-types.md) | Result of the registration:

- `true` — the type is registered
- `false` — the type is not registered: `TYPE_ID` or `NAME` is not passed, a value exceeds the allowed length, or field keys are not passed in uppercase ||
|| **time**
[`time`](../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Type is already registered"
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! Admin permissions required | The method is called by a user without administrator permissions ||
|| `403` | `ACCESS_DENIED` | Access denied! Application context required | The method is called via an inbound webhook or without application authorization ||
|| `400` | Empty value | Type is already registered | A type with this `TYPE_ID` is already registered by the current application ||
|| `400` | Empty value | Type ID "CONFIGURABLE" is reserved | `TYPE_ID` = `CONFIGURABLE` is passed together with `IS_CONFIGURABLE_TYPE` = `Y` ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-activity-type-list.md)
- [{#T}](./crm-activity-type-delete.md)
- [{#T}](../activity-base/crm-activity-add.md)
- [{#T}](../configurable/crm-activity-configurable-add.md)
