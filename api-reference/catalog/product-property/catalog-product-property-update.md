# Update Product or Variation Property catalog.productProperty.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: user with permission to view the catalog

The method `catalog.productProperty.update` modifies the fields of a product or variation property.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_product_property.id`](../data-types.md#catalog_product_property) | Property identifier. 

Property identifiers can be obtained using the [catalog.productProperty.list](./catalog-product-property-list.md) method. ||
|| **fields***
[`object`](../../data-types.md) | Set of fields to update the property [(detailed description)](#fields) ||
|#

### Fields Parameter {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **iblockId***
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the trade catalog. 

Identifiers can be obtained using the [catalog.catalog.list](../catalog/catalog-catalog-list.md) method. ||
|| **name**
[`string`](../../data-types.md) | Property name ||
|| **propertyType**
[`string`](../../data-types.md) | Base type of the property. Cannot be changed ||
|| **active**
[`char`](../../data-types.md) | Activity status. Allowed values:
- `Y` — yes
- `N` — no ||
|| **sort**
[`integer`](../../data-types.md) | Sort index ||
|| **code**
[`string`](../../data-types.md) | Symbolic code of the property. The property code can consist of Latin letters, numbers, and underscores. The first character cannot be a digit ||
|| **defaultValue**
[`text`](../../data-types.md) | Default property value ||
|| **userType**
[`string`](../../data-types.md) | User-defined property type. Cannot be changed ||
|| **rowCount**
[`integer`](../../data-types.md) | Number of input field rows ||
|| **colCount**
[`integer`](../../data-types.md) | Number of input field columns ||
|| **listType**
[`char`](../../data-types.md) | Appearance of the list. Allowed values:
- `L` — dropdown list
- `C` — set of checkboxes ||
|| **multiple**
[`char`](../../data-types.md) | Multiple value indicator. Allowed values:
- `Y` — yes
- `N` — no ||
|| **xmlId**
[`string`](../../data-types.md) | External identifier of the property ||
|| **fileType**
[`string`](../../data-types.md) | List of file extensions for property type `F` ||
|| **multipleCnt**
[`integer`](../../data-types.md) | Number of fields for inputting multiple values ||
|| **linkIblockId**
[`catalog_catalog.id`](../data-types.md#catalog_catalog) | Identifier of the related information block. 

Available identifiers can be obtained using the [catalog.catalog.list](../catalog/catalog-catalog-list.md) method. ||
|| **withDescription**
[`char`](../../data-types.md) | Indicator of storing the value description. Allowed values:
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
[`string`](../../data-types.md) | Field hint ||
|| **userTypeSettings**
[`object`](../../data-types.md) | Settings for the user-defined type. Only scalar values and nested objects of scalar values are supported.

If `userType` is specified but `userTypeSettings` is not, the settings are not validated by the method. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":115,"fields":{"iblockId":19,"name":"Size","propertyType":"L","isRequired":"Y","active":"Y"}}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.productProperty.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"id":115,"fields":{"iblockId":19,"name":"Size","propertyType":"L","isRequired":"Y","active":"Y"},"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/catalog.productProperty.update
    ```

- JS

    ```js
    try {
        const response = await $b24.callMethod('catalog.productProperty.update', {
            id: 115,
            fields: {
                iblockId: 19,
                name: 'Size',
                propertyType: 'L',
                isRequired: 'Y',
                active: 'Y'
            }
        });

        console.log(response.getData().result);
    }
    catch (error) {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.productProperty.update',
                [
                    'id' => 115,
                    'fields' => [
                        'iblockId' => 19,
                        'name' => 'Size',
                        'propertyType' => 'L',
                        'isRequired' => 'Y',
                        'active' => 'Y',
                    ]
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
        'catalog.productProperty.update',
        {
            id: 115,
            fields: {
                iblockId: 19,
                name: 'Size',
                propertyType: 'L',
                isRequired: 'Y',
                active: 'Y'
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
        'catalog.productProperty.update',
        [
            'id' => 115,
            'fields' => [
                'iblockId' => 19,
                'name' => 'Size',
                'propertyType' => 'L',
                'isRequired' => 'Y',
                'active' => 'Y',
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
            "code": null,
            "colCount": 30,
            "defaultValue": null,
            "fileType": null,
            "filtrable": "Y",
            "hint": null,
            "iblockId": 19,
            "id": 115,
            "name": "Size",
            "isRequired": "Y",
            "linkIblockId": null,
            "listType": "L",
            "multiple": "N",
            "multipleCnt": null,
            "propertyType": "L",
            "rowCount": 1,
            "searchable": "N",
            "sort": 500,
            "timestampX": "2026-03-19T20:46:43+02:00",
            "userType": null,
            "userTypeSettings": null,
            "withDescription": null,
            "xmlId": null
        }
    },
    "time": {
        "start": 1773946003,
        "finish": 1773946003.953583,
        "duration": 0.9535830020904541,
        "processing": 0,
        "date_start": "2026-03-19T21:46:43+02:00",
        "date_finish": "2026-03-19T21:46:43+02:00",
        "operating_reset_at": 1773946603,
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
[`catalog_product_property`](../data-types.md#catalog_product_property) | Object with information about the updated property ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "0",
    "error_description": "Required fields: iblockId"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `0` | Required fields: iblockId | The required parameter `iblockId` is not passed in `fields` ||
|| `400` | `0` | Access Denied | Insufficient rights to view the catalog ||
|| `400` | Empty value | productProperty does not exist | Property with the specified `id` not found ||
|| `400` | `0` | The specified property does not belong to a product catalog | The property does not belong to the trade catalog ||
|| `400` | `0` | Invalid property type specified | An invalid combination of `propertyType` and `userType` was provided ||
|| `400` | `0` | Invalid custom property type settings specified | An invalid format of `userTypeSettings` was provided ||
|| `400` | `0` | Property code cannot start with a digit | Invalid format of the `code` parameter ||
|| `400` | `0` | Invalid information block code | Invalid format of the `iblockId` parameter ||
|| `400` | `100` | Invalid value {`...`} to match with parameter {id}. Should be value of type int | Invalid data type for the `id` parameter value ||
|| `400` | `0` | Wrong format of field `...` | A parameter with a type that does not match the field format was provided ||
|| `400` | `0` | Error updating product property | Internal error while updating the property ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-product-property-add.md)
- [{#T}](./catalog-product-property-get.md)
- [{#T}](./catalog-product-property-list.md)
- [{#T}](./catalog-product-property-delete.md)
- [{#T}](./catalog-product-property-get-fields.md)