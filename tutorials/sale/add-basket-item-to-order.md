# How to Add a Line Item to an Order with an Arbitrary Price

> Scope: [`sale`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — administrator
>
> - [sale.basketitem.add](../../api-reference/sale/basket-item/sale-basket-item-add.md) and [sale.order.get](../../api-reference/sale/order/sale-order-get.md) — administrator
> - [sale.basketitem.list](../../api-reference/sale/basket-item/sale-basket-item-list.md) — store manager

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A basket line item is a row of an order: a product or service with a quantity, price, and currency. The method [sale.basketitem.add](../../api-reference/sale/basket-item/sale-basket-item-add.md) adds such a row to an order that already exists.

Let us go through two cases where the price is set by the integration rather than by the catalog:

- the product is in the catalog, but it has to be sold at a different price — with a discount or a markup
- the line item is not in the catalog at all: a one-time service, contract work, delivery

Both cases are covered by the same method, only the set of fields differs. If the price must not be changed and has to come from the catalog, use [sale.basketitem.addCatalogProduct](../../api-reference/sale/basket-item/sale-basket-item-add-catalog-product.md) — it substitutes the price and the product attributes on its own.

The scenario consists of two steps, and they are independent. Complete the one that fits your task, or both in a row, as in the examples below.

1. Add a product from the catalog and assign your own price using the method [sale.basketitem.add](../../api-reference/sale/basket-item/sale-basket-item-add.md)
2. Add a line item that is not in the catalog using the same method with a different set of fields

The result of both steps is verified with the methods [sale.basketitem.list](../../api-reference/sale/basket-item/sale-basket-item-list.md) and [sale.order.get](../../api-reference/sale/order/sale-order-get.md).

## Prepare the Data

Collect the values before the first call.

- `orderId` — identifier of the order the line item is added to. The order must already exist: the method only adds a row to it. The list of orders is returned by [sale.order.list](../../api-reference/sale/order/sale-order-list.md), and a new order can be created with the method [sale.order.add](../../api-reference/sale/order/sale-order-add.md). The examples use order `891`
- `currency` — currency of the line item. It must match the currency of the order, otherwise the method returns an error. The examples use the currency `EUR`
- `productId` — identifier of the product from the catalog. You can find it with the method [catalog.product.list](../../api-reference/catalog/product/catalog-product-list.md). The examples use product `7075`. For a line item that is not in the catalog, pass `0`

The identifiers `891` and `7075` are example values. Replace them with your own.

The full list of line item fields is returned by the method [sale.basketitem.getFields](../../api-reference/sale/basket-item/sale-basket-item-get-fields.md).

### How to Set the Price Manually

Four fields are responsible for the manual price.

- `customPrice` — indicates that the price is set by the integration. With the value `Y`, the catalog data is ignored
- `basePrice` — original price without a discount or a markup
- `price` — final price you sell at
- `discountPrice` — amount of the discount or the markup

The three numeric fields are bound by the condition `basePrice = price + discountPrice`. A discount is a positive `discountPrice`, a markup is a negative one. For example, the product costs 1030 in the catalog and has to be sold for 1100: the markup equals 70, therefore `discountPrice` = -70.

{% note warning "" %}

The method does not verify this condition and saves any three numbers you pass. Calculate them yourself, otherwise the order will display an incorrect discount.

{% endnote %}

{% include [Note on examples](../../_includes/examples.md) %}

## 1\. Add a Product from the Catalog and Assign Your Own Price

Pass the product identifier in `productId` and set the price manually. The name, unit of measurement, and external codes are taken by the method from the product card.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'sale.basketitem.add',
        params: {
            fields: {
                orderId: 891,
                productId: 7075,
                quantity: 4,
                currency: 'EUR',
                customPrice: 'Y', // we set the price ourselves, the catalog price is not applied
                basePrice: 1030, // product price in the catalog
                price: 1100, // selling price
                discountPrice: -70, // markup, therefore the value is negative
            }
        },
        requestId: 'basketitem-add-catalog'
    })

    if (response.isSuccess) {
        console.log(response.getData().result)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    try:
        result = client.sale.basketitem.add(
            fields={
                "orderId": 891,
                "productId": 7075,
                "quantity": 4,
                "currency": "EUR",
                "customPrice": "Y",  # we set the price ourselves, the catalog price is not applied
                "basePrice": 1030,  # product price in the catalog
                "price": 1100,  # selling price
                "discountPrice": -70,  # markup, therefore the value is negative
            },
        ).response.result
        print(result)
    except BitrixAPIError as error:
        print(error)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $sb->getSaleScope()->basketItem()->add([
        'orderId' => 891,
        'productId' => 7075,
        'quantity' => 4,
        'currency' => 'EUR',
        'customPrice' => 'Y', // we set the price ourselves, the catalog price is not applied
        'basePrice' => 1030, // product price in the catalog
        'price' => 1100, // selling price
        'discountPrice' => -70, // markup, therefore the value is negative
    ]);

    echo '<PRE>';
    print_r($result->getId());
    echo '</PRE>';
    ```

- Go

    ```go
    // Setup in an empty folder — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/' && go run .
    //
    // The example is self-contained: it creates a product with a price and an order,
    // adds a line item with a manual price, prints it, and cleans up after itself.
    // It runs on any Bitrix24, nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"errors"
    	"fmt"
    	"log"
    	"os"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- setup: a product with a price and an empty order

    	iblockID, err := firstCatalog(ctx, core)
    	if err != nil {
    		return err
    	}
    	productID, err := addProduct(ctx, core, iblockID, "Sample line item (b24gosdk)")
    	if err != nil {
    		return err
    	}
    	defer del(ctx, core, "catalog.product.delete", productID)

    	if err := addPrice(ctx, core, productID, 1030); err != nil {
    		return err
    	}
    	orderID, err := addOrder(ctx, core)
    	if err != nil {
    		return err
    	}
    	defer del(ctx, core, "sale.order.delete", orderID)

    	// --- the scenario of this step itself

    	// The price is set manually, therefore customPrice = "Y". The markup is expressed
    	// by a NEGATIVE discountPrice: the base price is 1030, we sell for 1100.
    	res, err := core.Call(ctx, "sale.basketitem.add", b24.Params{
    		"fields": b24.Params{
    			"orderId":       orderID,
    			"productId":     productID,
    			"quantity":      4,
    			"currency":      "EUR",
    			"customPrice":   "Y",
    			"basePrice":     1030,
    			"price":         1100,
    			"discountPrice": -70,
    		},
    	})
    	if err != nil {
    		// The error code is compared with errors.Is rather than as a string: a typo in
    		// a literal would compile and silently take another branch.
    		if errors.Is(err, b24.ErrAccessDenied) {
    			return fmt.Errorf("the webhook lacks the sale permission: %w", err)
    		}
    		return fmt.Errorf("sale.basketitem.add: %w", err)
    	}

    	// The method wraps the response in an object with the basketItem key.
    	raw, ok := b24.Unwrap(res.Result, "basketItem")
    	if !ok {
    		return fmt.Errorf("no basketItem in the response: %s", res.Result)
    	}
    	var item struct {
    		ID        b24.ID  `json:"id"`
    		Quantity  float64 `json:"quantity"`
    		Price     float64 `json:"price"`
    		BasePrice float64 `json:"basePrice"`
    	}
    	if err := json.Unmarshal(raw, &item); err != nil {
    		return fmt.Errorf("parsing the line item: %w", err)
    	}

    	fmt.Printf("line item %d: %.0f x %.2f at the base price %.2f\n",
    		item.ID, item.Quantity, item.Price, item.BasePrice)
    	return nil
    }

    // --- helpers: preparing the data and cleaning up

    func firstCatalog(ctx context.Context, core *b24.Core) (b24.ID, error) {
    	res, err := core.Call(ctx, "catalog.catalog.list", b24.Params{
    		"filter": b24.Params{"iblockTypeId": "CRM_PRODUCT_CATALOG"},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return 0, fmt.Errorf("catalog.catalog.list: %w", err)
    	}
    	var out struct {
    		Catalogs []struct {
    			IblockID b24.ID `json:"iblockId"`
    		} `json:"catalogs"`
    	}
    	if err := json.Unmarshal(res.Result, &out); err != nil {
    		return 0, err
    	}
    	if len(out.Catalogs) == 0 {
    		return 0, fmt.Errorf("there is no commercial catalog in Bitrix24")
    	}
    	return out.Catalogs[0].IblockID, nil
    }

    func addProduct(ctx context.Context, core *b24.Core, iblockID b24.ID, name string) (b24.ID, error) {
    	res, err := core.Call(ctx, "catalog.product.add", b24.Params{
    		"fields": b24.Params{"iblockId": iblockID, "name": name, "active": "Y"},
    	})
    	if err != nil {
    		return 0, fmt.Errorf("catalog.product.add: %w", err)
    	}
    	// add responds with the element key, while get responds with the product key for the same object.
    	raw, ok := b24.Unwrap(res.Result, "element", "id")
    	if !ok {
    		return 0, fmt.Errorf("no element.id in %s", res.Result)
    	}
    	var id b24.ID
    	return id, json.Unmarshal(raw, &id)
    }

    func addPrice(ctx context.Context, core *b24.Core, productID b24.ID, price float64) error {
    	res, err := core.Call(ctx, "catalog.priceType.list", nil, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("catalog.priceType.list: %w", err)
    	}
    	var types struct {
    		PriceTypes []struct {
    			ID b24.ID `json:"id"`
    		} `json:"priceTypes"`
    	}
    	if err := json.Unmarshal(res.Result, &types); err != nil {
    		return err
    	}
    	if len(types.PriceTypes) == 0 {
    		return fmt.Errorf("there are no price types in Bitrix24")
    	}
    	_, err = core.Call(ctx, "catalog.price.add", b24.Params{
    		"fields": b24.Params{
    			"productId": productID, "catalogGroupId": types.PriceTypes[0].ID,
    			"price": price, "currency": "EUR",
    		},
    	})
    	return err
    }

    func addOrder(ctx context.Context, core *b24.Core) (b24.ID, error) {
    	res, err := core.Call(ctx, "sale.persontype.list", nil, b24.WithIdempotent())
    	if err != nil {
    		return 0, fmt.Errorf("sale.persontype.list: %w", err)
    	}
    	var types struct {
    		PersonTypes []struct {
    			ID b24.ID `json:"id"`
    		} `json:"personTypes"`
    	}
    	if err := json.Unmarshal(res.Result, &types); err != nil {
    		return 0, err
    	}
    	if len(types.PersonTypes) == 0 {
    		return 0, fmt.Errorf("there are no payer types in Bitrix24")
    	}

    	res, err = core.Call(ctx, "sale.order.add", b24.Params{
    		"fields": b24.Params{
    			"lid": "s1", "personTypeId": types.PersonTypes[0].ID, "currency": "EUR",
    		},
    	})
    	if err != nil {
    		return 0, fmt.Errorf("sale.order.add: %w", err)
    	}
    	raw, ok := b24.Unwrap(res.Result, "order", "id")
    	if !ok {
    		return 0, fmt.Errorf("no order.id in %s", res.Result)
    	}
    	var id b24.ID
    	return id, json.Unmarshal(raw, &id)
    }

    // del removes what was created. A cleanup error is printed but not returned: it must
    // not mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, id b24.ID) {
    	if _, err := core.Call(ctx, method, b24.Params{"id": id}); err != nil {
    		fmt.Fprintf(os.Stderr, "%s(%d): %v\n", method, id, err)
    	}
    }
    ```

{% endlist %}

Response:

```json
{
    "result": {
        "basketItem": {
            "barcodeMulti": "N",
            "basePrice": 1030,
            "canBuy": "Y",
            "catalogXmlId": "FUTURE-QUICKBOOKS-CATALOG",
            "currency": "EUR",
            "customPrice": "Y",
            "dateInsert": "2026-08-18T09:12:34+03:00",
            "dateUpdate": "2026-08-18T09:12:34+03:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";N;s:6:\"HEIGHT\";N;s:6:\"LENGTH\";N;}",
            "discountPrice": -70,
            "id": 1173,
            "measureCode": "796",
            "measureName": "pcs",
            "name": "Coffee machine",
            "orderId": 891,
            "price": 1100,
            "productId": 7075,
            "productXmlId": "7075",
            "properties": [],
            "quantity": 4,
            "reservations": [],
            "sort": 100,
            "vatIncluded": "N",
            "vatRate": null,
            "weight": 0,
            "xmlId": "bx_6a8405e2d0998"
        }
    },
    "total": 1,
    "time": {
        "start": 1787037154,
        "finish": 1787037155.489497,
        "duration": 1.4894969463348389,
        "processing": 1,
        "date_start": "2026-08-18T10:12:34+03:00",
        "date_finish": "2026-08-18T10:12:35+03:00",
        "operating_reset_at": 1787037225,
        "operating": 4.078084945678711
    }
}
```

The name `name`, the unit of measurement `measureCode` and `measureName`, and the external codes `catalogXmlId` and `productXmlId` came from the product card. Save the line item `id` — it is located in `result.basketItem.id`. This identifier is passed to the `id` parameter of the methods [sale.basketitem.update](../../api-reference/sale/basket-item/sale-basket-item-update.md) and [sale.basketitem.delete](../../api-reference/sale/basket-item/sale-basket-item-delete.md) if the row has to be modified or deleted.

## 2\. Add a Line Item That Is Not in the Catalog

For a one-time service or contract work there is no product in the catalog, so pass `productId: 0` and fill in the attributes yourself.

- `name` — name of the line item. The method does not require this field, but without it the order row remains unnamed
- `measureCode` and `measureName` — code and label of the unit of measurement. The code `796` stands for a piece. The units of measurement configured in Bitrix24 are returned by the method [catalog.measure.list](../../api-reference/catalog/measure/catalog-measure-list.md)
- `weight` — weight of the line item
- `dimensions` — dimensions of the line item as a serialized array
- `vatRate` — tax rate as a fraction of one: `0.1` means 10 %. To specify the "No VAT" rate, pass an empty string
- `vatIncluded` — whether the tax is included in the price
- `canBuy` — whether the line item is available for purchase
- `sort` — position of the row in the list of order line items
- `xmlId` and `productXmlId` — external codes of the line item and of the product. They are handy for linking the order row to a record in your system

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'sale.basketitem.add',
        params: {
            fields: {
                orderId: 891,
                productId: 0, // the line item is not in the catalog
                name: 'Equipment setup',
                quantity: 2,
                currency: 'EUR',
                customPrice: 'Y',
                basePrice: 1000, // price without a discount
                price: 900, // selling price
                discountPrice: 100, // discount, therefore the value is positive
                canBuy: 'Y',
                weight: 40,
                measureCode: '796',
                measureName: 'pcs',
                sort: 400,
                xmlId: 'service-setup-1',
                dimensions: 'a:3:{s:5:"WIDTH";i:244;s:6:"HEIGHT";i:100;s:6:"LENGTH";i:31;}', // serialized array
                vatRate: 0.1, // rate of 10 %
                vatIncluded: 'Y',
                productXmlId: 'service-setup',
            }
        },
        requestId: 'basketitem-add-custom'
    })

    if (response.isSuccess) {
        console.log(response.getData().result)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    try:
        result = client.sale.basketitem.add(
            fields={
                "orderId": 891,
                "productId": 0,  # the line item is not in the catalog
                "name": "Equipment setup",
                "quantity": 2,
                "currency": "EUR",
                "customPrice": "Y",
                "basePrice": 1000,  # price without a discount
                "price": 900,  # selling price
                "discountPrice": 100,  # discount, therefore the value is positive
                "canBuy": "Y",
                "weight": 40,
                "measureCode": "796",
                "measureName": "pcs",
                "sort": 400,
                "xmlId": "service-setup-1",
                "dimensions": 'a:3:{s:5:"WIDTH";i:244;s:6:"HEIGHT";i:100;s:6:"LENGTH";i:31;}',  # serialized array
                "vatRate": 0.1,  # rate of 10 %
                "vatIncluded": "Y",
                "productXmlId": "service-setup",
            },
        ).response.result
        print(result)
    except BitrixAPIError as error:
        print(error)
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $sb->getSaleScope()->basketItem()->add([
        'orderId' => 891,
        'productId' => 0, // the line item is not in the catalog
        'name' => 'Equipment setup',
        'quantity' => 2,
        'currency' => 'EUR',
        'customPrice' => 'Y',
        'basePrice' => 1000, // price without a discount
        'price' => 900, // selling price
        'discountPrice' => 100, // discount, therefore the value is positive
        'canBuy' => 'Y',
        'weight' => 40,
        'measureCode' => '796',
        'measureName' => 'pcs',
        'sort' => 400,
        'xmlId' => 'service-setup-1',
        'dimensions' => 'a:3:{s:5:"WIDTH";i:244;s:6:"HEIGHT";i:100;s:6:"LENGTH";i:31;}', // serialized array
        'vatRate' => 0.1, // rate of 10 %
        'vatIncluded' => 'Y',
        'productXmlId' => 'service-setup',
    ]);

    echo '<PRE>';
    print_r($result->getId());
    echo '</PRE>';
    ```
{% endlist %}

