# Add Product Property or Variation catalog.productProperty.add

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
[`string`](../../data-types.md) | Property name ||
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
[`integer`](../../data-types.md) | Sort index ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the property. The property code can consist of Latin letters, numbers, and underscores. The first character cannot be a digit ||
|| **defaultValue**
[`text`](../../data-types.md) | Default value of the property ||
|| **userType**
[`string`](../../data-types.md) | Custom type of the property. The value must correspond to the specified `propertyType`.

Examples of values:
- `DateTime` — date and time
- `Money` — monetary value with currency
- `SKU` — binding to product variations
- `directory` — binding to a directory
- `employee` — binding to an employee
- `UserID` — binding to a user
- `EList` — selection of an item from a list
- `EAutocomplete` — binding to elements with auto-search
- `SectionAuto` — binding to sections with auto-search
- `HTML` — value in HTML format
- `map_google` — coordinates and address on Google Maps
- `DiskFile` — binding to a file from Bitrix24.Drive
- `ECrm` — binding to CRM elements
- `BoolEnum` — checkbox based on a list, use this value with `propertyType = L` ||
|| **rowCount**
[`integer`](../../data-types.md) | Number of input field rows ||
|| **colCount**
[`integer`](../../data-types.md) | Number of input field columns ||
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
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the related information block. 

Available identifiers can be obtained using the method [catalog.catalog.list](../catalog/catalog-catalog-list.md) ||
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

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productProperty.add', {
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
        });

        console.log(response.getData().result);
    }
    catch (error) {
    	console.error('Error:', error);
    }
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
            "timestampX": "2026-03-19T15:46:23+02:00",
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
        "date_start": "2026-03-19T16:46:23+02:00",
        "date_finish": "2026-03-19T16:46:23+02:00",
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
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Invalid property type specified"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

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

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-update.md)
- [{#T}](./catalog-product-property-get.md)
- [{#T}](./catalog-product-property-list.md)
- [{#T}](./catalog-product-property-delete.md)
- [{#T}](./catalog-product-property-get-fields.md)