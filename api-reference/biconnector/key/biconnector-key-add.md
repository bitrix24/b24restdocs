# Create Access Key biconnector.key.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the permission to manage BI connector keys, for example an administrator

The method `biconnector.key.add` creates a BI connector access key and returns its identifier.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md). When called via a webhook, the method returns the `insufficient_auth_type` error

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Key fields [(detailed description)](#fields). If you do not need to set any fields, pass an empty object ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **ACTIVE**
[`string`](../../data-types.md) | Whether to enable the key right after it is created. Possible values:

- `Y` — the key is enabled
- `N` — the key is disabled

Default — `N` ||
|| **CONNECTION**
[`string`](../../data-types.md) | Database connection from which the BI connector returns data for the key. In cloud Bitrix24, it is `bi`.

If the connection is not found, the key is created with the default connection ||
|#

Bitrix24 generates the key value itself. The key belongs to the user on whose behalf the method is called: you cannot pass another user. The key value is returned by the [biconnector.key.list](./biconnector-key-list.md) method in the `ACCESS_KEY` field.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "fields": {
                 "ACTIVE": "Y"
             },
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.key.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'biconnector.key.add',
        params: {
          fields: {
            ACTIVE: 'Y',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info('Key created, ID:', response.getData()!.result)
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
      async function addKey() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.key.add',
            params: {
              fields: {
                ACTIVE: 'Y',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info('Key created, ID:', response.getData().result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addKey)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        # b24pysdk has no ready-made wrapper for biconnector.key.*, so the method
        # is called directly through bitrix_token.call_method()
        response = bitrix_token.call_method(
            api_method="biconnector.key.add",
            params={
                "fields": {
                    "ACTIVE": "Y",
                },
            },
        )
        print("Key created, ID:", response["result"])
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
                'biconnector.key.add',
                [
                    'fields' => [
                        'ACTIVE' => 'Y',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Key created, ID: ' . print_r($result, true);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.key.add',
        {
            fields: {
                ACTIVE: 'Y',
            },
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.info('Key created, ID:', result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.key.add',
        [
            'fields' => [
                'ACTIVE' => 'Y',
            ],
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error'] . ': ' . $result['error_description'];
    } else {
        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "biconnector.key.add", b24.Params{
    	"fields": b24.Params{
    		"ACTIVE": "Y",
    	},
    })
    if err != nil {
    	return fmt.Errorf("biconnector.key.add: %w", err)
    }

    var id int
    if err := json.Unmarshal(res.Result, &id); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("key created, ID:", id)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 15,
    "time": {
        "start": 1795525345,
        "finish": 1795525346.017652,
        "duration": 1.0176520347595215,
        "processing": 0,
        "date_start": "2026-11-24T14:02:25+01:00",
        "date_finish": "2026-11-24T14:02:26+01:00",
        "operating_reset_at": 1795525920,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created key ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "100",
    "error_description": "Could not find value for parameter {fields}"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `100` | Could not find value for parameter {fields} | The `fields` parameter is not passed. If you do not need to set any fields, pass an empty object ||
|| `400` | `insufficient_auth_type` | The request requires higher privileges than provided. | The method was called via a webhook, not from an application ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have the permission to manage BI connector keys ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-key-update.md)
- [{#T}](./biconnector-key-list.md)
- [{#T}](./biconnector-key-delete.md)
