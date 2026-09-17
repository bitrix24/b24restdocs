# Publish a Block landing.block.publication

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "publish" permission for the site

The `landing.block.publication` method publishes a single block of a page.

The method moves only the specified block into the published version of the page. The changes to the other blocks stay in the draft until the whole page is published with the [landing.landing.publication](../../page/methods/landing-landing-publication.md) method.

The page identifier does not need to be passed — the method determines the page from the block identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The internal scope of landing pages. It is unrelated to the REST scope `landing` in the method name.

The `scope` value has to match the site type [(detailed description)](../../types.md).

For the `PAGE`, `STORE`, and `SMN` types the parameter is not passed. For knowledge bases, group knowledge bases, and the main page, pass `KNOWLEDGE`, `GROUP`, and `MAINPAGE` ||
|| **block***
[`integer`](../../../data-types.md) | The identifier of the block in the page draft.

The block identifier can be retrieved with the [landing.block.getlist](./landing-block-get-list.md) method with the [`params.edit_mode = true`](./landing-block-get-list.md#params) parameter. A block identifier from the published version of the page does not suit the method: it will not find such a block and will return `null` ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "block": 6058
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.block.publication.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "block": 6058,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.block.publication.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean | null>({
        method: 'landing.block.publication',
        params: {
          block: 6058,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Block published:', result)
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
      async function publishBlock() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.block.publication',
            params: {
              block: 6058,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Block published:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', publishBlock)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        # The method has no dedicated wrapper in b24pysdk, so call it by name
        bitrix_response = bitrix_token.call_method(
            "landing.block.publication",
            {
                "block": 6058,
            },
        )
        result = bitrix_response["result"]
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
                'landing.block.publication',
                [
                    'block' => 6058,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error publishing block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.block.publication',
        {
            block: 6058
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.block.publication',
        [
            'block' => 6058,
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the "SDK for Go" section
    res, err := client.Core().Call(ctx, "landing.block.publication", b24.Params{
    	"block": 6058,
    })
    if err != nil {
    	return fmt.Errorf("landing.block.publication: %w", err)
    }

    // result is three-valued, so parse it into a pointer: nil means the block does not exist
    var ok *bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    switch {
    case ok == nil:
    	fmt.Println("block not found")
    case *ok:
    	fmt.Println("block published")
    default:
    	fmt.Println("no publish permission")
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1789577637,
        "finish": 1789577637.917601,
        "duration": 0.9176011085510254,
        "processing": 0,
        "date_start": "2026-09-16T19:53:57+03:00",
        "date_finish": "2026-09-16T19:53:57+03:00",
        "operating_reset_at": 1789578237,
        "operating": 0.18278789520263672
    }
}
```

If the block is not found, the response arrives with the same `200` status:

```json
{
    "result": null,
    "time": {
        "start": 1789578538,
        "finish": 1789578538.756383,
        "duration": 0.756382942199707,
        "processing": 0,
        "date_start": "2026-09-16T20:08:58+03:00",
        "date_finish": "2026-09-16T20:08:58+03:00",
        "operating_reset_at": 1789578565,
        "operating": 0.14619207382202148
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) \| `null` | The result of publishing the block.

Possible values:
`true` — the block is published,
`false` — the user has no publish permission,
`null` — the block with the identifier passed was not found.

The `false` and `null` values arrive with the HTTP status `200` and without the `error` field ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: block"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

Not every unsuccessful call arrives with an error. An unknown block identifier and a missing publish permission return the `200` status without the `error` field, so check the `result` value rather than the presence of an error alone.

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required `block` parameter was not passed ||
|| `LANDING_NOT_EXIST` | The page with this block was not found in the selected scope or is not available to the user. A common reason is that the `scope` value does not match the site type ||
|| `PUBLIC_PAGE_REACHED` | The plan has a limit on the number of published pages ||
|| `LANDING_PAYMENT_FAILED` | The page was added from an app, and a subscription is required to publish it ||
|| `LANDING_PAYMENT_FAILED_BLOCK` | The page contains a block from an app, and a subscription is required to publish it ||
|| `PUBLIC_SITE_REACHED` | The plan has a limit on the number of created or published sites ||
|| `PUBLIC_SITE_REACHED_FREE` | Publishing sites is temporarily available on paid plans only ||
|| `PHONE_NOT_CONFIRMED` | Publishing requires a confirmed phone number ||
|| `EMAIL_NOT_CONFIRMED` | Publishing requires a confirmed e-mail ||
|| `URLCHECKER_FAIL` | Malicious content was detected on the page ||
|| `LICENSE_EXPIRED` | The license of your product has expired ||
|#

Publishing a block goes through the same path as publishing the whole page, so the method returns the same plan limits and site checks as [landing.landing.publication](../../page/methods/landing-landing-publication.md).

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Your Learning

- [{#T}](./landing-block-update-nodes.md)
- [{#T}](./landing-block-update-content.md)
- [{#T}](./landing-block-get-list.md)
- [{#T}](../../page/methods/landing-landing-publication.md)
