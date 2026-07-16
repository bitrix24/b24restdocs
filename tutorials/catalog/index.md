# How to Add a Product with Custom Property Values

> Scope: [`catalog`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: an administrator or a user with permissions to modify the property information block, add a product, and change the product sale price

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A product in the Commercial catalog can be supplemented with custom properties: a list, a multiple list, a file, or multiple files. The values of these properties are passed during product creation in the `propertyN` fields, where `N` is the property identifier.

For example, we will add a product with a color, several sizes, a certificate, and an image gallery. We use a separate method for the price because `catalog.product.add` creates the product card, while prices are added using `catalog.price.*` methods.

The scenario consists of three steps.

1. Create product properties using the [catalog.productProperty.add](../../api-reference/catalog/product-property/catalog-product-property-add.md) and [catalog.productPropertyEnum.add](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-add.md) methods.
2. Add a product using the [catalog.product.add](../../api-reference/catalog/product/catalog-product-add.md) method and pass the property values in `propertyN`.
3. Add a price using the [catalog.price.add](../../api-reference/catalog/price/catalog-price-add.md) method.

Before running the examples, prepare the environment:

- Install the SDK for your language: `npm install @bitrix24/b24jssdk`, `composer require bitrix24/b24phpsdk:"^3.0"`, or `pip install b24pysdk`
- The examples are executed on a server and authorized via an [incoming webhook](../../local-integrations/local-webhooks.md) with the `catalog` permission. Replace the webhook address with your own
- Files to be uploaded must be accessible to the example code via a path on the server

## 1. Prepare Properties

To add a product, you need the following values:

- `iblockId` — the Commercial catalog identifier. This can be retrieved using the [catalog.catalog.list](../../api-reference/catalog/catalog/catalog-catalog-list.md) method.
- `catalogGroupId` — the price type identifier. This can be retrieved using the [catalog.priceType.list](../../api-reference/catalog/price-type/catalog-price-type-list.md) method.
- property identifiers returned by the [catalog.productProperty.add](../../api-reference/catalog/product-property/catalog-product-property-add.md) method.
- list value identifiers returned by the [catalog.productPropertyEnum.add](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-add.md) method.

If the properties have already been created, do not repeat the first step. Use the existing property and list value identifiers.

In this example, we will create four properties:

- `Color` — a list property
- `Sizes` — a multiple list property
- `Certificate` — a file property
- `Gallery` — a multiple file property

{% note warning "A single list property requires at least two values" %}

For the single list property `multiple: N`, create at least two list values. If there is only one value, Bitrix24 defines such a property as "Yes/No" — `userType: BoolEnum`. The list value identifier in field `propertyN` will not be saved: the product will return `propertyN: "N"`, and no error will occur.

Therefore, for the `Color` property, we will create two values — `Blue` and `Red`. This restriction does not apply to multiple list properties.

You can check how the property was defined using the [catalog.product.getFieldsByFilter](../../api-reference/catalog/product/catalog-product-get-fields-by-filter.md) method — the response for field `propertyN` will contain `userType: BoolEnum`. The [catalog.productProperty.get](../../api-reference/catalog/product-property/catalog-product-property-get.md) method will return `userType: null`, because the "Yes/No" type is calculated based on the number of list values.

{% endnote %}

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    const iblockId = 23

    async function callMethod(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    async function addProperty(fields) {
        const result = await callMethod(
            'catalog.productProperty.add',
            { fields },
            `property-add-${fields.code}`
        )

        return result.productProperty
    }

    async function addPropertyEnum(propertyId, value, xmlId, sort) {
        const result = await callMethod(
            'catalog.productPropertyEnum.add',
            { fields: { propertyId, value, xmlId, sort } },
            `property-enum-add-${xmlId}`
        )

        return result.productPropertyEnum
    }

    async function prepareProperties() {
        const colorProperty = await addProperty({
            iblockId: iblockId,
            name: 'Color',
            code: 'COLOR',
            propertyType: 'L',
            listType: 'L',
            multiple: 'N',
            active: 'Y',
            sort: 100
        })

        const sizeProperty = await addProperty({
            iblockId: iblockId,
            name: 'Sizes',
            code: 'SIZES',
            propertyType: 'L',
            listType: 'C',
            multiple: 'Y',
            active: 'Y',
            sort: 200
        })

        const certificateProperty = await addProperty({
            iblockId: iblockId,
            name: 'Certificate',
            code: 'CERTIFICATE',
            propertyType: 'F',
            multiple: 'N',
            active: 'Y',
            sort: 300
        })

        const galleryProperty = await addProperty({
            iblockId: iblockId,
            name: 'Gallery',
            code: 'GALLERY',
            propertyType: 'F',
            multiple: 'Y',
            active: 'Y',
            sort: 400
        })

        const colorBlue = await addPropertyEnum(colorProperty.id, 'Blue', 'BLUE', 100)
        const colorRed = await addPropertyEnum(colorProperty.id, 'Red', 'RED', 200)
        const sizeM = await addPropertyEnum(sizeProperty.id, 'M', 'M', 100)
        const sizeL = await addPropertyEnum(sizeProperty.id, 'L', 'L', 200)

        console.log({
            colorPropertyId: colorProperty.id,
            colorBlueId: colorBlue.id,
            colorRedId: colorRed.id,
            sizePropertyId: sizeProperty.id,
            sizeValueIds: [sizeM.id, sizeL.id],
            certificatePropertyId: certificateProperty.id,
            galleryPropertyId: galleryProperty.id
        })
    }

    try {
        await prepareProperties()
    } catch (error) {
        console.error('Error:', error.message)
    } finally {
        $b24.destroy()
    }
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Monolog\Handler\StreamHandler;
    use Monolog\Logger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $iblockId = 23;

    function addProperty($b24, array $fields): array
    {
        return $b24->core->call('catalog.productProperty.add', ['fields' => $fields])
            ->getResponseData()
            ->getResult()['productProperty'];
    }

    function addPropertyEnum($b24, int $propertyId, string $value, string $xmlId, int $sort): array
    {
        return $b24->core->call(
            'catalog.productPropertyEnum.add',
            [
                'fields' => [
                    'propertyId' => $propertyId,
                    'value' => $value,
                    'xmlId' => $xmlId,
                    'sort' => $sort,
                ],
            ]
        )->getResponseData()->getResult()['productPropertyEnum'];
    }

    try
    {
        $colorProperty = addProperty($b24, [
            'iblockId' => $iblockId,
            'name' => 'Color',
            'code' => 'COLOR',
            'propertyType' => 'L',
            'listType' => 'L',
            'multiple' => 'N',
            'active' => 'Y',
            'sort' => 100,
        ]);

        $sizeProperty = addProperty($b24, [
            'iblockId' => $iblockId,
            'name' => 'Sizes',
            'code' => 'SIZES',
            'propertyType' => 'L',
            'listType' => 'C',
            'multiple' => 'Y',
            'active' => 'Y',
            'sort' => 200,
        ]);

        $certificateProperty = addProperty($b24, [
            'iblockId' => $iblockId,
            'name' => 'Certificate',
            'code' => 'CERTIFICATE',
            'propertyType' => 'F',
            'multiple' => 'N',
            'active' => 'Y',
            'sort' => 300,
        ]);

        $galleryProperty = addProperty($b24, [
            'iblockId' => $iblockId,
            'name' => 'Gallery',
            'code' => 'GALLERY',
            'propertyType' => 'F',
            'multiple' => 'Y',
            'active' => 'Y',
            'sort' => 400,
        ]);

        $colorBlue = addPropertyEnum($b24, $colorProperty['id'], 'Blue', 'BLUE', 100);
        $colorRed = addPropertyEnum($b24, $colorProperty['id'], 'Red', 'RED', 200);
        $sizeM = addPropertyEnum($b24, $sizeProperty['id'], 'M', 'M', 100);
        $sizeL = addPropertyEnum($b24, $sizeProperty['id'], 'L', 'L', 200);

        print_r([
            'colorPropertyId' => $colorProperty['id'],
            'colorBlueId' => $colorBlue['id'],
            'colorRedId' => $colorRed['id'],
            'sizePropertyId' => $sizeProperty['id'],
            'sizeValueIds' => [
                $sizeM['id'],
                $sizeL['id'],
            ],
            'certificatePropertyId' => $certificateProperty['id'],
            'galleryPropertyId' => $galleryProperty['id'],
        ]);
    }
    catch (BaseException $exception)
    {
        echo 'Error: '.$exception->getMessage();
    }
    ?>
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    iblock_id = 23

    def add_property(fields):
        return client.catalog.product_property.add(fields=fields).response.result["productProperty"]

    def add_property_enum(property_id, value, xml_id, sort):
        return client.catalog.product_property_enum.add(
            fields={
                "propertyId": property_id,
                "value": value,
                "xmlId": xml_id,
                "sort": sort,
            },
        ).response.result["productPropertyEnum"]

    try:
        color_property = add_property({
            "iblockId": iblock_id,
            "name": "Color",
            "code": "COLOR",
            "propertyType": "L",
            "listType": "L",
            "multiple": "N",
            "active": "Y",
            "sort": 100,
        })

        size_property = add_property({
            "iblockId": iblock_id,
            "name": "Sizes",
            "code": "SIZES",
            "propertyType": "L",
            "listType": "C",
            "multiple": "Y",
            "active": "Y",
            "sort": 200,
        })

        certificate_property = add_property({
            "iblockId": iblock_id,
            "name": "Certificate",
            "code": "CERTIFICATE",
            "propertyType": "F",
            "multiple": "N",
            "active": "Y",
            "sort": 300,
        })

        gallery_property = add_property({
            "iblockId": iblock_id,
            "name": "Gallery",
            "code": "GALLERY",
            "propertyType": "F",
            "multiple": "Y",
            "active": "Y",
            "sort": 400,
        })

        color_blue = add_property_enum(color_property["id"], "Blue", "BLUE", 100)
        color_red = add_property_enum(color_property["id"], "Red", "RED", 200)
        size_m = add_property_enum(size_property["id"], "M", "M", 100)
        size_l = add_property_enum(size_property["id"], "L", "L", 200)

    except BitrixAPIError as error:
        print(f"Error: {error}")

    else:
        print({
            "colorPropertyId": color_property["id"],
            "colorBlueId": color_blue["id"],
            "colorRedId": color_red["id"],
            "sizePropertyId": size_property["id"],
            "sizeValueIds": [size_m["id"], size_l["id"]],
            "certificatePropertyId": certificate_property["id"],
            "galleryPropertyId": gallery_property["id"],
        })
    ```

{% endlist %}

After completing the first step, save the property and list value identifiers. They will be required when creating the product.

```json
{
    "colorPropertyId": 431,
    "colorBlueId": 1739,
    "colorRedId": 1740,
    "sizePropertyId": 432,
    "sizeValueIds": [
        1741,
        1742
    ],
    "certificatePropertyId": 433,
    "galleryPropertyId": 434
}
```

When rerunning the example, change the `code` properties and `xmlId` list values or use the already created identifiers. Otherwise, the methods may return a duplicate error.

## 2. Add a Product with Property Values

The [catalog.product.add](../../api-reference/catalog/product/catalog-product-add.md) method accepts property values in the `fields` parameter. The field name is formed as `propertyN`, where `N` is the property identifier.

Use different value formats for different property types:

- List property — the list value identifier
- Multiple list property — an array of list value identifiers
- File property — a `{value: {fileData: [fileName, base64]}}` object
- Multiple file property — an array of `{value: {fileData: [fileName, base64]}}` objects

To run the example, create a folder `pictures` next to the example file and add the files `certificate.pdf`, `gallery-1.jpg`, and `gallery-2.jpg`.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { readFile } from 'node:fs/promises'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    const iblockId = 23
    const colorPropertyId = 431
    const colorBlueId = 1739
    const sizePropertyId = 432
    const sizeValueIds = [1741, 1742]
    const certificatePropertyId = 433
    const galleryPropertyId = 434

    async function encodeFile(filePath) {
        const content = await readFile(filePath)

        return [filePath.split('/').pop(), content.toString('base64')]
    }

    async function addProduct() {
        const fields = {
            iblockId: iblockId,
            name: 'Printed T-shirt',
            active: 'Y',
            sort: 100,
            ['property' + colorPropertyId]: colorBlueId,
            ['property' + sizePropertyId]: sizeValueIds,
            ['property' + certificatePropertyId]: {
                value: {
                    fileData: await encodeFile('pictures/certificate.pdf')
                }
            },
            ['property' + galleryPropertyId]: [
                {
                    value: {
                        fileData: await encodeFile('pictures/gallery-1.jpg')
                    }
                },
                {
                    value: {
                        fileData: await encodeFile('pictures/gallery-2.jpg')
                    }
                }
            ]
        }

        const response = await $b24.actions.v2.call.make({
            method: 'catalog.product.add',
            params: { fields },
            requestId: 'product-add'
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        const productId = Number(response.getData().result.element.id)
        console.log('Product added: ' + productId)
    }

    try {
        await addProduct()
    } catch (error) {
        console.error('Error:', error.message)
    } finally {
        $b24.destroy()
    }
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Monolog\Handler\StreamHandler;
    use Monolog\Logger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $iblockId = 23;
    $colorPropertyId = 431;
    $colorBlueId = 1739;
    $sizePropertyId = 432;
    $sizeValueIds = [1741, 1742];
    $certificatePropertyId = 433;
    $galleryPropertyId = 434;

    function encodeFile(string $path): array
    {
        if (!file_exists($path))
        {
            throw new RuntimeException('File not found: '.$path);
        }

        return [
            basename($path),
            base64_encode(file_get_contents($path)),
        ];
    }

    try
    {
        $fields = [
            'iblockId' => $iblockId,
            'name' => 'Printed T-shirt',
            'active' => 'Y',
            'sort' => 100,
            'property'.$colorPropertyId => $colorBlueId,
            'property'.$sizePropertyId => $sizeValueIds,
            'property'.$certificatePropertyId => [
                'value' => [
                    'fileData' => encodeFile('pictures/certificate.pdf'),
                ],
            ],
            'property'.$galleryPropertyId => [
                [
                    'value' => [
                        'fileData' => encodeFile('pictures/gallery-1.jpg'),
                    ],
                ],
                [
                    'value' => [
                        'fileData' => encodeFile('pictures/gallery-2.jpg'),
                    ],
                ],
            ],
        ];

        $productId = $b24->getCatalogScope()->product()->add($fields)->product()->id;

        echo 'Product added: '.$productId;
    }
    catch (BaseException|RuntimeException $exception)
    {
        echo 'Error: '.$exception->getMessage();
    }
    ?>
    ```

- Python

    ```python
    # pip install b24pysdk
    import base64

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    iblock_id = 23
    color_property_id = 431
    color_blue_id = 1739
    size_property_id = 432
    size_value_ids = [1741, 1742]
    certificate_property_id = 433
    gallery_property_id = 434

    def encode_file(path):
        with open(path, "rb") as file:
            return [path.split("/")[-1], base64.b64encode(file.read()).decode()]

    fields = {
        "iblockId": iblock_id,
        "name": "Printed T-shirt",
        "active": "Y",
        "sort": 100,
        f"property{color_property_id}": color_blue_id,
        f"property{size_property_id}": size_value_ids,
        f"property{certificate_property_id}": {
            "value": {
                "fileData": encode_file("pictures/certificate.pdf"),
            },
        },
        f"property{gallery_property_id}": [
            {
                "value": {
                    "fileData": encode_file("pictures/gallery-1.jpg"),
                },
            },
            {
                "value": {
                    "fileData": encode_file("pictures/gallery-2.jpg"),
                },
            },
        ],
    }

    try:
        element = client.catalog.product.add(fields=fields).response.result["element"]
    except BitrixAPIError as error:
        print(f"Error: {error}")
    else:
        print(f"Product added: {element['id']}")
    ```

{% endlist %}

If the product is added successfully, the method returns a `element` object. The response will contain the product fields and the custom property values. File properties are returned with a link to the uploaded file rather than the original Base64 string.

```json
{
    "result": {
        "element": {
            "id": 1267,
            "iblockId": 23,
            "name": "Printed T-shirt",
            "active": "Y",
            "property431": {
                "value": "1739",
                "valueEnum": "Blue",
                "valueId": "9816"
            },
            "property432": [
                {
                    "value": "1741",
                    "valueEnum": "M",
                    "valueId": "9817"
                },
                {
                    "value": "1742",
                    "valueEnum": "L",
                    "valueId": "9818"
                }
            ],
            "property433": {
                "value": {
                    "id": "4801",
                    "url": "/rest/catalog.product.download?fields%5BfieldName%5D=property433&fields%5BfileId%5D=4801&fields%5BproductId%5D=1267",
                    "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=property433&fields%5BfileId%5D=4801&fields%5BproductId%5D=1267"
                },
                "valueId": "9819"
            },
            "property434": [
                {
                    "value": {
                        "id": "4803",
                        "url": "/rest/catalog.product.download?fields%5BfieldName%5D=property434&fields%5BfileId%5D=4803&fields%5BproductId%5D=1267",
                        "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=property434&fields%5BfileId%5D=4803&fields%5BproductId%5D=1267"
                    },
                    "valueId": "9820"
                },
                {
                    "value": {
                        "id": "4805",
                        "url": "/rest/catalog.product.download?fields%5BfieldName%5D=property434&fields%5BfileId%5D=4805&fields%5BproductId%5D=1267",
                        "urlMachine": "/rest/catalog.product.download?fields%5BfieldName%5D=property434&fields%5BfileId%5D=4805&fields%5BproductId%5D=1267"
                    },
                    "valueId": "9821"
                }
            ]
        }
    }
}
```

Retain the `element.id` value from the response. This is the identifier of the created product, which must be passed to the `productId` parameter when adding a price.

## 3. Add a Product Price

The [catalog.product.add](../../api-reference/catalog/product/catalog-product-add.md) method does not add a product price. To ensure the product can be used in sales scenarios with a price, call [catalog.price.add](../../api-reference/catalog/price/catalog-price-add.md).

In the examples below, replace `1267` with the `element.id` value obtained in the previous step.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    const productId = 1267
    const catalogGroupId = 1

    try {
        const response = await $b24.actions.v2.call.make({
            method: 'catalog.price.add',
            params: {
                fields: {
                    productId: productId,
                    catalogGroupId: catalogGroupId,
                    price: 4900,
                    currency: 'EUR'
                }
            },
            requestId: 'price-add'
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        console.log('Price added: ' + response.getData().result.price.id)
    } catch (error) {
        console.error('Error:', error.message)
    } finally {
        $b24.destroy()
    }
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Monolog\Handler\StreamHandler;
    use Monolog\Logger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $productId = 1267;
    $catalogGroupId = 1;

    try
    {
        $price = $b24->core->call(
            'catalog.price.add',
            [
                'fields' => [
                    'productId' => $productId,
                    'catalogGroupId' => $catalogGroupId,
                    'price' => 4900,
                    'currency' => 'EUR',
                ],
            ]
        )->getResponseData()->getResult()['price'];

        echo 'Price added: '.$price['id'];
    }
    catch (BaseException $exception)
    {
        echo 'Error: '.$exception->getMessage();
    }
    ?>
    ```

- Python

    ```python
    # pip install b24pysdk
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    product_id = 1267
    catalog_group_id = 1

    try:
        price = client.catalog.price.add(
            fields={
                "productId": product_id,
                "catalogGroupId": catalog_group_id,
                "price": 4900,
                "currency": "EUR",
            },
        ).response.result["price"]
    except BitrixAPIError as error:
        print(f"Error: {error}")
    else:
        print(f"Price added: {price['id']}")
    ```

{% endlist %}

If the price is added successfully, the method returns a `price` object.

```json
{
    "result": {
        "price": {
            "id": 987,
            "productId": 1267,
            "catalogGroupId": 1,
            "price": 4900,
            "priceScale": 4900,
            "currency": "EUR",
            "extraId": null,
            "quantityFrom": null,
            "quantityTo": null,
            "timestampX": "2024-11-01T17:00:55+03:00"
        }
    }
}
```

## Verify the Result

Open the product card in the catalog. The card will display the property values `Color`, `Sizes`, `Certificate`, and `Gallery`, and the product prices will show the `4900 EUR` value.

For automatic verification, call:

- [catalog.product.get](../../api-reference/catalog/product/catalog-product-get.md) with the `id` of the created product. The response should contain `name`, `iblockId`, and the `propertyN` fields for the created properties, such as `property431`, `property432`
- [catalog.price.list](../../api-reference/catalog/price/catalog-price-list.md) with a filter by the `productId` of the created product. The response should contain the price with `price: 4900` and `currency: EUR`

If the method returns an error, check the request data.

- `The specified iblock is not a product catalog` — the identifier passed in `iblockId` is an information block that is not a Commercial catalog
- `Invalid property type specified` — an invalid combination of `propertyType` and `userType` was passed
- `Only list properties are supported` — a list value is being added to a property whose type is not `L`
- `Required fields: iblockId, name, propertyType` — required property fields were not passed
- `A value with xmlId '...' already exists.` — a list value with this `xmlId` already exists. Use the existing value identifier or pass a new `xmlId`
- `Property code cannot start with a digit` — the `code` value of the property starts with a digit
- `Access Denied` — the user does not have permission to modify the catalog, properties, product, or price
- `Validate price error. Catalog price group is wrong` — an incorrect price type was passed in `catalogGroupId`

The method may not return an error but may also fail to retain the property value.

- `propertyN: "N"` in the response instead of a list value — a single-value list property has only one value, so Bitrix24 identified it as a "Yes/No" property. Add a second list value to the property using the [catalog.productPropertyEnum.add](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-add.md) method
- `propertyN: null` when updating a product — the [catalog.product.update](../../api-reference/catalog/product/catalog-product-update.md) method does not accept a list value identifier directly. Pass it in the `{value: 1739}` format

## Continue Learning

- [{#T}](../../api-reference/catalog/product/catalog-product-add.md)
- [{#T}](../../api-reference/catalog/product/catalog-product-get.md)
- [{#T}](../../api-reference/catalog/product-property/catalog-product-property-add.md)
- [{#T}](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-add.md)
- [{#T}](../../api-reference/catalog/price/catalog-price-add.md)
- [{#T}](../../api-reference/catalog/price/catalog-price-list.md)
- [{#T}](../../api-reference/catalog/product/catalog-product-list.md)
