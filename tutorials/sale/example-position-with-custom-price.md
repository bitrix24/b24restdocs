# Create a Position with a Product from the Catalog in a Quantity of 4 Units at an Arbitrary Price

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5147,"quantity":4,"productId":6544,"currency":"EUR","price":1100,"discountPrice":-1070,"customPrice":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.basketitem.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"orderId":5147,"quantity":4,"productId":6544,"currency":"EUR","price":1100,"discountPrice":-1070,"customPrice":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.basketitem.add
    ```

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const response = await $b24.actions.v2.call.make({
        method: 'sale.basketitem.add',
        params: {
            fields: { // minimum set of required fields
                orderId: 5147,
                quantity: 4,
                productId: 6544,
                currency: 'EUR',
                price: 1100,
                discountPrice: -1070, // catalog price – 30 EUR, specify markup
                customPrice: 'Y',
            }
        },
        requestId: 'basketitem-add'
    })

    if (response.isSuccess) {
        console.log(response.getData().result)
    } else {
        console.error(response.getErrorMessages().join('; '))
    }
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
        'orderId' => 5147,
        'quantity' => 4,
        'productId' => 6544,
        'currency' => 'EUR',
        'price' => 1100,
        'discountPrice' => -1070,
        'customPrice' => 'Y',
    ]);

    echo '<PRE>';
    print_r($result->getId());
    echo '</PRE>';
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
                "orderId": 5147,
                "quantity": 4,
                "productId": 6544,
                "currency": "EUR",
                "price": 1100,
                "discountPrice": -1070,
                "customPrice": "Y",
            },
        ).response.result
        print(result)
    except BitrixAPIError as error:
        print(error)
    ```

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained: it creates a product with a price and an order, adds a line item
    // with a manual price, displays it, and cleans up after itself. It runs on any
    // portal, nothing needs to be edited.
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
    	productID, err := addProduct(ctx, core, iblockID, "Line item example (b24gosdk)")
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

    	// --- the page scenario itself

    	// The price is set manually, so customPrice = "Y". A markup is expressed
    	// by a NEGATIVE discountPrice: the base price is 1030, it is sold for 1100.
    	res, err := core.Call(ctx, "sale.basketitem.add", b24.Params{
    		"fields": b24.Params{
    			"orderId":       orderID,
    			"productId":     productID,
    			"quantity":      4,
    			"currency":      "EUR",
    			"price":         1100,
    			"discountPrice": -70,
    			"customPrice":   "Y",
    		},
    	})
    	if err != nil {
    		// The error code is compared with errors.Is rather than as a string: a typo in the
    		// literal would compile and silently take a different branch.
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
    		return fmt.Errorf("parse line item: %w", err)
    	}

    	fmt.Printf("line item %d: %.0f x %.2f at a base price of %.2f\n",
    		item.ID, item.Quantity, item.Price, item.BasePrice)
    	return nil
    }

    // --- helpers: data setup and cleanup

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
    		return 0, fmt.Errorf("the portal has no commercial catalog")
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
    	// add responds with the element key, while get responds with the product key for the same entity.
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
    		return fmt.Errorf("the portal has no price types")
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
    		return 0, fmt.Errorf("the portal has no payer types")
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

    // del removes what was created. A cleanup error is printed but not returned: it must not
    // mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, id b24.ID) {
    	if _, err := core.Call(ctx, method, b24.Params{"id": id}); err != nil {
    		fmt.Fprintf(os.Stderr, "%s(%d): %v\n", method, id, err)
    	}
    }
    ```

{% endlist %}

## Result

```json
{
    "result": {
        "basketItem": {
            "basePrice": 30,
            "canBuy": "Y",
            "catalogXmlId": "FUTURE-1C-CATALOG",
            "currency": "EUR",
            "customPrice": "N",
            "dateInsert": "2024-04-23T15:59:37+02:00",
            "dateUpdate": "2024-04-23T15:59:37+02:00",
            "dimensions": "a:3:{s:5:\"WIDTH\";N;s:6:\"HEIGHT\";N;s:6:\"LENGTH\";N;}",
            "discountPrice": -1070,
            "id": 6790,
            "measureCode": "768",
            "measureName": "pcs",
            "name": "Product",
            "orderId": 5147,
            "price": 1000,
            "productId": 1245,
            "productXmlId": "1245",
            "properties": [],
            "quantity": 1,
            "reservations": [],
            "sort": 100,
            "vatIncluded": "N",
            "vatRate": null,
            "weight": 0,
            "xmlId": "bx_6627bec8c4fdc"
        }
    },
    "total": 1,
    "time": {
        "start": 1713880776.108755,
        "finish": 1713880777.704221,
        "duration": 1.595465898513794,
        "processing": 0.973701000213623,
        "date_start": "2024-04-23T15:59:36+02:00",
        "date_finish": "2024-04-23T15:59:37+02:00",
        "operating": 0
    }
}
```
