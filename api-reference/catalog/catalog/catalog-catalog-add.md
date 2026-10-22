# Add Trade Catalog catalog.catalog.add

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

The `catalog.catalog.add` method turns an existing information block into a trade catalog.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Object with the fields of the new trade catalog. [Field description](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`integer`](../../data-types.md) | Identifier of the information block to turn into a trade catalog. The information block must exist and must not already be a trade catalog.

The trade catalog identifier is the same as the information block identifier. `iblockId` cannot be changed after the catalog is created ||
|| **vatId**
[`integer`](../../data-types.md) | Identifier of the catalog VAT rate.

The identifier can be retrieved using the [catalog.vat.list](../vat/catalog-vat-list-rest-v3.md) method.

Default: `0` — no rate is set ||
|| **subscription**
[`string`](../../data-types.md) | Content sales flag. Used only in the on-premise version.

Possible values:

- `Y` — yes
- `N` — no

Any value other than `Y` is saved as `N`.

Default: `N` ||
|| **productIblockId**
[`integer`](../../data-types.md) | Identifier of the product information block. Pass it if you are creating an offers catalog for this information block. The value cannot be the same as `iblockId`.

Pass it together with `skuPropertyId`.

The identifier can be retrieved using the [catalog.catalog.list](./catalog-catalog-list-rest-v3.md) method.

Default: `0` — a product catalog ||
|| **skuPropertyId**
[`integer`](../../data-types.md) | Identifier of an active property of the `iblockId` information block of the "Binding to Elements" type that refers to the `productIblockId` information block. A variation stores the identifier of its parent product in this property.

Pass it together with `productIblockId`.

The identifier can be retrieved using the [catalog.productProperty.list](../product-property/catalog-product-property-list.md) method.

Default: `0` ||
|#

The `id`, `iblockTypeId`, `lid`, and `name` fields are read-only. If you pass them in `fields`, the method returns an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.catalog.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":87,"vatId":3}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.catalog.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":87,"vatId":3},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.catalog.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type CatalogAddResult = {
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
      const response = await $b24.actions.v3.call.make<CatalogAddResult>({
        method: 'catalog.catalog.add',
        params: {
          fields: {
            iblockId: 87,
            vatId: 3,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Catalog added:', result.item.id)
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
      async function addCatalog() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.catalog.add',
            params: {
              fields: {
                iblockId: 87,
                vatId: 3,
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Catalog added:', result.item.id)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addCatalog)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "iblockId": 87,
        "vatId": 3,
    }

    try:
        bitrix_response = client.catalog.catalog.add(
            fields=fields,
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
                'catalog.catalog.add',
                [
                    'fields' => [
                        'iblockId' => 87,
                        'vatId' => 3,
                    ],
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
        'catalog.catalog.add',
        {
            fields: {
                iblockId: 87,
                vatId: 3,
            },
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
        'catalog.catalog.add',
        [
            'fields' => [
                'iblockId' => 87,
                'vatId' => 3,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.catalog.add", b24.Params{
    	"fields": b24.Params{
    		"iblockId": 87,
    		"vatId":    3,
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.catalog.add: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	ID    int    `json:"id"`
    	VatID int    `json:"vatId"`
    	Name  string `json:"name"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parsing the response: %w", err)
    }
    fmt.Println(item.ID, item.Name)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

The method returns the created trade catalog with all its fields.

```json
{
    "result": {
        "item": {
            "id": 87,
            "iblockId": 87,
            "subscription": "N",
            "vatId": 3,
            "productIblockId": 0,
            "skuPropertyId": 0,
            "iblockTypeId": "lists",
            "lid": "s1",
            "name": "TestList"
        }
    },
    "time": {
        "start": 1792667012,
        "finish": 1792667012.061191,
        "duration": 0.06119108200073242,
        "processing": 0,
        "date_start": "2026-10-22T14:03:32+02:00",
        "date_finish": "2026-10-22T14:03:32+02:00",
        "operating_reset_at": 1792667580,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Object with the fields of the created trade catalog ||
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
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYALREADYEXISTSEXCEPTION",
        "message": "Record with ID 87 already exists."
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `iblockId` | This request requires data type `int` in field `iblockId` | Make sure the value is a number, not a string ||
|| `-` | The specified information block does not exist | Pass the identifier of an existing information block ||
|| `-` | The specified product information block does not exist | Check the `productIblockId` value ||
|| `-` | The commercial catalog cannot be a SKU information block of itself | Pass the identifier of another information block in `productIblockId`. If the request contains several errors, their texts arrive in one line separated by spaces ||
|| `-` | The product information block ID is specified, but the product information block binding property ID is missing | Pass `skuPropertyId` together with `productIblockId` ||
|| `-` | The product information block binding property ID is specified, but the product information block ID is missing | Pass `productIblockId` together with `skuPropertyId` ||
|| `-` | The specified binding property does not exist or is inactive | Specify an active property of the `iblockId` information block ||
|| `-` | Internal error adding catalog. Try adding again. | Repeat the request ||
|#

#### Field Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `iblockId` | Required field `iblockId` is empty | Add `iblockId` to `fields` ||
|| `#FIELD#` | Field `#FIELD#` is read-only | Remove the read-only fields from `fields`: `id`, `iblockTypeId`, `lid`, `name` ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Unknown field `#FIELD#` found in entity `CatalogDto` | Pass in `fields` only the fields from the [Parameter fields](#fields) table ||
|#

#### Duplicate Creation Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYALREADYEXISTSEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `iblockId` | Record with ID `#ID#` already exists | The information block is already a trade catalog. Update it using the [catalog.catalog.update](./catalog-catalog-update.md) method ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | No permission to change trade catalog settings. Call the method on behalf of an administrator ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-catalog-update.md)
- [{#T}](./catalog-catalog-get-rest-v3.md)
- [{#T}](./catalog-catalog-list-rest-v3.md)
- [{#T}](./catalog-catalog-delete.md)
- [{#T}](./catalog-catalog-field-list.md)
