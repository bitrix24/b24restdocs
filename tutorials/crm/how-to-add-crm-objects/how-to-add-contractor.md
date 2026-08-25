# How to Create a Vendor in CRM

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, both permissions are required — to create and to read items of a CRM object
>
> - [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) — a user with permission to create items of a CRM object
> - [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — a user with permission to read items of a CRM object
> - [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A vendor in Bitrix24 is a CRM contact or company that belongs to a system category with a special code:

- `CATALOG_CONTRACTOR_CONTACT` — for a contact

- `CATALOG_CONTRACTOR_COMPANY` — for a company

There is no dedicated method for creating a vendor. A contact or company becomes a vendor when the identifier of the system category is passed in the `categoryId` field. This identifier cannot be hardcoded as a constant, so first retrieve it by the category code and then create the object.

As a result of the scenario, a contact appears in the vendor category, and the method returns its `id`. Inventory management methods require this identifier. For example, the [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md) method links a vendor to an inventory document.

The scenario consists of two steps.

1. Retrieve the `id` of the system vendor category using the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method
2. Create a contact or a company using the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method, passing this `id` in the `categoryId` field

## Before You Start

- The webhook is created on behalf of a user who has permission to create contacts and companies in CRM

- The `crm` scope is selected in the webhook permissions

- Inventory management is enabled in Bitrix24: the system vendor categories are created together with it

- The webhook user has access to the vendor category: step 1 returns only the categories visible to that user, and step 2 checks the permission to create items in that particular category

- The webhook URL grants full access within its scope. Retain the URL in an environment variable and never publish it in open code

- You have decided what you are creating: a contact or a company. The `entityTypeId`, the system category code, and the name fields depend on this choice

The values for a contact and a company differ — pick the column for your object.

#|
|| **What to pass** | **Contact** | **Company** ||
|| `entityTypeId` | `3` | `4` ||
|| System category code | `CATALOG_CONTRACTOR_CONTACT` | `CATALOG_CONTRACTOR_COMPANY` ||
|| Name fields | `name` and `lastName` | `title` ||
|#

The examples below create a contact. What to replace for a company is described in the [Key Considerations](#company) section.

## 1. Retrieve the Vendor Category ID {#category-id}

Use the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with the following parameters:

- `entityTypeId` — the [CRM object type](../../../api-reference/crm/data-types.md#object_type) identifier, a required parameter. Specify `3` — a contact

- `filter[code]` — a filter by category code. Specify `CATALOG_CONTRACTOR_CONTACT`. Without the filter, the method returns all contact categories, including the general one

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const resultCategory = await $b24.actions.v2.call.make({
        method: 'crm.category.list',
        params: {
            entityTypeId: 3, // 3 — contact
            filter: {
                code: 'CATALOG_CONTRACTOR_CONTACT' // Code of the system vendor category
            }
        },
        requestId: 'category-list'
    });

    const categories = resultCategory.getData().result.categories;
    const categoryId = categories.length ? categories[0].id : null;
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

    # the b24pysdk wrapper accepts only entity_type_id, so the category is selected by code in the response
    categories = client.crm.category.list(
        entity_type_id=3,  # 3 — contact
    ).response.result["categories"]

    contractor_categories = [
        category
        for category in categories
        if category["code"] == "CATALOG_CONTRACTOR_CONTACT"
    ]
    category_id = contractor_categories[0]["id"] if contractor_categories else None
    ```


- PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // crm.category.list has no wrapper in the SDK — calling the method directly
    $result = $sb->core->call(
        'crm.category.list',
        [
            'entityTypeId' => 3, // 3 — contact
            'filter' => [
                'code' => 'CATALOG_CONTRACTOR_CONTACT' // Code of the system vendor category
            ]
        ]
    );

    $categories = $result->getResponseData()->getResult()['categories'] ?? [];
    $categoryId = $categories[0]['id'] ?? null;
    ```
{% endlist %}

In the response, the method returns a `categories` array. Retain the `id` of the first element — it has to be passed to step 2. In the example, `id`: `15`.

```json
{
    "result": {
        "categories": [
            {
                "id": 15,
                "name": "Supplier contacts",
                "sort": 500,
                "entityTypeId": 3,
                "isDefault": "N",
                "isSystem": "Y",
                "code": "CATALOG_CONTRACTOR_CONTACT"
            }
        ]
    },
    "total": 1
}
```

The `isSystem` flag with the value `Y` confirms that the category was created by the system and not by a user.

{% note warning "" %}

Before calling step 2, make sure that the `categories` array is not empty. Calling the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method without `categoryId` does not return an error: the contact is created in the general category, but it does not appear in the vendor list.

{% endnote %}

## 2. Create the Vendor

Use the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method with the following parameters:

- `entityTypeId` — the [CRM object type](../../../api-reference/crm/data-types.md#object_type) identifier, a required parameter. Specify `3` — a contact

- `fields[categoryId]` — the category identifier from the [crm.category.list](#category-id) step, `15` in the example. This is the field that turns the contact into a vendor

- `fields[name]` and `fields[lastName]` — the first name and the last name of the contact

- `fields[fm]` — an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) multifields for phone numbers and email addresses

- `fields[comments]` — a comment in the item card

Bitrix24 retains phone numbers and email addresses as multifields rather than as separate fields. Each element of the `fm` array contains:

- `typeId` — the multifield type, `PHONE` or `EMAIL`

- `valueType` — the value type, such as `WORK` or `MOBILE`

- `value` — the value itself

{% list tabs %}

- JS

    ```javascript
    const resultItem = await $b24.actions.v2.call.make({
        method: 'crm.item.add',
        params: {
            entityTypeId: 3, // 3 — contact
            fields: {
                name: 'Klaus', // First name
                lastName: 'Weber', // Last name
                categoryId: categoryId, // Category identifier from step 1
                fm: [ // Phone numbers and email addresses
                    { typeId: 'PHONE', valueType: 'WORK', value: '+49 900 000 00 00' },
                    { typeId: 'PHONE', valueType: 'MOBILE', value: '+49 495 111 22 33' },
                    { typeId: 'EMAIL', valueType: 'WORK', value: 'supplier@example.com' }
                ],
                comments: 'Electronics supplier' // Comment
            }
        },
        requestId: 'item-add'
    });

    const contractorId = resultItem.getData().result.item.id;
    ```

- Python

    ```python
    item = client.crm.item.add(
        entity_type_id=3,  # 3 — contact
        fields={
            "name": "Klaus",  # First name
            "lastName": "Weber",  # Last name
            "categoryId": category_id,  # Category identifier from step 1
            "fm": [  # Phone numbers and email addresses
                {"typeId": "PHONE", "valueType": "WORK", "value": "+49 900 000 00 00"},
                {"typeId": "PHONE", "valueType": "MOBILE", "value": "+49 495 111 22 33"},
                {"typeId": "EMAIL", "valueType": "WORK", "value": "supplier@example.com"},
            ],
            "comments": "Electronics supplier",  # Comment
        },
    ).response.result["item"]

    contractor_id = item["id"]
    ```


- PHP

    ```php
    $result = $sb->getCRMScope()->item()->add(
        3, // 3 — contact
        [
            'name' => 'Klaus', // First name
            'lastName' => 'Weber', // Last name
            'categoryId' => $categoryId, // Category identifier from step 1
            'fm' => [ // Phone numbers and email addresses
                [ 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => '+49 900 000 00 00' ],
                [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+49 495 111 22 33' ],
                [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'supplier@example.com' ]
            ],
            'comments' => 'Electronics supplier' // Comment
        ]
    );

    $contractorId = $result->item()->id;
    ```
{% endlist %}

In the response, the method returns an `item` object with the full set of contact fields. The response is shortened, showing the fields that confirm the result.

```json
{
    "result": {
        "item": {
            "id": 2643,
            "entityTypeId": 3,
            "categoryId": 15,
            "name": "Klaus",
            "lastName": "Weber",
            "comments": "Electronics supplier",
            "hasPhone": "Y",
            "hasEmail": "Y",
            "createdTime": "2026-08-19T14:56:05+03:00",
            "createdBy": 1,
            "assignedById": 1,
            "fm": [
                {
                    "id": 8533,
                    "valueType": "WORK",
                    "value": "+49 900 000 00 00",
                    "typeId": "PHONE"
                },
                {
                    "id": 8535,
                    "valueType": "MOBILE",
                    "value": "+49 495 111 22 33",
                    "typeId": "PHONE"
                },
                {
                    "id": 8537,
                    "valueType": "WORK",
                    "value": "supplier@example.com",
                    "typeId": "EMAIL"
                }
            ]
        }
    }
}
```

Retain the `id`. In the example, `id`: `2643`.

## Verify the Result

Open the contact list in CRM and switch to the "Supplier contacts" category — its name came in the `name` field in step 1. The new contact "Klaus Weber" appears in this category with the phone numbers and the email address from the request. It is not present in the general contact category.

Through REST, vendors are returned by the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method with the following parameters:

- `entityTypeId` — `3` for contacts

- `filter[categoryId]` — the category identifier from step 1, `15` in the example

- `filter[id]` — the vendor identifier from step 2, `2643` in the example. Without it, the method returns the first page of the vendor list: if there are many vendors, the new item may not be on it

{% list tabs %}

- JS

    ```javascript
    const checkResult = await $b24.actions.v2.call.make({
        method: 'crm.item.list',
        params: {
            entityTypeId: 3,
            filter: { categoryId: categoryId, id: contractorId },
            select: ['id', 'name', 'lastName', 'categoryId']
        },
        requestId: 'item-list'
    });

    console.dir(checkResult.getData().result.items);
    ```

- Python

    ```python
    check_result = client.crm.item.list(
        3,
        filter={"categoryId": category_id, "id": contractor_id},
        select=["id", "name", "lastName", "categoryId"],
    ).response.result["items"]

    print(check_result)
    ```


- PHP

    ```php
    $checkResult = $sb->getCRMScope()->item()->list(
        3,
        [],
        ['categoryId' => $categoryId, 'id' => $contractorId],
        ['id', 'name', 'lastName', 'categoryId']
    );

    print_r($checkResult->getItems());
    ```
{% endlist %}

The scenario is complete if the `items` array contains an element with the `id` from step 2 and its `categoryId` matches the identifier of the system category.

```json
{
    "result": {
        "items": [
            {
                "id": 2643,
                "name": "Klaus",
                "lastName": "Weber",
                "categoryId": 15
            }
        ]
    }
}
```

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `ACCESS_DENIED` | The webhook user does not have permission to create items of the object with this `entityTypeId`. Check which user the webhook was created on behalf of ||
|| `allowed_only_intranet_user` | The webhook is created on behalf of an external user. The scenario is available only to Bitrix24 employees ||
|| `NOT_FOUND` | The value passed in `entityTypeId` does not match any CRM object. A contact requires `3`, a company requires `4` ||
|| `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid field value. There are two common reasons: an unsupported `typeId` or `valueType` in the `fm` multifield, or a `categoryId` that belongs to another object type. The text `Invalid value for field "Category"` means that `entityTypeId` and the category code do not match: for example, `entityTypeId`: `4` with a contact category ||
|| `100` | A non-array value was passed to a multiple field. Make sure that `fm` is passed as an array even if there is only one phone number ||
|#

Fields that the object does not have are not treated as an error — the method discards them. With an incorrect set of fields, the method does not refuse the request, and the contact or the company is created incomplete.

An empty `categories` array in step 1 is not a method error. There are two reasons: inventory management is not enabled in Bitrix24, so the system vendor categories do not exist, or the webhook user has no access to this category — the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method returns only the categories that the user is allowed to read.

To tell the reasons apart, run step 1 with an administrator webhook. If the administrator sees the category and the original webhook does not, the cause is the permissions of its user. If the administrator does not see it either, inventory management is not enabled.

Step 1 does not create anything, so it can be repeated any number of times. If step 2 returned the error, the vendor was not created: fix the `fields` and repeat only that step.

## Key Considerations {#company}

- To create a vendor company, replace `entityTypeId` with `4`, the category code with `CATALOG_CONTRACTOR_COMPANY`, and the `name` and `lastName` fields with `title`. The `entityTypeId` and the category code have to be changed together, otherwise the method returns the `CRM_FIELD_ERROR_VALUE_NOT_VALID` error

- If you switch to a company but keep the contact fields, the company is still created, yet it receives an automatic name such as "Company #3009". Pass the `title` yourself

- A vendor category cannot be created manually: the [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) method prohibits adding system categories

- The [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method does not check for duplicates. Running the example again creates a second vendor with the same data. Before creating one, search for the vendor by phone number or email address using the [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method

- The category identifier differs across Bitrix24 instances. Do not carry the value `15` from the example into production code, retrieve it in step 1

## Code Example

The script retrieves the identifier of the system vendor category and creates a contact in it. The `entityTypeId`, the category code, and the name fields are moved to variables — for a company, it is enough to change them in one place.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const entityTypeId = 3; // 3 — contact, for a company specify 4
    const categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // for a company specify CATALOG_CONTRACTOR_COMPANY
    const nameFields = { name: 'Klaus', lastName: 'Weber' }; // for a company specify { title: 'Elektronik GmbH' }

    async function createContractor() {
        try {
            const resultCategory = await $b24.actions.v2.call.make({
                method: 'crm.category.list',
                params: {
                    entityTypeId: entityTypeId,
                    filter: { code: categoryCode }
                },
                requestId: 'category-list'
            });

            const categories = resultCategory.getData().result.categories;
            if (!categories.length) {
                console.error('Vendor category not found: check inventory management and the access of the webhook user');
                return;
            }
            const categoryId = categories[0].id;

            const resultItem = await $b24.actions.v2.call.make({
                method: 'crm.item.add',
                params: {
                    entityTypeId: entityTypeId,
                    fields: {
                        ...nameFields,
                        categoryId: categoryId,
                        fm: [
                            { typeId: 'PHONE', valueType: 'WORK', value: '+49 900 000 00 00' },
                            { typeId: 'PHONE', valueType: 'MOBILE', value: '+49 495 111 22 33' },
                            { typeId: 'EMAIL', valueType: 'WORK', value: 'supplier@example.com' }
                        ],
                        comments: 'Electronics supplier'
                    }
                },
                requestId: 'item-add'
            });

            console.log('Vendor created, id:', resultItem.getData().result.item.id);
        } catch (error) {
            console.error('Vendor not created:', error.message);
        }
    }

    createContractor();
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

    entity_type_id = 3  # 3 — contact, for a company specify 4
    category_code = "CATALOG_CONTRACTOR_CONTACT"  # for a company specify CATALOG_CONTRACTOR_COMPANY
    name_fields = {"name": "Klaus", "lastName": "Weber"}  # for a company specify {"title": "Elektronik GmbH"}

    try:
        # the b24pysdk wrapper accepts only entity_type_id, so the category is selected by code in the response
        categories = client.crm.category.list(
            entity_type_id=entity_type_id,
        ).response.result["categories"]

        contractor_categories = [
            category
            for category in categories
            if category["code"] == category_code
        ]
        if not contractor_categories:
            print("Vendor category not found: check inventory management and the access of the webhook user")
        else:
            item = client.crm.item.add(
                entity_type_id,
                {
                    **name_fields,
                    "categoryId": contractor_categories[0]["id"],
                    "fm": [
                        {"typeId": "PHONE", "valueType": "WORK", "value": "+49 900 000 00 00"},
                        {"typeId": "PHONE", "valueType": "MOBILE", "value": "+49 495 111 22 33"},
                        {"typeId": "EMAIL", "valueType": "WORK", "value": "supplier@example.com"},
                    ],
                    "comments": "Electronics supplier",
                },
            ).response.result["item"]

            print(f"Vendor created, id: {item['id']}")
    except BitrixAPIError as error:
        print(f"Vendor not created: {error}")
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $entityTypeId = 3; // 3 — contact, for a company specify 4
    $categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // for a company specify CATALOG_CONTRACTOR_COMPANY
    $nameFields = ['name' => 'Klaus', 'lastName' => 'Weber']; // for a company specify ['title' => 'Elektronik GmbH']

    try {
        // crm.category.list has no wrapper in the SDK — calling the method directly
        $resultCategory = $sb->core->call(
            'crm.category.list',
            [
                'entityTypeId' => $entityTypeId,
                'filter' => ['code' => $categoryCode]
            ]
        );

        $categories = $resultCategory->getResponseData()->getResult()['categories'] ?? [];
        if (empty($categories)) {
            echo 'Vendor category not found: check inventory management and the access of the webhook user';
            return;
        }
        $categoryId = $categories[0]['id'];

        $resultItem = $sb->getCRMScope()->item()->add(
            $entityTypeId,
            array_merge(
                $nameFields,
                [
                    'categoryId' => $categoryId,
                    'fm' => [
                        [ 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => '+49 900 000 00 00' ],
                        [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+49 495 111 22 33' ],
                        [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'supplier@example.com' ]
                    ],
                    'comments' => 'Electronics supplier'
                ]
            )
        );

        echo 'Vendor created, id: ' . $resultItem->item()->id;
    } catch (\Throwable $e) {
        echo 'Vendor not created: ' . $e->getMessage();
    }
    ```
{% endlist %}

## Continue Learning

- [{#T}](../how-to-get-lists/how-to-get-contractors.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-add.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-list.md)
- [{#T}](../../../api-reference/crm/universal/category/crm-category-list.md)
- [{#T}](../../../api-reference/crm/data-types.md)
- [{#T}](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md)
- [{#T}](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-list.md)
