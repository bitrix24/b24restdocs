# How to Work with the Binding to Information Block Sections Field

> Scope: [`crm`, `lists`, `catalog`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: the methods require permissions from several modules, all permissions listed below are required
>
> - [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) — a CRM administrator
> - [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) and [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) — a user with permission to modify and read deals
> - [lists.get](../../../api-reference/lists/lists/lists-get.md) and [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md) — a user with "Read" access permission for the list
> - [lists.section.add](../../../api-reference/lists/sections/lists-section-add.md) — a user with "Edit" access permission for the required list
> - [catalog.catalog.list](../../../api-reference/catalog/catalog/catalog-catalog-list.md) and [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) — an administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The Binding to Information Block Sections field stores section identifiers: list folders or product catalog sections. In method responses, the field appears as the number `237` or the array `[19, 33]`, without section names.

A section is a node of a tree. It has a parent and a nesting level, and the binding stores only the node itself: nested sections are not included in the value. If you need the child sections as well, collect them yourself by the parent identifier.

The field is bound to a single information block through the `IBLOCK_ID` setting, so retrieve the information block identifier in advance: use the `lists.*` methods for lists and the `catalog.*` methods for the catalog.

Let us walk through the scenario with deals. Create two fields: a single-value field that references a list section and a multiple field that references catalog sections. Fill them in a specific deal, read them back, and expand the identifiers into section names.

The scenario consists of five steps.

1. Find the information block using the [lists.get](../../../api-reference/lists/lists/lists-get.md) and [catalog.catalog.list](../../../api-reference/catalog/catalog/catalog-catalog-list.md) methods
2. Create the binding fields using the [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method
3. Retrieve the section identifiers using the [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md) and [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) methods
4. Write the values using the [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) method
5. Expand the values into names using the [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md), [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md), and [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) methods

As a result, both fields in the deal are filled in, and the stored identifiers let you retrieve the section names.

## Before You Start

Prepare the scenario data:

- **The information block to bind to.** This is a Bitrix24 list or a product catalog. You will retrieve its identifier in the first step
- **Sections in that information block.** A list may have none: in that case [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md) returns an empty array and the sections have to be created
- **The deal where the fields will be filled in.** You will need its `id`. The fields themselves are created for all deals at once, not for a single one
- **REST access.** A webhook or an application with the `crm`, `lists`, and `catalog` scopes. Only a CRM administrator can create fields, and catalog sections are also available to an administrator only

A webhook executes requests with the permissions of the user who created it. If that user has no access to the list or the catalog, the methods return an access error even though the calls themselves are correct.

The examples below use the list with identifier `123`, the product catalog with identifier `25`, and deal `8419`. In your Bitrix24, these values will be different: take the information block identifiers from the responses of the first step and the deal identifier from your own deal.

For server-side JS examples with `B24Hook`, Node.js 18, 20, 22 or newer is required. For new projects, use version 22 or later. B24JsSDK is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`. For examples with b24pysdk, Python 3.9 or newer is required.

Store the webhook URL in an environment variable and do not publish it in open code.

{% include [Example Note](../../../_includes/examples.md) %}

## 1. Find the Information Block and Its Identifier

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

A catalog with a non-null `productIblockId` is the trade offers information block. For this scenario, take the catalog with `productIblockId: null`, which is `25` in the example.

## 2. Create the Binding Fields

The [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md) method creates a custom field for all deals. Pass the parameters:

- `FIELD_NAME` — field code. The parameter is required. If the code does not start with `UF_CRM_`, the prefix is added automatically
- `USER_TYPE_ID` — field type, `iblock_section` for the binding to sections
- `MULTIPLE` — `Y` for several values, `N` for one
- `EDIT_FORM_LABEL` — field title in the deal card, specified by language
- `SETTINGS.IBLOCK_ID` — information block identifier from the first step. Without it, the method returns an error
- `SETTINGS.DISPLAY` — type of the control in the deal card: `UI`, `DIALOG`, `LIST`, or `CHECKBOX`

The full list of field types is returned by the [crm.userfield.types](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md) method. For the binding to individual information block elements, there is a paired type, `iblock_element`.

Save the identifiers of the created fields from the response: they let you read the settings with the [crm.deal.userfield.get](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-get.md) method.

{% list tabs %}

- JS

    ```js
    const listFieldId = await callMethod(
        'crm.deal.userfield.add',
        {
            fields: {
                FIELD_NAME: 'UF_CRM_IBS_LIST',
                USER_TYPE_ID: 'iblock_section',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'List section' },
                SETTINGS: { IBLOCK_ID: 123, DISPLAY: 'UI' }
            }
        },
        'userfield-add-list-section'
    )

    const catalogFieldId = await callMethod(
        'crm.deal.userfield.add',
        {
            fields: {
                FIELD_NAME: 'UF_CRM_IBS_CAT',
                USER_TYPE_ID: 'iblock_section',
                MULTIPLE: 'Y',
                EDIT_FORM_LABEL: { en: 'Catalog sections' },
                SETTINGS: { IBLOCK_ID: 25, DISPLAY: 'UI' }
            }
        },
        'userfield-add-catalog-section'
    )

    console.log(listFieldId, catalogFieldId)
    ```

- PHP

    ```php
    $listFieldId = callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_IBS_LIST',
            'USER_TYPE_ID' => 'iblock_section',
            'MULTIPLE' => 'N',
            'EDIT_FORM_LABEL' => ['en' => 'List section'],
            'SETTINGS' => ['IBLOCK_ID' => 123, 'DISPLAY' => 'UI'],
        ],
    ]);

    $catalogFieldId = callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_IBS_CAT',
            'USER_TYPE_ID' => 'iblock_section',
            'MULTIPLE' => 'Y',
            'EDIT_FORM_LABEL' => ['en' => 'Catalog sections'],
            'SETTINGS' => ['IBLOCK_ID' => 25, 'DISPLAY' => 'UI'],
        ],
    ]);

    print_r([$listFieldId, $catalogFieldId]);
    ```

- Python

    ```python
    list_field_id = call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_IBS_LIST",
                "USER_TYPE_ID": "iblock_section",
                "MULTIPLE": "N",
                "EDIT_FORM_LABEL": {"en": "List section"},
                "SETTINGS": {"IBLOCK_ID": 123, "DISPLAY": "UI"},
            },
        },
    )

    catalog_field_id = call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_IBS_CAT",
                "USER_TYPE_ID": "iblock_section",
                "MULTIPLE": "Y",
                "EDIT_FORM_LABEL": {"en": "Catalog sections"},
                "SETTINGS": {"IBLOCK_ID": 25, "DISPLAY": "UI"},
            },
        },
    )

    print(list_field_id, catalog_field_id)
    ```

{% endlist %}

The response contains the field identifier:

```json
{
    "result": 6007773
}
```

Abbreviated response of [crm.deal.userfield.get](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-get.md) with the field settings:

```json
{
    "result": {
        "ID": "6007773",
        "ENTITY_ID": "CRM_DEAL",
        "FIELD_NAME": "UF_CRM_IBS_LIST",
        "USER_TYPE_ID": "iblock_section",
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

Only five settings are stored. The `FIELD_NAME` value is needed in the fourth step, because values are written under that key.

## 3. Retrieve the Section Identifiers

List sections are returned by the [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md) method. Pass the parameters:

- `IBLOCK_TYPE_ID` — information block type, the same as in the first step
- `IBLOCK_ID` — list identifier
- `FILTER` — selection conditions, for example the `ID` of a specific section

Catalog sections are returned by the [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) method. Pass the parameters:

- `filter.iblockId` — catalog identifier, otherwise the response includes sections of all catalogs
- `select` — section fields, `id`, `iblockId`, `name`, and `iblockSectionId` are enough for this scenario

Both methods return a flat list of sections, and the tree is built by the reference to the parent: `IBLOCK_SECTION_ID` for a list and `iblockSectionId` for a catalog. For root sections, this field is empty.

If the list has just been created, it has no sections and the method returns an empty array. A section is created by the [lists.section.add](../../../api-reference/lists/sections/lists-section-add.md) method with the required parameters `IBLOCK_TYPE_ID`, `IBLOCK_ID`, `SECTION_CODE`, and `FIELDS.NAME`.

{% list tabs %}

- JS

    ```js
    let listSections = await callMethod(
        'lists.section.get',
        {
            IBLOCK_TYPE_ID: 'lists',
            IBLOCK_ID: 123
        },
        'lists-section-get'
    )

    if (listSections.length === 0) {
        await callMethod(
            'lists.section.add',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 123,
                SECTION_CODE: 'first_section',
                FIELDS: { NAME: 'First section' }
            },
            'lists-section-add'
        )

        listSections = await callMethod(
            'lists.section.get',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 123
            },
            'lists-section-get-again'
        )
    }

    const catalogSections = await callMethod(
        'catalog.section.list',
        {
            select: ['id', 'iblockId', 'name', 'iblockSectionId'],
            filter: { iblockId: 25 }
        },
        'catalog-section-list'
    )

    const listSectionId = Number(listSections[0].ID)
    const catalogSectionIds = catalogSections.sections.slice(0, 2).map((section) => section.id)

    console.log(listSectionId, catalogSectionIds)
    ```

- PHP

    ```php
    $listSections = callMethod($serviceBuilder, 'lists.section.get', [
        'IBLOCK_TYPE_ID' => 'lists',
        'IBLOCK_ID' => 123,
    ]);

    if ($listSections === []) {
        callMethod($serviceBuilder, 'lists.section.add', [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 123,
            'SECTION_CODE' => 'first_section',
            'FIELDS' => ['NAME' => 'First section'],
        ]);

        $listSections = callMethod($serviceBuilder, 'lists.section.get', [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 123,
        ]);
    }

    $catalogSections = callMethod($serviceBuilder, 'catalog.section.list', [
        'select' => ['id', 'iblockId', 'name', 'iblockSectionId'],
        'filter' => ['iblockId' => 25],
    ]);

    $listSectionId = (int)$listSections[0]['ID'];
    $catalogSectionIds = array_map(
        static fn(array $section): int => (int)$section['id'],
        array_slice($catalogSections['sections'], 0, 2)
    );

    print_r([$listSectionId, $catalogSectionIds]);
    ```

- Python

    ```python
    list_sections = call_method(
        "lists.section.get",
        {"IBLOCK_TYPE_ID": "lists", "IBLOCK_ID": 123},
    )

    if not list_sections:
        call_method(
            "lists.section.add",
            {
                "IBLOCK_TYPE_ID": "lists",
                "IBLOCK_ID": 123,
                "SECTION_CODE": "first_section",
                "FIELDS": {"NAME": "First section"},
            },
        )

        list_sections = call_method(
            "lists.section.get",
            {"IBLOCK_TYPE_ID": "lists", "IBLOCK_ID": 123},
        )

    catalog_sections = call_method(
        "catalog.section.list",
        {
            "select": ["id", "iblockId", "name", "iblockSectionId"],
            "filter": {"iblockId": 25},
        },
    )

    list_section_id = int(list_sections[0]["ID"])
    catalog_section_ids = [
        section["id"] for section in catalog_sections["sections"][:2]
    ]

    print(list_section_id, catalog_section_ids)
    ```

{% endlist %}

Abbreviated response of [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md):

```json
{
    "result": [
        {
            "ID": "237",
            "IBLOCK_ID": "123",
            "IBLOCK_SECTION_ID": null,
            "NAME": "Section for documentation checks",
            "DEPTH_LEVEL": "1",
            "CODE": "doc_check_section_1"
        }
    ],
    "total": 1
}
```

Abbreviated response of [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md):

```json
{
    "result": {
        "sections": [
            { "id": 19, "iblockId": 25, "iblockSectionId": 31, "name": "Tours" },
            { "id": 31, "iblockId": 25, "iblockSectionId": null, "name": "Clothing" },
            { "id": 33, "iblockId": 25, "iblockSectionId": 31, "name": "Footwear" }
        ]
    },
    "total": 3
}
```

In the example, you have the list section `237` and the catalog sections `19` and `33`. Both catalog sections are nested in section `31`: if you bind the deal to section `31`, sections `19` and `33` will not be included in the field value.

## 4. Write the Values

The [crm.deal.update](../../../api-reference/crm/deals/crm-deal-update.md) method writes values to the deal fields. Pass the parameters:

- `id` — deal identifier
- `fields` — object with field codes. Pass a number to the single-value field and an array of numbers to the multiple field

Bitrix24 does not validate the identifiers you pass: the method accepts a nonexistent section as well as a section of another information block and returns `true`. Check the sections before writing. Look up the list section with the [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md) method and a filter by `ID`: an empty array means the section is not in that list. Check the catalog sections with the [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) method by passing an array of identifiers to the filter, which shows in a single call which of them belong to the required catalog.

{% list tabs %}

- JS

    ```js
    async function isListSectionValid(iblockTypeId, iblockId, id) {
        const found = await callMethod(
            'lists.section.get',
            {
                IBLOCK_TYPE_ID: iblockTypeId,
                IBLOCK_ID: iblockId,
                FILTER: { ID: id }
            },
            `lists-section-check-${id}`
        )

        return found.length > 0
    }

    async function filterCatalogSections(iblockId, ids) {
        if (ids.length === 0) {
            return []
        }

        const found = await callMethod(
            'catalog.section.list',
            {
                select: ['id'],
                filter: { iblockId, id: ids }
            },
            'catalog-section-check'
        )

        return found.sections.map((section) => section.id)
    }

    if (!(await isListSectionValid('lists', 123, listSectionId))) {
        throw new Error(`Section ${listSectionId} is not in list 123`)
    }

    const validCatalogSectionIds = await filterCatalogSections(25, catalogSectionIds)

    await callMethod(
        'crm.deal.update',
        {
            id: 8419,
            fields: {
                UF_CRM_IBS_LIST: listSectionId,
                UF_CRM_IBS_CAT: validCatalogSectionIds
            }
        },
        'deal-update-section-bindings'
    )
    ```

- PHP

    ```php
    function isListSectionValid($serviceBuilder, string $iblockTypeId, int $iblockId, int $id): bool
    {
        $found = callMethod($serviceBuilder, 'lists.section.get', [
            'IBLOCK_TYPE_ID' => $iblockTypeId,
            'IBLOCK_ID' => $iblockId,
            'FILTER' => ['ID' => $id],
        ]);

        return $found !== [];
    }

    function filterCatalogSections($serviceBuilder, int $iblockId, array $ids): array
    {
        if ($ids === []) {
            return [];
        }

        $found = callMethod($serviceBuilder, 'catalog.section.list', [
            'select' => ['id'],
            'filter' => ['iblockId' => $iblockId, 'id' => $ids],
        ]);

        return array_map(
            static fn(array $section): int => (int)$section['id'],
            $found['sections']
        );
    }

    if (!isListSectionValid($serviceBuilder, 'lists', 123, $listSectionId)) {
        throw new RuntimeException('Section ' . $listSectionId . ' is not in list 123');
    }

    $validCatalogSectionIds = filterCatalogSections($serviceBuilder, 25, $catalogSectionIds);

    callMethod($serviceBuilder, 'crm.deal.update', [
        'id' => 8419,
        'fields' => [
            'UF_CRM_IBS_LIST' => $listSectionId,
            'UF_CRM_IBS_CAT' => $validCatalogSectionIds,
        ],
    ]);
    ```

- Python

    ```python
    def is_list_section_valid(iblock_type_id, iblock_id, section):
        found = call_method(
            "lists.section.get",
            {
                "IBLOCK_TYPE_ID": iblock_type_id,
                "IBLOCK_ID": iblock_id,
                "FILTER": {"ID": section},
            },
        )

        return len(found) > 0

    def filter_catalog_sections(iblock_id, ids):
        if not ids:
            return []

        found = call_method(
            "catalog.section.list",
            {
                "select": ["id"],
                "filter": {"iblockId": iblock_id, "id": ids},
            },
        )

        return [section["id"] for section in found["sections"]]

    if not is_list_section_valid("lists", 123, list_section_id):
        raise RuntimeError(f"Section {list_section_id} is not in list 123")

    valid_catalog_section_ids = filter_catalog_sections(25, catalog_section_ids)

    call_method(
        "crm.deal.update",
        {
            "id": 8419,
            "fields": {
                "UF_CRM_IBS_LIST": list_section_id,
                "UF_CRM_IBS_CAT": valid_catalog_section_ids,
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

Section names are not stored in the deal. Retrieve the list section with the [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md) method and a filter by `ID`, and the catalog sections with the [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) method and an array of identifiers in the filter: all the required sections come in a single call.

{% list tabs %}

- JS

    ```js
    const deal = await callMethod('crm.deal.get', { id: 8419 }, 'deal-get')

    const boundListSectionId = Number(deal.UF_CRM_IBS_LIST)
    const boundCatalogSectionIds = deal.UF_CRM_IBS_CAT ?? []

    const listSectionNames = boundListSectionId > 0
        ? await callMethod(
            'lists.section.get',
            {
                IBLOCK_TYPE_ID: 'lists',
                IBLOCK_ID: 123,
                FILTER: { ID: boundListSectionId }
            },
            'lists-section-resolve'
        )
        : []

    const catalogSectionNames = boundCatalogSectionIds.length > 0
        ? await callMethod(
            'catalog.section.list',
            {
                select: ['id', 'name', 'iblockSectionId'],
                filter: { iblockId: 25, id: boundCatalogSectionIds }
            },
            'catalog-section-resolve'
        )
        : { sections: [] }

    console.log(listSectionNames[0]?.NAME)
    console.table(catalogSectionNames.sections)
    ```

- PHP

    ```php
    $deal = callMethod($serviceBuilder, 'crm.deal.get', ['id' => 8419]);

    $boundListSectionId = (int)$deal['UF_CRM_IBS_LIST'];
    $boundCatalogSectionIds = $deal['UF_CRM_IBS_CAT'] ?? [];

    $listSectionNames = $boundListSectionId > 0
        ? callMethod($serviceBuilder, 'lists.section.get', [
            'IBLOCK_TYPE_ID' => 'lists',
            'IBLOCK_ID' => 123,
            'FILTER' => ['ID' => $boundListSectionId],
        ])
        : [];

    $catalogSectionNames = $boundCatalogSectionIds !== []
        ? callMethod($serviceBuilder, 'catalog.section.list', [
            'select' => ['id', 'name', 'iblockSectionId'],
            'filter' => ['iblockId' => 25, 'id' => $boundCatalogSectionIds],
        ])
        : ['sections' => []];

    print_r($listSectionNames[0]['NAME'] ?? null);
    print_r($catalogSectionNames['sections']);
    ```

- Python

    ```python
    deal = call_method("crm.deal.get", {"id": 8419})

    bound_list_section_id = int(deal["UF_CRM_IBS_LIST"] or 0)
    bound_catalog_section_ids = deal.get("UF_CRM_IBS_CAT") or []

    list_section_names = (
        call_method(
            "lists.section.get",
            {
                "IBLOCK_TYPE_ID": "lists",
                "IBLOCK_ID": 123,
                "FILTER": {"ID": bound_list_section_id},
            },
        )
        if bound_list_section_id > 0
        else []
    )

    catalog_section_names = (
        call_method(
            "catalog.section.list",
            {
                "select": ["id", "name", "iblockSectionId"],
                "filter": {"iblockId": 25, "id": bound_catalog_section_ids},
            },
        )
        if bound_catalog_section_ids
        else {"sections": []}
    )

    print(list_section_names[0]["NAME"] if list_section_names else None)
    print(catalog_section_names["sections"])
    ```

{% endlist %}

Abbreviated response of [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md):

```json
{
    "result": {
        "ID": "8419",
        "TITLE": "Information block section binding check",
        "UF_CRM_IBS_LIST": "237",
        "UF_CRM_IBS_CAT": [19, 33]
    }
}
```

Abbreviated response of [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) with a filter by identifiers:

```json
{
    "result": {
        "sections": [
            { "id": 19, "iblockSectionId": 31, "name": "Tours" },
            { "id": 33, "iblockSectionId": 31, "name": "Footwear" }
        ]
    },
    "total": 2
}
```

If the response contains fewer sections than there are identifiers in the field, some of the bindings point to sections that are not in that catalog.

## Verify the Result

The scenario is complete if both fields are filled in after reading the deal and a section is found for every identifier.

What to check in the responses:

- `UF_CRM_IBS_LIST` contains a string with the section identifier rather than `"0"` or an empty string
- `UF_CRM_IBS_CAT` contains an array of section identifiers
- [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md) with a filter by that `ID` returned one section rather than an empty array
- [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) returned as many sections as there are identifiers in the field

Open the deal card in the interface: the List Section and Catalog Sections fields show the section names. An empty field in the card together with a non-empty value in the response means that the stored section is not in the bound information block.

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code or Error Text** | **Reason and Action** ||
|| `The 'FIELD_NAME' field is not found.` | The field code is not passed to [crm.deal.userfield.add](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md). Pass `FIELD_NAME` ||
|| `ERROR_CORE`, `Select the information block to link the field to` | The field settings contain no `SETTINGS.IBLOCK_ID`. Pass the information block identifier from the first step ||
|| `0`, `Invalid information block type` | [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md) is called with an information block type that is not a list. For catalog sections, use [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | A required parameter is not passed to [lists.section.add](../../../api-reference/lists/sections/lists-section-add.md). Its name is specified in the error text: `SECTION_CODE` or `NAME` ||
|| `200040300040`, `Access Denied` | [catalog.section.get](../../../api-reference/catalog/section/catalog-section-get.md) is called with the identifier of a nonexistent section or without administrator rights. The access message arrives in both cases, so check the sections with the [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) method ||
|#

If there was no error but the binding does not work, check the stored value with the [crm.deal.get](../../../api-reference/crm/deals/crm-deal-get.md) method.

- The value `"0"` means that a string was passed to the field instead of a number. A non-numeric value is cast to zero, and the method returns no error
- The value `"1"` in a single-value field means that an array was passed to it. A single-value field accepts only a number, and an array is cast to one rather than to its first element
- There is a value but the card is empty: the stored identifier belongs to a nonexistent section or to a section of another information block. Check the section with the [lists.section.get](../../../api-reference/lists/sections/lists-section-get.md) or [catalog.section.list](../../../api-reference/catalog/section/catalog-section-list.md) method and write the value again

To clear the binding, pass an empty string to the field. Running the scenario again overwrites the values and creates no duplicates.

## Key Points

- The field is bound to a single information block. The information block type is not stored in the settings, only `IBLOCK_ID` is
- Bitrix24 does not check that the section exists and belongs to the bound information block. Validating the identifiers is the task of your integration
- The binding stores a single node of the tree. Nested sections are not added automatically: to get the whole branch, select the sections by the parent in `IBLOCK_SECTION_ID` or `iblockSectionId`
- A single-value field is returned as a string, a multiple field as an array of numbers
- Catalog sections are available to an administrator only, and list sections to a user with read access permission for that list
- For the binding to individual information block elements, use the paired `iblock_element` field type covered in the [{#T}](./how-to-use-iblock-binding-field.md) tutorial
- For other CRM objects, fields are created with the methods of the same name, for example [crm.lead.userfield.add](../../../api-reference/crm/leads/userfield/crm-lead-userfield-add.md), and in a smart process with the [userfieldconfig.add](../../../api-reference/crm/universal/userfieldconfig/userfieldconfig-add.md) method

## Code Example

The complete scenario in a single script: it finds the information blocks, creates both fields, retrieves and checks the sections, writes the values, and expands them into names.

{% list tabs %}

- JS

    ```js
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const LIST_IBLOCK_TYPE = 'lists'
    const DEAL_ID = 8419

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
        const catalogIblockId = catalogs.catalogs.find((item) => item.productIblockId === null).iblockId

        await callMethod('crm.deal.userfield.add', {
            fields: {
                FIELD_NAME: 'UF_CRM_IBS_LIST',
                USER_TYPE_ID: 'iblock_section',
                MULTIPLE: 'N',
                EDIT_FORM_LABEL: { en: 'List section' },
                SETTINGS: { IBLOCK_ID: listIblockId, DISPLAY: 'UI' }
            }
        }, 'userfield-add-list-section')

        await callMethod('crm.deal.userfield.add', {
            fields: {
                FIELD_NAME: 'UF_CRM_IBS_CAT',
                USER_TYPE_ID: 'iblock_section',
                MULTIPLE: 'Y',
                EDIT_FORM_LABEL: { en: 'Catalog sections' },
                SETTINGS: { IBLOCK_ID: catalogIblockId, DISPLAY: 'UI' }
            }
        }, 'userfield-add-catalog-section')

        let listSections = await callMethod('lists.section.get', {
            IBLOCK_TYPE_ID: LIST_IBLOCK_TYPE,
            IBLOCK_ID: listIblockId
        }, 'lists-section-get')

        if (listSections.length === 0) {
            await callMethod('lists.section.add', {
                IBLOCK_TYPE_ID: LIST_IBLOCK_TYPE,
                IBLOCK_ID: listIblockId,
                SECTION_CODE: 'first_section',
                FIELDS: { NAME: 'First section' }
            }, 'lists-section-add')

            listSections = await callMethod('lists.section.get', {
                IBLOCK_TYPE_ID: LIST_IBLOCK_TYPE,
                IBLOCK_ID: listIblockId
            }, 'lists-section-get-again')
        }

        const catalogSections = await callMethod('catalog.section.list', {
            select: ['id', 'iblockId', 'name', 'iblockSectionId'],
            filter: { iblockId: catalogIblockId }
        }, 'catalog-section-list')

        const listSectionId = Number(listSections[0].ID)
        const catalogSectionIds = catalogSections.sections.slice(0, 2).map((section) => section.id)

        const checked = await callMethod('catalog.section.list', {
            select: ['id'],
            filter: { iblockId: catalogIblockId, id: catalogSectionIds }
        }, 'catalog-section-check')

        await callMethod('crm.deal.update', {
            id: DEAL_ID,
            fields: {
                UF_CRM_IBS_LIST: listSectionId,
                UF_CRM_IBS_CAT: checked.sections.map((section) => section.id)
            }
        }, 'deal-update-section-bindings')

        const deal = await callMethod('crm.deal.get', { id: DEAL_ID }, 'deal-get')

        const boundListSection = await callMethod('lists.section.get', {
            IBLOCK_TYPE_ID: LIST_IBLOCK_TYPE,
            IBLOCK_ID: listIblockId,
            FILTER: { ID: Number(deal.UF_CRM_IBS_LIST) }
        }, 'lists-section-resolve')

        const boundCatalogSections = await callMethod('catalog.section.list', {
            select: ['id', 'name', 'iblockSectionId'],
            filter: { iblockId: catalogIblockId, id: deal.UF_CRM_IBS_CAT ?? [] }
        }, 'catalog-section-resolve')

        console.log(boundListSection[0]?.NAME)
        console.table(boundCatalogSections.sections)
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
    const DEAL_ID = 8419;

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
            'FIELD_NAME' => 'UF_CRM_IBS_LIST',
            'USER_TYPE_ID' => 'iblock_section',
            'MULTIPLE' => 'N',
            'EDIT_FORM_LABEL' => ['en' => 'List section'],
            'SETTINGS' => ['IBLOCK_ID' => $listIblockId, 'DISPLAY' => 'UI'],
        ],
    ]);

    callMethod($serviceBuilder, 'crm.deal.userfield.add', [
        'fields' => [
            'FIELD_NAME' => 'UF_CRM_IBS_CAT',
            'USER_TYPE_ID' => 'iblock_section',
            'MULTIPLE' => 'Y',
            'EDIT_FORM_LABEL' => ['en' => 'Catalog sections'],
            'SETTINGS' => ['IBLOCK_ID' => $catalogIblockId, 'DISPLAY' => 'UI'],
        ],
    ]);

    $listSections = callMethod($serviceBuilder, 'lists.section.get', [
        'IBLOCK_TYPE_ID' => LIST_IBLOCK_TYPE,
        'IBLOCK_ID' => $listIblockId,
    ]);

    if ($listSections === []) {
        callMethod($serviceBuilder, 'lists.section.add', [
            'IBLOCK_TYPE_ID' => LIST_IBLOCK_TYPE,
            'IBLOCK_ID' => $listIblockId,
            'SECTION_CODE' => 'first_section',
            'FIELDS' => ['NAME' => 'First section'],
        ]);

        $listSections = callMethod($serviceBuilder, 'lists.section.get', [
            'IBLOCK_TYPE_ID' => LIST_IBLOCK_TYPE,
            'IBLOCK_ID' => $listIblockId,
        ]);
    }

    $catalogSections = callMethod($serviceBuilder, 'catalog.section.list', [
        'select' => ['id', 'iblockId', 'name', 'iblockSectionId'],
        'filter' => ['iblockId' => $catalogIblockId],
    ]);

    $listSectionId = (int)$listSections[0]['ID'];
    $catalogSectionIds = array_map(
        static fn(array $section): int => (int)$section['id'],
        array_slice($catalogSections['sections'], 0, 2)
    );

    $checked = callMethod($serviceBuilder, 'catalog.section.list', [
        'select' => ['id'],
        'filter' => ['iblockId' => $catalogIblockId, 'id' => $catalogSectionIds],
    ]);

    callMethod($serviceBuilder, 'crm.deal.update', [
        'id' => DEAL_ID,
        'fields' => [
            'UF_CRM_IBS_LIST' => $listSectionId,
            'UF_CRM_IBS_CAT' => array_map(
                static fn(array $section): int => (int)$section['id'],
                $checked['sections']
            ),
        ],
    ]);

    $deal = callMethod($serviceBuilder, 'crm.deal.get', ['id' => DEAL_ID]);

    $boundListSection = callMethod($serviceBuilder, 'lists.section.get', [
        'IBLOCK_TYPE_ID' => LIST_IBLOCK_TYPE,
        'IBLOCK_ID' => $listIblockId,
        'FILTER' => ['ID' => (int)$deal['UF_CRM_IBS_LIST']],
    ]);

    $boundCatalogSections = callMethod($serviceBuilder, 'catalog.section.list', [
        'select' => ['id', 'name', 'iblockSectionId'],
        'filter' => ['iblockId' => $catalogIblockId, 'id' => $deal['UF_CRM_IBS_CAT'] ?? []],
    ]);

    print_r($boundListSection[0]['NAME'] ?? null);
    print_r($boundCatalogSections['sections']);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook

    LIST_IBLOCK_TYPE = "lists"
    DEAL_ID = 8419

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
                "FIELD_NAME": "UF_CRM_IBS_LIST",
                "USER_TYPE_ID": "iblock_section",
                "MULTIPLE": "N",
                "EDIT_FORM_LABEL": {"en": "List section"},
                "SETTINGS": {"IBLOCK_ID": list_iblock_id, "DISPLAY": "UI"},
            },
        },
    )

    call_method(
        "crm.deal.userfield.add",
        {
            "fields": {
                "FIELD_NAME": "UF_CRM_IBS_CAT",
                "USER_TYPE_ID": "iblock_section",
                "MULTIPLE": "Y",
                "EDIT_FORM_LABEL": {"en": "Catalog sections"},
                "SETTINGS": {"IBLOCK_ID": catalog_iblock_id, "DISPLAY": "UI"},
            },
        },
    )

    list_sections = call_method(
        "lists.section.get",
        {"IBLOCK_TYPE_ID": LIST_IBLOCK_TYPE, "IBLOCK_ID": list_iblock_id},
    )

    if not list_sections:
        call_method(
            "lists.section.add",
            {
                "IBLOCK_TYPE_ID": LIST_IBLOCK_TYPE,
                "IBLOCK_ID": list_iblock_id,
                "SECTION_CODE": "first_section",
                "FIELDS": {"NAME": "First section"},
            },
        )

        list_sections = call_method(
            "lists.section.get",
            {"IBLOCK_TYPE_ID": LIST_IBLOCK_TYPE, "IBLOCK_ID": list_iblock_id},
        )

    catalog_sections = call_method(
        "catalog.section.list",
        {
            "select": ["id", "iblockId", "name", "iblockSectionId"],
            "filter": {"iblockId": catalog_iblock_id},
        },
    )

    list_section_id = int(list_sections[0]["ID"])
    catalog_section_ids = [section["id"] for section in catalog_sections["sections"][:2]]

    checked = call_method(
        "catalog.section.list",
        {
            "select": ["id"],
            "filter": {"iblockId": catalog_iblock_id, "id": catalog_section_ids},
        },
    )

    call_method(
        "crm.deal.update",
        {
            "id": DEAL_ID,
            "fields": {
                "UF_CRM_IBS_LIST": list_section_id,
                "UF_CRM_IBS_CAT": [section["id"] for section in checked["sections"]],
            },
        },
    )

    deal = call_method("crm.deal.get", {"id": DEAL_ID})

    bound_list_section = call_method(
        "lists.section.get",
        {
            "IBLOCK_TYPE_ID": LIST_IBLOCK_TYPE,
            "IBLOCK_ID": list_iblock_id,
            "FILTER": {"ID": int(deal["UF_CRM_IBS_LIST"] or 0)},
        },
    )

    bound_catalog_sections = call_method(
        "catalog.section.list",
        {
            "select": ["id", "name", "iblockSectionId"],
            "filter": {
                "iblockId": catalog_iblock_id,
                "id": deal.get("UF_CRM_IBS_CAT") or [],
            },
        },
    )

    print(bound_list_section[0]["NAME"] if bound_list_section else None)
    print(bound_catalog_sections["sections"])
    ```

{% endlist %}

## Continue Learning

- [{#T}](./how-to-use-iblock-binding-field.md)
- [{#T}](../../../api-reference/crm/universal/user-defined-fields/crm-userfield-types.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-add.md)
- [{#T}](../../../api-reference/lists/sections/lists-section-get.md)
- [{#T}](../../../api-reference/lists/sections/lists-section-add.md)
- [{#T}](../../../api-reference/catalog/section/catalog-section-list.md)
- [{#T}](../../../api-reference/catalog/catalog/catalog-catalog-list.md)
- [{#T}](../../../api-reference/crm/deals/crm-deal-update.md)
