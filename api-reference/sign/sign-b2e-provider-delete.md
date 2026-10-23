# Delete a Signing Provider sign.b2e.provider.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sign.b2e.provider.delete` disconnects an external signing provider from Bitrix24.

The method works only in the context of the [application](../../settings/app-installation/index.md) authorization.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Provider identifier.

The identifier is returned by the [sign.b2e.provider.add](./sign-b2e-provider-add.md) method, and the list of providers with identifiers is returned by the [sign.b2e.provider.tail](./sign-b2e-provider-tail.md) method ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":42,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.provider.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // On a sign service failure result holds error and error_description instead of true
    type ProviderDeleteResult = boolean | { error: number, error_description: string }

    try {
      const response = await $b24.actions.v2.call.make<ProviderDeleteResult>({
        method: 'sign.b2e.provider.delete',
        params: {
          id: 42,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        if (result === true) {
          console.info('Provider deleted')
        } else if (typeof result === 'object') {
          console.error(result.error_description)
        }
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
      async function deleteSignProvider() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sign.b2e.provider.delete',
            params: {
              id: 42
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          // On a sign service failure result holds error and error_description instead of true
          const result = response.getData().result
          if (result === true) {
            console.info('Provider deleted')
          } else if (typeof result === 'object') {
            console.error(result.error_description)
          }
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteSignProvider)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.call(
            "sign.b2e.provider.delete",
            {
                "id": 42,
            },
        ).response
        result = bitrix_response.result
        # On a sign service failure result holds error and error_description
        if isinstance(result, dict):
            print(f"Sign service error: {result['error_description']}")
        else:
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
                'sign.b2e.provider.delete',
                [
                    'id' => 42
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```javascript
    BX24.callMethod(
        'sign.b2e.provider.delete',
        {
            id: 42
        },
        result => {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.dir(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'sign.b2e.provider.delete',
        [
            'id' => 42
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } elseif (is_array($result['result'])) {
        echo 'Sign service error: ' . $result['result']['error_description'];
    } else {
        echo 'Provider deleted';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sign.b2e.provider.delete", b24.Params{
    	"id": 42,
    })
    if err != nil {
    	return fmt.Errorf("sign.b2e.provider.delete: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	// on a sign service failure result holds error and error_description
    	return fmt.Errorf("sign service response: %s", res.Result)
    }
    fmt.Println("provider deleted:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1792738607,
        "finish": 1792738608.01666,
        "duration": 1.0166599750518799,
        "processing": 1,
        "date_start": "2026-10-23T09:56:47+03:00",
        "date_finish": "2026-10-23T09:56:48+03:00",
        "operating_reset_at": 1792739160,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Deletion result. `true` means the provider has been deleted. A deleted provider is no longer returned by the [sign.b2e.provider.tail](./sign-b2e-provider-tail.md) method and cannot be updated ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method Application context required"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

If the Bitrix24 sign service returns an error without its own code, the method responds with HTTP status **200**, and `result` contains an object with the `error` and `error_description` fields instead of `true`. Consider the call successful only if `result` equals `true`.

```json
{
    "result": {
        "error": 0,
        "error_description": "External provider with id 42 doesnt exist"
    },
    "time": {...}
}
```

If the error text contains the code `SIGN_CLIENT_CONNECTION_ERROR`, `INCORRECT_JSON`, `INCORRECT_HTTP_STATUS`, `INCORRECT_DATA`, or `INCORRECT_REGISTER_RESPONSE`, Bitrix24 did not receive a response from the service. The provider may have been deleted anyway. You can check this with the [sign.b2e.provider.tail](./sign-b2e-provider-tail.md) method.

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | The method was called outside an application with OAuth authorization, for example, through a webhook ||
|| `403` | `ACCESS_DENIED` | Access denied! Access denied | The user is not an administrator, or e-Signature for HR is not available on Bitrix24 ||
|| `403` | `ACCESS_DENIED` | Access denied! User authorization required | The request was made without user authorization ||
|| `200` | `0` in `result.error` | External provider with id #ID# doesnt exist | The provider with the `id` identifier was not found or has already been deleted ||
|| `200` | Sign service code | Sign service error text | The sign service did not delete the provider and returned its own error code ||
|| `200` | `0` in `result.error` | Cannot perform action. Please try again later (error code: #CODE#). | The sign service returned an error, or Bitrix24 did not receive its response. `#CODE#` is replaced with the code of the cause. The text is returned in the user's language ||
|| `200` | `0` in `result.error` | This action is not available yet, please try again later. | The sign service version does not support this action yet ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-provider-add.md)
- [{#T}](./sign-b2e-provider-update.md)
- [{#T}](./sign-b2e-provider-tail.md)
- [{#T}](./index.md)
