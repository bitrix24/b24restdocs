# How to Retrieve a List of Vendors

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with access permission to view contacts or companies in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Vendors are contacts and companies in CRM marked with a system category:

- `CATALOG_CONTRACTOR_CONTACT` — for contacts,
- `CATALOG_CONTRACTOR_COMPANY` — for companies.

To retrieve a list of vendors, we will sequentially execute two methods:

1. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — to obtain the category ID for the contact or company.
2. [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — to get the list of vendors based on the filter.

## 1. Retrieve the Vendor Category ID

We will use the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with the following parameters:

- `entityTypeId` — the [CRM object type](../../../api-reference/crm/data-types.md#object_type) identifier. Specify `3` for contacts. For companies, use `4`.
- `filter[code]` — a filter by category code. Specify `CATALOG_CONTRACTOR_CONTACT` for contacts. For companies, use `CATALOG_CONTRACTOR_COMPANY`.

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: 'crm.category.list',
        params: {
            entityTypeId: 3,
            filter: {
                code: 'CATALOG_CONTRACTOR_CONTACT'
            }
        }
    });

    console.log(result.getData().result);
    ```

- PHP
  
    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $serviceBuilder->core->call(
        'crm.category.list',
        [
            'entityTypeId' => 3,
            'filter' => [
                'code' => 'CATALOG_CONTRACTOR_CONTACT'
            ]
        ]
    );
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    result = client.crm.category.list(
        entity_type_id=3,
    ).response.result
    categories = [
        category
        for category in result.get("categories", [])
        if category.get("code") == "CATALOG_CONTRACTOR_CONTACT"
    ]
    print(categories)
    ```

{% endlist %}

As a result, we will obtain the category ID. In the example, `id`:`15`. The ID may vary across different Bitrix24 instances.

```json
{
  "result": {
    "categories": [
      {
        "id": 15,
        "name": "Lieferantenkontakte",
        "entityTypeId": 3,
        "isSystem": "Y",
        "code": "CATALOG_CONTRACTOR_CONTACT"
      }
    ]
  }
}
```

## 2. Retrieve the List of Vendors

We will filter the items using the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method with the following parameters:

- `entityTypeId` — the [CRM object type](../../../api-reference/crm/data-types.md#object_type) identifier. Specify `3` for contacts. For companies, use `4`.

- `select` — a list of fields to return. All available fields can be retrieved using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method.

- `filter[categoryId]` — the system category identifier from step 1. In the example, `15`.

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.item.list',
        params: {
            entityTypeId: 3,
            select: ['id', 'name', 'lastName', 'categoryId'],
            filter: {
                categoryId: 15
            }
        }
    });

    console.log(result.getData().result);
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getCRMScope()->item()->list(
        3,
        [],
        [
            'categoryId' => 15
        ],
        ['id', 'name', 'lastName', 'categoryId']
    );
    ```

- Python

    ```python
    result = client.crm.item.list(
        entity_type_id=3,
        select=["id", "name", "lastName", "categoryId"],
        filter={
            "categoryId": 15,
        },
    ).response.result
    ```

{% endlist %}

As a result, you will receive a list of contacts that are vendors.

```json
{
  "result": {
    "items": [
      {
        "id": 2185,
        "name": "Er",
        "lastName": null,
        "categoryId": 15
      },
      {
        "id": 2443,
        "name": "Klaus",
        "lastName": "Weber",
        "categoryId": 15
      }
    ]
  },
  "total": 2
}
```

Use the vendor identifiers from the example, `id`: `2185` and `id`: `2443`, in the inventory management method [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md).

## Code Example

{% list tabs %}

- JS
  
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    var entityTypeId = 3; // 3 - Kontakt; für das Unternehmen geben Sie 4 an
    var categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // für das Unternehmen geben Sie CATALOG_CONTRACTOR_COMPANY an

    const resultCategory = await $b24.actions.v2.call.make({
        method: 'crm.category.list',
        params: {
            entityTypeId: entityTypeId,
            filter: { code: categoryCode }
        }
    });

    if (!resultCategory.isSuccess) {
        console.error(resultCategory.getErrorMessages().join('; '));
    } else {
        var categories = resultCategory.getData().result.categories || [];
        if (!categories.length) {
            console.error('Lieferantenkategorie nicht gefunden');
        } else {
            var categoryId = categories[0].id;

            const resultItems = await $b24.actions.v2.call.make({
                method: 'crm.item.list',
                params: {
                    entityTypeId: entityTypeId,
                    select: ['id', 'name', 'lastName', 'categoryId'],
                    filter: { categoryId: categoryId },
                    order: { ID: 'DESC' }
                }
            });

            if (!resultItems.isSuccess) {
                console.error(resultItems.getErrorMessages().join('; '));
            } else {
                console.log(resultItems.getData().result);
            }
        }
    }
    ```

- PHP
  
    ```php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $entityTypeId = 3; // 3 - Kontakt; für das Unternehmen geben Sie 4 an
    $categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // für das Unternehmen geben Sie CATALOG_CONTRACTOR_COMPANY an

    try {
        $resultCategory = $serviceBuilder->core->call(
            'crm.category.list',
            [
                'entityTypeId' => $entityTypeId,
                'filter' => [
                    'code' => $categoryCode
                ]
            ]
        );

        $categories = $resultCategory->getResponseData()->getResult()['categories'] ?? [];
        if (empty($categories)) {
            echo 'Lieferantenkategorie nicht gefunden';
            return;
        }

        $categoryId = $categories[0]['id'];

        $resultItems = $serviceBuilder->getCRMScope()->item()->list(
            $entityTypeId,
            [
                'ID' => 'DESC'
            ],
            [
                'categoryId' => $categoryId
            ],
            ['id', 'name', 'lastName', 'categoryId']
        );

        print_r($resultItems->getItems());
    } catch (\Throwable $e) {
        echo $e->getMessage();
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

    entity_type_id = 3

    category_code = (
        "CATALOG_CONTRACTOR_CONTACT"
        if entity_type_id == 3
        else "CATALOG_CONTRACTOR_COMPANY"
    )

    try:
        categories_response = client.crm.category.list(
            entity_type_id=entity_type_id,
        ).response.result.get("categories", [])
        categories = [
            category
            for category in categories_response
            if category.get("code") == category_code
        ]
    except BitrixAPIError as error:
        print(error)
    else:
        if not categories:
            print("Lieferantenkategorie nicht gefunden")
        else:
            try:
                items_result = client.crm.item.list(
                    entity_type_id=entity_type_id,
                    select=["id", "name", "lastName", "categoryId"],
                    filter={"categoryId": categories[0]["id"]},
                    order={"ID": "DESC"},
                ).response.result
            except BitrixAPIError as error:
                print(error)
            else:
                print(items_result)
    ```

{% endlist %}
