# Get Values of Product or Variation Property Fields v 3.0 catalog.product.property.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" access permission

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.property.get` method returns the fields of a product or variation property by its identifier, for example its name, type, and symbolic code.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Property identifier.

The identifier can be retrieved using the [catalog.product.property.list](./catalog-product-property-list-rest-v3.md) method or from the [catalog.product.property.add](./catalog-product-property-add-rest-v3.md) response ||
|| **select**
[`array`](../../data-types.md) | List of property fields to return in the response.

Available fields:

- `id` — property identifier
- `timestampX` — date and time of the last modification
- `iblockId` — trade catalog identifier
- `name` — name
- `active` — activity
- `sort` — sort order
- `code` — symbolic code
- `defaultValue` — default value
- `propertyType` — base type
- `userType` — custom type
- `userTypeSettings` — custom type settings
- `rowCount` — number of rows in the input field
- `colCount` — number of columns in the input field
- `listType` — list appearance
- `multiple` — multiple property
- `multipleCnt` — number of fields for multiple values
- `xmlId` — external identifier
- `fileType` — file extensions
- `linkIblockId` — linked information block identifier
- `withDescription` — whether a description can be added to a value
- `searchable` — participation in search
- `filtrable` — participation in filtering
- `isRequired` — whether the property is required
- `hint` — hint
- `controlId` — name of the property field in information block forms

Default: all fields ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.get`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":295,"select":["id","iblockId","name","code","propertyType","listType","multiple","controlId"]}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":295,"select":["id","iblockId","name","code","propertyType","listType","multiple","controlId"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.get
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductPropertyGetResult = {
      item: {
        id: number
        iblockId: number
        name: string
        code: string | null
        propertyType: string
        listType: string
        multiple: boolean
        controlId: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductPropertyGetResult>({
        method: 'catalog.product.property.get',
        params: {
          id: 295,
          select: ['id', 'iblockId', 'name', 'code', 'propertyType', 'listType', 'multiple', 'controlId'],
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Property:', result.item.id, result.item.name, result.item.propertyType)
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
      async function getProductProperty() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.get',
            params: {
              id: 295,
              select: ['id', 'iblockId', 'name', 'code', 'propertyType', 'listType', 'multiple', 'controlId'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Property:', result.item.id, result.item.name, result.item.propertyType)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getProductProperty)
    </script>
    ```

- Python

    The `catalog.product.property.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.get"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "id": 295,
                "select": ["id", "iblockId", "name", "code", "propertyType", "listType", "multiple", "controlId"],
            },
            prefer_version=3,
        )
        result = response["result"]
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"code: {error.code}",
            f"message: {error.error.message}",
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
                'catalog.product.property.get',
                [
                    'id' => 295,
                    'select' => ['id', 'iblockId', 'name', 'code', 'propertyType', 'listType', 'multiple', 'controlId'],
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
        'catalog.product.property.get',
        {
            id: 295,
            select: ['id', 'iblockId', 'name', 'code', 'propertyType', 'listType', 'multiple', 'controlId'],
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
        'catalog.product.property.get',
        [
            'id' => 295,
            'select' => ['id', 'iblockId', 'name', 'code', 'propertyType', 'listType', 'multiple', 'controlId'],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.product.property.get", b24.Params{
    	"id":     295,
    	"select": []string{"id", "iblockId", "name", "code", "propertyType", "listType", "multiple", "controlId"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("catalog.product.property.get: %w", err)
    }

    var result struct {
    	Item struct {
    		ID           int    `json:"id"`
    		Name         string `json:"name"`
    		PropertyType string `json:"propertyType"`
    		ControlID    string `json:"controlId"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &result); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(result.Item.ID, result.Item.Name, result.Item.PropertyType, result.Item.ControlID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "item": {
            "id": 295,
            "iblockId": 25,
            "name": "New",
            "code": "NEWPRODUCT",
            "propertyType": "L",
            "listType": "C",
            "multiple": false,
            "controlId": "PROPERTY_295"
        }
    },
    "time": {
        "start": 1794924748,
        "finish": 1794924748.634806,
        "duration": 0.6348059177398682,
        "processing": 0,
        "date_start": "2026-11-17T17:12:28+02:00",
        "date_finish": "2026-11-17T17:12:28+02:00",
        "operating_reset_at": 1794925320,
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
[`object`](../../data-types.md) | Property fields. The set of fields depends on `select`. [Field description](#item) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Item Object {#item}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Property identifier ||
|| **timestampX**
[`datetime`](../../data-types.md) | Date and time of the last property modification ||
|| **iblockId**
[`integer`](../../data-types.md) | Identifier of the trade catalog the property belongs to ||
|| **name**
[`string`](../../data-types.md) | Property name ||
|| **active**
[`boolean`](../../data-types.md) | Property activity:
- `true` — the property is active
- `false` — the property is inactive ||
|| **sort**
[`integer`](../../data-types.md) | Sort order ||
|| **code**
[`string`](../../data-types.md) or `null` | Symbolic code of the property ||
|| **defaultValue**
[`string`](../../data-types.md) or `null` | Default value ||
|| **propertyType**
[`string`](../../data-types.md) | Base property type:
- `N` — number
- `S` — string
- `L` — list
- `F` — file
- `E` — link to elements
- `G` — link to sections ||
|| **userType**
[`string`](../../data-types.md) or `null` | Custom property type, for example `Money` or `DateTime`. `null` for a property without a custom type and for a checkbox property ||
|| **userTypeSettings**
[`object`](../../data-types.md) or [`array`](../../data-types.md) | Custom type settings. If there are no settings, an empty array `[]` ||
|| **rowCount**
[`integer`](../../data-types.md) | Number of rows in the input field ||
|| **colCount**
[`integer`](../../data-types.md) | Number of columns in the input field ||
|| **listType**
[`string`](../../data-types.md) | List appearance:
- `L` — dropdown list
- `C` — checkboxes ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple property:
- `true` — the property stores multiple values
- `false` — a single value ||
|| **multipleCnt**
[`integer`](../../data-types.md) or `null` | Number of fields for entering multiple values ||
|| **xmlId**
[`string`](../../data-types.md) or `null` | External identifier of the property ||
|| **fileType**
[`string`](../../data-types.md) or `null` | File extensions for a property of type `F` ||
|| **linkIblockId**
[`integer`](../../data-types.md) or `null` | For properties of type `E` and `G` — identifier of the information block whose elements or sections the property is linked to ||
|| **withDescription**
[`boolean`](../../data-types.md) | `true` — a description can be added to the property value ||
|| **searchable**
[`boolean`](../../data-types.md) | `true` — property values are included in search ||
|| **filtrable**
[`boolean`](../../data-types.md) | `true` — the property is used in product filtering ||
|| **isRequired**
[`boolean`](../../data-types.md) | `true` — the property is required on the product card ||
|| **hint**
[`string`](../../data-types.md) or `null` | Hint for the property field ||
|| **controlId**
[`string`](../../data-types.md) | Name of the property field in information block forms in the `PROPERTY_<id>` format, for example `PROPERTY_1291` ||
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
|| `id` | Required field `id` is missing | Pass `id` as an integer greater than `0` ||
|#

#### Object Not Found Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `id` | Entity with ID `#ID#` was not found | The property was not found or does not belong to a trade catalog. Check the identifier using the [catalog.product.property.list](./catalog-product-property-list-rest-v3.md) method ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `select` | Unknown field `#FIELD#` found in entity `ProductPropertyDto` | Specify only the fields from the `select` parameter description in `select` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" access permission ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-add-rest-v3.md)
- [{#T}](./catalog-product-property-update-rest-v3.md)
- [{#T}](./catalog-product-property-list-rest-v3.md)
- [{#T}](./catalog-product-property-delete-rest-v3.md)
- [{#T}](./catalog-product-property-field-list.md)
