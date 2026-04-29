# How to Retrieve a List of Vendors

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with access permission to view contacts or companies in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Vendors are contacts and companies in CRM marked with a system category:

- `CATALOG_CONTRACTOR_CONTACT` — for contacts,
- `CATALOG_CONTRACTOR_COMPANY` — for companies.

To retrieve a list of vendors, we will sequentially execute two methods:

1. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — to obtain the category ID for the contact or company.
2. [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — to get the list of vendors based on the filter.

## 1. Retrieve the Vendor Category ID

We will use the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with the following parameters:

- `entityTypeId` — the ID of the [CRM object type](../../../api-reference/crm/data-types.md#object_type). Use `3` for contacts. For companies, use `4`.
- `filter[code]` — filter by category code. Use `CATALOG_CONTRACTOR_CONTACT` for contacts. For companies, use `CATALOG_CONTRACTOR_COMPANY`.

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.category.list',
        {
            entityTypeId: 3,
            filter: {
                code: 'CATALOG_CONTRACTOR_CONTACT'
            }
        },
        function(result) {
            console.log(result.data());
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
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
            auth_token="your-webhook-token",
        )
    )

    result = client.crm.category.list(
        entity_type_id=3,
        filter={
            "code": "CATALOG_CONTRACTOR_CONTACT",
        },
    ).response.result
    ```

{% endlist %}

As a result, we will obtain the category ID. In the example, `id`:`15`. The ID may vary across different Bitrix24 instances.

```json
{
  "result": {
    "categories": [
      {
        "id": 15,
        "name": "Vendor Contacts",
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

- `entityTypeId` — the ID of the [CRM object type](../../../api-reference/crm/data-types.md#object_type). Use `3` for contacts. For companies, use `4`.

- `select` — a list of fields to output. All available fields can be retrieved using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method.

- `filter[categoryId]` - the ID of the system category from step 1. In this example, `15`.

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.item.list',
        {
            entityTypeId: 3,
            select: ['id', 'name', 'lastName', 'categoryId'],
            filter: {
                categoryId: 15
            }
        },
        function(result) {
            console.log(result.data());
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => 3,
            'select' => ['id', 'name', 'lastName', 'categoryId'],
            'filter' => [
                'categoryId' => 15
            ]
        ]
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

As a result, we will obtain a list of contacts that are vendors.

```json
{
  "result": {
    "items": [
      {
        "id": 2185,
        "name": "He",
        "lastName": null,
        "categoryId": 15
      },
      {
        "id": 2443,
        "name": "Ivan",
        "lastName": "Ivanov",
        "categoryId": 15
      }
    ]
  },
  "total": 2
}
```

The vendor IDs, in this example `id`: `2185` and `id`: `2443`, should be used in the inventory method [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md).

## Code Example

{% list tabs %}

- JS
  
    ```javascript
    var entityTypeId = 3; // 3 - contact; for company use 4
    var categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // for company use CATALOG_CONTRACTOR_COMPANY

    BX24.callMethod(
        'crm.category.list',
        {
            entityTypeId: entityTypeId,
            filter: { code: categoryCode }
        },
        function(resultCategory) {
            if (resultCategory.error()) {
                console.error(resultCategory.error() + ': ' + resultCategory.error_description());
                return;
            }

            var categories = resultCategory.data().categories || [];
            if (!categories.length) {
                console.error('Vendor category not found');
                return;
            }

            var categoryId = categories[0].id;

            BX24.callMethod(
                'crm.item.list',
                {
                    entityTypeId: entityTypeId,
                    select: ['id', 'name', 'lastName', 'categoryId'],
                    filter: { categoryId: categoryId },
                    order: { ID: 'DESC' }
                },
                function(resultItems) {
                    if (resultItems.error()) {
                        console.error(resultItems.error() + ': ' + resultItems.error_description());
                    } else {
                        console.log(resultItems.data());
                    }
                }
            );
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $entityTypeId = 3; // 3 - contact; for company use 4
    $categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // for company use CATALOG_CONTRACTOR_COMPANY

    $resultCategory = CRest::call(
        'crm.category.list',
        [
            'entityTypeId' => $entityTypeId,
            'filter' => [
                'code' => $categoryCode
            ]
        ]
    );

    if (!empty($resultCategory['error_description'])) {
        echo $resultCategory['error_description'];
        return;
    }

    $categories = $resultCategory['result']['categories'] ?? [];
    if (empty($categories)) {
        echo 'Vendor category not found';
        return;
    }

    $categoryId = $categories[0]['id'];

    $resultItems = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => $entityTypeId,
            'select' => ['id', 'name', 'lastName', 'categoryId'],
            'filter' => [
                'categoryId' => $categoryId
            ],
            'order' => [
                'ID' => 'DESC'
            ]
        ]
    );

    if (!empty($resultItems['error_description'])) {
        echo $resultItems['error_description'];
    } else {
        print_r($resultItems['result']);
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            auth_token="your-webhook-token",
        )
    )

    entity_type_id = 3

    category_code = (
        "CATALOG_CONTRACTOR_CONTACT"
        if entity_type_id == 3
        else "CATALOG_CONTRACTOR_COMPANY"
    )

    try:
        categories = client.crm.category.list(
            entity_type_id=entity_type_id,
            filter={"code": category_code},
        ).response.result.get("categories", [])
    except BitrixAPIError as error:
        print(error)
    else:
        if not categories:
            print("Vendor category not found")
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