# Get Access Permission Names access.name

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`basic`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `access.name` retrieves the names of access permissions.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ACCESS***
[`array`](../../data-types.md) | List of access codes for which names need to be retrieved.

Code formats:

- `U<id>` — user, for example `U1`
- `G<id>` — user group, for example `G2`
- `AU` — all authorized users

If the parameter is not provided or is empty, the method returns `false` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "ACCESS": ["G2", "AU"]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/access.name
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "ACCESS": ["G2", "AU"],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/access.name
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type AccessNameItem = {
      provider: string
      name: string
      provider_id: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AccessNameResult = Record<string, AccessNameItem>

    try {
      const response = await $b24.actions.v2.call.make<AccessNameResult>({
        method: 'access.name',
        params: {
          ACCESS: ['G2', 'AU'],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(Object.keys(result), result)
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
      async function getAccessNames() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'access.name',
            params: {
              ACCESS: ['G2', 'AU'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(Object.keys(result), result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAccessNames)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.access.name(
            access=[
                "G2",
                "AU",
            ],
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
                'access.name',
                [
                    'ACCESS' => ['G2', 'AU']
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling access.name: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "access.name",
        {
            "ACCESS": ["G2", "AU"]
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'access.name',
        [
            'ACCESS' => ['G2','AU']
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "access.name", b24.Params{
    	"ACCESS": []string{"G2", "AU"},
    })
    if err != nil {
    	return fmt.Errorf("access.name: %w", err)
    }

    keys, ok := b24.Keys(res.Result)
    if !ok {
    	return fmt.Errorf("expected an object in the response")
    }
    fmt.Println("fields in response:", len(keys))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "G2": {
            "provider": "",
            "name": "All visitors",
            "provider_id": "other"
        },
        "AU": {
            "provider": "",
            "name": "All authorized users",
            "provider_id": "other"
        }
    },
    "time": {
        "start": 1722002504.2838,
        "finish": 1722002504.32483,
        "duration": 0.0410301685333252,
        "processing": 0.00145506858825684,
        "date_start": "2024-07-26T14:01:44+00:00",
        "date_finish": "2024-07-26T14:01:44+00:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md)\|[`boolean`](../../data-types.md) | An object where the key is an access code from the `ACCESS` parameter and the value is the description of that code.

The structure is described [below](#access-item).

If the `ACCESS` parameter is not provided or is empty, the method returns `false` ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

### Access Code Description {#access-item}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Name of the access code in the Bitrix24 language, for example `All authorized users` ||
|| **provider**
[`string`](../../data-types.md) | Name of the access code provider. For the `G` and `AU` codes, it is an empty string ||
|| **provider_id**
[`string`](../../data-types.md) | Identifier of the access code provider, for example `other` for the `G2` and `AU` codes ||
|#

Codes that do not exist in Bitrix24 are not returned in the response.

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./method-get.md)
- [{#T}](./scope.md)
- [{#T}](./app-info.md)
- [{#T}](./feature-get.md)
- [{#T}](./server-time.md)
- [{#T}](./methods.md)