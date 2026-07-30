# How to Create a CRM Object with Products, Discounts, and Taxes

> Scope: [`crm`](../../../api-reference/scopes/permissions.md), [`catalog`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods:
> - [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md) — a user with permission to view the product catalog and permission to read the commercial catalog information block
> - [catalog.price.list](../../../api-reference/catalog/price/catalog-price-list.md) — a user with permission to view the product catalog or permission to change prices
> - [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) — a user with permission to add an object of the selected type
> - [crm.item.productrow.set](../../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md) — a user with permission to modify a created CRM object
> - [crm.item.productrow.list](../../../api-reference/crm/universal/product-rows/crm-item-productrow-list.md) — a user with permission to read a created CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Product rows can be linked to a lead, deal, invoice, or estimate. In this example, we create a CRM object, find a product in the catalog, retrieve its price, and save several product rows with different tax and discount options.

The scenario consists of four steps.

1. Find a product using the [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md) method
2. Retrieve the product price using the [catalog.price.list](../../../api-reference/catalog/price/catalog-price-list.md) method
3. Create a CRM object using the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method
4. Save the product rows using the [crm.item.productrow.set](../../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md) method

## Prepare the Data

To run this example, you need:

- an incoming webhook with scopes `crm` and `catalog`
- the commercial catalog identifier `iblockId`. This can be retrieved using the [catalog.catalog.list](../../../api-reference/catalog/catalog/catalog-catalog-list.md) method
- the CRM object type to which the products should be linked

#|
|| **CRM Object** | **entityTypeId for crm.item.add** | **ownerType for crm.item.productrow.set** ||
|| Lead | `1` | `L` ||
|| Deal | `2` | `D` ||
|| Invoice | `31` | `SI` ||
|| Estimate | `7` | `Q` ||
|#

{% note info "" %}

For new integrations, create invoices as "Invoice (new)" with `entityTypeId = 31` and `ownerType = SI`. The old invoice type `INVOICE` is kept for compatibility and is not recommended for new scenarios.

{% endnote %}

Check which mandatory fields are configured for the selected object type in your Bitrix24. All mandatory fields must be passed in the `fields` of the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method.

For server-side JS examples with `B24Hook`, Node.js 20 or 22 and higher is required. B24JsSDK is an ES module: save the code in a file `.mjs` or add `"type": "module"` to `package.json`.

For examples using b24pysdk, Python 3.9 or newer is required.

## 1. Find a Product in the Catalog

Call [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md) with a filter by `iblockId`. In `select`, pass the mandatory fields `id` and `iblockId`, as well as `name`, to use the product name for diagnostics.

{% include [Note on examples](../../../_includes/examples.md) %}

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

The method returns products paginated. This example uses the first page, up to 50 products. If your catalog contains more products, iterate through the pages using the `start` parameter.

Abbreviated response:

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

Save `result.products[].id`. The product ID will be needed to obtain the price and for the `productId` parameter of the product row.

## 2. Retrieve the Product Price

The product price is stored separately from the product card. For each found product, call [catalog.price.list](../../../api-reference/catalog/price/catalog-price-list.md) with a filter by `productId` and select the first price greater than zero.

{% list tabs %}

- JS

    ```js
    async function getFirstPrice(productId) {
        const result = await call('catalog.price.list', {
            select: ['id', 'productId', 'price', 'currency'],
            filter: {
                productId: productId,
                '>price': 0,
            },
            order: {
                id: 'ASC',
            },
            start: 0,
        })

        return result.prices[0] ?? null
    }

    async function findProductWithPrice(iblockId) {
        const products = await getProducts(iblockId)

        for (const product of products) {
            const price = await getFirstPrice(product.id)

            if (price) {
                return { product, price }
            }
        }

        throw new Error('There is no active product in the catalog with a price greater than zero')
    }
    ```

- PHP

    ```php
    function getFirstPrice($b24, int $productId): ?array
    {
        $result = callMethod($b24, 'catalog.price.list', [
            'select' => ['id', 'productId', 'price', 'currency'],
            'filter' => [
                'productId' => $productId,
                '>price' => 0,
            ],
            'order' => [
                'id' => 'ASC',
            ],
            'start' => 0,
        ]);

        return $result['prices'][0] ?? null;
    }

    function findProductWithPrice($b24, int $iblockId): array
    {
        foreach (getProducts($b24, $iblockId) as $product) {
            $price = getFirstPrice($b24, (int)$product['id']);

            if ($price !== null) {
                return [
                    'product' => $product,
                    'price' => $price,
                ];
            }
        }

        throw new RuntimeException('There is no active product in the catalog with a price greater than zero');
    }
    ```

- Python

    ```python
    def get_first_price(product_id: int):
        result = call_method("catalog.price.list", {
            "select": ["id", "productId", "price", "currency"],
            "filter": {
                "productId": product_id,
                ">price": 0,
            },
            "order": {
                "id": "ASC",
            },
            "start": 0,
        })

        return result["prices"][0] if result["prices"] else None

    def find_product_with_price(iblock_id: int):
        for product in get_products(iblock_id):
            price = get_first_price(int(product["id"]))

            if price:
                return {
                    "product": product,
                    "price": price,
                }

        raise RuntimeError("There is no active product in the catalog with a price greater than zero")
    ```

{% endlist %}

Short response:

```json
{
    "result": {
        "prices": [
            {
                "id": 381,
                "productId": 1243,
                "price": 1000,
                "currency": "EUR"
            }
        ]
    },
    "total": 1
}
```

Retain `result.prices[].price` and `result.prices[].currency`. The price will be needed to calculate line items, the currency — for the field `currencyId` of the created CRM object.

## 3. Create a CRM Object

Call [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md). Pass:

- `entityTypeId` — the numeric identifier of the CRM object type
- `fields.title` — the object name
- `fields.currencyId` — the price currency from step 2

{% list tabs %}

- JS

    ```js
    async function createCrmItem(entityTypeId, title, currency) {
        const result = await call('crm.item.add', {
            entityTypeId: entityTypeId,
            fields: {
                title: title,
                currencyId: currency,
            },
        })

        return result.item.id
    }
    ```

- PHP

    ```php
    function createCrmItem($b24, int $entityTypeId, string $title, string $currency): int
    {
        $result = callMethod($b24, 'crm.item.add', [
            'entityTypeId' => $entityTypeId,
            'fields' => [
                'title' => $title,
                'currencyId' => $currency,
            ],
        ]);

        return (int)$result['item']['id'];
    }
    ```

- Python

    ```python
    def create_crm_item(entity_type_id: int, title: str, currency: str) -> int:
        result = call_method("crm.item.add", {
            "entityTypeId": entity_type_id,
            "fields": {
                "title": title,
                "currencyId": currency,
            },
        })

        return int(result["item"]["id"])
    ```

{% endlist %}

Short response:

```json
{
    "result": {
        "item": {
            "id": 342,
            "title": "Deal with products"
        }
    }
}
```

Retain `result.item.id`. The ID will be needed for the parameter `ownerId` of the [crm.item.productrow.set](../../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md) method.

## 4. Save Product Line Items

Call [crm.item.productrow.set](../../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md). Pass:

- `ownerType` — the short symbolic code of the CRM object type
- `ownerId` — the identifier of the object from step 3
- `productRows` — an array of product line items

The example saves four variations:

- a product with 20% tax, tax not included in the price
- a product with 20% tax, tax included in the price
- a product with a fixed discount in the price currency
- a product with a 10% discount

For the fixed discount, the example takes the smaller value: 100 currency units or half of the product price. This ensures the final price of the product line item does not become negative.

{% note info "" %}

The [crm.item.productrow.set](../../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md) method overwrites all product line items of the CRM object. Items that are not passed in `productRows` will be removed from the object.

{% endnote %}

{% list tabs %}

- JS

    ```js
    function buildProductRows(productId, basePrice) {
        const price = Number(basePrice)
        const fixedDiscount = Math.min(100, price / 2)

        return [
            {
                productId: productId,
                price: price,
                taxRate: 20,
                taxIncluded: 'N',
                quantity: 1,
                sort: 10,
            },
            {
                productId: productId,
                price: price * 1.2,
                taxRate: 20,
                taxIncluded: 'Y',
                quantity: 1,
                sort: 20,
            },
            {
                productId: productId,
                price: price - fixedDiscount,
                discountTypeId: 1,
                discountSum: fixedDiscount,
                quantity: 1,
                sort: 30,
            },
            {
                productId: productId,
                price: price * 0.9,
                discountTypeId: 2,
                discountRate: 10,
                quantity: 1,
                sort: 40,
            },
        ]
    }

    async function setProductRows(ownerType, ownerId, productRows) {
        const result = await call('crm.item.productrow.set', {
            ownerType: ownerType,
            ownerId: ownerId,
            productRows: productRows,
        })

        return result.productRows
    }
    ```

- PHP

    ```php
    function buildProductRows(int $productId, float $basePrice): array
    {
        $fixedDiscount = min(100, $basePrice / 2);

        return [
            [
                'productId' => $productId,
                'price' => $basePrice,
                'taxRate' => 20,
                'taxIncluded' => 'N',
                'quantity' => 1,
                'sort' => 10,
            ],
            [
                'productId' => $productId,
                'price' => $basePrice * 1.2,
                'taxRate' => 20,
                'taxIncluded' => 'Y',
                'quantity' => 1,
                'sort' => 20,
            ],
            [
                'productId' => $productId,
                'price' => $basePrice - $fixedDiscount,
                'discountTypeId' => 1,
                'discountSum' => $fixedDiscount,
                'quantity' => 1,
                'sort' => 30,
            ],
            [
                'productId' => $productId,
                'price' => $basePrice * 0.9,
                'discountTypeId' => 2,
                'discountRate' => 10,
                'quantity' => 1,
                'sort' => 40,
            ],
        ];
    }

    function setProductRows($b24, string $ownerType, int $ownerId, array $productRows): array
    {
        $result = callMethod($b24, 'crm.item.productrow.set', [
            'ownerType' => $ownerType,
            'ownerId' => $ownerId,
            'productRows' => $productRows,
        ]);

        return $result['productRows'];
    }
    ```

- Python

    ```python
    def build_product_rows(product_id: int, base_price: float):
        fixed_discount = min(100, base_price / 2)

        return [
            {
                "productId": product_id,
                "price": base_price,
                "taxRate": 20,
                "taxIncluded": "N",
                "quantity": 1,
                "sort": 10,
            },
            {
                "productId": product_id,
                "price": base_price * 1.2,
                "taxRate": 20,
                "taxIncluded": "Y",
                "quantity": 1,
                "sort": 20,
            },
            {
                "productId": product_id,
                "price": base_price - fixed_discount,
                "discountTypeId": 1,
                "discountSum": fixed_discount,
                "quantity": 1,
                "sort": 30,
            },
            {
                "productId": product_id,
                "price": base_price * 0.9,
                "discountTypeId": 2,
                "discountRate": 10,
                "quantity": 1,
                "sort": 40,
            },
        ]

    def set_product_rows(owner_type: str, owner_id: int, product_rows: list):
        result = call_method("crm.item.productrow.set", {
            "ownerType": owner_type,
            "ownerId": owner_id,
            "productRows": product_rows,
        })

        return result["productRows"]
    ```

{% endlist %}

Short response:

```json
{
    "result": {
        "productRows": [
            {
                "id": 17654,
                "ownerId": 342,
                "ownerType": "D",
                "productId": 1243,
                "price": 1000,
                "quantity": 1,
                "taxRate": 20,
                "taxIncluded": "N"
            }
        ]
    }
}
```

## Launch the Scenario

After adding the functions from the previous steps, select the required object type in the `crmEntity` settings. For a lead, specify `entityTypeId = 1` and `ownerType = L`, for a deal — `2` and `D`, for an invoice — `31` and `SI`, for an estimate — `7` and `Q`.

{% list tabs %}

- JS

    ```js
    const crmEntity = {
        entityTypeId: 2,
        ownerType: 'D',
        title: 'Deal with products',
    }

    const iblockId = 23

    const { product, price } = await findProductWithPrice(iblockId)
    const itemId = await createCrmItem(
        crmEntity.entityTypeId,
        crmEntity.title,
        price.currency,
    )
    const productRows = buildProductRows(product.id, price.price)
    const savedRows = await setProductRows(crmEntity.ownerType, itemId, productRows)

    console.log(`CRM object created #${itemId}`)
    console.log(`Product: ${product.name}`)
    console.log(savedRows)
    ```

- PHP

    ```php
    $crmEntity = [
        'entityTypeId' => 2,
        'ownerType' => 'D',
        'title' => 'Deal with products',
    ];

    $iblockId = 23;

    $productWithPrice = findProductWithPrice($b24, $iblockId);
    $product = $productWithPrice['product'];
    $price = $productWithPrice['price'];

    $itemId = createCrmItem(
        $b24,
        $crmEntity['entityTypeId'],
        $crmEntity['title'],
        $price['currency']
    );

    $productRows = buildProductRows((int)$product['id'], (float)$price['price']);
    $savedRows = setProductRows($b24, $crmEntity['ownerType'], $itemId, $productRows);

    print('CRM object created #' . $itemId . PHP_EOL);
    print('Product: ' . $product['name'] . PHP_EOL);
    print_r($savedRows);
    ```

- Python

    ```python
    crm_entity = {
        "entityTypeId": 2,
        "ownerType": "D",
        "title": "Deal with products",
    }

    iblock_id = 23

    product_with_price = find_product_with_price(iblock_id)
    product = product_with_price["product"]
    price = product_with_price["price"]

    item_id = create_crm_item(
        crm_entity["entityTypeId"],
        crm_entity["title"],
        price["currency"],
    )

    product_rows = build_product_rows(int(product["id"]), float(price["price"]))
    saved_rows = set_product_rows(crm_entity["ownerType"], item_id, product_rows)

    print("CRM object created #%s" % item_id)
    print("Product: %s" % product["name"])
    print(saved_rows)
    ```

{% endlist %}

## Verify the Result

Open the created CRM object in the interface and check the products tab. Four product line items with the same product but different calculations should appear in the list:

- tax not included in the price
- tax included in the price
- fixed discount
- percentage discount

You can verify the result via REST using the [crm.item.productrow.list](../../../api-reference/crm/universal/product-rows/crm-item-productrow-list.md) method. Pass the following filter:

- `=ownerType` — the short symbolic code of the CRM object type
- `=ownerId` — the identifier of the created CRM object

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `200040300010` | Insufficient permissions to read the catalog or prices. Check user permissions and scope `catalog` ||
|| `ACCESS_DENIED` | No permission to create or modify the CRM object. Check user permissions in CRM ||
|| `OWNER_NOT_FOUND` | An identifier for a non-existent CRM object was passed in `ownerId` ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | An unsupported CRM object type was passed in `ownerType` ||
|| `100` | Mandatory parameters were not passed. Check `entityTypeId`, `fields`, `ownerType`, `ownerId`, and `productRows` ||
|#

## Key Considerations

- [crm.item.productrow.set](../../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md) replaces all product rows of the CRM object
- [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md) returns products but does not return prices. Prices must be retrieved using the [catalog.price.list](../../../api-reference/catalog/price/catalog-price-list.md) method
- For products with variations, use the identifier of the specific product variation
- Rerunning the example creates a new CRM object and new product rows
- If the object total should be calculated based on product rows, do not pass a manual amount in `opportunity`

## Continue Learning

- [Get a list of products by filter catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md)
- [Get a list of prices by filter catalog.price.list](../../../api-reference/catalog/price/catalog-price-list.md)
- [Create a new CRM item crm.item.add](../../../api-reference/crm/universal/crm-item-add.md)
- [Save a CRM object product row crm.item.productrow.set](../../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md)
- [Get a list of product rows crm.item.productrow.list](../../../api-reference/crm/universal/product-rows/crm-item-productrow-list.md)
