# How to Create a Vendor in CRM

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to create contacts or companies in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Vendors are CRM contacts and companies marked with a system category:

- `CATALOG_CONTRACTOR_CONTACT` — for a contact,
- `CATALOG_CONTRACTOR_COMPANY` — for a company.

To create a vendor, we will sequentially execute two methods:

1. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) — retrieve the category identifier for a contact or a company.
2. [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) — create a contact or a company as a vendor.

## 1. Retrieve the Vendor Category ID

Use the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method with the following parameters:

- `entityTypeId` — the [CRM object type](../../../api-reference/crm/data-types.md#object_type) identifier. Specify `3` for contacts. For companies, use `4`.
- `filter[code]` — a filter by category code. Specify `CATALOG_CONTRACTOR_CONTACT` for a contact. For companies, use `CATALOG_CONTRACTOR_COMPANY`.

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

    client = Client(BitrixWebhook(domain="your-domain.bitrix24.com", webhook_token="user_id/webhook_key"))

    response = client.crm.category.list(
        entity_type_id=3,
    ).response
    categories = [
        category
        for category in response.result.get("categories", [])
        if category.get("code") == "CATALOG_CONTRACTOR_CONTACT"
    ]

    print(categories)
    ```

{% endlist %}

As a result, you will receive a category identifier. In the example `id`:`15`. The identifier may differ across different Bitrix24 instances.

```json
{
  "result": {
    "categories": [
      {
        "id": 15,
        "name": "Supplier contacts",
        "entityTypeId": 3,
        "isSystem": "Y",
        "code": "CATALOG_CONTRACTOR_CONTACT"
      }
    ]
  }
}
```

## 2. Create the Vendor

Use the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method with the following parameters:

- `entityTypeId` — the [CRM object type](../../../api-reference/crm/data-types.md#object_type) identifier. Specify `3` for contacts. For companies, use `4`.

- `fields[categoryId]` — the system category identifier from step 1. In the example, `15`.

- `fields[name]` — first name.
- `fields[lastName]` — last name. For a company, you can pass the `fields[title]` field — name instead of a first and last name.

- `fields[fm]` — an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) multipools for phone and email.
- `fields[comments]` — comment.
 
The system stores phone and email as a `fm` multipool array. Each item in the array contains:

- `typeId` — the multipool type, `PHONE` or `EMAIL`,
- `valueType` — the value type, such as `WORK` or `MOBILE`,
- `value` — the field value.

{% list tabs %}

- JS
  
    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.item.add',
        params: {
            entityTypeId: 3,
            fields: {
                name: 'Klaus',
                lastName: 'Weber',
                categoryId: 15,  // id from step 1
                fm: [
                    { typeId: 'PHONE', valueType: 'WORK', value: '+49 900 000 00 00' },
                    { typeId: 'PHONE', valueType: 'MOBILE', value: '+49 495 111 22 33' },
                    { typeId: 'EMAIL', valueType: 'WORK', value: 'supplier@example.com' }
                ],
                comments: 'Electronics supplier'
            }
        }
    });

    if (!result.isSuccess) {
        console.error(result.getErrorMessages().join('; '));
    } else {
        console.log(result.getData().result);
    }
    ```

- PHP
  
    ```php
    $result = $serviceBuilder->getCRMScope()->item()->add(
        3,
        [
            'name' => 'Klaus',
            'lastName' => 'Weber',
            'categoryId' => 15, // id from step 1
            'fm' => [
                [ 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => '+49 900 000 00 00' ],
                [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+49 495 111 22 33' ],
                [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'supplier@example.com' ]
            ],
            'comments' => 'Electronics supplier'
        ]
    );
    ```

- Python

    ```python
    response = client.crm.item.add(
        entity_type_id=3,
        fields={
            "name": "Klaus",
            "lastName": "Weber",
            "categoryId": 15,
            "fm": [
                {"typeId": "PHONE", "valueType": "WORK", "value": "+49 900 000 00 00"},
                {"typeId": "PHONE", "valueType": "MOBILE", "value": "+49 495 111 22 33"},
                {"typeId": "EMAIL", "valueType": "WORK", "value": "supplier@example.com"},
            ],
            "comments": "Electronics supplier",
        },
    ).response

    print(response.result)
    ```

{% endlist %}

As a result, the method will return a `item` object containing the data of the created vendor.

```json
{
    "result": {
        "item": {
            "id": 2449,
            "createdTime": "2025-12-29T13:18:40+03:00",
            "updatedTime": "2025-12-29T13:18:40+03:00",
            "createdBy": 1,
            "updatedBy": 1,
            "assignedById": 1,
            "opened": "Y",
            "companyId": null,
            "name": "Klaus",
            "lastName": "Weber",
            "secondName": null,
            "shortName": null,
            "photo": null,
            "post": null,
            "address": null,
            "comments": "Electronics supplier",
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
            "lastActivityTime": "2025-12-29T13:18:40+03:00",
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
                    "value": "+49 900 000 00 00",
                    "typeId": "PHONE"
                },
                {
                    "id": 8299,
                    "valueType": "MOBILE",
                    "value": "+49 495 111 22 33",
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
        "date_start": "2025-12-29T13:18:40+03:00",
        "date_finish": "2025-12-29T13:18:40+03:00",
        "operating_reset_at": 1767004120,
        "operating": 0.4402291774749756
    }
}
```

Use the vendor identifier, which is `id`: `2449` in the example, in the inventory management method [catalog.documentcontractor.add](../../../api-reference/catalog/documentcontractor/catalog-documentcontractor-add.md).

## Code Example

{% list tabs %}

- JS
  
    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    var entityTypeId = 3; // 3 - contact; for company specify 4
    var categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // for company specify CATALOG_CONTRACTOR_COMPANY
    var categoryId = null;

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
            console.error('Supplier category not found');
        } else {
            categoryId = categories[0].id;

            const resultItem = await $b24.actions.v2.call.make({
                method: 'crm.item.add',
                params: {
                    entityTypeId: entityTypeId,
                    fields: {
                        name: 'Klaus',
                        lastName: 'Weber',
                        categoryId: categoryId,
                        fm: [
                            { typeId: 'PHONE', valueType: 'WORK', value: '+49 900 000 00 00' },
                            { typeId: 'PHONE', valueType: 'MOBILE', value: '+49 495 111 22 33' },
                            { typeId: 'EMAIL', valueType: 'WORK', value: 'supplier@example.com' }
                        ],
                        comments: 'Electronics supplier'
                    }
                }
            });

            if (!resultItem.isSuccess) {
                console.error(resultItem.getErrorMessages().join('; '));
            } else {
                console.log(resultItem.getData().result);
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

    $entityTypeId = 3; // 3 - contact; for company specify 4
    $categoryCode = 'CATALOG_CONTRACTOR_CONTACT'; // for company specify CATALOG_CONTRACTOR_COMPANY

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
            echo 'Supplier category not found';
            return;
        }

        $categoryId = $categories[0]['id'];

        $resultItem = $serviceBuilder->getCRMScope()->item()->add(
            $entityTypeId,
            [
                'name' => 'Klaus',
                'lastName' => 'Weber',
                'categoryId' => $categoryId,
                'fm' => [
                    [ 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => '+49 900 000 00 00' ],
                    [ 'typeId' => 'PHONE', 'valueType' => 'MOBILE', 'value' => '+49 495 111 22 33' ],
                    [ 'typeId' => 'EMAIL', 'valueType' => 'WORK', 'value' => 'supplier@example.com' ]
                ],
                'comments' => 'Electronics supplier'
            ]
        );

        print_r($resultItem->item());
    } catch (\Throwable $e) {
        echo $e->getMessage();
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(BitrixWebhook(domain="your-domain.bitrix24.com", webhook_token="user_id/webhook_key"))

    entity_type_id = 3  # 3 - contact; for company use 4
    category_code = "CATALOG_CONTRACTOR_CONTACT"  # for company use CATALOG_CONTRACTOR_COMPANY

    try:
        category_response = client.crm.category.list(
            entity_type_id=entity_type_id,
        ).response
    except BitrixAPIError as error:
        print(error)
    else:
        categories = [
            category
            for category in category_response.result.get("categories", [])
            if category.get("code") == category_code
        ]
        if not categories:
            print("Supplier category not found")
        else:
            category_id = categories[0]["id"]

            try:
                item_response = client.crm.item.add(
                    entity_type_id=entity_type_id,
                    fields={
                        "name": "Klaus",
                        "lastName": "Weber",
                        "categoryId": category_id,
                        "fm": [
                            {"typeId": "PHONE", "valueType": "WORK", "value": "+49 900 000 00 00"},
                            {"typeId": "PHONE", "valueType": "MOBILE", "value": "+49 495 111 22 33"},
                            {"typeId": "EMAIL", "valueType": "WORK", "value": "supplier@example.com"},
                        ],
                        "comments": "Electronics supplier",
                    },
                ).response
            except BitrixAPIError as error:
                print(error)
            else:
                print(item_response.result)
    ```

{% endlist %}
