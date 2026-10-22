# Get Price Field catalog.price.field.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note info "" %}

This method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The method `catalog.price.field.get` returns the description of a price field by its name.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the field whose description you need to retrieve.

Available fields:

- `id` — price identifier
- `productId` — identifier of the product or variation
- `catalogGroupId` — price type identifier
- `price` — price
- `currency` — currency code
- `extraId` — markup identifier
- `quantityFrom` — lower bound of the quantity range
- `quantityTo` — upper bound of the quantity range
- `priceScale` — price in the base currency
- `timestampX` — date and time of the last change ||
|| **select**
[`array`](../../data-types.md) | Array of strings with the names of the description properties to return in the response.

Default: all description properties.

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
- `multiple` — whether the field has multiple values
- `elementType` — element type for composite fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.price.field.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"price","select":["name","type","requiredGroups","editableGroups"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.price.field.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"name":"price","select":["name","type","requiredGroups","editableGroups"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.price.field.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type PriceFieldGetResult = {
      item: {
        name?: string
        type?: string
        requiredGroups?: string[] | null
        editableGroups?: string[] | null
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<PriceFieldGetResult>({
        method: 'catalog.price.field.get',
        params: {
          name: 'price',
          select: [
            'name',
            'type',
            'requiredGroups',
            'editableGroups',
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Field description:', result.item)
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
      async function getPriceField() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.price.field.get',
            params: {
              name: 'price',
              select: [
                'name',
                'type',
                'requiredGroups',
                'editableGroups',
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Field description:', result.item)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getPriceField)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.price.field.get(
            name="price",
            select=[
                "name",
                "type",
                "requiredGroups",
                "editableGroups",
            ],
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
                'catalog.price.field.get',
                [
                    'name' => 'price',
                    'select' => [
                        'name',
                        'type',
                        'requiredGroups',
                        'editableGroups',
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
        'catalog.price.field.get',
        {
            name: 'price',
            select: [
                'name',
                'type',
                'requiredGroups',
                'editableGroups',
            ],
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
        'catalog.price.field.get',
        [
            'name' => 'price',
            'select' => [
                'name',
                'type',
                'requiredGroups',
                'editableGroups',
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
    res, err := client.Core().Call(ctx, "catalog.price.field.get", b24.Params{
    	"name":   "price",
    	"select": []string{"name", "type", "requiredGroups", "editableGroups"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.price.field.get: %w", err)
    }

    // The method wraps the response in an object with the "item" key.
    raw, ok := b24.Unwrap(res.Result, "item")
    if !ok {
    	return fmt.Errorf("the response has no item key")
    }

    var item struct {
    	Name           string   `json:"name"`
    	Type           string   `json:"type"`
    	RequiredGroups []string `json:"requiredGroups"`
    	EditableGroups []string `json:"editableGroups"`
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
            "name": "price",
            "type": "float",
            "requiredGroups": [
                "add"
            ],
            "editableGroups": [
                "add",
                "update"
            ]
        }
    },
    "time": {
        "start": 1792668102,
        "finish": 1792668102.97455,
        "duration": 0.9745500087738037,
        "processing": 0,
        "date_start": "2026-10-22T14:21:42+02:00",
        "date_finish": "2026-10-22T14:21:42+02:00",
        "operating_reset_at": 1792668660,
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

The description content depends on `select`. The table lists all properties that can be requested.

#|
|| **Name**
`type` | **Description** ||
|| **name**
[`string`](../../data-types.md) | Field name ||
|| **type**
[`string`](../../data-types.md) | Type in the metadata: `int`, `float`, `string`, `object`. For `timestampX`, `object` is returned, although in method responses the value is passed as an ISO 8601 string ||
|| **title**
[`string`](../../data-types.md) | Field title. Matches `name` ||
|| **description**
[`string`](../../data-types.md) or `null` | Field description. For price fields — `null` ||
|| **validationRules**
[`array`](../../data-types.md) | Array of objects with validation rules. For `currency`, `[{}]` is returned — a length limit of three characters; the rule parameters are not serialized. For other fields — `[]` ||
|| **requiredGroups**
[`array`](../../data-types.md) or `null` | Array of operations in which the field is required. For `productId`, `catalogGroupId`, `price`, and `currency` — `["add"]`, for other fields — `null` ||
|| **filterable**
[`boolean`](../../data-types.md) | `true` — the field is available for filtering. For all price fields — `true` ||
|| **sortable**
[`boolean`](../../data-types.md) | `true` — the field is available for sorting. For all price fields — `true` ||
|| **editable**
[`boolean`](../../data-types.md) | `true` — the field can be passed in the operations from `editableGroups`, `false` — it cannot ||
|| **editableGroups**
[`array`](../../data-types.md) or `null` | Array of operations in which the field can be set. For `price`, `currency`, `extraId`, `quantityFrom`, and `quantityTo` — `["add", "update"]`, for other fields — `null`.

The [catalog.price.add](./catalog-price-add-rest-v3.md) method accepts and requires the `productId` and `catalogGroupId` fields, although `editableGroups` is `null` for them ||
|| **multiple**
[`boolean`](../../data-types.md) | Whether the field has multiple values. For all price fields — `false` ||
|| **elementType**
[`string`](../../data-types.md) or `null` | Element type of a composite field. For all price fields — `null` ||
|#

## Error Handling

HTTP status: **404**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION",
        "message": "Field \"foo\" was not found."
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Data Not Found Errors

Error Code: `BITRIX_REST_V3_REALISATION_EXCEPTION_FIELDNOTFOUNDEXCEPTION`

HTTP status: **404**

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Field `#FIELD#` was not found | Specify a field name from the list of the `name` parameter ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `name` | Required field `name` is missing | Pass the `name` parameter with an existing field name ||
|#

#### Errors in the `select` Parameter

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `DtoFieldDto` | Pass in `select` an array of strings only from the list of the `select` parameter ||
|#

Error Code: `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Invalid field in `select`: `#SELECT#` | Pass `select` as an array of strings, for example `["name","type"]` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Check the user's permissions and the `catalog` scope ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-field-list.md)
- [{#T}](./catalog-price-get-rest-v3.md)
- [{#T}](./catalog-price-add-rest-v3.md)