Response:

```json
{
    "result": {
        "basketItem": {
            "basePrice": 1000,
            "canBuy": "Y",
            "currency": "EUR",
            "customPrice": "Y",
            "dateInsert": "2026-08-18T09:13:24+03:00",
            "dateUpdate": "2026-08-18T09:13:24+03:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";i:244;s:6:\"HEIGHT\";i:100;s:6:\"LENGTH\";i:31;}",
            "discountPrice": 100,
            "id": 1175,
            "measureCode": "796",
            "measureName": "pcs",
            "name": "Equipment setup",
            "orderId": 891,
            "price": 900,
            "productId": 0,
            "productXmlId": "service-setup",
            "properties": [],
            "quantity": 2,
            "reservations": [],
            "sort": 400,
            "vatIncluded": "Y",
            "vatRate": 0.1,
            "weight": 40,
            "xmlId": "service-setup-1"
        }
    },
    "total": 1,
    "time": {
        "start": 1787037204,
        "finish": 1787037204.794355,
        "duration": 0.7943549156188965,
        "processing": 0,
        "date_start": "2026-08-18T10:13:24+03:00",
        "date_finish": "2026-08-18T10:13:24+03:00",
        "operating_reset_at": 1787037427,
        "operating": 3.351419687271118
    }
}
```

