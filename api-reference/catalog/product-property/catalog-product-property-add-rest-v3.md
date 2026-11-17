# Add Product or Variation Property v 3.0 catalog.product.property.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: a user with the "View product catalog" permission and permission to edit the catalog information block

{% note info "" %}

The method belongs to REST 3.0. The call specifics and response format of the new API version are described in the [REST 3.0 overview](../../rest-v3.md).

{% endnote %}

The `catalog.product.property.add` method adds a product or variation property to the trade catalog and returns the fields of the new property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | An object with the fields of the new property. The required fields are `iblockId`, `name`, and `propertyType`. [Field description](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`integer`](../../data-types.md) | Trade catalog identifier. Products and variations have separate catalogs: for a product property, pass the product catalog identifier; for a variation property, pass the variation catalog identifier.

Both identifiers can be retrieved using the [catalog.catalog.list](../catalog/catalog-catalog-list-rest-v3.md) method. In the response, the variation catalog has the `productIblockId` field filled in.

The catalog cannot be changed after the property is created ||
|| **name***
[`string`](../../data-types.md) | Property name. An empty string causes an error ||
|| **propertyType***
[`string`](../../data-types.md) | Base property type.

Possible values:

- `N` — number
- `S` — string
- `L` — list
- `F` — file
- `E` — link to elements
- `G` — link to sections

The combinations of `propertyType` and `userType` supported by the product card are returned by the [catalog.product.property.type.list](./catalog-product-property-type-list.md) method ||
|| **userType**
[`string`](../../data-types.md) | Custom property type. It refines the base `propertyType` and is passed together with it. For example, a date and time property is `propertyType` = `S` and `userType` = `DateTime`.

Main values and the base `propertyType` to pass them with:

- `DateTime` — date and time, `S`
- `Money` — monetary value with currency, `S`
- `HTML` — text in HTML format, `S`
- `employee` — link to an employee, `S`
- `ECrm` — link to CRM elements, `S`
- `directory` — link to a directory, `S`
- `SKU` — link to variations, `E`
- `EList` — link to elements as a list, `E`
- `SectionAuto` — link to sections with autocomplete, `G`
- `BoolEnum` — checkbox, `L`

`BoolEnum` creates a checkbox property: a list with a single value `Y` displayed as a checkbox. The `BoolEnum` type is not visible in the response: `userType` is `null`, and `listType` is `C`. `BoolEnum` can be set only when adding a property.

If `userType` does not match `propertyType` or the type does not exist, the method returns an error ||
|| **userTypeSettings**
[`object`](../../data-types.md) | Custom type settings. Pass them only together with `userType`; otherwise, the method returns an error.

Values can be strings, numbers, booleans, or nested objects with such values. The method silently discards keys that the type does not have: for example, `HTML` retains `height`, but not an unknown key. The stored settings are returned in `userTypeSettings` in the response ||
|| **active**
[`boolean`](../../data-types.md) | Property activity.

Possible values:

- `true` — the property is active
- `false` — the property is inactive

Default: `true` ||
|| **sort**
[`integer`](../../data-types.md) | Sort order.

Default: `100` ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the property, for example `LINING_MATERIAL`. The code can consist of Latin letters, digits, and underscores and cannot start with a digit.

The method does not check that the code is unique: two properties can have the same code. To check whether a code is free, use the [catalog.product.property.list](./catalog-product-property-list-rest-v3.md) method with a filter by `code` ||
|| **defaultValue**
[`string`](../../data-types.md) | Default value of the property ||
|| **rowCount**
[`integer`](../../data-types.md) | Number of rows in the input field.

Default: `1` ||
|| **colCount**
[`integer`](../../data-types.md) | Number of columns in the input field.

Default: `30` ||
|| **listType**
[`string`](../../data-types.md) | Appearance of a property of type `L`.

Possible values:

- `L` — dropdown list
- `C` — checkboxes

Default: `L` ||
|| **multiple**
[`boolean`](../../data-types.md) | Multiple property.

Possible values:

- `true` — the property stores multiple values
- `false` — a single value

Default: `false` ||
|| **multipleCnt**
[`integer`](../../data-types.md) | Number of fields for entering multiple values ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the property ||
|| **fileType**
[`string`](../../data-types.md) | List of file extensions for a property of type `F`, for example `jpg, png` ||
|| **linkIblockId**
[`integer`](../../data-types.md) | For properties of type `E` and `G` — identifier of the information block whose elements or sections the property is linked to ||
|| **withDescription**
[`boolean`](../../data-types.md) | A description can be added to the property value.

