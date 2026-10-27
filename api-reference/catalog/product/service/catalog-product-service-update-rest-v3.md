# Update Service Fields v 3.0 catalog.product.service.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit product" permission and one of the "View product catalog", "View catalog products", or "Edit selling price" permissions

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../../rest-v3.md).

{% endnote %}

The `catalog.product.service.update` method updates the service fields and returns its fields after the update.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Service identifier.

The identifier can be retrieved using the [catalog.product.service.list](./catalog-product-service-list-rest-v3.md) method ||
|| **fields***
[`object`](../../../data-types.md) | An object with the service fields to update. Pass only the fields you change. [Field description](#fields)

An empty object `{}` returns the service unchanged ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../../data-types.md) | Name of the service ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code ||
|| **active**
[`string`](../../../data-types.md) | Service activity status.

Possible values:

- `Y` — yes
- `N` — no

Any value other than `Y` is saved as `N` ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the service.

The identifier can be retrieved using the [user.get](../../../user/user-get.md) method ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the service.

The identifier can be retrieved using the [user.get](../../../user/user-get.md) method ||
|| **dateActiveFrom**
[`datetime`](../../../data-types.md) | Activity start date in ISO 8601 format, for example `2026-11-01T10:00:00+02:00` ||
|| **dateActiveTo**
[`datetime`](../../../data-types.md) | Activity end date in ISO 8601 format ||
|| **previewText**
[`string`](../../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Preview description type.

Possible values:

- `text` — text
- `html` — HTML ||
|| **previewPicture**
[`object`](../../../data-types.md) | Preview picture. An object in the format `{"fileData": ["file_name.png", "base64 content"]}`.

Currently the method accepts this object without an error but does not save the file: the field stays `null` in the response. You can upload a picture using the previous version method [catalog.product.service.update](./catalog-product-service-update.md) ||
|| **detailText**
[`string`](../../../data-types.md) | Detail description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Detail description type.

Possible values:

- `text` — text
- `html` — HTML ||
|| **detailPicture**
[`object`](../../../data-types.md) | Detail picture. An object in the format `{"fileData": ["file_name.png", "base64 content"]}`.

Currently the method accepts this object without an error but does not save the file: the field stays `null` in the response. You can upload a picture using the previous version method [catalog.product.service.update](./catalog-product-service-update.md) ||
|| **iblockSectionId**
[`integer`](../../../data-types.md) | Identifier of the main catalog section.

The identifier can be retrieved using the [catalog.section.list](../../section/catalog-section-list-rest-v3.md) method.

For a section that does not exist, the method returns an access error ||
|| **iblockSection**
[`array`](../../../data-types.md) | An array of identifiers of the sections to link the service to, for example `[31, 45]`. If `iblockSectionId` is not passed, the first section in the array becomes the main one.

An empty array together with `iblockSectionId: 0` unlinks the service from all sections ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity ||
|| **quantityReserved**
[`double`](../../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../../data-types.md) | Quantity tracking mode.

Possible values:

- `Y` — yes
- `N` — no
- `D` — according to the catalog settings ||
|| **canBuyZero**
[`string`](../../../data-types.md) | Allow purchase when out of stock.

Possible values:

- `Y` — yes
- `N` — no
- `D` — according to the catalog settings ||
|| **subscribe**
[`string`](../../../data-types.md) | Allow subscription to the product.

Possible values:

- `Y` — yes
- `N` — no
- `D` — according to the catalog settings ||
|| **vatId**
[`integer`](../../../data-types.md) | VAT rate identifier.

The identifier can be retrieved using the [catalog.vat.list](../../vat/catalog-vat-list-rest-v3.md) method.

The rate existence is not checked on update: an identifier that does not exist is saved without an error ||
|| **vatIncluded**
[`string`](../../../data-types.md) | Whether VAT is included in the price.

Possible values:

- `Y` — yes
- `N` — no ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price ||
|| **purchasingCurrency**
[`string`](../../../data-types.md) | Currency code of the purchase price, for example `EUR`.

Currency codes can be retrieved using the [crm.currency.list](../../../crm/currency/crm-currency-list.md) method.

A currency code that does not exist is not saved on update, and no error is returned ||
|| **barcodeMulti**
[`string`](../../../data-types.md) | Unique barcodes for each item.

Possible values:

- `Y` — yes
- `N` — no ||
|| **weight**
[`double`](../../../data-types.md) | Weight ||
|| **length**
[`double`](../../../data-types.md) | Length ||
|| **width**
[`double`](../../../data-types.md) | Width ||
|| **height**
[`double`](../../../data-types.md) | Height ||
|| **measure**
[`integer`](../../../data-types.md) | Unit of measurement identifier.

The identifier can be retrieved using the [catalog.measure.list](../../measure/catalog-measure-list-rest-v3.md) method ||
|| **recurSchemeLength**
[`integer`](../../../data-types.md) | Payment period length. Used for content sales in the on-premise version ||
|| **recurSchemeType**
[`string`](../../../data-types.md) | Time unit of the payment period.

Possible values:

- `H` — hour
- `D` — day
- `W` — week
- `M` — month
- `Q` — quarter
- `S` — half-year
- `Y` — year
- `T` — two years

Used for content sales in the on-premise version ||
|| **trialPriceId**
[`integer`](../../../data-types.md) | Identifier of the product for a trial payment. Used for content sales in the on-premise version ||
|| **withoutOrder**
[`string`](../../../data-types.md) | Renewal without placing an order.

Possible values:

- `Y` — yes
- `N` — no

Used for content sales in the on-premise version ||
|| **quantityTraceRaw**
[`string`](../../../data-types.md) | Saved value of the quantity tracking mode.

Possible values:

- `Y` — yes
- `N` — no
- `D` — according to the catalog settings ||
|| **paymentType**
[`string`](../../../data-types.md) | Payment type.

Possible values:

- `S` — one-time
- `R` — recurring
- `T` — trial

Used for content sales in the on-premise version ||
|| **subscribeRaw**
[`string`](../../../data-types.md) | Saved value of the subscription permission.

Possible values:

- `Y` — yes
- `N` — no
- `D` — according to the catalog settings ||
|| **canBuyZeroRaw**
[`string`](../../../data-types.md) | Saved value of the out-of-stock purchase permission.

Possible values:

- `Y` — yes
- `N` — no
- `D` — according to the catalog settings ||
|| **properties**
[`object`](../../../data-types.md) | Service property values. The keys are `PROPERTY_{id}`, where `{id}` is the property identifier.

Pass the value of a single property as an object `{"VALUE": "..."}`, for example `{"PROPERTY_301": {"VALUE": "SRV-002"}}`: the method skips a string without an object and returns no error. For a multiple property, pass an array of values, for example `["linen", "cotton"]`, which replaces the previous values.

Property identifiers can be retrieved using the [catalog.productProperty.list](../../product-property/catalog-product-property-list.md) method ||
|#

The `iblockId` field cannot be changed. The `id`, `dateCreate`, `timestampX`, `type`, `available`, and `bundle` fields are read-only, and passing them causes an error.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.service.update`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7551,"fields":{"name":"Equipment installation and setup","sort":200,"properties":{"PROPERTY_301":{"VALUE":"SRV-002"}}}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7551,"fields":{"name":"Equipment installation and setup","sort":200,"properties":{"PROPERTY_301":{"VALUE":"SRV-002"}}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.service.update
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ServiceUpdateResult = {
      item: {
        id: number
        iblockId: number
        name: string
        xmlId: string | null
        active: 'Y' | 'N'
        sort: number
        type: number
        iblockSectionId: number | null
        vatId: number | null
        vatIncluded: 'Y' | 'N'
        measure: number | null
        previewText: string | null
        detailPicture: string | null
        timestampX: ISODate
        properties: Record<string, { VALUE: string, VALUE_ID: string } | Array<{ VALUE: string, VALUE_ID: string }> | null>
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ServiceUpdateResult>({
        method: 'catalog.product.service.update',
        params: {
          id: 7551,
          fields: {
            name: 'Equipment installation and setup',
            sort: 200,
            properties: {
              PROPERTY_301: {
                VALUE: 'SRV-002',
              },
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Service updated:', result.item.id, result.item.name)
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
      async function updateService() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.service.update',
            params: {
              id: 7551,
              fields: {
                name: 'Equipment installation and setup',
                sort: 200,
                properties: {
                  PROPERTY_301: {
                    VALUE: 'SRV-002',
                  },
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Service updated:', result.item.id, result.item.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateService)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.service.update(
            bitrix_id=7551,
            fields={
                "name": "Equipment installation and setup",
                "sort": 200,
                "properties": {
                    "PROPERTY_301": {
                        "VALUE": "SRV-002",
                    },
                },
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.product.service.update',
                [
                    'id' => 7551,
                    'fields' => [
                        'name' => 'Equipment installation and setup',
                        'sort' => 200,
                        'properties' => [
                            'PROPERTY_301' => [
                                'VALUE' => 'SRV-002',
                            ],
                        ],
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

    SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via cURL or fetch.

    ```js
    BX24.callMethod(
        'catalog.product.service.update',
        {
            id: 7551,
            fields: {
                name: 'Equipment installation and setup',
                sort: 200,
                properties: {
                    PROPERTY_301: {
                        VALUE: 'SRV-002',
                    },
                },
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
        'catalog.product.service.update',
        [
            'id' => 7551,
            'fields' => [
                'name' => 'Equipment installation and setup',
                'sort' => 200,
                'properties' => [
                    'PROPERTY_301' => [
                        'VALUE' => 'SRV-002',
                    ],
                ],
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
    res, err := client.Core().Call(ctx, "catalog.product.service.update", b24.Params{
    	"id":     7551,
    	"fields": b24.Params{
    		"name":       "Equipment installation and setup",
    		"sort":       200,
    		"properties": b24.Params{
    			"PROPERTY_301": b24.Params{
    				"VALUE": "SRV-002",
    			},
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.service.update: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("no item key in the response")
    }

    var item struct {
    	ID      int    `json:"id"`
    	Name    string `json:"name"`
    	Type    int    `json:"type"`
    	Measure int    `json:"measure"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Name, item.Type)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 7551,
            "iblockId": 25,
            "name": "Equipment installation and setup",
            "code": null,
            "xmlId": "DOCS-V3-SERVICE-1",
            "active": "Y",
            "sort": 200,
            "createdBy": 1,
            "modifiedBy": 1,
            "dateCreate": "2026-10-22T12:18:35+02:00",
            "timestampX": "2026-10-22T12:19:35+02:00",
            "dateActiveFrom": null,
            "dateActiveTo": null,
            "previewText": "Installation and connection of equipment",
            "previewTextType": "text",
            "previewPicture": null,
            "detailText": null,
            "detailTextType": "text",
            "detailPicture": "39453",
            "iblockSectionId": 45,
            "iblockSection": [
                45
            ],
            "type": 7,
            "available": "N",
            "bundle": "N",
            "quantity": 0,
            "quantityReserved": 0,
            "quantityTrace": "N",
            "canBuyZero": "Y",
            "subscribe": "Y",
            "vatId": 1,
            "vatIncluded": "Y",
            "purchasingPrice": null,
            "purchasingCurrency": null,
            "barcodeMulti": "N",
            "weight": 0,
            "length": null,
            "width": null,
            "height": null,
            "measure": 9,
            "recurSchemeLength": null,
            "recurSchemeType": "D",
            "trialPriceId": null,
            "withoutOrder": "N",
            "quantityTraceRaw": "N",
            "paymentType": "S",
            "subscribeRaw": "D",
            "canBuyZeroRaw": "Y",
            "properties": {
                "PROPERTY_309": null,
                "PROPERTY_293": null,
                "PROPERTY_295": null,
                "PROPERTY_297": null,
                "PROPERTY_299": null,
                "PROPERTY_301": {
                    "VALUE": "SRV-002",
                    "VALUE_ID": "4033"
                },
                "PROPERTY_303": null,
                "PROPERTY_305": null,
                "PROPERTY_307": null,
                "PROPERTY_311": null,
                "PROPERTY_313": null,
                "PROPERTY_315": null,
                "PROPERTY_329": null,
                "PROPERTY_343": null,
                "PROPERTY_359": null,
                "PROPERTY_431": null,
                "PROPERTY_433": null,
                "PROPERTY_435": null,
                "PROPERTY_893": null,
                "PROPERTY_897": null,
                "PROPERTY_901": null,
                "PROPERTY_917": null,
                "PROPERTY_921": null,
                "PROPERTY_931": null,
                "PROPERTY_933": null,
                "PROPERTY_935": null,
                "PROPERTY_1013": null,
                "PROPERTY_1015": null,
                "PROPERTY_1019": null,
                "PROPERTY_1021": null,
                "PROPERTY_1023": null,
                "PROPERTY_1077": null,
                "PROPERTY_1079": null,
                "PROPERTY_1081": null,
                "PROPERTY_1083": null,
                "PROPERTY_1085": null,
                "PROPERTY_1231": null
            }
        }
    },
    "time": {
        "start": 1792660775,
        "finish": 1792660775.091972,
        "duration": 0.09197211265563965,
        "processing": 0,
        "date_start": "2026-10-22T12:19:35+02:00",
        "date_finish": "2026-10-22T12:19:35+02:00",
        "operating_reset_at": 1792660980,
        "operating": 1.2356388568878174
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object with the response data ||
|| **result.item**
[`object`](../../../data-types.md) | Service fields after the update. [Field description](#item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Service identifier ||
|| **iblockId**
[`integer`](../../../data-types.md) | Identifier of the trade catalog information block ||
|| **name**
[`string`](../../../data-types.md) | Name of the service ||
|| **code**
[`string`](../../../data-types.md) | Symbolic code ||
|| **xmlId**
[`string`](../../../data-types.md) | External code. If the code was not passed on creation, it equals the service identifier ||
|| **active**
[`string`](../../../data-types.md) | Service activity status: `Y` — active, `N` — inactive ||
|| **sort**
[`integer`](../../../data-types.md) | Sort order ||
|| **createdBy**
[`integer`](../../../data-types.md) | Identifier of the user who created the service ||
|| **modifiedBy**
[`integer`](../../../data-types.md) | Identifier of the user who last modified the service ||
|| **dateCreate**
[`datetime`](../../../data-types.md) | Creation date and time ||
|| **timestampX**
[`datetime`](../../../data-types.md) | Date and time of the last modification ||
|| **dateActiveFrom**
[`datetime`](../../../data-types.md) | Activity start date ||
|| **dateActiveTo**
[`datetime`](../../../data-types.md) | Activity end date ||
|| **previewText**
[`string`](../../../data-types.md) | Preview description ||
|| **previewTextType**
[`string`](../../../data-types.md) | Preview description type: `text` or `html` ||
|| **previewPicture**
[`string`](../../../data-types.md) | Preview picture file identifier as a string. `null` if there is no picture ||
|| **detailText**
[`string`](../../../data-types.md) | Detail description ||
|| **detailTextType**
[`string`](../../../data-types.md) | Detail description type: `text` or `html` ||
|| **detailPicture**
[`string`](../../../data-types.md) | Detail picture file identifier as a string. `null` if there is no picture. The file can be retrieved using the [catalog.product.service.download](./catalog-product-service-download-rest-v3.md) method ||
|| **iblockSectionId**
[`integer`](../../../data-types.md) | Main section identifier. `null` if the service is not linked to a section ||
|| **iblockSection**
[`array`](../../../data-types.md) | An array of identifiers of all sections the service is linked to ||
|| **type**
[`integer`](../../../data-types.md) | Product type. Always `7` for a service ||
|| **available**
[`string`](../../../data-types.md) | Available for purchase: `Y` — yes, `N` — no ||
|| **bundle**
[`string`](../../../data-types.md) | Set flag: `Y` — yes, `N` — no ||
|| **quantity**
[`double`](../../../data-types.md) | Quantity ||
|| **quantityReserved**
[`double`](../../../data-types.md) | Reserved quantity ||
|| **quantityTrace**
[`string`](../../../data-types.md) | Effective quantity tracking mode, taking the catalog settings into account: `Y` — enabled, `N` — disabled ||
|| **canBuyZero**
[`string`](../../../data-types.md) | Effective out-of-stock purchase permission, taking the catalog settings into account: `Y` — yes, `N` — no ||
|| **subscribe**
[`string`](../../../data-types.md) | Effective subscription permission, taking the catalog settings into account: `Y` — yes, `N` — no ||
|| **vatId**
[`integer`](../../../data-types.md) | VAT rate identifier. `null` if no rate is set ||
|| **vatIncluded**
[`string`](../../../data-types.md) | VAT is included in the price: `Y` — yes, `N` — no ||
|| **purchasingPrice**
[`double`](../../../data-types.md) | Purchase price. `null` if not set ||
|| **purchasingCurrency**
[`string`](../../../data-types.md) | Purchase price currency. `null` if not set ||
|| **barcodeMulti**
[`string`](../../../data-types.md) | Unique barcodes for each item: `Y` — yes, `N` — no ||
|| **weight**
[`double`](../../../data-types.md) | Weight ||
|| **length**
[`double`](../../../data-types.md) | Length. `null` if not set ||
|| **width**
[`double`](../../../data-types.md) | Width. `null` if not set ||
|| **height**
[`double`](../../../data-types.md) | Height. `null` if not set ||
|| **measure**
[`integer`](../../../data-types.md) | Unit of measurement identifier ||
|| **recurSchemeLength**
[`integer`](../../../data-types.md) | Payment period length. Used for content sales in the on-premise version ||
|| **recurSchemeType**
[`string`](../../../data-types.md) | Time unit of the payment period, for example `D` — day. Used for content sales in the on-premise version ||
|| **trialPriceId**
[`integer`](../../../data-types.md) | Identifier of the product for a trial payment. Used for content sales in the on-premise version ||
|| **withoutOrder**
[`string`](../../../data-types.md) | Renewal without placing an order: `Y` — yes, `N` — no ||
|| **quantityTraceRaw**
[`string`](../../../data-types.md) | Saved value of the quantity tracking mode: `Y`, `N`, or `D` — according to the catalog settings ||
|| **paymentType**
[`string`](../../../data-types.md) | Payment type: `S` — one-time, `R` — recurring, `T` — trial ||
|| **subscribeRaw**
[`string`](../../../data-types.md) | Saved value of the subscription permission: `Y`, `N`, or `D` — according to the catalog settings ||
|| **canBuyZeroRaw**
[`string`](../../../data-types.md) | Saved value of the out-of-stock purchase permission: `Y`, `N`, or `D` — according to the catalog settings ||
|| **properties**
[`object`](../../../data-types.md) | Service property values. The keys are `PROPERTY_{id}`, where `{id}` is the property identifier. A property value is an object `{"VALUE": "...", "VALUE_ID": "..."}`, and for a multiple property it is an array of such objects. `null` if no value is set ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION",
        "message": "Entity with ID 999999 was not found."
    }
}
```

{% include notitle [Error handling](../../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | At least one of the required fields must be not empty: id, filter | Pass the service `id`. A second message is returned together with this one: "Only one of the following values must be provided: id, filter" ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `-` | Product - `#ID#` is not exists in catalog - `#IBLOCK_ID#` | Remove `iblockId` from `fields`: a service cannot be moved to another catalog ||
|| `-` | Name is not specified.`<br>` | Pass a non-empty value in `name` ||
|| `-` | User createdBy is not exists | Pass the identifier of an existing user in `createdBy` ||
|| `-` | User modifiedBy is not exists | Pass the identifier of an existing user in `modifiedBy` ||
|| `-` | The properties field accepts only the property keys PROPERTY_{id}, unknown keys: `#KEYS#` | Pass property keys in the `PROPERTY_{id}` format, for example `PROPERTY_301` ||
|| `-` | The properties field names one property by several keys: `#KEYS#` | Pass each property with a single key ||
|| `#FIELD#` | This request requires data type `#TYPE#` in field `#FIELD#` | Pass a value of the required type ||
|| `-` | Internal error updating product. Try updating again. | Repeat the request ||
|#

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `#FIELD#` | Field `#FIELD#` is read-only | Remove the read-only fields from `fields`: `id`, `dateCreate`, `timestampX`, `type`, `available`, `bundle` ||
|#

#### Filter Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDFILTEREXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `filter` | Invalid filter `filter` | The method does not support `filter`. Pass the service `id` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | There is no service with this identifier. The error is also returned for the identifier of a product of another type. Check the identifier using the [catalog.product.service.list](./catalog-product-service-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductDto` | Pass only the fields from the [Parameter fields](#fields) table in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

HTTP status: **403**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions. The error is also returned if the section from `iblockSectionId` does not exist: check the identifier using the [catalog.section.list](../../section/catalog-section-list-rest-v3.md) method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-add-rest-v3.md)
- [{#T}](./catalog-product-service-get-rest-v3.md)
- [{#T}](./catalog-product-service-list-rest-v3.md)
- [{#T}](./catalog-product-service-delete-rest-v3.md)
- [{#T}](./catalog-product-service-field-list.md)
