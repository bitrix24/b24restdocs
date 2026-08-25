# Add Product Property or Variation catalog.productProperty.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to modify the product catalog

The method `catalog.productProperty.add` adds a property to a product or variation.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Set of fields for the new property [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the trade catalog. 

Existing identifiers can be obtained using the method [catalog.catalog.list](../catalog/catalog-catalog-list.md) ||
|| **name***
[`string`](../../data-types.md) | Name of the property ||
|| **propertyType***
[`string`](../../data-types.md) | Basic type of the property. Allowed values:
- `N` — number
- `S` — string
- `L` — list
- `F` — file
- `E` — binding to elements
- `G` — binding to sections ||
|| **active**
[`char`](../../data-types.md) | Activity status. Allowed values:
- `Y` — yes
- `N` — no
||
|| **sort**
[`integer`](../../data-types.md) | Sorting index ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the property. The property code can consist of Latin letters, numbers, and underscores. The first character cannot be a digit ||
|| **defaultValue**
[`text`](../../data-types.md) | Default value of the property ||
|| **userType**
[`string`](../../data-types.md) | Custom property type. The value must correspond to the specified `propertyType`.

Value examples:
- `DateTime` — date and time
- `Money` — monetary value with currency
- `SKU` — link to product variations
- `directory` — link to a directory
- `employee` — link to an employee
- `UserID` — link to a user
- `EList` — select item from a list
- `EAutocomplete` — link to elements with auto-search
- `SectionAuto` — link to sections with auto-search
- `HTML` — value in HTML format
- `map_google` — coordinates and address on Google Maps
- `map_yandex` — coordinates and address on Yandex Maps
- `DiskFile` — link to a file from Bitrix24.Drive
- `ECrm` — link to CRM elements
- `BoolEnum` — checkbox based on a list; use this value together with `propertyType = L` ||
|| **rowCount**
[`integer`](../../data-types.md) | Number of rows in the input field ||
|| **colCount**
[`integer`](../../data-types.md) | Number of columns in the input field ||
|| **listType**
[`char`](../../data-types.md) | Appearance of the list. Allowed values:
- `L` — dropdown list
- `C` — set of checkboxes ||
|| **multiple**
[`char`](../../data-types.md) | Indicator of multiple values. Allowed values:
- `Y` — yes
- `N` — no ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the property ||
|| **fileType**
[`string`](../../data-types.md) | List of file extensions for property type `F` ||
|| **multipleCnt**
[`integer`](../../data-types.md) | Number of fields for entering multiple values ||
|| **linkIblockId**
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the linked information block. 

Available identifiers can be obtained using the [catalog.catalog.list](../catalog/catalog-catalog-list.md) method ||
|| **withDescription**
[`char`](../../data-types.md) | Indicator of storing the description of the value. Allowed values:
- `Y` — yes
- `N` — no ||
|| **searchable**
[`char`](../../data-types.md) | Indicator of participation in search. Allowed values:
- `Y` — yes
- `N` — no ||
|| **filtrable**
[`char`](../../data-types.md) | Indicator of participation in filtering. Allowed values:
- `Y` — yes
- `N` — no ||
|| **isRequired**
[`char`](../../data-types.md) | Indicator of required value. Allowed values:
- `Y` — yes
- `N` — no ||
|| **hint**
[`string`](../../data-types.md) | Hint for the field ||
|| **userTypeSettings**
[`object`](../../data-types.md) | Settings for the custom type. Only scalar values and nested objects from scalar values are supported ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"fields":{"iblockId":19,"name":"Category","code":"CATEGORY","propertyType":"S","userType":"directory","multiple":"N","isRequired":"N","active":"Y","sort":100,"userTypeSettings":{"tableName":"b_hlbd_categories"}}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productProperty.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"fields":{"iblockId":19,"name":"Category","code":"CATEGORY","propertyType":"S","userType":"directory","multiple":"N","isRequired":"N","active":"Y","sort":100,"userTypeSettings":{"tableName":"b_hlbd_categories"}},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productProperty.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ProductPropertyResult = {
      productProperty: {
        active: string,
        code: string | null,
        colCount: number,
        defaultValue: string | null,
        fileType: string | null,
        filtrable: string,
        hint: string | null,
        iblockId: number,
        id: number,
        isRequired: string,
        linkIblockId: number | null,
        listType: string,
        multiple: string,
        multipleCnt: number | null,
        name: string,
        propertyType: string,
        rowCount: number,
        searchable: string,
        sort: number,
        timestampX: ISODate,
        userType: string | null,
        userTypeSettings: Record<string, unknown> | null,
        withDescription: string | null,
        xmlId: string | null,
      },
    }

    try {
      const response = await $b24.actions.v2.call.make<ProductPropertyResult>({
        method: 'catalog.productProperty.add',
        params: {
          fields: {
            iblockId: 19,
            name: 'Category',
            code: 'CATEGORY',
            propertyType: 'S',
            userType: 'directory',
            multiple: 'N',
            isRequired: 'N',
            active: 'Y',
            sort: 100,
            userTypeSettings: {
              tableName: 'b_hlbd_categories', // existing catalog in Bitrix24
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.productProperty.id, result.productProperty.name)
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
      async function addProductProperty() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'catalog.productProperty.add',
            params: {
              fields: {
                iblockId: 19,
                name: 'Category',
                code: 'CATEGORY',
                propertyType: 'S',
                userType: 'directory',
                multiple: 'N',
                isRequired: 'N',
                active: 'Y',
                sort: 100,
                userTypeSettings: {
                  tableName: 'b_hlbd_categories', // existing catalog in Bitrix24
                },
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
          console.info(result.productProperty.id, result.productProperty.name)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addProductProperty)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.catalog.product_property.add(
            fields={
                "iblockId": 19,
                "name": "Category",
                "propertyType": "S",
                "multiple": "N",
                "active": "Y",
                "sort": 100,
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
                'catalog.productProperty.add',
                [
                    'fields' => [
                        'iblockId' => 19,
                        'name' => 'Category',
                        'code' => 'CATEGORY',
                        'propertyType' => 'S',
                        'userType' => 'directory',
                        'multiple' => 'N',
                        'isRequired' => 'N',
                        'active' => 'Y',
                        'sort' => 100,
                        'userTypeSettings' => [
                            'tableName' => 'b_hlbd_categories', // existing directory in Bitrix24
                        ],
                    ],
                ]
            );

        print_r($response->getResponseData()->getResult());
    } catch (\Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.productProperty.add',
        {
            fields: {
                iblockId: 19,
                name: 'Category',
                code: 'CATEGORY',
                propertyType: 'S',
                userType: 'directory',
                multiple: 'N',
                isRequired: 'N',
                active: 'Y',
                sort: 100,
                userTypeSettings: {
                    tableName: 'b_hlbd_categories', // existing directory in Bitrix24
                }
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.productProperty.add',
        [
            'fields' => [
                'iblockId' => 19,
                'name' => 'Category',
                'code' => 'CATEGORY',
                'propertyType' => 'S',
                'userType' => 'directory',
                'multiple' => 'N',
                'isRequired' => 'N',
                'active' => 'Y',
                'sort' => 100,
                'userTypeSettings' => [
                    'tableName' => 'b_hlbd_categories', // existing directory in Bitrix24
                ],
            ]
        ]
    );

    print_r($result);
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "catalog.productProperty.add", b24.Params{
    	"fields": b24.Params{
    		"iblockId":     19,
    		"name":         "Category",
    		"code":         "CATEGORY",
    		"propertyType": "S",
    		"userType":     "directory",
    		"multiple":     "N",
    		"isRequired":   "N",
    		"active":       "Y",
    		"sort":         100,
    		"userTypeSettings": b24.Params{
    			"tableName": "b_hlbd_categories",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("catalog.productProperty.add: %w", err)
    }

    // The method wraps the response in an object with the "productProperty" key.
    raw, ok := b24.Unwrap(res.Result, "productProperty")
    if !ok {
    	return fmt.Errorf("no productProperty key in the response")
    }

    var item struct {
    	Active    string `json:"active"`
    	Code      string `json:"code"`
    	ColCount  int    `json:"colCount"`
    	Filtrable string `json:"filtrable"`
    	IblockID  b24.ID `json:"iblockId"`
    	ID        b24.ID `json:"id"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Active, item.Code)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "productProperty": {
            "active": "Y",
            "code": "CATEGORY",
            "colCount": 30,
            "defaultValue": null,
            "fileType": null,
            "filtrable": "N",
            "hint": null,
            "iblockId": 19,
            "id": 659,
            "isRequired": "N",
            "linkIblockId": null,
            "listType": "L",
            "multiple": "N",
            "multipleCnt": null,
            "name": "Category",
            "propertyType": "S",
            "rowCount": 1,
            "searchable": "N",
            "sort": 100,
            "timestampX": "2026-03-19T15:46:23+03:00",
            "userType": "directory",
            "userTypeSettings": {
                "group": "N",
                "multiple": "N",
                "size": 1,
                "tableName": "b_hlbd_categories",
                "width": 0
            },
            "withDescription": null,
            "xmlId": null
        }
    },
    "time": {
        "start": 1773927983,
        "finish": 1773927983.409049,
        "duration": 0.40904903411865234,
        "processing": 0,
        "date_start": "2026-03-19T16:46:23+03:00",
        "date_finish": "2026-03-19T16:46:23+03:00",
        "operating_reset_at": 1773928583,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root object of the response ||
|| **productProperty**
[`catalog_product_property`](../data-types.md#catalog_product_property) | Object with information about the added property ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Invalid property type specified"
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Required fields: iblockId, name, propertyType | Required fields not provided in `fields` ||
|| `400` | `0` | Access Denied | No permission to modify the information block ||
|| `400` | `0` | The specified iblock is not a product catalog | The provided `iblockId` is not a trade catalog ||
|| `400` | `0` | Invalid property type specified | An invalid combination of `propertyType` and `userType` was provided ||
|| `400` | `0` | Invalid custom property type settings specified | An invalid format for `userTypeSettings` was provided ||
|| `400` | `0` | Property code cannot start with a digit | Invalid format for the `code` parameter ||
|| `400` | `0` | Wrong format of field `...` | A parameter with a type that does not match the field format was provided ||
|| `400` | `0` | Error adding property | Internal error while creating the property ||
|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-update.md)
- [{#T}](./catalog-product-property-get.md)
- [{#T}](./catalog-product-property-list.md)
- [{#T}](../../../tutorials/catalog/index.md)
- [{#T}](./catalog-product-property-delete.md)
- [{#T}](./catalog-product-property-get-fields.md)
