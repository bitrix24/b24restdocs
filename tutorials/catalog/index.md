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

- JS code is executed in a Bitrix24 application where the `BX24` object is available.
- PHP code uses the `CRest` class; configure a webhook or OAuth authorization for method calls.
- Files to be uploaded must be accessible to the example code: in JS via a relative application URL, and in PHP via a server path.

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

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    const iblockId = 23;

    function callMethod(method, params) {
        return new Promise((resolve, reject) => {
            BX24.callMethod(method, params, function(result) {
                if (result.error()) {
                    reject(result.error() + ': ' + result.error_description());
                    return;
                }

                resolve(result.data());
            });
        });
    }

    async function prepareProperties() {
        const colorProperty = await callMethod(
            'catalog.productProperty.add',
            {
                fields: {
                    iblockId: iblockId,
                    name: 'Color',
                    code: 'COLOR',
                    propertyType: 'L',
                    listType: 'L',
                    multiple: 'N',
                    active: 'Y',
                    sort: 100
                }
            }
        );

        const sizeProperty = await callMethod(
            'catalog.productProperty.add',
            {
                fields: {
                    iblockId: iblockId,
                    name: 'Sizes',
                    code: 'SIZES',
                    propertyType: 'L',
                    listType: 'C',
                    multiple: 'Y',
                    active: 'Y',
                    sort: 200
                }
            }
        );

        const certificateProperty = await callMethod(
            'catalog.productProperty.add',
            {
                fields: {
                    iblockId: iblockId,
                    name: 'Certificate',
                    code: 'CERTIFICATE',
                    propertyType: 'F',
                    multiple: 'N',
                    active: 'Y',
                    sort: 300
                }
            }
        );

        const galleryProperty = await callMethod(
            'catalog.productProperty.add',
            {
                fields: {
                    iblockId: iblockId,
                    name: 'Gallery',
                    code: 'GALLERY',
                    propertyType: 'F',
                    multiple: 'Y',
                    active: 'Y',
                    sort: 400
                }
            }
        );

        const colorBlue = await callMethod(
            'catalog.productPropertyEnum.add',
            {
                fields: {
                    propertyId: colorProperty.productProperty.id,
                    value: 'Blue',
                    xmlId: 'BLUE',
                    sort: 100
                }
            }
        );

        const sizeM = await callMethod(
            'catalog.productPropertyEnum.add',
            {
                fields: {
                    propertyId: sizeProperty.productProperty.id,
                    value: 'M',
                    xmlId: 'M',
                    sort: 100
                }
            }
        );

        const sizeL = await callMethod(
            'catalog.productPropertyEnum.add',
            {
                fields: {
                    propertyId: sizeProperty.productProperty.id,
                    value: 'L',
                    xmlId: 'L',
                    sort: 200
                }
            }
        );

        console.log({
            colorPropertyId: colorProperty.productProperty.id,
            colorBlueId: colorBlue.productPropertyEnum.id,
            sizePropertyId: sizeProperty.productProperty.id,
            sizeValueIds: [
                sizeM.productPropertyEnum.id,
                sizeL.productPropertyEnum.id
            ],
            certificatePropertyId: certificateProperty.productProperty.id,
            galleryPropertyId: galleryProperty.productProperty.id
        });
    }

    prepareProperties().catch(console.error);
    ```

- PHP

    ```php
    <?php
    require_once('crest.php');

    $iblockId = 23;

    function callRestMethod(string $method, array $params): array
    {
        $result = CRest::call($method, $params);

        if (!empty($result['error']))
        {
            throw new RuntimeException($result['error_description']);
        }

        return $result['result'];
    }

    try
    {
        $colorProperty = callRestMethod(
            'catalog.productProperty.add',
            [
                'fields' => [
                    'iblockId' => $iblockId,
                    'name' => 'Color',
                    'code' => 'COLOR',
                    'propertyType' => 'L',
                    'listType' => 'L',
                    'multiple' => 'N',
                    'active' => 'Y',
                    'sort' => 100,
                ],
            ]
        );

        $sizeProperty = callRestMethod(
            'catalog.productProperty.add',
            [
                'fields' => [
                    'iblockId' => $iblockId,
                    'name' => 'Sizes',
                    'code' => 'SIZES',
                    'propertyType' => 'L',
                    'listType' => 'C',
                    'multiple' => 'Y',
                    'active' => 'Y',
                    'sort' => 200,
                ],
            ]
        );

        $certificateProperty = callRestMethod(
            'catalog.productProperty.add',
            [
                'fields' => [
                    'iblockId' => $iblockId,
                    'name' => 'Certificate',
                    'code' => 'CERTIFICATE',
                    'propertyType' => 'F',
                    'multiple' => 'N',
                    'active' => 'Y',
                    'sort' => 300,
                ],
            ]
        );

        $galleryProperty = callRestMethod(
            'catalog.productProperty.add',
            [
                'fields' => [
                    'iblockId' => $iblockId,
                    'name' => 'Gallery',
                    'code' => 'GALLERY',
                    'propertyType' => 'F',
                    'multiple' => 'Y',
                    'active' => 'Y',
                    'sort' => 400,
                ],
            ]
        );

        $colorBlue = callRestMethod(
            'catalog.productPropertyEnum.add',
            [
                'fields' => [
                    'propertyId' => $colorProperty['productProperty']['id'],
                    'value' => 'Blue',
                    'xmlId' => 'BLUE',
                    'sort' => 100,
                ],
            ]
        );

        $sizeM = callRestMethod(
            'catalog.productPropertyEnum.add',
            [
                'fields' => [
                    'propertyId' => $sizeProperty['productProperty']['id'],
                    'value' => 'M',
                    'xmlId' => 'M',
                    'sort' => 100,
                ],
            ]
        );

        $sizeL = callRestMethod(
            'catalog.productPropertyEnum.add',
            [
                'fields' => [
                    'propertyId' => $sizeProperty['productProperty']['id'],
                    'value' => 'L',
                    'xmlId' => 'L',
                    'sort' => 200,
                ],
            ]
        );

        print_r([
            'colorPropertyId' => $colorProperty['productProperty']['id'],
            'colorBlueId' => $colorBlue['productPropertyEnum']['id'],
            'sizePropertyId' => $sizeProperty['productProperty']['id'],
            'sizeValueIds' => [
                $sizeM['productPropertyEnum']['id'],
                $sizeL['productPropertyEnum']['id'],
            ],
            'certificatePropertyId' => $certificateProperty['productProperty']['id'],
            'galleryPropertyId' => $galleryProperty['productProperty']['id'],
        ]);
    }
    catch (Throwable $exception)
    {
        echo 'Error: '.$exception->getMessage();
    }
    ?>
    ```

{% endlist %}

After completing the first step, save the property and list value identifiers. They will be required when creating the product.

```json
{
    "colorPropertyId": 431,
    "colorBlueId": 1739,
    "sizePropertyId": 432,
    "sizeValueIds": [
        1740,
        1741
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
    const iblockId = 23;
    const colorPropertyId = 431;
    const colorBlueId = 1739;
    const sizePropertyId = 432;
    const sizeValueIds = [1740, 1741];
    const certificatePropertyId = 433;
    const galleryPropertyId = 434;

    function fileToBase64(filePath) {
        return fetch(filePath)
            .then(response => response.blob())
            .then(blob => new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = () => resolve(reader.result.split(',')[1]);
                reader.onerror = reject;
                reader.readAsDataURL(blob);
            }));
    }

    async function addProduct() {
        const certificateBase64 = await fileToBase64('pictures/certificate.pdf');
        const galleryFirstBase64 = await fileToBase64('pictures/gallery-1.jpg');
        const gallerySecondBase64 = await fileToBase64('pictures/gallery-2.jpg');

        const fields = {
            iblockId: iblockId,
            name: 'Printed T-shirt',
            active: 'Y',
            sort: 100,
            ['property' + colorPropertyId]: colorBlueId,
            ['property' + sizePropertyId]: sizeValueIds,
            ['property' + certificatePropertyId]: {
                value: {
                    fileData: [
                        'certificate.pdf',
                        certificateBase64
                    ]
                }
            },
            ['property' + galleryPropertyId]: [
                {
                    value: {
                        fileData: [
                            'gallery-1.jpg',
                            galleryFirstBase64
                        ]
                    }
                },
                {
                    value: {
                        fileData: [
                            'gallery-2.jpg',
                            gallerySecondBase64
                        ]
                    }
                }
            ]
        };

        BX24.callMethod(
            'catalog.product.add',
            {
                fields: fields
            },
            function(result) {
                if (result.error()) {
                    console.error(result.error() + ': ' + result.error_description());
                    return;
                }

                const productId = Number(result.data().element.id);
                console.log('Product added: ' + productId);
            }
        );
    }

    addProduct().catch(console.error);
    ```

- PHP

    ```php
    <?php
    require_once('crest.php');

    $iblockId = 23;
    $colorPropertyId = 431;
    $colorBlueId = 1739;
    $sizePropertyId = 432;
    $sizeValueIds = [1740, 1741];
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

        $result = CRest::call(
            'catalog.product.add',
            [
                'fields' => $fields,
            ]
        );

        if (!empty($result['error']))
        {
            echo 'Error: '.$result['error_description'];
            return;
        }

        $productId = (int)$result['result']['element']['id'];
        echo 'Product added: '.$productId;
    }
    catch (Throwable $exception)
    {
        echo 'Error: '.$exception->getMessage();
    }
    ?>
    ```

{% endlist %}

If the product is added successfully, the method returns a `element` object. The response will contain the product fields and the custom property values.

```json
{
    "result": {
        "element": {
            "id": 1267,
            "iblockId": 23,
            "name": "Printed T-shirt",
            "property431": {
                "value": "1739",
                "valueId": "9816"
            },
            "property432": [
                {
                    "value": "1740",
                    "valueId": "9817"
                },
                {
                    "value": "1741",
                    "valueId": "9818"
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
    const productId = 1267;
    const catalogGroupId = 1;

    BX24.callMethod(
        'catalog.price.add',
        {
            fields: {
                productId: productId,
                catalogGroupId: catalogGroupId,
                price: 4900,
                currency: 'EUR'
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error() + ': ' + result.error_description());
                return;
            }

            console.log('Price added: ' + result.data().price.id);
        }
    );
    ```

- PHP

    ```php
    <?php
    require_once('crest.php');

    $productId = 1267;
    $catalogGroupId = 1;

    $result = CRest::call(
        'catalog.price.add',
        [
            'fields' => [
                'productId' => $productId,
                'catalogGroupId' => $catalogGroupId,
                'price' => 4900,
                'currency' => 'EUR',
            ],
        ]
    );

    if (!empty($result['error']))
    {
        echo 'Error: '.$result['error_description'];
    }
    else
    {
        echo 'Price added: '.$result['result']['price']['id'];
    }
    ?>
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
            "currency": "EUR"
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
- `Validate price error. Catalog price group is wrong` — an invalid price type was passed in `catalogGroupId`

## Continue Learning

- [{#T}](../../api-reference/catalog/product/catalog-product-add.md)
- [{#T}](../../api-reference/catalog/product/catalog-product-get.md)
- [{#T}](../../api-reference/catalog/product-property/catalog-product-property-add.md)
- [{#T}](../../api-reference/catalog/product-property-enum/catalog-product-property-enum-add.md)
- [{#T}](../../api-reference/catalog/price/catalog-price-add.md)
- [{#T}](../../api-reference/catalog/price/catalog-price-list.md)
- [{#T}](../../api-reference/catalog/product/catalog-product-list.md)