The response contains no `catalogXmlId` and `barcodeMulti` fields: a line item without a product has no card in the catalog to take them from. The value `productId: 0` confirms that the row is not linked to the catalog.

## Verify the Result

The line items of an order are returned by the method [sale.basketitem.list](../../api-reference/sale/basket-item/sale-basket-item-list.md). Let us filter them by the order identifier. The examples use the same SDK client as the previous steps — the initialization is not repeated.

{% list tabs %}

- JS

    ```js
    const response = await $b24.actions.v2.call.make({
        method: 'sale.basketitem.list',
        params: {
            select: ['id', 'productId', 'name', 'quantity', 'basePrice', 'price', 'discountPrice', 'customPrice'],
            filter: { '=orderId': 891 }
        },
        requestId: 'basketitem-list'
    })

    if (response.isSuccess) {
        console.log(response.getData().result.basketItems)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
    ```

- Python

    ```python
    try:
        result = client.sale.basketitem.list(
            select=["id", "productId", "name", "quantity", "basePrice", "price", "discountPrice", "customPrice"],
            filter={"=orderId": 891},
        ).response.result
        print(result)
    except BitrixAPIError as error:
        print(error)
    ```


- PHP

    ```php
    $result = $sb->getSaleScope()->basketItem()->list(
        ['id', 'productId', 'name', 'quantity', 'basePrice', 'price', 'discountPrice', 'customPrice'],
        ['=orderId' => 891]
    );

    echo '<PRE>';
    print_r($result->getBasketItems());
    echo '</PRE>';
    ```
{% endlist %}

