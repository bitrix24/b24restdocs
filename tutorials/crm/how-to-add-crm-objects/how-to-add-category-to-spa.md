# How to Create a New Sales Funnel with Stages in a Smart Process

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — administrative access to the CRM section
>
> - [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) and [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) — a user with administrative access to the CRM section
> - [crm.status.update](../../../api-reference/crm/status/crm-status-update.md) and [crm.status.add](../../../api-reference/crm/status/crm-status-add.md) — a user with the "Allow changing settings" permission in CRM
> - [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — a user with permission to read at least one CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A funnel is a separate branch of work with a CRM object that has its own set of stages and its own item card settings. Funnels are created to split work by department or by process type. In procurement, for example, you can create separate funnels for requests, deliveries, and acceptance.

Smart process stages are retained in a CRM directory. The directory name is built from two numbers: the identifier of the smart process type and the identifier of the funnel. Both numbers are known only after the first two calls, so the scenario runs in a strict order: first find the type, then create the funnel, and only after that work with its stages.

As a result of the scenario, the smart process gets a funnel with six stages: three "In Progress" stages, one "Success" stage, and two "Failure" stages. The first "In Progress" stage is renamed.

The scenario consists of five steps.

1. Retrieve the `entityTypeId` of the smart process using the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method
2. Create the funnel using the [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) method and retrieve its `id`
3. Retrieve the pre-installed stages of the funnel using the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method and take the `ID` of the first one
4. Rename that stage using the [crm.status.update](../../../api-reference/crm/status/crm-status-update.md) method
5. Add your own stage using the [crm.status.add](../../../api-reference/crm/status/crm-status-add.md) method

## Before You Start

- The smart process is already created in Bitrix24, and you know its name. The examples use the `Equipment procurement` smart process — substitute the name of your own

- Funnels and stages are enabled for the smart process. This is confirmed by the `isCategoriesEnabled` and `isStagesEnabled` fields with the value `Y` in the step 1 response. With funnels disabled, step 2 returns the `ENTITY_TYPE_NOT_SUPPORTED` error

- The webhook is created on behalf of a user with administrative access to the CRM section. Without this access, step 1 returns the `ACCESS_DENIED` error

- The same user has the "Allow changing settings" permission in CRM. Steps 4 and 5 require it: without it, they return `Access denied.` after the funnel has already been created

- The `crm` scope is selected in the webhook permissions

- The webhook URL grants full access within its scope. Retain the URL in an environment variable and never publish it in open code

## 1. Retrieve the Smart Process Type ID {#entity-type-id}

Use the [crm.type.list](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md) method with a filter:

- `filter[title]` — the smart process name. Specify `Equipment procurement`

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: 'crm.type.list',
        params: {
            filter: { title: 'Equipment procurement' } // Smart process name
        },
        requestId: 'type-list'
    });

    const types = result.getData().result.types;
    const entityTypeId = types.length ? types[0].entityTypeId : null;
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

    types = client.crm.type.list(
        filter={"title": "Equipment procurement"},  # Smart process name
    ).response.result["types"]

    entity_type_id = int(types[0]["entityTypeId"]) if types else None
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

    $types = $sb->getCRMScope()->type()->list(
        order: [],
        filter: ['title' => 'Equipment procurement'] // Smart process name
    )->getTypes();

    $entityTypeId = $types[0]->entityTypeId ?? null;
    ```
{% endlist %}

In the response, the method returns a `types` array. Retain the `entityTypeId` — it has to be passed to steps 2, 3, and 5. In the example, `entityTypeId`: `177`.

The `title` filter searches for an exact match. Before calling step 2, make sure that the `types` array is not empty.

```json
{
    "result": {
        "types": [
            {
                "id": 7,
                "title": "Equipment procurement",
                "code": "",
                "createdBy": 1,
                "entityTypeId": 177,
                "customSectionId": null,
                "isCategoriesEnabled": "Y",
                "isStagesEnabled": "Y",
                "isBeginCloseDatesEnabled": "Y",
                "isClientEnabled": "Y",
                "isUseInUserfieldEnabled": "Y",
                "isLinkWithProductsEnabled": "Y",
                "isMycompanyEnabled": "Y",
                "isDocumentsEnabled": "Y",
                "isSourceEnabled": "Y",
                "isObserversEnabled": "Y",
                "isRecyclebinEnabled": "Y",
                "isAutomationEnabled": "Y",
                "isBizProcEnabled": "Y",
                "isSetOpenPermissions": "Y",
                "isPaymentsEnabled": "N",
                "isCountersEnabled": "N",
                "createdTime": "2021-11-26T10:52:17+03:00",
                "updatedTime": "2024-11-12T15:32:39+03:00",
                "updatedBy": 1,
                "isInitialized": "Y"
            }
        ]
    },
    "total": 1
}
```

{% note warning "" %}

The following steps require exactly the `entityTypeId`, not the `id`. These are different numbers: a smart process with `id`: `7` has the type identifier `177`. If you substitute the `id`, the [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) method returns the `NOT_FOUND` error.

{% endnote %}

## 2. Create a New Funnel {#category-id}

Use the [crm.category.add](../../../api-reference/crm/universal/category/crm-category-add.md) method with the following parameters:

- `entityTypeId` — the type identifier from the [crm.type.list](#entity-type-id) step, a required parameter. In the example, `177`

- `fields[name]` — the funnel name, a required parameter. Specify `New funnel`

- `fields[sort]` — the sorting index. It determines the position of the funnel in the funnel list of the smart process

{% list tabs %}

- JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.category.add',
        params: {
            entityTypeId: entityTypeId, // Type identifier from step 1
            fields: {
                name: 'New funnel', // Funnel name
                sort: 100 // Sorting index
            }
        },
        requestId: 'category-add'
    });

    const categoryId = result.getData().result.category.id;
    ```

