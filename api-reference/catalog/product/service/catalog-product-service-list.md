# Get List of Services catalog.product.service.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with one of the "View product catalog", "View catalog products", or "Edit selling price" access permissions and the permission to read the catalog information block

The `catalog.product.service.list` method returns a list of catalog services with filtering, sorting, and pagination.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select***
[`array`](../../../data-types.md) | Fields of the [catalog_product_service](../../data-types.md#catalog_product_service) object to return. Be sure to include `id` and `iblockId`, for example `["id", "iblockId", "name"]`.

To retrieve property values, specify `propertyN` keys, where `N` is the property identifier.

The method skips unknown fields without an error ||
|| **filter***
[`object`](../../../data-types.md) | An object for filtering services in `{"field_1": "value_1", ... "field_N": "value_N"}` format.

Possible values for `field` correspond to the fields of the [catalog_product_service](../../data-types.md#catalog_product_service) object and the `propertyN` property keys. The method skips unknown fields without an error.

Required field: `iblockId` — the identifier of the trade catalog products information block, a single number. You can retrieve it using the [catalog.catalog.list](../../catalog/catalog-catalog-list.md) method.

A key can be assigned an additional prefix that specifies the filter behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `%` — LIKE, substring search in any position of the string. The `%` character does not need to be passed in the filter value
- `!%` — NOT LIKE, values without the substring. The `%` character does not need to be passed in the filter value
- `=` — equal, exact match. Used by default. For IN searches, you can pass multiple values as an array
- `!=` — not equal
- `!` — not equal. For NOT IN searches, you can pass multiple values as an array

The method does not support the `=%`, `%=`, `!=%`, `!%=`, `><`, and `!><` prefixes: a condition with such a prefix is skipped without an error, and the method returns services without applying this condition.

For `propertyN` properties, the value format in `filter` depends on the condition type:
- if you are filtering a date or date-time type property by range, use a prefix in the key and pass the value in the `value` field
- if you are filtering a numeric, string, or list property by exact match, pass the value directly without `value`
- for `IN` and `NOT IN` searches, you can pass an array of values

Example of filtering by properties:

```js
filter: {
    iblockId: 25,
    '>=property424': {
        value: '2025-05-29T12:00:00+01:00'
    },
    '<=property424': {
        value: '2025-05-30T13:00:00+01:00'
    },
    property996: 9636,
    property997: [9636, 568, 570, 9658],
}
```
||
|| **order**
[`object`](../../../data-types.md) |
An object for sorting selected services in `{"field_1": "order_1", ... "field_N": "order_N"}` format.

Possible values for `field` correspond to the fields of the [catalog_product_service](../../data-types.md#catalog_product_service) object. The method skips unknown fields without an error. If the parameter is not passed, services are sorted by `id` in ascending order.

Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order
||
|| **start**
[`integer`](../../../data-types.md) | The parameter is used to control pagination.

The results page size is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100` and so on.

Formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page.

If `start` exceeds the number of services found, the method returns the last page
||
|#

{% note warning "Working with service prices" %}

To retrieve service prices, use the [catalog.price.*](../../price/index.md) methods.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","bundle","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","iblockSectionId","modifiedBy","previewPicture","previewText","previewTextType","sort","timestampX","type","vatId","vatIncluded","xmlId","property301","property305"],"filter":{"iblockId":25,">id":10,"vatId":[1,2]},"order":{"id":"desc"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.service.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["id","iblockId","name","active","available","bundle","code","createdBy","dateActiveFrom","dateActiveTo","dateCreate","detailPicture","detailText","detailTextType","iblockSectionId","modifiedBy","previewPicture","previewText","previewTextType","sort","timestampX","type","vatId","vatIncluded","xmlId","property301","property305"],"filter":{"iblockId":25,">id":10,"vatId":[1,2]},"order":{"id":"desc"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.product.service.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ServiceItem = {
      id: number
      iblockId: number
      name: string
      active: string
      available: string
      bundle: string
      code: string
      createdBy: number
      dateActiveFrom: ISODate | null
      dateActiveTo: ISODate | null
      dateCreate: ISODate | null
      detailPicture: { id: string; url: string; urlMachine: string } | null
      detailText: string | null
      detailTextType: string
      iblockSectionId: number
      modifiedBy: number
      previewPicture: { id: string; url: string; urlMachine: string } | null
      previewText: string | null
      previewTextType: string
      sort: number
      timestampX: ISODate | null
      type: number
      vatId: number
      vatIncluded: string
      xmlId: string
    }

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ServiceListResult = {
      services: ServiceItem[]
    }

    try {
      // catalog.product.service.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<ServiceListResult>({
        method: 'catalog.product.service.list',
        params: {
          select: [
            'id',
            'iblockId',
            'name',
            'active',
            'available',
            'bundle',
            'code',
            'createdBy',
            'dateActiveFrom',
            'dateActiveTo',
            'dateCreate',
            'detailPicture',
            'detailText',
            'detailTextType',
            'iblockSectionId',
            'modifiedBy',
            'previewPicture',
            'previewText',
            'previewTextType',
            'sort',
            'timestampX',
            'type',
            'vatId',
            'vatIncluded',
            'xmlId',
            'property301',
            'property305',
          ],
          filter: {
            iblockId: 25,
            '>id': 10,
            vatId: [1, 2],
          },
          order: {
            id: 'desc',
          },
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Services on this page:', result.services.length, result.services)
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
      async function listServices() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // catalog.product.service.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.service.list',
            params: {
              select: [
                'id',
                'iblockId',
                'name',
                'active',
                'available',
                'bundle',
                'code',
                'createdBy',
                'dateActiveFrom',
                'dateActiveTo',
                'dateCreate',
                'detailPicture',
                'detailText',
                'detailTextType',
                'iblockSectionId',
                'modifiedBy',
                'previewPicture',
                'previewText',
                'previewTextType',
                'sort',
                'timestampX',
                'type',
                'vatId',
                'vatIncluded',
                'xmlId',
                'property301',
                'property305',
              ],
              filter: {
                iblockId: 25,
                '>id': 10,
                vatId: [1, 2],
              },
              order: {
                id: 'desc',
              },
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Services on this page:', result.services.length, result.services)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listServices)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.service.list(
            select=[
                "id",
                "iblockId",
                "name",
                "active",
                "available",
                "bundle",
                "code",
                "createdBy",
                "dateActiveFrom",
                "dateActiveTo",
                "dateCreate",
                "detailPicture",
                "detailText",
                "detailTextType",
                "iblockSectionId",
                "modifiedBy",
                "previewPicture",
                "previewText",
                "previewTextType",
                "sort",
                "timestampX",
                "type",
                "vatId",
                "vatIncluded",
                "xmlId",
                "property301",
                "property305",
            ],
            filter={
                "iblockId": 25,
                ">id": 10,
                "vatId": [
                    1,
                    2,
                ],
            },
            order={
                "id": "desc",
            },
            start=0,
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
                'catalog.product.service.list',
                [
                    'select' => [
                        'id',
                        'iblockId',
                        'name',
                        'active',
                        'available',
                        'bundle',
                        'code',
                        'createdBy',
                        'dateActiveFrom',
                        'dateActiveTo',
                        'dateCreate',
                        'detailPicture',
                        'detailText',
                        'detailTextType',
                        'iblockSectionId',
                        'modifiedBy',
                        'previewPicture',
                        'previewText',
                        'previewTextType',
                        'sort',
                        'timestampX',
                        'type',
                        'vatId',
                        'vatIncluded',
                        'xmlId',
                        'property301',
                        'property305',
                    ],
                    'filter' => [
                        'iblockId' => 25,
                        '>id'      => 10,
                        'vatId'    => [1, 2],
                    ],
                    'order'  => [
                        'id' => 'desc',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching service list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "catalog.product.service.list", {
            "select": [
                "id",
                "iblockId",
                "name",
                "active",
                "available",
                "bundle",
                "code",
                "createdBy",
                "dateActiveFrom",
                "dateActiveTo",
                "dateCreate",
                "detailPicture",
                "detailText",
                "detailTextType",
                "iblockSectionId",
                "modifiedBy",
                "previewPicture",
                "previewText",
                "previewTextType",
                "sort",
                "timestampX",
                "type",
                "vatId",
                "vatIncluded",
                "xmlId",
                "property301",
                "property305",
            ],
            "filter": {
                "iblockId": 25,
                ">id": 10,
                "vatId": [1, 2],
            },
            "order": {
                "id": "desc",
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.product.service.list',
        [
            'select' => [
                'id',
                'iblockId',
                'name',
                'active',
                'available',
                'bundle',
                'code',
                'createdBy',
                'dateActiveFrom',
                'dateActiveTo',
                'dateCreate',
                'detailPicture',
                'detailText',
                'detailTextType',
                'iblockSectionId',
                'modifiedBy',
                'previewPicture',
                'previewText',
                'previewTextType',
                'sort',
                'timestampX',
                'type',
                'vatId',
                'vatIncluded',
                'xmlId',
                'property301',
                'property305',
            ],
            'filter' => [
                'iblockId' => 25,
                '>id' => 10,
                'vatId' => [1, 2],
            ],
            'order' => [
                'id' => 'desc',
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
    res, err := client.Core().Call(ctx, "catalog.product.service.list", b24.Params{
    	"select": []string{"id", "iblockId", "name", "active", "available", "bundle", "code", "createdBy", "dateActiveFrom", "dateActiveTo", "dateCreate", "detailPicture", "detailText", "detailTextType", "iblockSectionId", "modifiedBy", "previewPicture", "previewText", "previewTextType", "sort", "timestampX", "type", "vatId", "vatIncluded", "xmlId", "property301", "property305"},
    	"filter": b24.Params{
    		"iblockId": 25,
    		">id":      10,
    		"vatId":    []int{1, 2},
    	},
    	"order": b24.Params{
    		"id": "desc",
    	},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.service.list: %w", err)
    }

    // The method wraps the response in an object with the "services" key.
    raw, ok := b24.Unwrap(res.Result, "services")
    if !ok {
    	return fmt.Errorf("no services key in the response")
    }

    var items []struct {
    	Active         string `json:"active"`
    	Available      string `json:"available"`
    	Bundle         string `json:"bundle"`
    	Code           string `json:"code"`
    	CreatedBy      int    `json:"createdBy"`
    	DateActiveFrom string `json:"dateActiveFrom"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.Active)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "services": [
            {
                "active": "Y",
                "available": "N",
                "bundle": "N",
                "code": "service",
                "createdBy": 1,
                "dateActiveFrom": null,
                "dateActiveTo": null,
                "dateCreate": "2025-05-12T12:04:01+01:00",
                "detailPicture": null,
                "detailText": null,
                "detailTextType": "text",
                "iblockId": 25,
                "iblockSectionId": null,
                "id": 6969,
                "modifiedBy": 1,
                "name": "Service",
                "previewPicture": null,
                "previewText": null,
                "previewTextType": "text",
                "property301": null,
                "property305": null,
                "sort": 100,
                "timestampX": "2025-05-12T12:04:01+01:00",
                "type": 7,
                "vatId": 1,
                "vatIncluded": "Y",
                "xmlId": "6969"
            }
        ]
    },
    "total": 1,
    "time": {
        "start": 1793963897,
        "finish": 1793963897.165754,
        "duration": 0.16575407981872559,
        "processing": 0,
        "date_start": "2026-11-06T14:18:17+01:00",
        "date_finish": "2026-11-06T14:18:17+01:00",
        "operating_reset_at": 1793964360,
        "operating": 0.13507294654846191
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root response item ||
|| **result.services**
[`catalog_product_service[]`](../../data-types.md#catalog_product_service) | Array of objects containing information about the selected services ||
|| **total**
[`integer`](../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"200040300010",
    "error_description":"Access denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `200040300010` | Access Denied | The user has none of the "View product catalog", "View catalog products", or "Edit selling price" access permissions ||
|| `400` | `200040300030` | Access Denied | The user does not have the permission to read the catalog information block ||
|| `400` | `0` | Required select fields: id, iblockId | `select` does not contain the `id` or `iblockId` field. The text lists the missing fields ||
|| `400` | `0` | Required filter fields: iblockId | `filter` does not contain the `iblockId` field ||
|| `400` | `0` | iblock is not catalog | `iblockId` is not a trade catalog information block ||
|| `400` | `0` | productType is not allowed for this catalog | `iblockId` contains a variations information block. Services are stored in the products information block ||
|| `400` | `0` | Wrong format value of filter field `iblockId`. | `iblockId` contains an array. Pass a single number ||
|| `400` | `100` | Invalid value {...} to match with parameter {select}. Should be value of type array. | The `select` parameter is not an array ||
|| `400` | `100` | Invalid value {...} to match with parameter {filter}. Should be value of type array. | The `filter` parameter is not an object ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-service-add.md)
- [{#T}](./catalog-product-service-update.md)
- [{#T}](./catalog-product-service-get.md)
- [{#T}](./catalog-product-service-download.md)
- [{#T}](./catalog-product-service-delete.md)
- [{#T}](./catalog-product-service-get-fields-by-filter.md)
