# How to Get a List of Vendors

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to view contacts or companies in CRM

Vendors are contacts and companies in CRM that have a system category designation:

- `CATALOG_CONTRACTOR_CONTACT` — for contacts,
- `CATALOG_CONTRACTOR_COMPANY` — for companies.

To obtain a list of vendors, we will sequentially execute two methods:

1. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — to get the category ID for the contact or company.
2. [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — to retrieve the list of vendors based on a filter.

## 1. Get the Vendor Category ID

We will use the method [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) with the following parameters:

- `entityTypeId` — the ID of the [CRM object type](../../../api-reference/crm/data-types.md#object_type). Use `3` for contacts. For companies, use `4`.
- `filter[code]` — filter by category code. Use `CATALOG_CONTRACTOR_CONTACT` for contacts. For companies, use `CATALOG_CONTRACTOR_COMPANY`.

{% include [Example Notes](../../../_includes/examples.md) %}

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

## 2. Get the List of Vendors

We will filter the items using the method [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) with the following parameters:

- `entityTypeId` — the ID of the [CRM object type](../../../api-reference/crm/data-types.md#object_type). Use `3` for contacts. For companies, use `4`.

- `select` — list of fields to output. All available fields can be obtained using the method [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md).

- `filter[categoryId]` - the ID of the system category from step 1. In the example, `15`.

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
        "name": "John",
        "lastName": "Doe",
        "categoryId": 15
      }
    ]
  },
  "total": 2
}
```

The vendor IDs, in the example `id`: `2185` and `id`: `2443`, should be used in the inventory method [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md).

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

{% endlist %}