Response:

```json
{
    "result": {
        "basketItems": [
            {
                "basePrice": 1030,
                "customPrice": "Y",
                "discountPrice": -70,
                "id": 1173,
                "name": "Coffee machine",
                "price": 1100,
                "productId": 7075,
                "quantity": 4
            },
            {
                "basePrice": 1000,
                "customPrice": "Y",
                "discountPrice": 100,
                "id": 1175,
                "name": "Equipment setup",
                "price": 900,
                "productId": 0,
                "quantity": 2
            }
        ]
    },
    "total": 2
}
```

Both rows are in place, the prices and quantities match what you passed, and `customPrice: "Y"` confirms that the catalog price was not applied.

The order total and the tax are returned by the method [sale.order.get](../../api-reference/sale/order/sale-order-get.md). For the two line items from the examples the response contains the values:

```json
{
    "price": 6200,
    "taxValue": 163.63636364
}
```

The total 6200 is 4 × 1100 plus 2 × 900. The tax 163.64 is 10 % of 1800: this is the cost of the second line item, for which we passed `vatRate: 0.1`. Had the field value been `10`, Bitrix24 would have calculated a rate of 1000 %.

In the interface the result is visible in the order card: the new rows appear in the list of products.

## If the Method Returns an Error

Check the request data.

