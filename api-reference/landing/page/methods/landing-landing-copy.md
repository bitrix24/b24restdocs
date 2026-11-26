# Copy Page landing.landing.copy

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.copy` copies a page and returns the identifier of the new page. You can also copy a page from the trash.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal landing parameter: the type of sites the method works with. It is not related to the REST scope `landing`.

For Knowledge Base pages, pass `KNOWLEDGE`; for group knowledge base pages, pass `GROUP`. The value is case-insensitive. Without `scope`, the method cannot find a Knowledge Base page and returns the `LANDING_NOT_EXIST` error. For sites of the `PAGE`, `STORE`, `SMN`, and `VIBE` types, the parameter is not needed [(detailed description)](../../types.md) ||
|| **lid***
[`integer`](../../../data-types.md) | Identifier of the source page.

The page identifier can be obtained using the method [landing.landing.getList](./landing-landing-get-list.md) ||
|| **toSiteId**
[`integer`](../../../data-types.md) | Identifier of the target site. If this parameter is not provided or set to `0`, the copy will be created in the same site where the source page is located.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) ||
|| **toFolderId**
[`integer`](../../../data-types.md) | Identifier of the folder to place the copy in. The folder must be on the target site `toSiteId`.

If the parameter is not provided or set to `0`, the copy is placed in the root of the target site. The copy is also placed there, without an error, if the folder is not found or is on another site.

The folder identifier can be obtained using the method [landing.site.getFolders](../../site/landing-site-get-folders.md) ||
|| **skipSystem**
[`boolean`](../../../data-types.md) | Flag for copying the system attribute of the page. Default is `false`.

If set to `true`, the new page is created as non-system (`SYS = N`)
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 1688,
        "toSiteId": 305,
        "toFolderId": 95
      }' \
      "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/landing.landing.copy.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 1688,
        "toSiteId": 305,
        "toFolderId": 95,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put_your_bitrix24_address**/rest/landing.landing.copy.json"
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
        method: 'landing.landing.copy',
        params: {
          lid: 1688,
          toSiteId: 305,
          toFolderId: 95,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('New page ID:', result)
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
      async function copyLandingPage() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.copy',
            params: {
              lid: 1688,
              toSiteId: 305,
              toFolderId: 95,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('New page ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', copyLandingPage)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.landing.landing.copy(
            lid=1688,
            to_site_id=305,
            to_folder_id=95,
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
                'landing.landing.copy',
                [
                    'lid' => 1688,
                    'toSiteId' => 305,
                    'toFolderId' => 95,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error copying landing page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.copy',
        {
            lid: 1688,
            toSiteId: 305,
            toFolderId: 95
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
        'landing.landing.copy',
        [
            'lid' => 1688,
            'toSiteId' => 305,
            'toFolderId' => 95,
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
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "landing.landing.copy", b24.Params{
    	"lid":        1688,
    	"toSiteId":   305,
    	"toFolderId": 95,
    })
    if err != nil {
    	return fmt.Errorf("landing.landing.copy: %w", err)
    }

    var value b24.ID
    if err := json.Unmarshal(res.Result, &value); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("result:", value)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 2237,
    "time": {
        "start": 1773706491,
        "finish": 1773706492.91233,
        "duration": 1.912329912185669,
        "processing": 1,
        "date_start": "2026-03-17T03:14:51+01:00",
        "date_finish": "2026-03-17T03:14:52+01:00",
        "operating_reset_at": 1773707091,
        "operating": 0.9924919605255127
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Identifier of the created copy of the page. If the source page is published, the copy is also published. If the destination already has a page with the same title, the suffix "(copy)" is added to the title of the copy: "Promotion (copy)" ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "LANDING_NOT_EXIST",
    "error_description": "Landing not found"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `lid` is not provided ||
|| `LANDING_NOT_EXIST` | Page not found: the source page does not exist, the current user cannot access it, or `scope` is not passed for a Knowledge Base page ||
|| `SITE_NOT_FOUND` | Site not found: `toSiteId` contains a non-existent site ||
|| `ACCESS_DENIED` | Access to create the page is denied: the user does not have the "edit" access permission for the site ||
|| `TYPE_ERROR` | Data type error in the method call parameters, for example, `lid` is passed as an array ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-add-by-template.md)
- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-move.md)
- [{#T}](./landing-landing-publication.md)
- [{#T}](./landing-landing-update.md)