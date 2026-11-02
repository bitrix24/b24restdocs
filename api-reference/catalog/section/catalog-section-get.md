# Get Field Values of the Trade Catalog Section catalog.section.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" or "View catalog products" permission and permission to read the section in the information block

The method `catalog.section.get` returns the field values of the trade catalog section by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_section.id`](../data-types.md#catalog_section) | Identifier of the catalog section ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id": 31}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.section.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id": 31, "auth": "**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.section.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type CatalogSectionGetResult = {
      section: {
        active: string
        code: string | null
        description: string
        descriptionType: string
        iblockId: number
        iblockSectionId: number | null
        id: number
        name: string
        sort: number
        xmlId: string | null
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<CatalogSectionGetResult>({
        method: 'catalog.section.get',
        params: {
          id: 31,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.section.id, result.section.name)
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
      async function getCatalogSection() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.section.get',
            params: {
              id: 31,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.section.id, result.section.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCatalogSection)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.section.get(
            bitrix_id=31,
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
                'catalog.section.get',
                [
                    'id' => 31
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
        'catalog.section.get',
        {
            id: 31
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
        'catalog.section.get',
        [
            'id' => 31
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.section.get", b24.Params{
    	"id": 31,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.section.get: %w", err)
    }

    // The method wraps the response in an object with the "section" key.
    raw, ok := b24.Unwrap(res.Result, "section")
    if !ok {
    	return fmt.Errorf("no section key in the response")
    }

    var item struct {
    	Active          string `json:"active"`
    	Code            string `json:"code"`
    	Description     string `json:"description"`
    	DescriptionType string `json:"descriptionType"`
    	IblockID        b24.ID `json:"iblockId"`
    	IblockSectionID b24.ID `json:"iblockSectionId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Active, item.Code)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "section": {
            "active": "Y",
            "code": "clothes",
            "description": null,
            "descriptionType": "text",
            "iblockId": 25,
            "iblockSectionId": null,
            "id": 31,
            "name": "Clothing",
            "sort": 50,
            "xmlId": "clothes-01"
        }
    },
    "time": {
        "start": 1793615591,
        "finish": 1793615591.776121,
        "duration": 0.7761209011077881,
        "processing": 0,
        "date_start": "2026-11-02T13:33:11+03:00",
        "date_finish": "2026-11-02T13:33:11+03:00",
        "operating_reset_at": 1793616180,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **result.section**
[`catalog_section`](../data-types.md#catalog_section) | Object containing information about the catalog section with the specified identifier ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "200040300040",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | No "View product catalog" or "View catalog products" permission ||
|| `400` | `200040300040` | Access Denied | No permission to read the section in the information block, or the section with the specified `id` does not exist ||
|| `400` | `100` | Could not find value for parameter {id} | The required parameter `id` is not passed ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-section-add.md)
- [{#T}](./catalog-section-update.md)
- [{#T}](./catalog-section-list.md)
- [{#T}](./catalog-section-delete.md)
- [{#T}](./catalog-section-get-fields.md)