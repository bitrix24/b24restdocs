# How to Retrieve a List of Vendors

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — permission to read contacts or companies in CRM
>
> - [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — a user with permission to read items of a CRM object
> - [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

There is no separate "vendor" object in CRM. Vendors are contacts and companies from a system pipeline with a special code:

- `CATALOG_CONTRACTOR_CONTACT` — for a contact

- `CATALOG_CONTRACTOR_COMPANY` — for a company

The identifier of this pipeline is different in every Bitrix24 and cannot be hardcoded as a constant. That is why we first request the identifier by the pipeline code and then filter the items by it.

As a result of the scenario, we get a list of vendors with their identifiers. These identifiers are accepted by the inventory management methods — for example, [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md) links a vendor to an inventory document.

The scenario consists of two steps.

1. Retrieve the `id` of the system vendor pipeline using the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method
2. Retrieve the items of that pipeline using the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method

## Before You Start

- The webhook is created on behalf of a user who has permission to read contacts and companies in CRM

- The `crm` scope is selected in the webhook permissions

- Inventory management is enabled in Bitrix24: the system vendor pipelines are created together with it

- The webhook user has access to the vendor pipeline. Step 1 returns only the pipelines the user is allowed to read, and step 2 — only the items visible to them

- The webhook URL grants full access within its scope. Retain the URL in an environment variable and never publish it in open code

- You have decided which records you retrieve: contacts or companies

#|
|| **What we pass** | **Contact** | **Company** ||
|| `entityTypeId` | `3` | `4` ||
|| System pipeline code | `CATALOG_CONTRACTOR_CONTACT` | `CATALOG_CONTRACTOR_COMPANY` ||
|| Name fields in `select` | `name` and `lastName` | `title` ||
|#

The examples below request contacts. What to replace for companies is described in the [Key Considerations](#company) section.

## 1. Retrieve the Vendor Pipeline ID

We will use the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with the following parameters:

- `entityTypeId` — the [CRM object type](../../../api-reference/crm/data-types.md#object_type) identifier, a required parameter. We will specify `3` — a contact

- `filter[code]` — a filter by pipeline code. We will specify `CATALOG_CONTRACTOR_CONTACT`. Without the filter, the method returns all contact pipelines, including the general one

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
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // crm.category.list has no wrapper in the SDK — we call the method directly
    $result = $serviceBuilder->core->call(
        'crm.category.list',
        [
            'entityTypeId' => 3, // 3 — a contact
            'filter' => [
                'code' => 'CATALOG_CONTRACTOR_CONTACT' // The system vendor pipeline code
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

    # the b24pysdk wrapper accepts only entity_type_id, so we select the pipeline by code in the response
    result = client.crm.category.list(
        entity_type_id=3,  # 3 — a contact
    ).response.result
    categories = [
        category
        for category in result.get("categories", [])
        if category.get("code") == "CATALOG_CONTRACTOR_CONTACT"
    ]
    print(categories)
    ```

{% endlist %}

As a result, we get the pipeline identifier. In the example, `id`: `15`. In your Bitrix24 the value will be different — do not carry `15` over into working code, request the identifier with this step.

```json
{
  "result": {
    "categories": [
      {
        "id": 15,
        "name": "Vendor Contacts",
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

The `isSystem`: `Y` flag confirms that the pipeline was created by the system, not by a user. Retain the `id` of the first item of the `categories` array — in step 2 it becomes the value of the `categoryId` filter.

{% note warning "" %}

Before calling step 2, check that the `categories` array is not empty. If `categoryId`: `null` is passed in the filter, the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method returns not vendors but the contacts of the general pipeline.

{% endnote %}

## 2. Retrieve the List of Vendors

We will filter the items using the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method with the following parameters:

- `entityTypeId` — the [CRM object type](../../../api-reference/crm/data-types.md#object_type) identifier, a required parameter. We will specify `3` — a contact

- `filter[categoryId]` — the system pipeline identifier from step 1. In the example, `15`

- `select` — a list of fields to return. We will specify `id`, `name`, `lastName`, and `categoryId`. The full set of object fields is returned by the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method with the same `entityTypeId`

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

As a result, we get a list of vendor contacts.

```json
{
  "result": {
    "items": [
      {
        "id": 2185,
        "name": "Stefan",
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

The `lastName` field can be empty: only the first name is required for a contact. When printing the list, join `name` and `lastName` with a space and trim the extra spaces.

## Verify the Result

The scenario is complete if the `categoryId` field of every item of the `items` array matches the `id` of the pipeline from step 1.

- The `total` field shows how many vendors were found in total. Per call, the method returns no more than 50 items. If `total` is greater than 50, the response holds only the first page — retrieve the rest with repeated calls using the `start` parameter: `50`, `100`, and so on

- In the interface, the same list opens in the CRM → Contacts section. Switch to the pipeline with the title from the `name` field of step 1 — by default it is "Vendor Contacts". The number of items in it matches `total`

If `total` equals zero, the method worked correctly and there are simply no vendors in Bitrix24. A vendor can be created following the [How to Create a Vendor in CRM](../how-to-add-crm-objects/how-to-add-contractor.md) scenario.

## Errors and Diagnostics

If the method returned an error, check the request data.

#|
|| **Code** | **Cause and Action** ||
|| `NOT_FOUND` | `entityTypeId` holds a value that matches no CRM object. Contacts require `3`, companies — `4` ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | In step 1, a CRM object that has no pipelines is passed. Vendors exist only for contacts and companies ||
|| `INVALID_ARG_VALUE` `Invalid filter: field 'field' is not allowed in filter` | In step 2, the `filter` holds a field that cannot be filtered by. The list of available fields is returned by the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method ||
|| `allowed_only_intranet_user` | The webhook is created on behalf of an external user. The scenario is available to Bitrix24 employees only ||
|#

An empty `categories` array in step 1 is not a method error. There are two reasons for it:

- Inventory management is not enabled in Bitrix24, so there are no system vendor pipelines

- The webhook user has no access to the vendor pipeline

To tell the reasons apart, run step 1 with an administrator webhook. If the administrator sees the pipeline and the original webhook does not, the cause is the permissions of its user. If the administrator does not see it either, inventory management is not enabled.

An empty `items` array in step 2 with a non-empty step 1 means that the pipeline has no items visible to the user. Check that `categoryId` is taken from the response of step 1 and not hardcoded as a number from another Bitrix24.

Both methods only read data, so after an error the scenario can be repeated from any step.

## Key Considerations {#company}

- To retrieve vendor companies, replace `entityTypeId` with `4`, the pipeline code with `CATALOG_CONTRACTOR_COMPANY`, and the `name` and `lastName` fields in `select` with `title`. `entityTypeId` and the pipeline code have to be changed together: with `entityTypeId`: `4`, the contact code returns an empty `categories` array

- Contacts and companies are different CRM objects and cannot be retrieved in a single call. To build a combined list of vendors, run the scenario twice and merge the results in your code

- The vendor pipeline cannot be created or deleted via REST: [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) forbids adding system pipelines, and [crm.category.delete](../../../api-reference/crm/universal/category/crm-category-delete.md) responds to a deletion with the `REMOVING_DISABLED` error

## Code Example

The code goes through both steps and prints the list of vendors. The only things to replace are the webhook URL and, for companies, the values of `entityTypeId` and the pipeline code in the first lines of the example.

{% list tabs %}

- JS
  
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const entityTypeId = 3; // 3 — a contact; for a company specify 4
    const categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // for a company specify CATALOG_CONTRACTOR_COMPANY

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
        const categories = resultCategory.getData().result.categories || [];
        if (!categories.length) {
            console.error('Vendor pipeline not found');
        } else {
            const categoryId = categories[0].id;

            const resultItems = await $b24.actions.v2.call.make({
                method: 'crm.item.list',
                params: {
                    entityTypeId: entityTypeId,
                    select: ['id', 'name', 'lastName', 'categoryId'],
                    filter: { categoryId: categoryId },
                    order: { id: 'DESC' }
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
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $entityTypeId = 3; // 3 — a contact; for a company specify 4
    $categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // for a company specify CATALOG_CONTRACTOR_COMPANY

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
            echo 'Vendor pipeline not found';
            return;
        }

        $categoryId = $categories[0]['id'];

        $resultItems = $serviceBuilder->getCRMScope()->item()->list(
            $entityTypeId,
            [
                'id' => 'DESC'
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

    entity_type_id = 3  # 3 — a contact; for a company specify 4

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
            print("Vendor pipeline not found")
        else:
            try:
                items_result = client.crm.item.list(
                    entity_type_id=entity_type_id,
                    select=["id", "name", "lastName", "categoryId"],
                    filter={"categoryId": categories[0]["id"]},
                    order={"id": "DESC"},
                ).response.result
            except BitrixAPIError as error:
                print(error)
            else:
                print(items_result)
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/crm-item-list.md)
- [{#T}](../../../api-reference/crm/universal/category/crm-category-list.md)
- [{#T}](../how-to-add-crm-objects/how-to-add-contractor.md)
- [{#T}](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md)
- [{#T}](./how-to-get-elements-by-stage-filter.md)