Possible values:

- `true` — yes
- `false` — no

Default: `false` ||
|| **searchable**
[`boolean`](../../data-types.md) | Property values are included in search.

Possible values:

- `true` — yes
- `false` — no

Default: `false` ||
|| **filtrable**
[`boolean`](../../data-types.md) | The property is used in product filtering.

Possible values:

- `true` — yes
- `false` — no

Default: `false` ||
|| **isRequired**
[`boolean`](../../data-types.md) | The property is required on the product card.

Possible values:

- `true` — yes
- `false` — no

Default: `false` ||
|| **hint**
[`string`](../../data-types.md) | Hint for the property field ||
|#

The `active`, `multiple`, `withDescription`, `searchable`, `filtrable`, and `isRequired` flags are passed as `true` or `false`. The strings `Y` and `N` cause an error.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% note info "" %}

The new API call differs by adding the `/api/` segment to the request URL:

`https://{installation_address}/rest/api/{user_id}/{webhook_token}/catalog.product.property.add`

{% endnote %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Lining Material","propertyType":"S","code":"LINING_MATERIAL","sort":500,"filtrable":true,"hint":"What the lining is made of"}}' \
    https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/catalog.product.property.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"iblockId":25,"name":"Lining Material","propertyType":"S","code":"LINING_MATERIAL","sort":500,"filtrable":true,"hint":"What the lining is made of"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/api/catalog.product.property.add
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    type ProductPropertyAddResult = {
      item: {
        id: number
        iblockId: number
        name: string
        propertyType: string
        userType: string | null
        code: string | null
        sort: number
        active: boolean
        multiple: boolean
        isRequired: boolean
        controlId: string
      }
    }

    try {
      const response = await $b24.actions.v3.call.make<ProductPropertyAddResult>({
        method: 'catalog.product.property.add',
        params: {
          fields: {
            iblockId: 25,
            name: 'Lining Material',
            propertyType: 'S',
            code: 'LINING_MATERIAL',
            sort: 500,
            filtrable: true,
            hint: 'What the lining is made of',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Property created:', result.item.id, result.item.controlId)
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
      async function addProductProperty() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v3.call.make({
            method: 'catalog.product.property.add',
            params: {
              fields: {
                iblockId: 25,
                name: 'Lining Material',
                propertyType: 'S',
                code: 'LINING_MATERIAL',
                sort: 500,
                filtrable: true,
                hint: 'What the lining is made of',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Property created:', result.item.id, result.item.controlId)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addProductProperty)
    </script>
    ```

- Python

    The `catalog.product.property.*` REST 3.0 methods have no ready-made wrappers in `b24pysdk`, and the SDK sends to REST 3.0 only the methods listed in `Config().api_v3_methods`. Add the method to this list and call it with `call_method` of the client token, passing `prefer_version=3`.

    ```python
    from b24pysdk import Config
    from b24pysdk.errors import BitrixSDKException
    from b24pysdk.errors.v3 import BitrixAPIError

    method = "catalog.product.property.add"
    Config().configure(api_v3_methods=Config().api_v3_methods | {method})
    bitrix_token = client.get_token()

    try:
        response = bitrix_token.call_method(
            api_method=method,
            params={
                "fields": {
                    "iblockId": 25,
                    "name": "Lining Material",
                    "propertyType": "S",
                    "code": "LINING_MATERIAL",
                    "sort": 500,
                    "filtrable": True,
                    "hint": "What the lining is made of",
                },
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
                'catalog.product.property.add',
                [
                    'fields' => [
                        'iblockId' => 25,
                        'name' => 'Lining Material',
                        'propertyType' => 'S',
                        'code' => 'LINING_MATERIAL',
                        'sort' => 500,
                        'filtrable' => true,
                        'hint' => 'What the lining is made of',
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
        'catalog.product.property.add',
        {
            fields: {
                iblockId: 25,
                name: 'Lining Material',
                propertyType: 'S',
                code: 'LINING_MATERIAL',
                sort: 500,
                filtrable: true,
                hint: 'What the lining is made of',
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
        'catalog.product.property.add',
        [
            'fields' => [
                'iblockId' => 25,
                'name' => 'Lining Material',
                'propertyType' => 'S',
                'code' => 'LINING_MATERIAL',
                'sort' => 500,
                'filtrable' => true,
                'hint' => 'What the lining is made of',
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
    res, err := client.Core().Call(ctx, "catalog.product.property.add", b24.Params{
    	"fields": b24.Params{
    		"iblockId":     25,
    		"name":         "Lining Material",
    		"propertyType": "S",
    		"code":         "LINING_MATERIAL",
    		"sort":         500,
    		"filtrable":    true,
    		"hint":         "What the lining is made of",
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.product.property.add: %w", err)
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
            "id": 1291,
            "timestampX": "2026-11-17T16:40:30+02:00",
            "iblockId": 25,
            "name": "Lining Material",
            "active": true,
            "sort": 500,
            "code": "LINING_MATERIAL",
            "defaultValue": null,
            "propertyType": "S",
            "userType": null,
            "userTypeSettings": [],
            "rowCount": 1,
            "colCount": 30,
            "listType": "L",
            "multiple": false,
            "multipleCnt": null,
            "xmlId": null,
            "fileType": null,
            "linkIblockId": null,
            "withDescription": false,
            "searchable": false,
            "filtrable": true,
            "isRequired": false,
            "hint": "What the lining is made of",
            "controlId": "PROPERTY_1291"
        }
    },
    "time": {
        "start": 1794922830,
        "finish": 1794922830.333162,
        "duration": 0.3331620693206787,
        "processing": 0,
        "date_start": "2026-11-17T16:40:30+02:00",
        "date_finish": "2026-11-17T16:40:30+02:00",
        "operating_reset_at": 1794923400,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object with the response data ||
|| **result.item**
[`object`](../../data-types.md) | Fields of the created property. [Field description](#item) ||
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
[`string`](../../data-types.md) | Name of the property field in the information block forms in the `PROPERTY_<id>` format, for example `PROPERTY_1291` ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION",
        "message": "Error validating object.",
        "validation": [
            {
                "message": "Required field \"propertyType\" is empty.",
                "field": "propertyType"
            }
        ]
    }
}
```

{% include notitle [Error handling](../../../_includes/error-info-v3.md) %}

### Possible Error Codes

#### Object Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_DTOVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `iblockId` | Required field `iblockId` is empty | Pass `iblockId` in `fields` ||
|| `name` | Required field `name` is empty | Pass `name` in `fields` ||
|| `propertyType` | Required field `propertyType` is empty | Pass `propertyType` in `fields` ||
|| `#FIELD#` | Field "#FIELD#" is read-only | Remove the `id`, `timestampX`, and `controlId` fields from `fields`: the system fills them in ||
|#

#### Request Validation Errors

Error Code: `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `fields` | Required field `fields` is missing | Pass the `fields` object ||
|| `-` | Required fields: iblockId | Pass an identifier greater than `0` in `iblockId` ||
|| `-` | The specified iblock is not a product catalog | The information block was not found or is not a trade catalog. Check the identifier using the [catalog.catalog.list](../catalog/catalog-catalog-list-rest-v3.md) method ||
|| `#FIELD#` | This request requires data type `#TYPE#` in field `#FIELD#` | Pass a value of the required type. Pass flags as `true` or `false`, not as the strings `Y` and `N` ||
|| `-` | Name is not specified. | Pass a non-empty string in `name` ||
|| `-` | Invalid property type specified | Pass a `propertyType` from the list of possible values and a `userType` that matches it. The type combinations are returned by the [catalog.product.property.type.list](./catalog-product-property-type-list.md) method ||
|| `-` | Custom property type settings are not supported for this property type | Remove `userTypeSettings` or pass `userType` together with them ||
|| `-` | Invalid custom property type settings specified | Pass an object in `userTypeSettings` whose values are strings, numbers, booleans, or nested objects with such values ||
|| `-` | Property code cannot start with digit.: `#CODE#` | Start `code` with a Latin letter or an underscore ||
|#

#### Unknown Field Error

Error Code: `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Unknown field `#FIELD#` found in entity `ProductPropertyDto` | Pass only the fields from the `fields` parameter description in `fields` ||
|#

#### Access Error

Error Code: `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION`

#|
|| **Field** | **Error description** | **How to Fix** ||
|| `-` | Access denied | Make sure the user has the "View product catalog" permission and permission to edit the catalog information block ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-update-rest-v3.md)
- [{#T}](./catalog-product-property-get-rest-v3.md)
- [{#T}](./catalog-product-property-list-rest-v3.md)
- [{#T}](./catalog-product-property-delete-rest-v3.md)
- [{#T}](./catalog-product-property-field-list.md)
- [{#T}](./catalog-product-property-type-list.md)