- Python

    ```python
    category = client.crm.category.add(
        entity_type_id=entity_type_id,  # Type identifier from step 1
        fields={
            "name": "New funnel",  # Funnel name
            "sort": 100,  # Sorting index
        },
    ).response.result["category"]

    category_id = int(category["id"])
    ```


- PHP

    ```php
    // crm.category.add has no wrapper in the SDK — calling the method directly
    $result = $sb->core->call(
        'crm.category.add',
        [
            'entityTypeId' => $entityTypeId, // Type identifier from step 1
            'fields' => [
                'name' => 'New funnel', // Funnel name
                'sort' => 100 // Sorting index
            ]
        ]
    );

    $categoryId = $result->getResponseData()->getResult()['category']['id'];
    ```
{% endlist %}

In the response, the method returns a `category` object. Retain the `id` — it has to be passed to steps 3 and 5. In the example, `id`: `87`.

```json
{
    "result": {
        "category": {
            "id": 87,
            "name": "New funnel",
            "sort": 100,
            "entityTypeId": 177,
            "isDefault": "N"
        }
    }
}
```

Bitrix24 immediately fills the new funnel with default stages, so there is no need to create them separately.

{% note warning "" %}

The method does not check for duplicates: running the scenario again adds a second funnel with the same name.

{% endnote %}

## 3. Retrieve the Stages of the Created Funnel {#stage-id}

