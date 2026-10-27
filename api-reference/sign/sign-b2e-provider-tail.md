# Get the List of Signing Providers sign.b2e.provider.tail

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`sign.b2e`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `sign.b2e.provider.tail` returns the list of external signing providers connected to Bitrix24.

The method works only in the context of the [application](../../settings/app-installation/index.md) authorization.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **limit**
[`integer`](../data-types.md) | Number of records per page.

The parameter accepts values from 1 to 1000. If the value is out of range, the method uses the default value.

Defaults to 20 ||
|| **offset**
[`integer`](../data-types.md) | Number of records to skip from the beginning of the list. Used for pagination together with `limit`.

Defaults to 0 ||
|#

## Code Examples

{% include [Note on Examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"limit":10,"offset":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sign.b2e.provider.tail
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProviderItem = {
      id: number
      title: string
      description: string
      iconUri: string
      companyRegUri: string
      documentSignUri: string
      publicKeyUri: string
    }

    // On a sign service failure result holds error and error_description instead of the list
    type ProviderTailResult = ProviderItem[] | { error: number, error_description: string }

    try {
      // The method pages with limit and offset, not with start,
      // so the SDK list helpers do not apply: request the next page with a new offset
      const response = await $b24.actions.v2.call.make<ProviderTailResult>({
        method: 'sign.b2e.provider.tail',
        params: {
          limit: 10,
          offset: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        if (Array.isArray(result)) {
          result.forEach(provider => console.info(provider.id, provider.title))
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
      async function fetchSignProviders() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          // The method pages with limit and offset, not with start,
          // so the SDK list helpers do not apply: request the next page with a new offset
          const response = await $b24.actions.v2.call.make({
            method: 'sign.b2e.provider.tail',
            params: {
              limit: 10,
              offset: 0
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          // On a sign service failure result holds error and error_description instead of the list
          const result = response.getData().result
          if (Array.isArray(result)) {
            result.forEach(provider => console.info(provider.id, provider.title))
          } else {
            console.error(result.error_description)
          }
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchSignProviders)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.call(
            "sign.b2e.provider.tail",
            {
                "limit": 10,
                "offset": 0,
            },
        ).response
        result = bitrix_response.result
        # On a sign service failure result holds error and error_description
        if isinstance(result, dict):
            print(f"Sign service error: {result['error_description']}")
        else:
            for provider in result:
                print(provider["id"], provider["title"])
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
                'sign.b2e.provider.tail',
                [
                    'limit' => 10,
                    'offset' => 0
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
        'sign.b2e.provider.tail',
        {
            limit: 10,
            offset: 0
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
        'sign.b2e.provider.tail',
        [
            'limit' => 10,
            'offset' => 0
        ]
    );

    if (isset($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } elseif (isset($result['result']['error_description'])) {
        echo 'Sign service error: ' . $result['result']['error_description'];
    } else {
        echo '<PRE>';
        print_r($result['result']);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "sign.b2e.provider.tail", b24.Params{
    	"limit":  10,
    	"offset": 0,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("sign.b2e.provider.tail: %w", err)
    }

    var items []struct {
    	ID              int    `json:"id"`
    	Title           string `json:"title"`
    	Description     string `json:"description"`
    	IconURI         string `json:"iconUri"`
    	CompanyRegURI   string `json:"companyRegUri"`
    	DocumentSignURI string `json:"documentSignUri"`
    	PublicKeyURI    string `json:"publicKeyUri"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	// on a sign service failure result holds error and error_description
    	return fmt.Errorf("sign service response: %s", res.Result)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Title)
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "id": 41,
            "title": "Docs check provider",
            "description": "Provider for documentation check",
            "iconUri": "https://provider.sign.com/bitrix/icon.png",
            "companyRegUri": "https://provider.sign.com/bitrix/register/",
            "documentSignUri": "https://provider.sign.com/bitrix/sign/",
            "publicKeyUri": "https://provider.sign.com/bitrix/public.key"
        },
        {
            "id": 43,
            "title": "Docs check provider",
            "description": "Provider for documentation check",
            "iconUri": "https://provider.sign.com/bitrix/icon.png",
            "companyRegUri": "",
            "documentSignUri": "https://provider.sign.com/bitrix/sign/",
            "publicKeyUri": "https://provider.sign.com/bitrix/public.key"
        }
    ],
    "time": {
        "start": 1792738605,
        "finish": 1792738605.693056,
        "duration": 0.6930561065673828,
        "processing": 0,
        "date_start": "2026-10-23T09:56:45+03:00",
        "date_finish": "2026-10-23T09:56:45+03:00",
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
[`array`](../data-types.md) | List of external signing providers [(detailed description)](#result). Deleted providers are not included in the list. If there are no providers, an empty array is returned ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Element of the result array {#result}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../data-types.md) | Provider identifier ||
|| **title**
[`string`](../data-types.md) | Provider name ||
|| **description**
[`string`](../data-types.md) | Short description of the provider ||
|| **iconUri**
[`string`](../data-types.md) | Address of the provider icon for the Bitrix24 interface ||
|| **companyRegUri**
[`string`](../data-types.md) | Address of the page where a company registers with the provider. If the address is not set, an empty string is returned ||
|| **documentSignUri**
[`string`](../data-types.md) | Address of the provider's document signing page ||
|| **publicKeyUri**
[`string`](../data-types.md) | Address of the provider's public key. The key is used to verify document signatures ||
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

If the Bitrix24 sign service returns an error without its own code, the method responds with HTTP status **200**, and `result` contains an object with the `error` and `error_description` fields instead of an array. Make sure `result` is an array before iterating over the providers.

```json
{
    "result": {
        "error": 0,
        "error_description": "Cannot perform action. Please try again later (error code: SIGN_CLIENT_CONNECTION_ERROR)."
    },
    "time": {
        "start": 1792738605,
        "finish": 1792738605.693056,
        "duration": 0.6930561065673828,
        "processing": 0,
        "date_start": "2026-10-23T09:56:45+03:00",
        "date_finish": "2026-10-23T09:56:45+03:00",
        "operating_reset_at": 1792739160,
        "operating": 0
    }
}
```

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method Application context required | The method was called outside an application with OAuth authorization, for example, through a webhook ||
|| `403` | `ACCESS_DENIED` | Access denied! Access denied | The user is not an administrator, or e-Signature for HR is not available on Bitrix24 ||
|| `403` | `ACCESS_DENIED` | Access denied! User authorization required | The request was made without user authorization ||
|| `200` | Sign service code | Sign service error text | The sign service did not retrieve the list and returned its own error code ||
|| `200` | `0` in `result.error` | Cannot perform action. Please try again later (error code: #CODE#). | The sign service returned an error, or Bitrix24 did not receive its response. `#CODE#` is replaced with the code of the cause. The text is returned in the user's language ||
|| `200` | `0` in `result.error` | This action is not available yet, please try again later. | The sign service version does not support this action yet ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./sign-b2e-provider-add.md)
- [{#T}](./sign-b2e-provider-update.md)
- [{#T}](./sign-b2e-provider-delete.md)
- [{#T}](./sign-b2e-company-provider-list.md)
- [{#T}](./index.md)
