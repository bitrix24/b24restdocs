# Add a Signing Provider sign.b2e.provider.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sign.b2e.provider.add` connects an external signing provider to Bitrix24.

The method works only in the context of the [application](../../settings/app-installation/index.md) authorization.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../data-types.md) | Object with the fields of the new provider. [Field description](#fields) ||
|#

### Parameter fields {#fields}

Pass full addresses with the `http` or `https` scheme. Required fields cannot be empty strings.

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../data-types.md) | Provider name ||
|| **description***
[`string`](../data-types.md) | Short description of the provider ||
|| **iconUri***
[`string`](../data-types.md) | Address of the provider icon for the Bitrix24 interface ||
|| **companyRegUri**
[`string`](../data-types.md) | Address of the page where a company registers with the provider ||
|| **documentSignUri***
[`string`](../data-types.md) | Address of the provider's document signing page ||
|| **publicKeyUri***
[`string`](../data-types.md) | Address of the provider's public key. The key is used to verify document signatures ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"title":"Test provider name","description":"Brief text about provider","iconUri":"https://provider.sign.com/bitrix/icon.png","companyRegUri":"https://provider.sign.com/bitrix/register/","documentSignUri":"https://provider.sign.com/bitrix/sign/","publicKeyUri":"https://provider.sign.com/bitrix/public.key"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.provider.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // On a sign service failure result holds error and error_description instead of the id
    type ProviderAddResult = number | { error: number, error_description: string }

    try {
      const response = await $b24.actions.v2.call.make<ProviderAddResult>({
        method: 'sign.b2e.provider.add',
        params: {
          fields: {
            title: 'Test provider name',
            description: 'Brief text about provider',
            iconUri: 'https://provider.sign.com/bitrix/icon.png',
            companyRegUri: 'https://provider.sign.com/bitrix/register/',
            documentSignUri: 'https://provider.sign.com/bitrix/sign/',
            publicKeyUri: 'https://provider.sign.com/bitrix/public.key',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        if (typeof result === 'number') {
          console.info('Provider id:', result)
        } else {
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
      async function addSignProvider() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'sign.b2e.provider.add',
            params: {
              fields: {
                title: 'Test provider name',
                description: 'Brief text about provider',
                iconUri: 'https://provider.sign.com/bitrix/icon.png',
                companyRegUri: 'https://provider.sign.com/bitrix/register/',
                documentSignUri: 'https://provider.sign.com/bitrix/sign/',
                publicKeyUri: 'https://provider.sign.com/bitrix/public.key'
              }
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          // On a sign service failure result holds error and error_description instead of the id
          const result = response.getData().result
          if (typeof result === 'number') {
            console.info('Provider id:', result)
          } else {
            console.error(result.error_description)
          }
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addSignProvider)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.call(
            "sign.b2e.provider.add",
            {
                "fields": {
                    "title": "Test provider name",
                    "description": "Brief text about provider",
                    "iconUri": "https://provider.sign.com/bitrix/icon.png",
                    "companyRegUri": "https://provider.sign.com/bitrix/register/",
                    "documentSignUri": "https://provider.sign.com/bitrix/sign/",
                    "publicKeyUri": "https://provider.sign.com/bitrix/public.key",
                },
            },
        ).response
        result = bitrix_response.result
        # On a sign service failure result holds error and error_description
        if isinstance(result, dict):
            print(f"Sign service error: {result['error_description']}")
        else:
            print(f"Provider ID: {result}")
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
                'sign.b2e.provider.add',
                [
                    'fields' => [
                        'title' => 'Test provider name',
                        'description' => 'Brief text about provider',
                        'iconUri' => 'https://provider.sign.com/bitrix/icon.png',
                        'companyRegUri' => 'https://provider.sign.com/bitrix/register/',
                        'documentSignUri' => 'https://provider.sign.com/bitrix/sign/',
                        'publicKeyUri' => 'https://provider.sign.com/bitrix/public.key'
                    ]
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
        'sign.b2e.provider.add',
        {
            fields: {
                title: 'Test provider name',
                description: 'Brief text about provider',
                iconUri: 'https://provider.sign.com/bitrix/icon.png',
                companyRegUri: 'https://provider.sign.com/bitrix/register/',
                documentSignUri: 'https://provider.sign.com/bitrix/sign/',
                publicKeyUri: 'https://provider.sign.com/bitrix/public.key'
            }
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
        'sign.b2e.provider.add',
        [
            'fields' => [
                'title' => 'Test provider name',
                'description' => 'Brief text about provider',
                'iconUri' => 'https://provider.sign.com/bitrix/icon.png',
                'companyRegUri' => 'https://provider.sign.com/bitrix/register/',
                'documentSignUri' => 'https://provider.sign.com/bitrix/sign/',
                'publicKeyUri' => 'https://provider.sign.com/bitrix/public.key'
            ]
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } elseif (is_array($result['result'])) {
        echo 'Sign service error: ' . $result['result']['error_description'];
    } else {
        echo 'Provider ID: ' . $result['result'];
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sign.b2e.provider.add", b24.Params{
    	"fields": b24.Params{
    		"title":           "Test provider name",
    		"description":     "Brief text about provider",
    		"iconUri":         "https://provider.sign.com/bitrix/icon.png",
    		"companyRegUri":   "https://provider.sign.com/bitrix/register/",
    		"documentSignUri": "https://provider.sign.com/bitrix/sign/",
    		"publicKeyUri":    "https://provider.sign.com/bitrix/public.key",
    	},
    })
    if err != nil {
    	return fmt.Errorf("sign.b2e.provider.add: %w", err)
    }

    var id int
    if err := json.Unmarshal(res.Result, &id); err != nil {
    	// on a sign service failure result holds error and error_description
    	return fmt.Errorf("sign service response: %s", res.Result)
    }
    fmt.Println("provider ID:", id)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 41,
    "time": {
        "start": 1792738603,
        "finish": 1792738603.104434,
        "duration": 0.10443401336669922,
        "processing": 0,
        "date_start": "2026-10-23T09:56:43+03:00",
        "date_finish": "2026-10-23T09:56:43+03:00",
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
[`integer`](../data-types.md) | Identifier of the added provider. Required for the [sign.b2e.provider.update](./sign-b2e-provider-update.md) and [sign.b2e.provider.delete](./sign-b2e-provider-delete.md) methods ||
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

Errors of the Bitrix24 sign service are returned with HTTP status **200** in one of two forms.

If an error has a code, the response contains `error` and `error_description` at the top level, like a regular method error. Validation errors for the name and addresses are returned this way:

```json
{
    "error": 100,
    "error_description": "PublicKeyUri must be valid",
    "ex": {
        "status": 200,
        "ex": 100
    }
}
```

If an error has no code, `result` contains an object with the `error` and `error_description` fields instead of the identifier. Check the type of `result` before using the value as an identifier:

```json
{
    "result": {
        "error": 0,
        "error_description": "The field Description is required."
    },
    "time": {
        "start": 1792738603,
        "finish": 1792738603.104434,
        "duration": 0.10443401336669922,
        "processing": 0,
        "date_start": "2026-10-23T09:56:43+03:00",
        "date_finish": "2026-10-23T09:56:43+03:00",
        "operating_reset_at": 1792739160,
        "operating": 0
    }
}
```

If the error text contains the code `SIGN_CLIENT_CONNECTION_ERROR`, `INCORRECT_JSON`, `INCORRECT_HTTP_STATUS`, `INCORRECT_DATA`, or `INCORRECT_REGISTER_RESPONSE`, Bitrix24 did not receive a response from the service. The provider may have been added anyway. Before calling the method again, check the list of providers with the [sign.b2e.provider.tail](./sign-b2e-provider-tail.md) method.

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | The method was called outside an application with OAuth authorization, for example, through a webhook ||
|| `403` | `ACCESS_DENIED` | Access denied! Access denied | The user is not an administrator, or e-Signature for HR is not available on Bitrix24 ||
|| `403` | `ACCESS_DENIED` | Access denied! User authorization required | The request was made without user authorization ||
|| `200` | `100` | Title must be provided | The `title` field contains an empty string ||
|| `200` | `100` | IconUri must be valid | The `iconUri` field contains an address without the `http` or `https` scheme ||
|| `200` | `100` | CompanyRegUri must be empty be valid | The `companyRegUri` field contains an address without the `http` or `https` scheme ||
|| `200` | `100` | DocumentSignUri must be valid | The `documentSignUri` field contains an address without the `http` or `https` scheme ||
|| `200` | `100` | PublicKeyUri must be valid | The `publicKeyUri` field contains an address without the `http` or `https` scheme ||
|| `200` | `0` in `result.error` | Cannot perform action. Please try again later (error code: 100). | A required field is missing: `title`, `description`, `iconUri`, `documentSignUri`, or `publicKeyUri` ||
|| `200` | `0` in `result.error` | The field Description is required. | The `description` field contains an empty string ||
|| `200` | `0` in `result.error` | Cannot perform action. Please try again later (error code: #CODE#). | The sign service returned an error, or Bitrix24 did not receive its response. `#CODE#` is replaced with the code of the cause. The text is returned in the user's language ||
|| `200` | `0` in `result.error` | This action is not available yet, please try again later. | The sign service version does not support this action yet ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-provider-update.md)
- [{#T}](./sign-b2e-provider-tail.md)
- [{#T}](./sign-b2e-provider-delete.md)
- [{#T}](./sign-b2e-company-provider-list.md)
- [{#T}](./index.md)