The stages are kept in a CRM directory. For smart process stages, the directory name is built by the formula `DYNAMIC_{entityTypeId}_STAGE_{categoryId}`, where `entityTypeId` is the type identifier from the [crm.type.list](#entity-type-id) step and `categoryId` is the funnel identifier from the [crm.category.add](#category-id) step. In the example, this gives `DYNAMIC_177_STAGE_87`.

Use the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method with the following parameters:

- `filter[ENTITY_ID]` — the directory name built by the formula

- `order[SORT]` — sorting by the `SORT` index. Pass it explicitly so that the first element of the array is always the first stage of the "In Progress" group

{% list tabs %}

- JS

    ```javascript
    const entityId = `DYNAMIC_${entityTypeId}_STAGE_${categoryId}`;

    const result = await $b24.actions.v2.call.make({
        method: 'crm.status.list',
        params: {
            filter: { ENTITY_ID: entityId }, // Stage directory of this funnel
            order: { SORT: 'ASC' } // Stages in ascending order of the sorting index
        },
        requestId: 'status-list'
    });

    const stages = result.getData().result;
    const firstStageId = stages[0].ID; // First stage of the "In Progress" group
    ```

- Python

    ```python
    entity_id = f"DYNAMIC_{entity_type_id}_STAGE_{category_id}"

    stages = client.crm.status.list(
        filter={"ENTITY_ID": entity_id},  # Stage directory of this funnel
        order={"SORT": "ASC"},  # Stages in ascending order of the sorting index
    ).response.result

    first_stage_id = int(stages[0]["ID"])  # First stage of the "In Progress" group
    ```


- PHP

    ```php
    $entityId = "DYNAMIC_{$entityTypeId}_STAGE_{$categoryId}";

    $stages = $sb->getCRMScope()->status()->list(
        order: ['SORT' => 'ASC'], // Stages in ascending order of the sorting index
        filter: ['ENTITY_ID' => $entityId] // Stage directory of this funnel
    )->getStatuses();

    $firstStageId = $stages[0]->ID; // First stage of the "In Progress" group
    ```
{% endlist %}

In the response, the method returns an array of stages sorted by the `SORT` field. Retain the `ID` of the first stage — it has to be passed to step 4. In the example, `ID`: `1073`. As in step 1, make sure the array is not empty before the next call.

Every new funnel receives five stages:

- three stages of the "In Progress" group — they have `SEMANTICS`: `null`

- one stage of the "Success" group — `SEMANTICS`: `S`

- one stage of the "Failure" group — `SEMANTICS`: `F`

The composition of the groups can be changed, but with limitations: the funnel retains at least one stage of each group, and there can be only one stage in the "Success" group.

```json
{
    "result": [
        {
            "ID": "1073",
            "ENTITY_ID": "DYNAMIC_177_STAGE_87",
            "STATUS_ID": "DT177_87:NEW",
            "NAME": "Start",
            "NAME_INIT": "Start",
            "SORT": "10",
            "SYSTEM": "Y",
            "CATEGORY_ID": "87",
            "COLOR": "#22B9FF",
            "SEMANTICS": null
        },
        {
            "ID": "1075",
            "ENTITY_ID": "DYNAMIC_177_STAGE_87",
            "STATUS_ID": "DT177_87:PREPARATION",
            "NAME": "Preparation",
            "NAME_INIT": "Preparation",
            "SORT": "20",
            "SYSTEM": "N",
            "CATEGORY_ID": "87",
            "COLOR": "#88B9FF",
            "SEMANTICS": null
        },
        {
            "ID": "1077",
            "ENTITY_ID": "DYNAMIC_177_STAGE_87",
            "STATUS_ID": "DT177_87:CLIENT",
            "NAME": "Approval",
            "NAME_INIT": "Approval",
            "SORT": "30",
            "SYSTEM": "N",
            "CATEGORY_ID": "87",
            "COLOR": "#10e5fc",
            "SEMANTICS": null
        },
        {
            "ID": "1079",
            "ENTITY_ID": "DYNAMIC_177_STAGE_87",
            "STATUS_ID": "DT177_87:SUCCESS",
            "NAME": "Success",
            "NAME_INIT": "Success",
            "SORT": "40",
            "SYSTEM": "Y",
            "CATEGORY_ID": "87",
            "COLOR": "#00ff00",
            "SEMANTICS": "S"
        },
        {
            "ID": "1081",
            "ENTITY_ID": "DYNAMIC_177_STAGE_87",
            "STATUS_ID": "DT177_87:FAIL",
            "NAME": "Failure",
            "NAME_INIT": "Failure",
            "SORT": "50",
            "SYSTEM": "Y",
            "CATEGORY_ID": "87",
            "COLOR": "#ff0000",
            "SEMANTICS": "F"
        }
    ],
    "total": 5
}
```

## 4. Modify a Pre-installed Stage

Rename the first stage using the [crm.status.update](../../../api-reference/crm/status/crm-status-update.md) method with the following parameters:

- `id` — the stage identifier from the [crm.status.list](#stage-id) step, a required parameter. In the example, `1073`

- `fields[NAME]` — the new stage name. Specify `First stage`

Directory field names are written in uppercase: `NAME`, `SORT`, `COLOR`.

The first stage is marked with `SYSTEM`: `Y`. System stages can be renamed freely, the limitation applies only to deletion.

{% list tabs %}

- JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.status.update',
        params: {
            id: firstStageId, // Stage identifier from step 3
            fields: {
                NAME: 'First stage' // New stage name
            }
        },
        requestId: 'status-update'
    });
    ```

- Python

    ```python
    client.crm.status.update(
        first_stage_id,  # Stage identifier from step 3
        fields={
            "NAME": "First stage",  # New stage name
        },
    ).response
    ```


- PHP

    ```php
    $result = $sb->getCRMScope()->status()->update(
        $firstStageId, // Stage identifier from step 3
        [
            'NAME' => 'First stage' // New stage name
        ]
    );
    ```
{% endlist %}

In the response, the method returns `true` — the stage has been renamed.

```json
{
    "result": true
}
```

## 5. Add a New Stage to the Funnel

Use the [crm.status.add](../../../api-reference/crm/status/crm-status-add.md) method with the `fields` object:

- `ENTITY_ID` — the directory name from the [crm.status.list](#stage-id) step, a required parameter. In the example, `DYNAMIC_177_STAGE_87`

- `STATUS_ID` — the stage code, a required parameter. In the example, the code is passed together with the funnel prefix — `DT177_87:MY_STAGE`, where the prefix is built by the formula `DT{entityTypeId}_{categoryId}` and the colon separates it from the stage code itself. If the code is passed without the prefix, Bitrix24 adds the prefix on its own: `MY_STAGE` produces the same `DT177_87:MY_STAGE`

- `NAME` — the stage name, a required parameter. Specify `My stage`

- `SORT` — the sorting index. Specify `60` so that the new stage comes after the "Failure" stage with the sorting value `50`

- `SEMANTICS` — the stage group: `S` — "Success", `F` — "Failure", an empty string — "In Progress". Specify `F`. For the stages of the "In Progress" group, the read methods return `null`, as in the step 3 response

In the kanban, the stages are lined up in ascending order of `SORT`: first "In Progress", then "Success", and "Failure" last.

{% list tabs %}

- JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.status.add',
        params: {
            fields: {
                ENTITY_ID: entityId, // Stage directory from step 3
                STATUS_ID: `DT${entityTypeId}_${categoryId}:MY_STAGE`, // Stage code
                NAME: 'My stage', // Stage name
                SORT: 60, // Sorting index
                SEMANTICS: 'F' // "Failure" group
            }
        },
        requestId: 'status-add'
    });

    const newStageId = result.getData().result;
    ```

