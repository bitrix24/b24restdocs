# How to Change Product Custom Field Values

> Scope: [`catalog`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods:
> - [catalog.product.list](../../api-reference/catalog/product/catalog-product-list.md), [catalog.product.get](../../api-reference/catalog/product/catalog-product-get.md), [catalog.productProperty.list](../../api-reference/catalog/product-property/catalog-product-property-list.md), [catalog.productPropertyEnum.list](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-list.md) — a user with catalog view permissions
> - [catalog.product.update](../../api-reference/catalog/product/catalog-product-update.md) — an administrator or a user with product edit permissions

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Product custom fields in the catalog are stored as Commercial catalog properties. Property values can be changed using the [catalog.product.update](../../api-reference/catalog/product/catalog-product-update.md) method.

The scenario consists of five steps:

1. Find a product using the [catalog.product.list](../../api-reference/catalog/product/catalog-product-list.md) method
2. Retrieve the product using the [catalog.product.get](../../api-reference/catalog/product/catalog-product-get.md) method
3. Find product properties using the [catalog.productProperty.list](../../api-reference/catalog/product-property/catalog-product-property-list.md) method
4. Retrieve list property values using the [catalog.productPropertyEnum.list](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-list.md) method
5. Update property values using the [catalog.product.update](../../api-reference/catalog/product/catalog-product-update.md) method

## Prepare the Data

To perform this example, you need:

- an incoming webhook with scope `catalog`
- the Commercial catalog identifier `iblockId`. This can be obtained using the [catalog.catalog.list](../../api-reference/catalog/catalog/catalog-catalog-list.md) method

In this example, two properties are being changed:

- a single-value list property `singlePropertyId`
- a multiple-value list property `multiplePropertyId`

You will obtain the property and list option identifiers in steps 3 and 4.

## 1. Find a Product

Call [catalog.product.list](../../api-reference/catalog/product/catalog-product-list.md) with a filter by `iblockId`. In `select`, pass `id`, `iblockId`, and `name` to select the product for update and display the name in diagnostics.

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    async function call(method, params) {
        const response = await $b24.actions.v2.call.make({ method, params })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    async function getProducts(iblockId) {
        const result = await call('catalog.product.list', {
            select: ['id', 'iblockId', 'name'],
            filter: {
                iblockId: iblockId,
                active: 'Y',
            },
            order: {
                id: 'ASC',
            },
            start: 0,
        })

        return result.products
    }
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;

    $webhookUrl = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/';
    $b24 = ServiceBuilderFactory::createServiceBuilderFromWebhook($webhookUrl);

    function callMethod($b24, string $method, array $params): array
    {
        return $b24->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    function getProducts($b24, int $iblockId): array
    {
        $result = callMethod($b24, 'catalog.product.list', [
            'select' => ['id', 'iblockId', 'name'],
            'filter' => [
                'iblockId' => $iblockId,
                'active' => 'Y',
            ],
            'order' => [
                'id' => 'ASC',
            ],
            'start' => 0,
        ]);

        return $result['products'];
    }
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import BitrixWebhook

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",
    )

    def call_method(method: str, params: dict):
        return token.call_method(method, params)["result"]

    def get_products(iblock_id: int):
        result = call_method("catalog.product.list", {
            "select": ["id", "iblockId", "name"],
            "filter": {
                "iblockId": iblock_id,
                "active": "Y",
            },
            "order": {
                "id": "ASC",
            },
            "start": 0,
        })

        return result["products"]
    ```

{% endlist %}

Shortened response:

```json
{
    "result": {
        "products": [
            {
                "id": 1243,
                "iblockId": 23,
                "name": "Monitor"
            }
        ]
    },
    "total": 1
}
```

Save `result.products[].id`. The product identifier will be required to retrieve current property values and to update the product.

The method returns products page by page. This example uses the first page, up to 50 products. If your catalog contains more products, iterate through the pages using the `start` parameter.

## 2. Retrieve the Product

Call [catalog.product.get](../../api-reference/catalog/product/catalog-product-get.md) and pass the product `id`.

{% list tabs %}

- JS

    ```js
    async function getProduct(productId) {
        const result = await call('catalog.product.get', {
            id: productId,
        })

        return result.product
    }
    ```

- PHP

    ```php
    function getProduct($b24, int $productId): array
    {
        $result = callMethod($b24, 'catalog.product.get', [
            'id' => $productId,
        ]);

        return $result['product'];
    }
    ```

- Python

    ```python
    def get_product(product_id: int):
        result = call_method("catalog.product.get", {
            "id": product_id,
        })

        return result["product"]
    ```

{% endlist %}

Shortened response:

```json
{
    "result": {
        "product": {
            "id": 1243,
            "iblockId": 23,
            "name": "Monitor",
            "property258": {
                "value": "433",
                "valueId": "9743"
            },
            "property259": [
                {
                    "value": "435",
                    "valueId": "9744"
                },
                {
                    "value": "437",
                    "valueId": "9745"
                }
            ]
        }
    }
}
```

Save `result.product.propertyN.valueId` for the single property and `result.product.propertyN[].valueId` for the multiple property. `valueId` is required in step 5 to update a populated property. If the property is currently empty and returns `null`, when filling it out for the first time, pass only `value`.

## 3. Find Product Properties

Call [catalog.productProperty.list](../../api-reference/catalog/product-property/catalog-product-property-list.md) using a filter for the `iblockId` from step 1. For list properties, the `propertyType` field is equal to `L`. The field `multiple` indicates whether the property is single or multiple.

Properties with `userType = BoolEnum` are updated with `Y` or `N` values rather than list option identifiers:

```json
{
    "fields": {
        "property295": {
            "value": "Y"
        }
    }
}
```

Next, in the example, select properties where `userType` is not equal to `BoolEnum` to change standard list properties via list option identifiers.

{% list tabs %}

- JS

    ```js
    async function getProductProperties(iblockId) {
        const result = await call('catalog.productProperty.list', {
            select: ['id', 'iblockId', 'name', 'propertyType', 'userType', 'multiple'],
            filter: {
                iblockId: iblockId,
                propertyType: 'L',
            },
            order: {
                id: 'ASC',
            },
            start: 0,
        })

        return result.productProperties.filter((property) => property.userType !== 'BoolEnum')
    }
    ```

- PHP

    ```php
    function getProductProperties($b24, int $iblockId): array
    {
        $result = callMethod($b24, 'catalog.productProperty.list', [
            'select' => ['id', 'iblockId', 'name', 'propertyType', 'userType', 'multiple'],
            'filter' => [
                'iblockId' => $iblockId,
                'propertyType' => 'L',
            ],
            'order' => [
                'id' => 'ASC',
            ],
            'start' => 0,
        ]);

        return array_values(array_filter(
            $result['productProperties'],
            static fn(array $property): bool => ($property['userType'] ?? null) !== 'BoolEnum'
        ));
    }
    ```

- Python

    ```python
    def get_product_properties(iblock_id: int):
        result = call_method("catalog.productProperty.list", {
            "select": ["id", "iblockId", "name", "propertyType", "userType", "multiple"],
            "filter": {
                "iblockId": iblock_id,
                "propertyType": "L",
            },
            "order": {
                "id": "ASC",
            },
            "start": 0,
        })

        return [
            property for property in result["productProperties"]
            if property.get("userType") != "BoolEnum"
        ]
    ```

{% endlist %}

Short response:

```json
{
    "result": {
        "productProperties": [
            {
                "id": 258,
                "iblockId": 23,
                "name": "Color",
                "propertyType": "L",
                "userType": null,
                "multiple": "N"
            },
            {
                "id": 259,
                "iblockId": 23,
                "name": "Package contents",
                "propertyType": "L",
                "userType": null,
                "multiple": "Y"
            }
        ]
    },
    "total": 2
}
```

Retain `result.productProperties[].id` for properties where `userType` is not equal to `BoolEnum`. The property ID is required for the field name `propertyN` in the [catalog.product.update](../../api-reference/catalog/product/catalog-product-update.md) method, for example `property258`.

## 4. Retrieve List Property Values

Call [catalog.productPropertyEnum.list](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-list.md) for each property.

{% list tabs %}

- JS

    ```js
    async function getPropertyEnumValues(propertyId) {
        const result = await call('catalog.productPropertyEnum.list', {
            select: ['id', 'propertyId', 'value', 'sort'],
            filter: {
                propertyId: propertyId,
            },
            order: {
                id: 'ASC',
            },
            start: 0,
        })

        return result.productPropertyEnums
    }
    ```

- PHP

    ```php
    function getPropertyEnumValues($b24, int $propertyId): array
    {
        $result = callMethod($b24, 'catalog.productPropertyEnum.list', [
            'select' => ['id', 'propertyId', 'value', 'sort'],
            'filter' => [
                'propertyId' => $propertyId,
            ],
            'order' => [
                'id' => 'ASC',
            ],
            'start' => 0,
        ]);

        return $result['productPropertyEnums'];
    }
    ```

- Python

    ```python
    def get_property_enum_values(property_id: int):
        result = call_method("catalog.productPropertyEnum.list", {
            "select": ["id", "propertyId", "value", "sort"],
            "filter": {
                "propertyId": property_id,
            },
            "order": {
                "id": "ASC",
            },
            "start": 0,
        })

        return result["productPropertyEnums"]
    ```

{% endlist %}

Short response:

```json
{
    "result": {
        "productPropertyEnums": [
            {
                "id": 433,
                "propertyId": 258,
                "value": "Red",
                "sort": 500
            },
            {
                "id": 435,
                "propertyId": 258,
                "value": "Blue",
                "sort": 500
            }
        ]
    },
    "total": 2
}
```

Retain `result.productPropertyEnums[].id` for the required list options. These identifiers must be passed in `value` when updating a list property.

## 5. Update Property Values

Call [catalog.product.update](../../api-reference/catalog/product/catalog-product-update.md). Pass the properties in `fields` in `propertyN` format, where `N` is the property identifier.

For a single property, pass an object:

```json
{
    "valueId": "9743",
    "value": 435
}
```

For a multiple property, pass an array of objects. If you need to retain an existing value string, pass it `valueId`. Multiple property values for which no  has been passed `valueId`, will be replaced.

{% list tabs %}

- JS

    ```js
    function buildPropertyUpdateFields(product, singlePropertyId, singleEnumId, multiplePropertyId, multipleEnumIds) {
        const singlePropertyName = `property${singlePropertyId}`
        const multiplePropertyName = `property${multiplePropertyId}`

        const fields = {
            [singlePropertyName]: {
                valueId: product[singlePropertyName]?.valueId,
                value: singleEnumId,
            },
            [multiplePropertyName]: multipleEnumIds.map((enumId, index) => ({
                valueId: product[multiplePropertyName]?.[index]?.valueId,
                value: enumId,
            })),
        }

        return fields
    }

    async function updateProductProperties(productId, fields) {
        const result = await call('catalog.product.update', {
            id: productId,
            fields: fields,
        })

        return result.element
    }
    ```

- PHP

    ```php
    function buildPropertyUpdateFields(
        array $product,
        int $singlePropertyId,
        int $singleEnumId,
        int $multiplePropertyId,
        array $multipleEnumIds
    ): array {
        $singlePropertyName = 'property' . $singlePropertyId;
        $multiplePropertyName = 'property' . $multiplePropertyId;

        $multipleValues = [];
        foreach ($multipleEnumIds as $index => $enumId) {
            $multipleValues[] = [
                'valueId' => $product[$multiplePropertyName][$index]['valueId'] ?? null,
                'value' => $enumId,
            ];
        }

        return [
            $singlePropertyName => [
                'valueId' => $product[$singlePropertyName]['valueId'] ?? null,
                'value' => $singleEnumId,
            ],
            $multiplePropertyName => $multipleValues,
        ];
    }

    function updateProductProperties($b24, int $productId, array $fields): array
    {
        $result = callMethod($b24, 'catalog.product.update', [
            'id' => $productId,
            'fields' => $fields,
        ]);

        return $result['element'];
    }
    ```

- Python

    ```python
    def build_property_update_fields(
        product: dict,
        single_property_id: int,
        single_enum_id: int,
        multiple_property_id: int,
        multiple_enum_ids: list,
    ):
        single_property_name = f"property{single_property_id}"
        multiple_property_name = f"property{multiple_property_id}"

        current_multiple_values = product.get(multiple_property_name) or []
        multiple_values = []
        for index, enum_id in enumerate(multiple_enum_ids):
            current_value = current_multiple_values[index] if index < len(current_multiple_values) else {}
            multiple_values.append({
                "valueId": current_value.get("valueId"),
                "value": enum_id,
            })

        return {
            single_property_name: {
                "valueId": (product.get(single_property_name) or {}).get("valueId"),
                "value": single_enum_id,
            },
            multiple_property_name: multiple_values,
        }

    def update_product_properties(product_id: int, fields: dict):
        result = call_method("catalog.product.update", {
            "id": product_id,
            "fields": fields,
        })

        return result["element"]
    ```

{% endlist %}

Short response:

```json
{
    "result": {
        "element": {
            "id": 1243,
            "iblockId": 23,
            "name": "Monitor",
            "property258": {
                "value": "435",
                "valueId": "9743"
            },
            "property259": [
                {
                    "value": "439",
                    "valueId": "9744"
                },
                {
                    "value": "441",
                    "valueId": "9745"
                }
            ]
        }
    }
}
```

Check the `result.element.propertyN` fields in the response. The value `value` must match the list option identifier that you passed in the request. Retain the new `valueId` from the response: it may change after the update and will be required for the next change to this property.

## Launch the Scenario

After adding the functions from the previous steps, replace:

- `iblockId` — the Commercial catalog identifier
- `singlePropertyId` — the single list property identifier
- `multiplePropertyId` — the multiple list property identifier
- `singleEnumId` — the new single property value
- `multipleEnumIds` — the new multiple property values

{% list tabs %}

- JS

    ```js
    const iblockId = 23
    const singlePropertyId = 258
    const multiplePropertyId = 259
    const singleEnumId = 435
    const multipleEnumIds = [439, 441]

    const products = await getProducts(iblockId)
    if (products.length === 0) {
        throw new Error('There are no active products in the catalog')
    }

    const productId = products[0].id
    const product = await getProduct(productId)
    const properties = await getProductProperties(iblockId)
    const singleEnumValues = await getPropertyEnumValues(singlePropertyId)
    const multipleEnumValues = await getPropertyEnumValues(multiplePropertyId)

    console.log('Product properties:', properties)
    console.log('Single property values:', singleEnumValues)
    console.log('Multiple property values:', multipleEnumValues)

    const fields = buildPropertyUpdateFields(
        product,
        singlePropertyId,
        singleEnumId,
        multiplePropertyId,
        multipleEnumIds,
    )

    const updatedProduct = await updateProductProperties(productId, fields)
    console.log(updatedProduct)
    ```

- PHP

    ```php
    $iblockId = 23;
    $singlePropertyId = 258;
    $multiplePropertyId = 259;
    $singleEnumId = 435;
    $multipleEnumIds = [439, 441];

    $products = getProducts($b24, $iblockId);
    if (empty($products)) {
        throw new RuntimeException('There are no active products in the catalog');
    }

    $productId = (int)$products[0]['id'];
    $product = getProduct($b24, $productId);
    $properties = getProductProperties($b24, $iblockId);
    $singleEnumValues = getPropertyEnumValues($b24, $singlePropertyId);
    $multipleEnumValues = getPropertyEnumValues($b24, $multiplePropertyId);

    print_r($properties);
    print_r($singleEnumValues);
    print_r($multipleEnumValues);

    $fields = buildPropertyUpdateFields(
        $product,
        $singlePropertyId,
        $singleEnumId,
        $multiplePropertyId,
        $multipleEnumIds
    );

    $updatedProduct = updateProductProperties($b24, $productId, $fields);
    print_r($updatedProduct);
    ```

- Python

    ```python
    iblock_id = 23
    single_property_id = 258
    multiple_property_id = 259
    single_enum_id = 435
    multiple_enum_ids = [439, 441]

    products = get_products(iblock_id)
    if not products:
        raise RuntimeError("There are no active products in the catalog")

    product_id = int(products[0]["id"])
    product = get_product(product_id)
    properties = get_product_properties(iblock_id)
    single_enum_values = get_property_enum_values(single_property_id)
    multiple_enum_values = get_property_enum_values(multiple_property_id)

    print("Product properties:", properties)
    print("Single property values:", single_enum_values)
    print("Multiple property values:", multiple_enum_values)

    fields = build_property_update_fields(
        product,
        single_property_id,
        single_enum_id,
        multiple_property_id,
        multiple_enum_ids,
    )

    updated_product = update_product_properties(product_id, fields)
    print(updated_product)
    ```

{% endlist %}

## Verify the Result

Open the product card in the catalog and check the property values. They should match the values passed in `singleEnumId` and `multipleEnumIds`.

You can verify the result via REST using the [catalog.product.get](../../api-reference/catalog/product/catalog-product-get.md) method. Pass the product `id` and check the `propertyN` fields in the response.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `200040300010` | Insufficient permissions to read the trade catalog. Check user permissions and scope `catalog` ||
|| `200040300040` | Insufficient permissions to read or change the product. Check user permissions for the catalog ||
|| `100` | A required parameter was not passed. Check `id`, `fields`, `filter`, and `select` ||
|| `0` | Catalog, product, property, or list value not found. Check `iblockId`, `productId`, `singlePropertyId`, `multiplePropertyId`, and value identifiers ||
|#

## Key Considerations

- `crm.product.*` methods are deprecated. For Commercial catalog products, use [catalog.product.*](../../api-reference/catalog/product/index.md) methods
- To change a product price, use [catalog.price.*](../../api-reference/catalog/price/index.md) methods
- To change product images, use [catalog.productImage.*](../../api-reference/catalog/product-image/index.md) methods or the `previewPicture` and `detailPicture` fields of the [catalog.product.update](../../api-reference/catalog/product/catalog-product-update.md) method
- If a property value is passed without `valueId`, the method will write a new value. After updating, save the new `valueId` from the response
- If not all current values of a multiple property are passed, existing values without `valueId` will be deleted
- List property values must be passed using identifiers from [catalog.productPropertyEnum.list](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-list.md)

## Continue Learning

- [Get a product by identifier catalog.product.get](../../api-reference/catalog/product/catalog-product-get.md)
- [Get a list of products by filter catalog.product.list](../../api-reference/catalog/product/catalog-product-list.md)
- [Update a product catalog.product.update](../../api-reference/catalog/product/catalog-product-update.md)
- [Get a list of product or variation properties catalog.productProperty.list](../../api-reference/catalog/product-property/catalog-product-property-list.md)
- [Get a list of list property values catalog.productPropertyEnum.list](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-list.md)
