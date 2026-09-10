# How to Work with the Binding to Information Block Elements Field

> Scope: [`crm`, `lists`, `catalog`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: the methods require permissions from several modules, all permissions listed below are required
>
> - [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) — a CRM administrator
> - [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) and [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) — a user with permission to modify and read deals
> - [lists.get](../../../api-reference/lists/lists/lists-get.md) and [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md) — a user with "Read" access permission for the list
> - [catalog.catalog.list](../../../api-reference/catalog/catalog/catalog-catalog-list.md) and [catalog.product.get](../../../api-reference/catalog/product/catalog-product-get.md) — an administrator
> - [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md) — a user with permission to view the product catalog and read the trade catalog information block

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Binding to Information Block Elements field stores identifiers of information block elements: list items or catalog products. In method responses, the field appears as the number `7007` or the array `[533, 541]`. Element names are not included, so retrieve them with a separate call.

The field is bound to a single information block through the `IBLOCK_ID` setting. The information block type is not stored in the settings, so retrieve the identifier in advance: use the `lists.*` methods for lists and the `catalog.*` methods for products.

Let us walk through the scenario with deals. Create two fields: a single-value field that references a list item and a multiple field that references catalog products. Fill them in a specific deal, read them back, and expand the identifiers into element names.

The scenario consists of five steps.

1. Find the information block using the [lists.get](../../../api-reference/lists/lists/lists-get.md) and [catalog.catalog.list](../../../api-reference/catalog/catalog/catalog-catalog-list.md) methods
2. Create the binding fields using the [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method
3. Retrieve the element identifiers using the [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md) and [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md) methods
4. Write the values using the [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) method
5. Expand the values into names using the [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md), [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md), and [catalog.product.get](../../../api-reference/catalog/product/catalog-product-get.md) methods

As a result, both fields in the deal are filled in, and the stored identifiers let you retrieve the names of the list item and the products.

## Before You Start

Prepare the scenario data:

- **The information block to bind to.** This is a Bitrix24 list or a product catalog. You will retrieve its identifier in the first step
- **The deal where the fields will be filled in.** You will need its `id`. The fields themselves are created for all deals at once, not for a single one
- **REST access.** A webhook or an application with the `crm`, `lists`, and `catalog` scopes. Only a CRM administrator can create fields

A webhook executes requests with the permissions of the user who created it. If that user has no access to the list or the catalog, the methods return an access error even though the calls themselves are correct.

The examples below use the list with identifier `123`, the product catalog with identifier `25`, and deal `8415`. In your Bitrix24, these values will be different: take the information block identifiers from the responses of the first step and the deal identifier from your own deal.

For server-side JS examples with `B24Hook`, Node.js 18, 20, 22 or newer is required. For new projects, use version 22 or later. B24JsSDK is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`. For examples with b24pysdk, Python 3.9 or newer is required.

Store the webhook URL in an environment variable and do not publish it in open code.

{% include [Example Note](../../../_includes/examples.md) %}

## 1. Find the Information Block and Its Identifier

Lists and product catalogs are information blocks of different types, and they are retrieved with different methods.

The [lists.get](../../../api-reference/lists/lists/lists-get.md) method returns lists of a single type. Pass the parameter:

- `IBLOCK_TYPE_ID` — information block type. `lists` stands for regular lists, `bitrix_processes` for workflow lists

The [catalog.catalog.list](../../../api-reference/catalog/catalog/catalog-catalog-list.md) method returns trade catalogs and takes no parameters.

Save the following from the responses:

- `ID` of the list — passed to the `IBLOCK_ID` setting of the single-value field
- `iblockId` of the catalog — passed to the `IBLOCK_ID` setting of the multiple field

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // The SDK has no typed wrappers for the list and catalog methods, so they are called through the SDK core
    async function callMethod(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({
            method,
            params,
            requestId
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    const lists = await callMethod(
        'lists.get',
        { IBLOCK_TYPE_ID: 'lists' },
        'lists-get'
    )

    const catalogs = await callMethod(
        'catalog.catalog.list',
        {},
        'catalog-catalog-list'
    )

    console.table(lists.map((list) => ({ ID: list.ID, NAME: list.NAME })))
    console.table(catalogs.catalogs)
    ```

- PHP

    ```php
    <?php

    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Psr\Log\NullLogger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // The SDK has no typed wrappers for the list and catalog methods, so they are called through the SDK core
    function callMethod($serviceBuilder, string $method, array $params = []): mixed
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    $lists = callMethod($serviceBuilder, 'lists.get', ['IBLOCK_TYPE_ID' => 'lists']);
    $catalogs = callMethod($serviceBuilder, 'catalog.catalog.list');

    print_r($lists);
    print_r($catalogs['catalogs']);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    bitrix_token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )
    # B24_HOOK_TOKEN = 'USER_ID/TOKEN'

    def call_method(method, params=None):
        # The SDK has no typed wrappers for the list and catalog methods, so they are called directly
        return bitrix_token.call_method(
            api_method=method,
            params=params or {},
        )["result"]

    lists = call_method("lists.get", {"IBLOCK_TYPE_ID": "lists"})
    catalogs = call_method("catalog.catalog.list")

    print(lists)
    print(catalogs["catalogs"])
    ```

{% endlist %}

Abbreviated response of [lists.get](../../../api-reference/lists/lists/lists-get.md):

```json
{
    "result": [
        {
            "ID": "123",
            "IBLOCK_TYPE_ID": "lists",
            "NAME": "Updated task list",
            "ACTIVE": "Y"
        }
    ],
    "total": 1
}
```

Abbreviated response of [catalog.catalog.list](../../../api-reference/catalog/catalog/catalog-catalog-list.md):

```json
{
    "result": {
        "catalogs": [
            {
                "id": 25,
                "iblockId": 25,
                "iblockTypeId": "CRM_PRODUCT_CATALOG",
                "name": "CRM Product Catalog",
                "productIblockId": null
            },
            {
                "id": 27,
                "iblockId": 27,
                "iblockTypeId": "CRM_PRODUCT_CATALOG",
                "name": "CRM Product Catalog (offers)",
                "productIblockId": 25
            }
        ]
    },
    "total": 2
}
```

A catalog with a non-null `productIblockId` is the trade offers information block. A binding to it stores product variations rather than the products themselves. For this scenario, take the catalog with `productIblockId: null`, which is `25` in the example.

## 2. Create the Binding Fields

The [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method creates a custom field for all deals. Pass the parameters:

- `FIELD_NAME` — field code. The parameter is required. If the code does not start with `UF_CRM_`, the prefix is added automatically: `MY_FIELD` becomes `UF_CRM_MY_FIELD`
- `USER_TYPE_ID` — field type, `iblock_element` for the binding to information block elements
- `MULTIPLE` — `Y` for several values, `N` for one
- `EDIT_FORM_LABEL` — field title in the deal card, specified by language
- `SETTINGS.IBLOCK_ID` — information block identifier from the first step. Without it, the method returns an error
- `SETTINGS.DISPLAY` — type of the control in the deal card: `UI`, `DIALOG`, `LIST`, or `CHECKBOX`

The full list of field types is returned by the [crm.userfield.types](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md) method. For the binding to information block sections, there is a separate type, `iblock_section`.

Save the identifiers of the created fields from the response: they let you read the settings with the [crm.deal.userfield.get](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-get.md) method.

{% list tabs %}

- JS

    ```js
    const listFieldId = await callMethod(
        'crm.deal.userfield.add',
        {
            fields: {
                FIELD_NAME: 'UF_CRM_IB_LIST',
                USER_TYPE_ID: 'iblock_element',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'List element' },
                SETTINGS: { IBLOCK_ID: 123, DISPLAY: 'UI' }
            }
        },
        'userfield-add-list'
    )

    const productFieldId = await callMethod(
        'crm.deal.userfield.add',
        {
            fields: {
                FIELD_NAME: 'UF_CRM_IB_PROD',
                USER_TYPE_ID: 'iblock_element',
                MULTIPLE: 'Y',
                EDIT_FORM_LABEL: { en: 'Catalog products' },
                SETTINGS: { IBLOCK_ID: 25, DISPLAY: 'UI' }
            }
        },
        'userfield-add-product'
    )

    console.log(listFieldId, productFieldId)
    ```

- PHP

    ```php
    $listFieldId = callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_IB_LIST',
            'USER_TYPE_ID' => 'iblock_element',
            'MULTIPLE' => 'N',
            'EDIT_FORM_LABEL' => ['en' => 'List element'],
            'SETTINGS' => ['IBLOCK_ID' => 123, 'DISPLAY' => 'UI'],
        ],
    ]);

    $productFieldId = callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_IB_PROD',
            'USER_TYPE_ID' => 'iblock_element',
            'MULTIPLE' => 'Y',
            'EDIT_FORM_LABEL' => ['en' => 'Catalog products'],
            'SETTINGS' => ['IBLOCK_ID' => 25, 'DISPLAY' => 'UI'],
        ],
    ]);

    print_r([$listFieldId, $productFieldId]);
    ```

- Python

    ```python
    list_field_id = call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_IB_LIST",
                "USER_TYPE_ID": "iblock_element",
                "MULTIPLE": "N",
                "EDIT_FORM_LABEL": {"en": "List element"},
                "SETTINGS": {"IBLOCK_ID": 123, "DISPLAY": "UI"},
            },
        },
    )

    product_field_id = call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_IB_PROD",
                "USER_TYPE_ID": "iblock_element",
                "MULTIPLE": "Y",
                "EDIT_FORM_LABEL": {"en": "Catalog products"},
                "SETTINGS": {"IBLOCK_ID": 25, "DISPLAY": "UI"},
            },
        },
    )

    print(list_field_id, product_field_id)
    ```

{% endlist %}

The response contains the field identifier:

```json
{
    "result": 6007761
}
```

The field settings can be read with the [crm.deal.userfield.get](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-get.md) method. Abbreviated response:

```json
{
    "result": {
        "ID": "6007761",
        "ENTITY_ID": "CRM_DEAL",
        "FIELD_NAME": "UF_CRM_IB_LIST",
        "USER_TYPE_ID": "iblock_element",
        "MULTIPLE": "N",
        "SETTINGS": {
            "DISPLAY": "UI",
            "LIST_HEIGHT": 1,
            "IBLOCK_ID": 123,
            "DEFAULT_VALUE": "",
            "ACTIVE_FILTER": "N"
        }
    }
}
```

Only five settings are stored. The information block type is not among them: the field knows only `IBLOCK_ID`. The `FIELD_NAME` value is needed in the fourth step, because values are written under that key.

## 3. Retrieve the Element Identifiers

List items are returned by the [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md) method. Pass the parameters:

- `IBLOCK_TYPE_ID` — information block type, the same as in the first step
- `IBLOCK_ID` — list identifier
- `ELEMENT_ID` — identifier of a single item, if you need a specific one

Catalog products are returned by the [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md) method. Pass the parameters:

- `filter.iblockId` — catalog identifier, otherwise the response includes products of all catalogs
- `select` — product fields, `id`, `iblockId`, and `name` are enough for this scenario
- `start` — pagination offset

Save the `ID` of the list item and the `id` values of the products from the responses: they are written to the fields in the next step.

{% list tabs %}

- JS

    ```js
    const listElements = await callMethod(
        'lists.element.get',
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 123
        },
        'lists-element-get'
    )

    const products = await callMethod(
        'catalog.product.list',
        {
            select: ['id', 'iblockId', 'name'],
            filter: { iblockId: 25 },
            start: 0
        },
        'catalog-product-list'
    )

    const elementId = Number(listElements[0].ID)
    const productIds = products.products.slice(0, 2).map((product) => product.id)

    console.log(elementId, productIds)
    ```

- PHP

    ```php
    $listElements = callMethod($serviceBuilder, 'lists.element.get', [
        'IBLOCK_TYPE_ID' => 'lists',
        'IBLOCK_ID' => 123,
    ]);

    $products = callMethod($serviceBuilder, 'catalog.product.list', [
        'select' => ['id', 'iblockId', 'name'],
        'filter' => ['iblockId' => 25],
        'start' => 0,
    ]);

    $elementId = (int)$listElements[0]['ID'];
    $productIds = array_map(
        static fn(array $product): int => (int)$product['id'],
        array_slice($products['products'], 0, 2)
    );

    print_r([$elementId, $productIds]);
    ```

- Python

    ```python
    list_elements = call_method(
        "lists.element.get",
        {
            "IBLOCK_TYPE_ID": "lists",
            "IBLOCK_ID": 123,
        },
    )

    products = call_method(
        "catalog.product.list",
        {
            "select": ["id", "iblockId", "name"],
            "filter": {"iblockId": 25},
            "start": 0,
        },
    )

    element_id = int(list_elements[0]["ID"])
    product_ids = [product["id"] for product in products["products"][:2]]

    print(element_id, product_ids)
    ```

{% endlist %}

Abbreviated response of [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md):

```json
{
    "result": [
        {
            "ID": "7007",
            "IBLOCK_ID": "123",
            "NAME": "Item for field checks",
            "IBLOCK_SECTION_ID": null
        }
    ],
    "total": 1
}
```

Abbreviated response of [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md):

```json
{
    "result": {
        "products": [
            { "id": 533, "iblockId": 25, "name": "Test item" },
            { "id": 541, "iblockId": 25, "name": "Simple product" }
        ]
    },
    "total": 2
}
```

You have the list item identifier `7007` and the product identifiers `533` and `541`.

## 4. Write the Values

The [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) method writes values to the deal fields. Pass the parameters:

- `id` — deal identifier
- `fields` — object with field codes. Pass a number to the single-value field and an array of numbers to the multiple field

Bitrix24 does not validate the identifiers you pass: the method accepts a nonexistent element as well as an element of another information block and returns `true`. Check the elements yourself before writing. Look up the list item with the [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md) method by `ELEMENT_ID`: an empty array in the response means the item is not in that list. Retrieve the product with the [catalog.product.get](../../../api-reference/catalog/product/catalog-product-get.md) method and compare its `iblockId` with the information block of the field.

{% list tabs %}

- JS

    ```js
    async function isListElementValid(iblockTypeId, iblockId, id) {
        const found = await callMethod(
            'lists.element.get',
            {
                IBLOCK_TYPE_ID: iblockTypeId,
                IBLOCK_ID: iblockId,
                ELEMENT_ID: id
            },
            `lists-element-check-${id}`
        )

        return found.length > 0
    }

    async function isProductValid(iblockId, id) {
        try {
            const found = await callMethod(
                'catalog.product.get',
                { id },
                `catalog-product-check-${id}`
            )

            return found.product.iblockId === iblockId
        } catch (error) {
            return false
        }
    }

    if (!(await isListElementValid('lists', 123, elementId))) {
        throw new Error(`Item ${elementId} is not in list 123`)
    }

    const validProductIds = []

    for (const productId of productIds) {
        if (await isProductValid(25, productId)) {
            validProductIds.push(productId)
        }
    }

    await callMethod(
        'crm.deal.update',
        {
            id: 8415,
            fields: {
                UF_CRM_IB_LIST: elementId,
                UF_CRM_IB_PROD: validProductIds
            }
        },
        'deal-update-bindings'
    )
    ```

- PHP

    ```php
    function isListElementValid($serviceBuilder, string $iblockTypeId, int $iblockId, int $id): bool
    {
        $found = callMethod($serviceBuilder, 'lists.element.get', [
            'IBLOCK_TYPE_ID' => $iblockTypeId,
            'IBLOCK_ID' => $iblockId,
            'ELEMENT_ID' => $id,
        ]);

        return $found !== [];
    }

    function isProductValid($serviceBuilder, int $iblockId, int $id): bool
    {
        try {
            $found = callMethod($serviceBuilder, 'catalog.product.get', ['id' => $id]);
        } catch (Throwable $error) {
            return false;
        }

        return (int)$found['product']['iblockId'] === $iblockId;
    }

    if (!isListElementValid($serviceBuilder, 'lists', 123, $elementId)) {
        throw new RuntimeException('Item ' . $elementId . ' is not in list 123');
    }

    $validProductIds = [];

    foreach ($productIds as $productId) {
        if (isProductValid($serviceBuilder, 25, $productId)) {
            $validProductIds[] = $productId;
        }
    }

    callMethod($serviceBuilder, 'crm.deal.update', [
        'id' => 8415,
        'fields' => [
            'UF_CRM_IB_LIST' => $elementId,
            'UF_CRM_IB_PROD' => $validProductIds,
        ],
    ]);
    ```

- Python

    ```python
    def is_list_element_valid(iblock_type_id, iblock_id, element):
        found = call_method(
            "lists.element.get",
            {
                "IBLOCK_TYPE_ID": iblock_type_id,
                "IBLOCK_ID": iblock_id,
                "ELEMENT_ID": element,
            },
        )

        return len(found) > 0

    def is_product_valid(iblock_id, product):
        try:
            found = call_method("catalog.product.get", {"id": product})
        except Exception:
            return False

        return found["product"]["iblockId"] == iblock_id

    if not is_list_element_valid("lists", 123, element_id):
        raise RuntimeError(f"Item {element_id} is not in list 123")

    valid_product_ids = [
        product_id for product_id in product_ids if is_product_valid(25, product_id)
    ]

    call_method(
        "crm.deal.update",
        {
            "id": 8415,
            "fields": {
                "UF_CRM_IB_LIST": element_id,
                "UF_CRM_IB_PROD": valid_product_ids,
            },
        },
    )
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": true
}
```

The `true` value confirms that the deal has been updated but says nothing about the correctness of the bindings. The values themselves can only be checked by reading them in the next step.

## 5. Expand the Values into Names

The [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) method returns the deal with all custom fields. A single-value field comes as a string, a multiple field as an array of numbers.

Element names are not stored in the deal. To retrieve them, request the list items with the [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md) method and the products with the [catalog.product.get](../../../api-reference/catalog/product/catalog-product-get.md) method, one identifier per call.

{% list tabs %}

- JS

    ```js
    const deal = await callMethod('crm.deal.get', { id: 8415 }, 'deal-get')

    const boundElementId = Number(deal.UF_CRM_IB_LIST)
    const boundProductIds = deal.UF_CRM_IB_PROD ?? []

    const elements = boundElementId > 0
        ? await callMethod(
            'lists.element.get',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 123,
                ELEMENT_ID: boundElementId
            },
            'lists-element-resolve'
        )
        : []

    const boundProducts = []

    for (const productId of boundProductIds) {
        const found = await callMethod(
            'catalog.product.get',
            { id: productId },
            `catalog-product-resolve-${productId}`
        )

        boundProducts.push({ id: found.product.id, name: found.product.name })
    }

    console.log(elements[0]?.NAME)
    console.table(boundProducts)
    ```

- PHP

    ```php
    $deal = callMethod($serviceBuilder, 'crm.deal.get', ['id' => 8415]);

    $boundElementId = (int)$deal['UF_CRM_IB_LIST'];
    $boundProductIds = $deal['UF_CRM_IB_PROD'] ?? [];

    $elements = $boundElementId > 0
        ? callMethod($serviceBuilder, 'lists.element.get', [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 123,
            'ELEMENT_ID' => $boundElementId,
        ])
        : [];

    $boundProducts = [];

    foreach ($boundProductIds as $productId) {
        $found = callMethod($serviceBuilder, 'catalog.product.get', ['id' => (int)$productId]);

        $boundProducts[] = [
            'id' => $found['product']['id'],
            'name' => $found['product']['name'],
        ];
    }

    print_r($elements[0]['NAME'] ?? null);
    print_r($boundProducts);
    ```

- Python

    ```python
    deal = call_method("crm.deal.get", {"id": 8415})

    bound_element_id = int(deal["UF_CRM_IB_LIST"] or 0)
    bound_product_ids = deal.get("UF_CRM_IB_PROD") or []

    elements = (
        call_method(
            "lists.element.get",
            {
                "IBLOCK_TYPE_ID": "lists",
                "IBLOCK_ID": 123,
                "ELEMENT_ID": bound_element_id,
            },
        )
        if bound_element_id > 0
        else []
    )

    bound_products = []

    for product_id in bound_product_ids:
        found = call_method("catalog.product.get", {"id": product_id})
        bound_products.append(
            {"id": found["product"]["id"], "name": found["product"]["name"]}
        )

    print(elements[0]["NAME"] if elements else None)
    print(bound_products)
    ```

{% endlist %}

Abbreviated response of [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md):

```json
{
    "result": {
        "ID": "8415",
        "TITLE": "Information block binding check",
        "UF_CRM_IB_LIST": "7007",
        "UF_CRM_IB_PROD": [533, 541]
    }
}
```

Abbreviated response of [catalog.product.get](../../../api-reference/catalog/product/catalog-product-get.md):

```json
{
    "result": {
        "product": {
            "id": 533,
            "iblockId": 25,
            "name": "Test item"
        }
    }
}
```

The `iblockId` field in the product response is the same identifier that is specified in the `IBLOCK_ID` setting of the field. A match confirms that the product belongs to the required catalog.

## Verify the Result

The scenario is complete if both fields are filled in after reading the deal and an element is found for every identifier.

What to check in the responses:

- `UF_CRM_IB_LIST` contains a string with the item identifier rather than `"0"` or an empty string
- `UF_CRM_IB_PROD` contains an array of product identifiers
- [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md) with that `ELEMENT_ID` returned one item rather than an empty array
- [catalog.product.get](../../../api-reference/catalog/product/catalog-product-get.md) returned the product and its `iblockId` matches the `IBLOCK_ID` of the field

Open the deal card in the interface: the List Element and Catalog Products fields show the element names. An empty field in the card together with a non-empty value in the response means that the stored element is not in the bound information block.

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code or Error Text** | **Reason and Action** ||
|| `The 'FIELD_NAME' field is not found.` | The field code is not passed to [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md). Pass `FIELD_NAME` ||
|| `ERROR_CORE`, `Select the information block to link the field to` | The field settings contain no `SETTINGS.IBLOCK_ID`. Pass the information block identifier from the first step ||
|| `ACCESS_DENIED`, `You do not have permission to view or edit the list.` | [lists.get](../../../api-reference/lists/lists/lists-get.md) is called with an information block type that is not a list, for example `catalog`. For products, use [catalog.catalog.list](../../../api-reference/catalog/catalog/catalog-catalog-list.md) ||
|| `product does not exist.` | [catalog.product.get](../../../api-reference/catalog/product/catalog-product-get.md) is called with the identifier of a nonexistent product. Retrieve the identifiers with the [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md) method ||
|#

If there was no error but the binding does not work, check the stored value with the [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) method.

- The value `"0"` means that a string was passed to the field instead of a number. A non-numeric value is cast to zero, and the method returns no error
- The value `"1"` in a single-value field means that an array was passed to it. A single-value field accepts only a number, and an array is cast to one rather than to its first element
- There is a value but the card is empty: the stored identifier belongs to a nonexistent element or to an element of another information block. Check the element with the [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md) or [catalog.product.get](../../../api-reference/catalog/product/catalog-product-get.md) method and write the value again
- The field is created but the card offers nothing to select: `SETTINGS.IBLOCK_ID` points to a nonexistent information block. Such a field is created without an error. Check the setting with the [crm.deal.userfield.get](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-get.md) method
- The card shows a control other than the one you specified: an unknown value was passed to `SETTINGS.DISPLAY` and was replaced with `UI`

To clear the binding, pass an empty string to the field. Running the scenario again overwrites the values and creates no duplicates.

## Key Points

- The field is bound to a single information block. The information block type is not stored in the settings, only `IBLOCK_ID` is
- Bitrix24 does not check that the element exists and belongs to the bound information block. Validating the identifiers is the task of your integration
- A single-value field is returned as a string, a multiple field as an array of numbers. Take this into account when parsing the response
- The trade offers information block is a separate catalog with a non-null `productIblockId`. A binding to it stores product variations rather than products
- [catalog.product.list](../../../api-reference/catalog/product/catalog-product-list.md) and [lists.element.get](../../../api-reference/lists/elements/lists-element-get.md) return elements in pages of 50. To iterate through all of them, increase `start`
- For other CRM objects, fields are created with the methods of the same name, for example [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md), and in a smart process with the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method
- For the binding to information block sections, use the `iblock_section` field type covered in the [{#T}](./how-to-use-iblock-section-binding-field.md) tutorial

## Code Example

The complete scenario in a single script: it finds the information blocks, creates both fields, checks the elements, writes the values, and expands them into names.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const LIST_IBLOCK_TYPE = 'lists'
    const DEAL_ID = 8415

    // The SDK has no typed wrappers for the list and catalog methods, so they are called through the SDK core
    async function callMethod(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    async function main() {
        const lists = await callMethod('lists.get', { IBLOCK_TYPE_ID: LIST_IBLOCK_TYPE }, 'lists-get')
        const catalogs = await callMethod('catalog.catalog.list', {}, 'catalog-catalog-list')

        const listIblockId = Number(lists[0].ID)
        const catalog = catalogs.catalogs.find((item) => item.productIblockId === null)
        const catalogIblockId = catalog.iblockId

        await callMethod('crm.deal.userfield.add', {
            fields: {
                FIELD_NAME: 'UF_CRM_IB_LIST',
                USER_TYPE_ID: 'iblock_element',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'List element' },
                SETTINGS: { IBLOCK_ID: listIblockId, DISPLAY: 'UI' }
            }
        }, 'userfield-add-list')

        await callMethod('crm.deal.userfield.add', {
            fields: {
                FIELD_NAME: 'UF_CRM_IB_PROD',
                USER_TYPE_ID: 'iblock_element',
                MULTIPLE: 'Y',
                EDIT_FORM_LABEL: { en: 'Catalog products' },
                SETTINGS: { IBLOCK_ID: catalogIblockId, DISPLAY: 'UI' }
            }
        }, 'userfield-add-product')

        const listElements = await callMethod('lists.element.get', {
            IBLOCK_TYPE_ID: LIST_IBLOCK_TYPE,
            IBLOCK_ID: listIblockId
        }, 'lists-element-get')

        const products = await callMethod('catalog.product.list', {
            select: ['id', 'iblockId', 'name'],
            filter: { iblockId: catalogIblockId },
            start: 0
        }, 'catalog-product-list')

        const elementId = Number(listElements[0].ID)
        const productIds = products.products.slice(0, 2).map((product) => product.id)

        const validProductIds = []

        for (const productId of productIds) {
            const found = await callMethod('catalog.product.get', { id: productId }, `catalog-product-check-${productId}`)

            if (found.product.iblockId === catalogIblockId) {
                validProductIds.push(productId)
            }
        }

        await callMethod('crm.deal.update', {
            id: DEAL_ID,
            fields: {
                UF_CRM_IB_LIST: elementId,
                UF_CRM_IB_PROD: validProductIds
            }
        }, 'deal-update-bindings')

        const deal = await callMethod('crm.deal.get', { id: DEAL_ID }, 'deal-get')

        const boundElements = await callMethod('lists.element.get', {
            IBLOCK_TYPE_ID: LIST_IBLOCK_TYPE,
            IBLOCK_ID: listIblockId,
            ELEMENT_ID: Number(deal.UF_CRM_IB_LIST)
        }, 'lists-element-resolve')

        const boundProducts = []

        for (const productId of deal.UF_CRM_IB_PROD ?? []) {
            const found = await callMethod('catalog.product.get', { id: productId }, `catalog-product-resolve-${productId}`)

            boundProducts.push({ id: found.product.id, name: found.product.name })
        }

        console.log(boundElements[0]?.NAME)
        console.table(boundProducts)
    }

    main().catch((error) => console.error(error.message))
    ```

- PHP

    ```php
    <?php

    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Psr\Log\NullLogger;
    use Symfony\Component\EventDispatcher\EventDispatcher;

    const LIST_IBLOCK_TYPE = 'lists';
    const DEAL_ID = 8415;

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // The SDK has no typed wrappers for the list and catalog methods, so they are called through the SDK core
    function callMethod($serviceBuilder, string $method, array $params = []): mixed
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    $lists = callMethod($serviceBuilder, 'lists.get', ['IBLOCK_TYPE_ID' => LIST_IBLOCK_TYPE]);
    $catalogs = callMethod($serviceBuilder, 'catalog.catalog.list');

    $listIblockId = (int)$lists[0]['ID'];
    $catalogIblockId = 0;

    foreach ($catalogs['catalogs'] as $catalog) {
        if ($catalog['productIblockId'] === null) {
            $catalogIblockId = (int)$catalog['iblockId'];
            break;
        }
    }

    callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_IB_LIST',
            'USER_TYPE_ID' => 'iblock_element',
            'MULTIPLE' => 'N',
            'EDIT_FORM_LABEL' => ['en' => 'List element'],
            'SETTINGS' => ['IBLOCK_ID' => $listIblockId, 'DISPLAY' => 'UI'],
        ],
    ]);

    callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_IB_PROD',
            'USER_TYPE_ID' => 'iblock_element',
            'MULTIPLE' => 'Y',
            'EDIT_FORM_LABEL' => ['en' => 'Catalog products'],
            'SETTINGS' => ['IBLOCK_ID' => $catalogIblockId, 'DISPLAY' => 'UI'],
        ],
    ]);

    $listElements = callMethod($serviceBuilder, 'lists.element.get', [
        'IBLOCK_TYPE_ID' => LIST_IBLOCK_TYPE,
        'IBLOCK_ID' => $listIblockId,
    ]);

    $products = callMethod($serviceBuilder, 'catalog.product.list', [
        'select' => ['id', 'iblockId', 'name'],
        'filter' => ['iblockId' => $catalogIblockId],
        'start' => 0,
    ]);

    $elementId = (int)$listElements[0]['ID'];
    $validProductIds = [];

    foreach (array_slice($products['products'], 0, 2) as $product) {
        $found = callMethod($serviceBuilder, 'catalog.product.get', ['id' => (int)$product['id']]);

        if ((int)$found['product']['iblockId'] === $catalogIblockId) {
            $validProductIds[] = (int)$product['id'];
        }
    }

    callMethod($serviceBuilder, 'crm.deal.update', [
        'id' => DEAL_ID,
        'fields' => [
            'UF_CRM_IB_LIST' => $elementId,
            'UF_CRM_IB_PROD' => $validProductIds,
        ],
    ]);

    $deal = callMethod($serviceBuilder, 'crm.deal.get', ['id' => DEAL_ID]);

    $boundElements = callMethod($serviceBuilder, 'lists.element.get', [
        'IBLOCK_TYPE_ID' => LIST_IBLOCK_TYPE,
        'IBLOCK_ID' => $listIblockId,
        'ELEMENT_ID' => (int)$deal['UF_CRM_IB_LIST'],
    ]);

    $boundProducts = [];

    foreach ($deal['UF_CRM_IB_PROD'] ?? [] as $productId) {
        $found = callMethod($serviceBuilder, 'catalog.product.get', ['id' => (int)$productId]);

        $boundProducts[] = [
            'id' => $found['product']['id'],
            'name' => $found['product']['name'],
        ];
    }

    print_r($boundElements[0]['NAME'] ?? null);
    print_r($boundProducts);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    LIST_IBLOCK_TYPE = "lists"
    DEAL_ID = 8415

    bitrix_token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )
    # B24_HOOK_TOKEN = 'USER_ID/TOKEN'

    def call_method(method, params=None):
        # The SDK has no typed wrappers for the list and catalog methods, so they are called directly
        return bitrix_token.call_method(
            api_method=method,
            params=params or {},
        )["result"]

    lists = call_method("lists.get", {"IBLOCK_TYPE_ID": LIST_IBLOCK_TYPE})
    catalogs = call_method("catalog.catalog.list")

    list_iblock_id = int(lists[0]["ID"])
    catalog_iblock_id = next(
        catalog["iblockId"]
        for catalog in catalogs["catalogs"]
        if catalog["productIblockId"] is None
    )

    call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_IB_LIST",
                "USER_TYPE_ID": "iblock_element",
                "MULTIPLE": "N",
                "EDIT_FORM_LABEL": {"en": "List element"},
                "SETTINGS": {"IBLOCK_ID": list_iblock_id, "DISPLAY": "UI"},
            },
        },
    )

    call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_IB_PROD",
                "USER_TYPE_ID": "iblock_element",
                "MULTIPLE": "Y",
                "EDIT_FORM_LABEL": {"en": "Catalog products"},
                "SETTINGS": {"IBLOCK_ID": catalog_iblock_id, "DISPLAY": "UI"},
            },
        },
    )

    list_elements = call_method(
        "lists.element.get",
        {"IBLOCK_TYPE_ID": LIST_IBLOCK_TYPE, "IBLOCK_ID": list_iblock_id},
    )

    products = call_method(
        "catalog.product.list",
        {
            "select": ["id", "iblockId", "name"],
            "filter": {"iblockId": catalog_iblock_id},
            "start": 0,
        },
    )

    element_id = int(list_elements[0]["ID"])
    valid_product_ids = []

    for product in products["products"][:2]:
        found = call_method("catalog.product.get", {"id": product["id"]})

        if found["product"]["iblockId"] == catalog_iblock_id:
            valid_product_ids.append(product["id"])

    call_method(
        "crm.deal.update",
        {
            "id": DEAL_ID,
            "fields": {
                "UF_CRM_IB_LIST": element_id,
                "UF_CRM_IB_PROD": valid_product_ids,
            },
        },
    )

    deal = call_method("crm.deal.get", {"id": DEAL_ID})

    bound_elements = call_method(
        "lists.element.get",
        {
            "IBLOCK_TYPE_ID": LIST_IBLOCK_TYPE,
            "IBLOCK_ID": list_iblock_id,
            "ELEMENT_ID": int(deal["UF_CRM_IB_LIST"] or 0),
        },
    )

    bound_products = []

    for product_id in deal.get("UF_CRM_IB_PROD") or []:
        found = call_method("catalog.product.get", {"id": product_id})
        bound_products.append(
            {"id": found["product"]["id"], "name": found["product"]["name"]}
        )

    print(bound_elements[0]["NAME"] if bound_elements else None)
    print(bound_products)
    ```

{% endlist %}

## Continue Learning

- [{#T}](./how-to-use-iblock-section-binding-field.md)
- [{#T}](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md)
- [{#T}](../../../api-reference/lists/lists/lists-get.md)
- [{#T}](../../../api-reference/lists/elements/lists-element-get.md)
- [{#T}](../../../api-reference/catalog/catalog/catalog-catalog-list.md)
- [{#T}](../../../api-reference/catalog/product/catalog-product-list.md)
- [{#T}](../../../api-reference/crm/deals/crm-deal-update.md)
