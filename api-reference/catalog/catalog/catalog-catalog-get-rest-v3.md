# Get Values of All Trade Catalog Fields v 3.0 catalog.catalog.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.catalog.get` method returns the field values of a trade catalog by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the trade catalog. Matches the identifier of the catalog information block.

The identifier can be retrieved using the [catalog.catalog.list](./catalog-catalog-list-rest-v3.md) method ||
|| **select**
[`array`](../../data-types.md) | List of trade catalog fields to return in the response, for example `["id","name","productIblockId"]`.

Available fields:

- `id` — trade catalog identifier
- `iblockId` — catalog information block identifier
- `subscription` — content sales flag
- `vatId` — VAT rate identifier
- `productIblockId` — product information block identifier for an offers catalog
- `skuPropertyId` — identifier of the property that links a variation to its product
- `iblockTypeId` — information block type
- `lid` — site identifier
- `name` — catalog name

The `id` field is returned only if it is specified in `select`. The `["*"]` value is not supported.

Default: all fields. An empty array `[]` also returns all fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.catalog.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":27,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.catalog.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CatalogGetResult = {
      item: {
        id: number
        iblockId: number
        subscription: 'Y' | 'N'
        vatId: number
        productIblockId: number
        skuPropertyId: number
        iblockTypeId: string
        lid: string
        name: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<CatalogGetResult>({
        method: 'catalog.catalog.get',
        params: {
          id: 27,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Catalog:', result.item.id, result.item.name)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getCatalog() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.catalog.get',
            params: {
              id: 27,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Catalog:', result.item.id, result.item.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCatalog)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.catalog.get(
            bitrix_id=27,
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.catalog.get',
                [
                    'id' => 27,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.catalog.get',
        {
            id: 27,
        },
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.catalog.get',
        [
            'id' => 27,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.catalog.get", b24.Params{
    	"id": 27,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.catalog.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID              int    `json:"id"`
    	Name            string `json:"name"`
    	ProductIblockID int    `json:"productIblockId"`
    	SkuPropertyID   int    `json:"skuPropertyId"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 27,
            "iblockId": 27,
            "subscription": "N",
            "vatId": 1,
            "productIblockId": 25,
            "skuPropertyId": 101,
            "iblockTypeId": "CRM_PRODUCT_CATALOG",
            "lid": "s1",
            "name": "CRM product catalog (offers)"
        }
    },
    "time": {
        "start": 1792665469,
        "finish": 1792665469.596805,
        "duration": 0.5968050956726074,
        "processing": 0,
        "date_start": "2026-10-22T13:37:49+02:00",
        "date_finish": "2026-10-22T13:37:49+02:00",
        "operating_reset_at": 1792666020,
        "operating": 0
    }
}
```

### Returned Data {#returned-data}

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Object with the trade catalog fields. The set of fields depends on `select` ||
|| **result.item.id**
[`integer`](../../data-types.md) | Identifier of the trade catalog. Matches `iblockId` ||
|| **result.item.iblockId**
[`integer`](../../data-types.md) | Identifier of the information block registered as a trade catalog ||
|| **result.item.subscription**
[`string`](../../data-types.md) | Content sales flag: `Y` — yes, `N` — no. Used only in the on-premise version ||
|| **result.item.vatId**
[`integer`](../../data-types.md) | Identifier of the catalog VAT rate. `0` — no rate is set ||
|| **result.item.productIblockId**
[`integer`](../../data-types.md) | Identifier of the product information block to which the offers catalog is linked. `0` — the catalog is not an offers catalog ||
|| **result.item.skuPropertyId**
[`integer`](../../data-types.md) | Identifier of the property in which a variation stores the identifier of its parent product. `0` — the catalog is not an offers catalog ||
|| **result.item.iblockTypeId**
[`string`](../../data-types.md) | Information block type. For CRM trade catalogs — `CRM_PRODUCT_CATALOG`, for a catalog based on a universal list — `lists` ||
|| **result.item.lid**
[`string`](../../data-types.md) | Identifier of the site to which the information block is linked, for example `s1` ||
|| **result.item.name**
[`string`](../../data-types.md) | Name of the trade catalog information block ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 99999 was not found."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Required field `id` is missing | Pass a positive number in `id`. The error also occurs when `id` is `0` or a non-numeric value ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no trade catalog with this identifier. Check the identifier using the [catalog.catalog.list](./catalog-catalog-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `CatalogDto` | Specify in `select` only the fields from the `select` parameter description ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | No permission to view trade catalog settings. Call the method on behalf of an administrator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-add.md)
- [{#T}](./catalog-catalog-update.md)
- [{#T}](./catalog-catalog-list-rest-v3.md)
- [{#T}](./catalog-catalog-is-offers-rest-v3.md)
- [{#T}](./catalog-catalog-field-list.md)