- Python

    ```python
    new_stage_id = client.crm.status.add(
        fields={
            "ENTITY_ID": entity_id,  # Stage directory from step 3
            "STATUS_ID": f"DT{entity_type_id}_{category_id}:MY_STAGE",  # Stage code
            "NAME": "My stage",  # Stage name
            "SORT": 60,  # Sorting index
            "SEMANTICS": "F",  # "Failure" group
        },
    ).response.result
    ```


- PHP

    ```php
    $result = $sb->getCRMScope()->status()->add(
        [
            'ENTITY_ID' => $entityId, // Stage directory from step 3
            'STATUS_ID' => 'DT' . $entityTypeId . '_' . $categoryId . ':MY_STAGE', // Stage code
            'NAME' => 'My stage', // Stage name
            'SORT' => 60, // Sorting index
            'SEMANTICS' => 'F' // "Failure" group
        ]
    );

    $newStageId = $result->getId();
    ```
{% endlist %}

In the response, the method returns the identifier of the created stage. In the example, `1083`.

```json
{
    "result": 1083
}
```

## Verify the Result

Open the smart process in Bitrix24 and switch to the "New funnel" funnel. The kanban shows six stages: the first one is named "First stage", and "My stage" comes after the "Failure" stage.

Through REST, the set of stages is returned by the same [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method for the directory from step 3.

{% list tabs %}

- JS

    ```javascript
    const checkResult = await $b24.actions.v2.call.make({
        method: 'crm.status.list',
        params: {
            filter: { ENTITY_ID: entityId },
            order: { SORT: 'ASC' }
        },
        requestId: 'status-list-check'
    });

    console.table(checkResult.getData().result.map(stage => ({
        NAME: stage.NAME,
        SORT: stage.SORT,
        SEMANTICS: stage.SEMANTICS
    })));
    ```

- Python

    ```python
    check_result = client.crm.status.list(
        filter={"ENTITY_ID": entity_id},
        order={"SORT": "ASC"},
    ).response.result

    for stage in check_result:
        print(stage["NAME"], stage["SORT"], stage["SEMANTICS"])
    ```


- PHP

    ```php
    $checkResult = $sb->getCRMScope()->status()->list(
        order: ['SORT' => 'ASC'],
        filter: ['ENTITY_ID' => $entityId]
    )->getStatuses();

    foreach ($checkResult as $stage) {
        echo $stage->NAME . ' | ' . $stage->SORT . ' | ' . $stage->SEMANTICS . PHP_EOL;
    }
    ```
{% endlist %}

The scenario is complete if the response contains six stages: the stage with `SORT`: `10` has the `NAME` field equal to `First stage`, and the array contains a stage with `STATUS_ID`: `DT177_87:MY_STAGE` and `SEMANTICS`: `F`.

```json
{
    "result": [
        {
            "ID": "1073",
            "STATUS_ID": "DT177_87:NEW",
            "NAME": "First stage",
            "SORT": "10",
            "SEMANTICS": null
        },
        {
            "ID": "1075",
            "STATUS_ID": "DT177_87:PREPARATION",
            "NAME": "Preparation",
            "SORT": "20",
            "SEMANTICS": null
        },
        {
            "ID": "1077",
            "STATUS_ID": "DT177_87:CLIENT",
            "NAME": "Approval",
            "SORT": "30",
            "SEMANTICS": null
        },
        {
            "ID": "1079",
            "STATUS_ID": "DT177_87:SUCCESS",
            "NAME": "Success",
            "SORT": "40",
            "SEMANTICS": "S"
        },
        {
            "ID": "1081",
            "STATUS_ID": "DT177_87:FAIL",
            "NAME": "Failure",
            "SORT": "50",
            "SEMANTICS": "F"
        },
        {
            "ID": "1083",
            "STATUS_ID": "DT177_87:MY_STAGE",
            "NAME": "My stage",
            "SORT": "60",
            "SEMANTICS": "F"
        }
    ],
    "total": 6
}
```

The response is shortened: every stage has other fields as well, some of them are shown in step 3. The full list of fields is returned by the [crm.status.fields](../../../api-reference/crm/status/crm-status-fields.md) method.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Error code or text** | **Reason and action** ||
|| `ACCESS_DENIED` | The webhook user does not have administrative access to the CRM section. The error is returned by steps 1 and 2 ||
|| `allowed_only_intranet_user` | The webhook is created on behalf of an external user. The scenario is available only to Bitrix24 employees ||
|| `NOT_FOUND` | The `entityTypeId` passed in step 2 does not match any smart process. There are two common reasons: the `id` was passed instead of the `entityTypeId`, or the `types` array in the step 1 response is empty ||
|| An empty `types` array in step 1 | Not an error but the result of the filter. The smart process name did not match: `filter[title]` searches for an exact match ||
|| An empty stage array in step 3 | Not an error but the result of the filter. Check the directory name against the formula `DYNAMIC_{entityTypeId}_STAGE_{categoryId}` and the `isStagesEnabled` field in the step 1 response ||
|| `ENTITY_TYPE_NOT_SUPPORTED` | Funnels are disabled for the smart process. Check the `isCategoriesEnabled` field in the step 1 response ||
|| `Field 'NAME' is required` | The funnel name was not passed in `fields[name]` in step 2 ||
|| `Invalid identifier.` | A non-numeric or empty stage `id` was passed in step 4 ||
|| `Status is not found.` | The `id` of a non-existent stage was passed in step 4. Take it from the step 3 response ||
|| `Specified entity type is not supported.` | The `ENTITY_ID` of a directory that does not exist was passed in step 5. Check the formula `DYNAMIC_{entityTypeId}_STAGE_{categoryId}` ||
|| `The specified status ID already exists.` | The `STATUS_ID` passed in step 5 is already taken in this funnel. Choose another code ||
|| `The field Title is required.` | The stage name was not passed in `NAME` in step 5 ||
|| `Access denied.` | The webhook user does not have the "Allow changing settings" permission in CRM. The error is returned by steps 4 and 5 ||
|#

Steps 1 and 3 do not create anything, so they can be repeated any number of times. Step 2 creates the funnel, so if steps 3, 4, or 5 fail, repeat only the failed step, taking the funnel `id` from the step 2 response.

## Key Considerations

- The existing funnels of a smart process are returned by the [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method, and an extra one created by a repeated run is deleted by the [crm.category.delete](../../../api-reference/crm/universal/category/crm-category-delete.md) method

- The `DYNAMIC_{entityTypeId}_STAGE_{categoryId}` and `DT{entityTypeId}_{categoryId}` formulas work only for smart processes. For deals, the stage directory is named `DEAL_STAGE_{categoryId}`, and the stages of the general deal funnel have no prefix at all. The list of directories is returned by the [crm.status.entity.types](../../../api-reference/crm/status/crm-status-entity-types.md) method

- Beyond the order in the kanban, the sorting defines the meaning of the stage for reports

- System stages marked with `SYSTEM`: `Y` are deleted by the [crm.status.delete](../../../api-reference/crm/status/crm-status-delete.md) method only with the `FORCED`: `Y` flag in the `params` parameter

## Code Example

The script creates a funnel in a smart process, renames the first pre-installed stage, adds its own stage to the "Failure" group, and prints the resulting table of stages by group.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const processTitle = 'Equipment procurement'; // Name of your smart process

    async function call(method, params) {
        const result = await $b24.actions.v2.call.make({ method, params });
        if (!result.isSuccess) {
            throw new Error(result.getErrorMessages().join('; '));
        }
        return result.getData().result;
    }

    function printStagesTable(stages) {
        const columns = { 'In Progress': [], 'Success': [], 'Failure': [] };

        stages.forEach(stage => {
            if (stage.SEMANTICS === 'S') {
                columns['Success'].push(stage.NAME);
            } else if (stage.SEMANTICS === 'F') {
                columns['Failure'].push(stage.NAME);
            } else {
                columns['In Progress'].push(stage.NAME);
            }
        });

        const maxRows = Math.max(
            columns['In Progress'].length,
            columns['Success'].length,
            columns['Failure'].length
        );

        const tableData = [];
        for (let i = 0; i < maxRows; i++) {
            tableData.push({
                'In Progress': columns['In Progress'][i] || '',
                'Success': columns['Success'][i] || '',
                'Failure': columns['Failure'][i] || ''
            });
        }

        console.table(tableData);
    }

    try {
        // 1. Retrieve the entityTypeId by the smart process name
        const types = (await call('crm.type.list', {
            filter: { title: processTitle }
        })).types;
        if (!types.length) {
            throw new Error(`Smart process "${processTitle}" not found`);
        }
        const entityTypeId = types[0].entityTypeId;

        // 2. Create the funnel
        const category = (await call('crm.category.add', {
            entityTypeId: entityTypeId,
            fields: { name: 'New funnel', sort: 100 }
        })).category;
        const categoryId = category.id;
        const entityId = `DYNAMIC_${entityTypeId}_STAGE_${categoryId}`;

        // 3. Retrieve the pre-installed stages
        const stages = await call('crm.status.list', {
            filter: { ENTITY_ID: entityId },
            order: { SORT: 'ASC' }
        });

        if (!stages.length) {
            throw new Error(`Stages not found: check the ${entityId} directory`);
        }

        // 4. Rename the first stage
        await call('crm.status.update', {
            id: stages[0].ID,
            fields: { NAME: 'First stage' }
        });

        // 5. Add your own stage to the "Failure" group
        await call('crm.status.add', {
            fields: {
                ENTITY_ID: entityId,
                STATUS_ID: `DT${entityTypeId}_${categoryId}:MY_STAGE`,
                NAME: 'My stage',
                SORT: 60,
                SEMANTICS: 'F'
            }
        });

        // Verify the result
        const finalStages = await call('crm.status.list', {
            filter: { ENTITY_ID: entityId },
            order: { SORT: 'ASC' }
        });
        printStagesTable(finalStages);
    } catch (error) {
        console.error(error.message);
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

    process_title = "Equipment procurement"  # Name of your smart process


    def print_stages_table(stages):
        columns = {"In Progress": [], "Success": [], "Failure": []}

        for stage in stages:
            if stage["SEMANTICS"] == "S":
                columns["Success"].append(stage["NAME"])
            elif stage["SEMANTICS"] == "F":
                columns["Failure"].append(stage["NAME"])
            else:
                columns["In Progress"].append(stage["NAME"])

        max_rows = max(len(column) for column in columns.values())

        print("Stage table:")
        for index in range(max_rows):
            row = [
                columns[group][index] if index < len(columns[group]) else ""
                for group in ("In Progress", "Success", "Failure")
            ]
            print(f"In Progress: {row[0]} | Success: {row[1]} | Failure: {row[2]}")


    try:
        # 1. Retrieve the entityTypeId by the smart process name
        types = client.crm.type.list(
            filter={"title": process_title},
        ).response.result["types"]
        if not types:
            raise SystemExit(f'Smart process "{process_title}" not found')
        entity_type_id = int(types[0]["entityTypeId"])

        # 2. Create the funnel
        category_id = int(
            client.crm.category.add(
                entity_type_id=entity_type_id,
                fields={"name": "New funnel", "sort": 100},
            ).response.result["category"]["id"]
        )
        entity_id = f"DYNAMIC_{entity_type_id}_STAGE_{category_id}"

        # 3. Retrieve the pre-installed stages
        stages = client.crm.status.list(
            filter={"ENTITY_ID": entity_id},
            order={"SORT": "ASC"},
        ).response.result

        if not stages:
            raise SystemExit(f"Stages not found: check the {entity_id} directory")

        # 4. Rename the first stage
        client.crm.status.update(
            int(stages[0]["ID"]),
            fields={"NAME": "First stage"},
        ).response

        # 5. Add your own stage to the "Failure" group
        client.crm.status.add(
            fields={
                "ENTITY_ID": entity_id,
                "STATUS_ID": f"DT{entity_type_id}_{category_id}:MY_STAGE",
                "NAME": "My stage",
                "SORT": 60,
                "SEMANTICS": "F",
            },
        ).response

        # Verify the result
        final_stages = client.crm.status.list(
            filter={"ENTITY_ID": entity_id},
            order={"SORT": "ASC"},
        ).response.result
    except BitrixAPIError as error:
        print(f"Error: {error}")
    else:
        print_stages_table(final_stages)
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

    $crm = $sb->getCRMScope();
    $processTitle = 'Equipment procurement'; // Name of your smart process

    try {
        // 1. Retrieve the entityTypeId by the smart process name
        $types = $crm->type()->list(
            order: [],
            filter: ['title' => $processTitle]
        )->getTypes();
        if (empty($types)) {
            throw new \RuntimeException('Smart process not found: ' . $processTitle);
        }
        $entityTypeId = $types[0]->entityTypeId;

        // 2. Create the funnel
        $result = $sb->core->call(
            'crm.category.add',
            [
                'entityTypeId' => $entityTypeId,
                'fields' => ['name' => 'New funnel', 'sort' => 100]
            ]
        );
        $categoryId = $result->getResponseData()->getResult()['category']['id'];
        $entityId = 'DYNAMIC_' . $entityTypeId . '_STAGE_' . $categoryId;

        // 3. Retrieve the pre-installed stages
        $stages = $crm->status()->list(
            order: ['SORT' => 'ASC'],
            filter: ['ENTITY_ID' => $entityId]
        )->getStatuses();

        if (empty($stages)) {
            throw new \RuntimeException('Stages not found: check the directory ' . $entityId);
        }

        // 4. Rename the first stage
        $crm->status()->update($stages[0]->ID, ['NAME' => 'First stage']);

        // 5. Add your own stage to the "Failure" group
        $crm->status()->add([
            'ENTITY_ID' => $entityId,
            'STATUS_ID' => 'DT' . $entityTypeId . '_' . $categoryId . ':MY_STAGE',
            'NAME' => 'My stage',
            'SORT' => 60,
            'SEMANTICS' => 'F',
        ]);

        // Verify the result
        $finalStages = $crm->status()->list(
            order: ['SORT' => 'ASC'],
            filter: ['ENTITY_ID' => $entityId]
        )->getStatuses();
    } catch (\Throwable $e) {
        echo 'Error: ' . $e->getMessage();
        exit;
    }

    $columns = ['In Progress' => [], 'Success' => [], 'Failure' => []];

    foreach ($finalStages as $stage) {
        if ($stage->SEMANTICS === 'S') {
            $columns['Success'][] = $stage->NAME;
        } elseif ($stage->SEMANTICS === 'F') {
            $columns['Failure'][] = $stage->NAME;
        } else {
            $columns['In Progress'][] = $stage->NAME;
        }
    }

    $maxRows = max(
        count($columns['In Progress']),
        count($columns['Success']),
        count($columns['Failure'])
    );

    echo "Stage table:\n";
    for ($i = 0; $i < $maxRows; $i++) {
        echo 'In Progress: ' . ($columns['In Progress'][$i] ?? '')
            . ' | Success: ' . ($columns['Success'][$i] ?? '')
            . ' | Failure: ' . ($columns['Failure'][$i] ?? '') . "\n";
    }
    ```
{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/category/crm-category-add.md)
- [{#T}](../../../api-reference/crm/universal/category/crm-category-list.md)
- [{#T}](../../../api-reference/crm/universal/category/crm-category-delete.md)
- [{#T}](../../../api-reference/crm/status/crm-status-add.md)
- [{#T}](../../../api-reference/crm/status/crm-status-list.md)
- [{#T}](../../../api-reference/crm/status/crm-status-update.md)
- [{#T}](../../../api-reference/crm/status/crm-status-delete.md)
- [{#T}](../../../api-reference/crm/status/crm-status-entity-types.md)
- [{#T}](../../../api-reference/crm/universal/user-defined-object-types/crm-type-list.md)
