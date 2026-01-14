# How to Create a Vendor in CRM

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create contacts or companies in CRM

Vendors are CRM contacts and companies with a system category designation:

- `CATALOG_CONTRACTOR_CONTACT` — for contacts,
- `CATALOG_CONTRACTOR_COMPANY` — for companies.

To create a vendor, we will sequentially execute two methods:

1. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — retrieve the category ID for the contact or company.
2. [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) — create a contact or company as a vendor.

## 1. Retrieve the Vendor Category ID

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

As a result, we will obtain the category ID. In the example, `id`:`15`. The ID may vary in different Bitrix24 instances.

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

## 2. Create the Vendor

We will use the method [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) with the following parameters:

- `entityTypeId` — the ID of the [CRM object type](../../../api-reference/crm/data-types.md#object_type). Use `3` for contacts. For companies, use `4`.

- `fields[categoryId]` — the ID of the system category from step 1. In the example, `15`.

- `fields[name]` — first name.
- `fields[lastName]` — last name. For companies, instead of first and last names, you can provide the `fields[title]` field — the name.

- `fields[fm]` — an array of multi-fields [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) for phone and email.
- `fields[comments]` — comments.
 
The system stores phone and email as an array of multi-fields `fm`. Each element of the array contains:

- `typeId` — the type of multi-field, `PHONE` or `EMAIL`,
- `valueType` — the type of value, for example, `WORK` or `MOBILE`,
- `value` — the field value.

{% list tabs %}

- JS
  
    ```javascript
    BX24.callMethod(
        'crm.item.add',
        {
            entityTypeId: 3, 
            fields: {
                name: 'John',
                lastName: 'Doe',
                categoryId: 15,  // id from step 1
                fm: [
                    { typeId: 'PHONE', valueType: 'WORK', value: '+1 900 000 00 00' },
                    { typeId: 'PHONE', valueType: 'MOBILE', value: '+1 495 111 22 33' },
                    { typeId: 'EMAIL', valueType: 'WORK', value: 'supplier@example.com' }
                ],
                comments: 'Electronics vendor'
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error() + ': ' + result.error_description());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP
  
    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.add',
        [
            'entityTypeId' => 3,
            'fields' => [
                'name' => 'John',
                'lastName' => 'Doe',
                'categoryId' => 15, // id from step 1
                'fm' => [
                    [ 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => '+1 900 000 00 00' ],
                    [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+1 495 111 22 33' ],
                    [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'supplier@example.com' ]
                ],
                'comments' => 'Electronics vendor'
            ]
        ]
    );
    ```

{% endlist %}

As a result, the method will return an `item` object with the data of the created vendor.

```json
{
    "result": {
        "item": {
            "id": 2449,
            "createdTime": "2025-12-29T13:18:40+01:00",
            "updatedTime": "2025-12-29T13:18:40+01:00",
            "createdBy": 1,
            "updatedBy": 1,
            "assignedById": 1,
            "opened": "Y",
            "companyId": null,
            "name": "John",
            "lastName": "Doe",
            "secondName": null,
            "shortName": null,
            "photo": null,
            "post": null,
            "address": null,
            "comments": "Electronics vendor",
            "leadId": null,
            "export": "Y",
            "webformId": null,
            "originatorId": null,
            "originId": null,
            "originVersion": null,
            "birthdate": null,
            "honorific": null,
            "hasPhone": "Y",
            "hasEmail": "Y",
            "hasImol": "N",
            "searchContent": null,
            "categoryId": 15,
            "lastActivityBy": 1,
            "lastActivityTime": "2025-12-29T13:18:40+01:00",
            "login": null,
            "emailHome": null,
            "emailWork": null,
            "emailMailing": null,
            "phoneMobile": null,
            "phoneWork": null,
            "phoneMailing": null,
            "imol": null,
            "email": null,
            "phone": null,
            "lastCommunicationTime": null,
            "lastCommunicationCallTime": null,
            "lastCommunicationEmailTime": null,
            "lastCommunicationImolTime": null,
            "lastCommunicationWebformTime": null,
            "observers": [],
            "companyIds": [],
            "entityTypeId": 3,
            "fm": [
                {
                    "id": 8297,
                    "valueType": "WORK",
                    "value": "+1 900 000 00 00",
                    "typeId": "PHONE"
                },
                {
                    "id": 8299,
                    "valueType": "MOBILE",
                    "value": "+1 495 111 22 33",
                    "typeId": "PHONE"
                },
                {
                    "id": 8301,
                    "valueType": "WORK",
                    "value": "supplier@example.com",
                    "typeId": "EMAIL"
                }
            ]
        }
    },
    "time": {
        "start": 1767003520,
        "finish": 1767003520.776535,
        "duration": 0.7765350341796875,
        "processing": 0,
        "date_start": "2025-12-29T13:18:40+01:00",
        "date_finish": "2025-12-29T13:18:40+01:00",
        "operating_reset_at": 1767004120,
        "operating": 0.4402291774749756
    }
}
```

The vendor ID, in the example `id`: `2449`, should be used in the inventory accounting method [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md).

## Code Example

{% list tabs %}

- JS
  
    ```javascript
    var entityTypeId = 3; // 3 - contact; for company use 4
    var categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // for company use CATALOG_CONTRACTOR_COMPANY
    var categoryId = null;

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

            categoryId = categories[0].id;

            BX24.callMethod(
                'crm.item.add',
                {
                    entityTypeId: entityTypeId,
                    fields: {
                        name: 'John',
                        lastName: 'Doe',
                        categoryId: categoryId,
                        fm: [
                            { typeId: 'PHONE', valueType: 'WORK', value: '+1 900 000 00 00' },
                            { typeId: 'PHONE', valueType: 'MOBILE', value: '+1 495 111 22 33' },
                            { typeId: 'EMAIL', valueType: 'WORK', value: 'supplier@example.com' }
                        ],
                        comments: 'Electronics vendor'
                    }
                },
                function(resultItem) {
                    if (resultItem.error()) {
                        console.error(resultItem.error() + ': ' + resultItem.error_description());
                    } else {
                        console.log(resultItem.data());
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

    $resultItem = CRest::call(
        'crm.item.add',
        [
            'entityTypeId' => $entityTypeId,
            'fields' => [
                'name' => 'John',
                'lastName' => 'Doe',
                'categoryId' => $categoryId,
                'fm' => [
                    [ 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => '+1 900 000 00 00' ],
                    [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+1 495 111 22 33' ],
                    [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'supplier@example.com' ]
                ],
                'comments' => 'Electronics vendor'
            ]
        ]
    );

    if (!empty($resultItem['error_description'])) {
        echo $resultItem['error_description'];
    } else {
        print_r($resultItem['result']);
    }
    ```

{% endlist %}