- `0` — `Required fields: orderId` — the required `orderId` was not passed
- `200140400009` — `Order not found` — there is no order with such an identifier. Check `orderId` with the method [sale.order.list](../../api-reference/sale/order/sale-order-list.md)
- `200140400011` — `Currency must be the currency of the order` — the currency of the line item does not match the currency of the order. Take the currency from the `currency` field of the order
- `200140400007` — `basket item is not saved - bad data` — the line item could not be saved. Check the set of fields with the method [sale.basketitem.getFields](../../api-reference/sale/basket-item/sale-basket-item-get-fields.md)

The first three errors are returned before anything is written to the order, so correct the data and repeat the entire call. After the error `200140400007`, first retrieve the order line items with the method [sale.basketitem.list](../../api-reference/sale/basket-item/sale-basket-item-list.md): this check runs after the order has already been saved.

## Key Considerations

- the method does not look for duplicates: a repeated call with the same fields adds one more row to the order instead of modifying the existing one. To modify a row, use [sale.basketitem.update](../../api-reference/sale/basket-item/sale-basket-item-update.md)
- passing `price` alone is enough for the line item to switch to a manual price: Bitrix24 sets `customPrice: "Y"` on its own. If the price has to stay the catalog one, do not pass `price`
- `vatRate` is set as a fraction rather than as a percentage: the value `0.1` means a rate of 10 %
- a line item with `productId: 0` is not linked to the catalog. Bitrix24 has nowhere to take the name, unit of measurement, weight, dimensions, and tax rate from, so pass them in every call

## Continue Learning

- [{#T}](../../api-reference/sale/basket-item/sale-basket-item-add.md)
- [{#T}](../../api-reference/sale/basket-item/sale-basket-item-add-catalog-product.md)
- [{#T}](../../api-reference/sale/basket-item/sale-basket-item-update.md)
- [{#T}](../../api-reference/sale/basket-item/sale-basket-item-get-fields.md)
- [{#T}](../../api-reference/sale/data-types.md)
