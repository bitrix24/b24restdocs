# Add Translation for Price Type Name catalog.priceTypeLang.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Manage Price Types" access permission

The method `catalog.priceTypeLang.add` adds a new translation for the price type name.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | Object with the fields of the new translation [(detailed description)](#fields) ||
|#

### Fields Parameter {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **catalogGroupId***
[`catalog_price_type.id`](../../data-types.md#catalog_price_type) | Identifier of the price type.

You can obtain the identifiers of price types using the method [catalog.priceType.list](../catalog-price-type-list.md)
||
|| **name***
[`string`](../../../data-types.md) | Translation of the price type name ||
|| **lang***
[`catalog_language.lid`](../../data-types.md#catalog_language) | Language identifier.

You can obtain the identifiers of languages using the method [catalog.priceTypeLang.getLanguages](./catalog-price-type-lang-get-languages.md)
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":1,"lang":"en","name":"PRICE"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceTypeLang.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":1,"lang":"en","name":"PRICE"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceTypeLang.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type PriceTypeLangAddResult = {
      priceTypeLang: {
        catalogGroupId: number
        id: number
        lang: string
        name: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<PriceTypeLangAddResult>({
        method: 'catalog.priceTypeLang.add',
        params: {
          fields: {
            catalogGroupId: 1,
            lang: 'en',
            name: 'PRICE',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.priceTypeLang)
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
      async function addPriceTypeLang() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.priceTypeLang.add',
            params: {
              fields: {
                catalogGroupId: 1,
                lang: 'en',
                name: 'PRICE',
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
          console.info(result.priceTypeLang)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addPriceTypeLang)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price_type_lang.add(
            fields={
                "catalogGroupId": 1,
                "lang": "kz",
                "name": "PRICE",
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
                'catalog.priceTypeLang.add',
                [
                    'fields' => [
                        'catalogGroupId' => 1,
                        'lang'           => "en",
                        'name'           => "PRICE",
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
        'catalog.priceTypeLang.add',
        {
            fields: {
                catalogGroupId: 1,
                lang: "en",
                name: "PRICE"
            }
        },
        function(result) {
            if (result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.priceTypeLang.add',
        [
            'fields' => [
                'catalogGroupId' => 1,
                'lang' => 'en',
                'name' => 'PRICE'
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
    res, err := client.Core().Call(ctx, "catalog.priceTypeLang.add", b24.Params{
    	"fields": b24.Params{
    		"catalogGroupId": 1,
    		"lang":           "kz",
    		"name":           "PRICE",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.priceTypeLang.add: %w", err)
    }

    // The method wraps the response in an object with the "priceTypeLang" key.
    raw, ok := b24.Unwrap(res.Result, "priceTypeLang")
    if !ok {
    	return fmt.Errorf("no priceTypeLang key in the response")
    }

    var item struct {
    	CatalogGroupID b24.ID `json:"catalogGroupId"`
    	ID             b24.ID `json:"id"`
    	Lang           string `json:"lang"`
    	Name           string `json:"name"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.CatalogGroupID, item.ID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "priceTypeLang": {
            "catalogGroupId": 1,
            "id": 319,
            "lang": "en",
            "name": "PRICE"
        }
    },
    "time": {
        "start": 1793699211,
        "finish": 1793699211.103497,
        "duration": 0.10349702835083008,
        "processing": 0,
        "date_start": "2026-11-03T12:46:51+03:00",
        "date_finish": "2026-11-03T12:46:51+03:00",
        "operating_reset_at": 1793699760,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **result.priceTypeLang**
[`catalog_price_type_lang`](../../data-types.md#catalog_price_type_lang) | Object containing information about the created translation of the price type name ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "200040300020",
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300020` | Access Denied | The user does not have the "Manage price types" access permission ||
|| `400` | `100` | Could not find value for parameter {fields} | The required parameter `fields` is not provided ||
|| `400` | `0` | Required fields: catalogGroupId, lang, name | Required fields are missing in `fields`. The message lists only the missing fields ||
|| `400` | `201000000000` | The specified price type does not exist | No price type from `catalogGroupId` exists ||
|| `400` | `201200000010` | The specified language does not exist | No language from `lang` exists ||
|| `400` | `400` | Internal server error. | A translation into this language already exists for the price type ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-lang-update.md)
- [{#T}](./catalog-price-type-lang-get.md)
- [{#T}](./catalog-price-type-lang-list.md)
- [{#T}](./catalog-price-type-lang-delete.md)
- [{#T}](./catalog-price-type-lang-get-languages.md)
- [{#T}](./catalog-price-type-lang-get-fields.md)
