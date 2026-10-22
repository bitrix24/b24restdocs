# Get Product Field catalog.product.field.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.product.field.get` returns the description of a product field by its name.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the field whose description to retrieve.

Available fields:

- `id` — product identifier
- `iblockId` — information block identifier
- `name` — name
- `code` — symbolic code
- `xmlId` — external code
- `active` — activity
- `sort` — sort order
- `createdBy` — created by
- `modifiedBy` — modified by
- `dateCreate` — creation date
- `timestampX` — modification date
- `dateActiveFrom` — start of activity
- `dateActiveTo` — end of activity
- `previewText` — preview text
- `previewTextType` — preview text type
- `previewPicture` — preview picture
- `detailText` — detail text
- `detailTextType` — detail text type
- `detailPicture` — detail picture
- `iblockSectionId` — main section
- `iblockSection` — all sections
- `type` — product type
- `available` — availability for purchase
- `bundle` — bundle flag
- `quantity` — quantity
- `quantityReserved` — reserved quantity
- `quantityTrace` — quantity tracking
- `canBuyZero` — purchase when out of stock
- `subscribe` — product subscription
- `vatId` — VAT rate
- `vatIncluded` — VAT included in the price
- `purchasingPrice` — purchase price
- `purchasingCurrency` — purchase price currency
- `barcodeMulti` — barcode for each item
- `weight` — weight
- `length` — length
- `width` — width
- `height` — height
- `measure` — unit of measurement
- `recurSchemeLength` — payment period length
- `recurSchemeType` — payment period unit
- `trialPriceId` — product for trial payment
- `withoutOrder` — renewal without an order
- `quantityTraceRaw` — stored tracking mode
- `paymentType` — payment type
- `subscribeRaw` — stored subscription permission
- `canBuyZeroRaw` — stored permission to purchase when out of stock
- `properties` — values of all product properties

Property keys `PROPERTY_{id}` are not supported: the method returns the "Field not found" error ||
|| **select**
[`array`](../../data-types.md) | Array of strings with the names of the description properties to return for each field.

Available fields:

- `name` — field name
- `type` — data type
- `title` — title
- `description` — description
- `validationRules` — validation rules
- `requiredGroups` — required groups
- `filterable` — whether the field is available in the filter
- `sortable` — whether the field is available for sorting
- `editable` — whether the field is editable
- `editableGroups` — groups of operations in which the field is editable
- `multiple` — whether the field is multiple
- `elementType` — element type for composite fields

The value `["*"]` is not supported.

Default: all properties ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"properties"}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"properties","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.field.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type FieldGetResult = {
      item: {
        name?: string
        type?: string
        title?: string
        editableGroups?: string[] | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<FieldGetResult>({
        method: 'catalog.product.field.get',
        params: {
          name: 'properties',
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field:', result.item)
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
      async function fieldGet() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.field.get',
            params: {
              name: 'properties',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fieldGet)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product.field.get(
            name="properties",
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
                'catalog.product.field.get',
                [
                    'name' => 'properties',
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
        'catalog.product.field.get',
        {
            name: 'properties',
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
        'catalog.product.field.get',
        [
            'name' => 'properties',
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.field.get", b24.Params{
    	"name":"properties",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.field.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	Name string `json:"name"`
    	Type string `json:"type"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Name, item.Type)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "name": "properties",
            "type": "array",
            "title": "properties",
            "description": null,
            "validationRules": [],
            "requiredGroups": null,
            "filterable": false,
            "sortable": false,
            "editable": true,
            "editableGroups": [
                "add",
                "update"
            ],
            "multiple": false,
            "elementType": null
        }
    },
    "time": {
        "start": 1792661048,
        "finish": 1792661048.738532,
        "duration": 0.7385320663452148,
        "processing": 0,
        "date_start": "2026-10-22T12:24:08+02:00",
        "date_finish": "2026-10-22T12:24:08+02:00",
        "operating_reset_at": 1792661640,
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
[`object`](../../data-types.md) | Field description. The [object properties](#item) depend on `select` ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

The set of description properties depends on `select`. The table contains all properties that can be requested.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field name: `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `createdBy`, `modifiedBy`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `previewText`, `previewTextType`, `previewPicture`, `detailText`, `detailTextType`, `detailPicture`, `iblockSectionId`, `iblockSection`, `type`, `available`, `bundle`, `quantity`, `quantityReserved`, `quantityTrace`, `canBuyZero`, `subscribe`, `vatId`, `vatIncluded`, `purchasingPrice`, `purchasingCurrency`, `barcodeMulti`, `weight`, `length`, `width`, `height`, `measure`, `recurSchemeLength`, `recurSchemeType`, `trialPriceId`, `withoutOrder`, `quantityTraceRaw`, `paymentType`, `subscribeRaw`, `canBuyZeroRaw`, `properties` ||
|| **type**
[`string`](../../data-types.md) | Type in the metadata: `int`, `string`, `float`, `object`, `array`. For date and file fields, `object` is returned, although in method responses dates are passed as ISO 8601 strings and files as a string with the identifier ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. For the fields of this object — `null` ||
|| **validationRules**
[`array`](../../data-types.md) | Validation rules. For the fields of this object — `[]` ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Array of strings with the names of operations in which the field is required. For `iblockId` and `name` — `["add"]`; for other fields — `null` ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` — the field is available for filtering in [catalog.product.list](./catalog-product-list-rest-v3.md), `false` — not available. `true` for the fields `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `createdBy`, `modifiedBy`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `iblockSectionId`, `type`, `available`, `bundle`, `quantity`, `quantityReserved`, `quantityTrace`, `canBuyZero`, `subscribe`, `vatId`, `vatIncluded`, `purchasingPrice`, `purchasingCurrency`, `barcodeMulti`, `weight`, `length`, `width`, `height`, `measure` ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` — the field is available for sorting, `false` — not available. `true` for the fields `id`, `iblockId`, `name`, `code`, `xmlId`, `active`, `sort`, `dateCreate`, `timestampX`, `dateActiveFrom`, `dateActiveTo`, `type`, `quantity` ||
|| **editable**
[`boolean`](../../data-types.md) | `true` — the field can be passed in the operations from `editableGroups`, `false` — it cannot. The flag does not replace the check of user permissions ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Array of strings with the names of operations in which the field can be set. For `iblockId` — `["add"]`; for `id`, `dateCreate`, `timestampX`, `type`, `available`, `bundle` — `null`; for other fields — `["add", "update"]` ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple value flag in the metadata. For all fields — `false` ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Element type of a composite field. For all fields — `null` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object.",
        "validation": [
            {
                "message": "Required field \"name\" is missing.",
                "field": "name"
            }
        ]
    }
}
```

{% include notitle [error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is missing | Pass the `name` parameter with an existing field name ||
|#

#### Field Not Found Error

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field `#FIELD#` was not found | Specify a field name from the `name` parameter description ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Pass only properties from the list: `name`, `type`, `title`, `description`, `validationRules`, `requiredGroups`, `filterable`, `sortable`, `editable`, `editableGroups`, `multiple`, `elementType` ||
|#

#### Select Format Error

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-field-list.md)
- [{#T}](./catalog-product-add-rest-v3.md)
- [{#T}](./catalog-product-update-rest-v3.md)
