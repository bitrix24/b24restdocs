# Delete Access Key biconnector.key.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the permission to manage BI connector keys, for example an administrator

The method `biconnector.key.delete` deletes a BI connector access key. After deletion, external systems no longer retrieve data with this key.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and deletes only the keys that this application created. When called via a webhook, the method returns the `insufficient_auth_type` error

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Key identifier. It is returned by the [biconnector.key.add](./biconnector-key-add.md) and [biconnector.key.list](./biconnector-key-list.md) methods ||
|#

To temporarily revoke access by a key without deleting it, disable the key with the [biconnector.key.update](./biconnector-key-update.md) method and the value `ACTIVE` = `N`.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "id": 15,
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.key.delete
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
        method: 'biconnector.key.delete',
        params: {
          id: 15,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info('Key deleted:', response.getData()!.result)
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
      async function deleteKey() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.key.delete',
            params: {
              id: 15,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info('Key deleted:', response.getData().result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteKey)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        # b24pysdk has no ready-made wrapper for biconnector.key.*, so the method
        # is called directly through bitrix_token.call_method()
        response = bitrix_token.call_method(
            api_method="biconnector.key.delete",
            params={
                "id": 15,
            },
        )
        print("Key deleted:", response["result"])
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
                'biconnector.key.delete',
                [
                    'id' => 15,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Key deleted: ' . print_r($result, true);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.key.delete',
        {
            id: 15,
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            console.info('Key deleted:', result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.key.delete',
        [
            'id' => 15,
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
    res, err := client.Core().Call(ctx, "biconnector.key.delete", b24.Params{
    	"id": 15,
    })
    if err != nil {
    	return fmt.Errorf("biconnector.key.delete: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("key deleted:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1795525359,
        "finish": 1795525359.890627,
        "duration": 0.8906269073486328,
        "processing": 0,
        "date_start": "2026-11-24T14:02:39+01:00",
        "date_finish": "2026-11-24T14:02:39+01:00",
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
[`boolean`](../../data-types.md) | `true` if the key was deleted ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **200**, **400**

```json
{
    "error": "KEY_NOT_FOUND",
    "error_description": "Key not found."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `200` | `KEY_NOT_FOUND` | Key not found. | The key does not exist, has already been deleted, or was created by another application ||
|| `400` | `100` | Could not find value for parameter {id} | The `id` parameter is not passed ||
|| `400` | `insufficient_auth_type` | The request requires higher privileges than provided. | The method was called via a webhook, not from an application ||
|| `400` | `ACCESS_DENIED` | Access denied | The user does not have the permission to manage BI connector keys ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-key-add.md)
- [{#T}](./biconnector-key-update.md)
- [{#T}](./biconnector-key-list.md)
