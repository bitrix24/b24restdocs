# How to Retrieve a List of Tasks from Deals

> Scope: [`crm, user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: a user with read access to deals in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The task list allows you to track current tasks and calls related to deals, deadlines, and responsible parties. To create a task table, we will sequentially execute the following methods:

1. [user.current](../../../api-reference/user/user-current.md) — find the `ID` of the current user,

2. [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — retrieve the `ID` of all deals for which the employee is responsible,

3. [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — generate a list of tasks related to the deals,

4. [user.get](../../../api-reference/user/user-get.md) — obtain information about the individuals responsible for the tasks.

## 1. Retrieve the ID of the Current User

To get the identifier of the current user, we will use the [user.current](../../../api-reference/user/user-current.md) method.

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

-  JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: 'user.current',
        params: {}
    });
    ```

-  PHP

    ```php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $currentUser = $sb->getUserScope()->user()->current()->user();
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

    result = client.user.current().response.result
    ```

{% endlist %}

As a result, we will receive the user identifier `"ID": "29"`.

```json
{
    "result": {
        "ID": "29",
        "ACTIVE": true,
        "NAME": "Klaus",
        "LAST_NAME": "Weber",
        ...
    }
}
```

## 2. Retrieve the List of Deal IDs for the Employee

To obtain the identifiers of the deals assigned to the employee, we will call the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method. We will pass the following parameters:

-  `entityTypeId` — the identifier for the CRM object type. You can retrieve the identifiers using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. We will specify the value — `2`, which corresponds to a deal.

-  `select` — an array of fields to select. We will specify `select: ['id','title']` to get the identifiers and titles of the deals.

-  `filter` — a selection filter. To filter deals by the `ID` of the responsible employee, we will specify the user identifier obtained in the previous request `assignedById: 29`.

{% note info "" %}

To make the request faster and return only relevant data, add a filter by stages `stageId`. For example, you can select deals in the *In Progress* stage.

[How to Filter Items by Stage Name](../../../tutorials/crm/how-to-get-lists/how-to-get-elements-by-stage-filter.md)

{% endnote %}

{% list tabs %}

-  JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.item.list',
        params: {
            entityTypeId: 2,
            select: ['id', 'title'],
            filter: {
                assignedById: 29
            }
        }
    });
    ```

-  PHP

    ```php
    $items = $sb->getCRMScope()->item()->list(
        2,
        [],
        ['assignedById' => 29],
        ['id', 'title']
    )->getItems();
    ```

- Python

    ```python
    result = client.crm.item.list(
        entity_type_id=2,
        select=["id", "title"],
        filter={"assignedById": 29},
    ).response.result
    ```

{% endlist %}

As a result, we will receive an array `items` with deal identifiers like `"id": 5111`.

```json
{
    "result": {
        "items": [
            { "id": 5111, "title": "Deal №1" },
            { "id": 5199, "title": "Deal №2" },
            { "id": 5257, "title": "Deal №3" }
        ]
    },
    "total": 3
}
```

## 3. Retrieve the List of Tasks for the Found Deals

To obtain the list of tasks, we will use the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method.

To select tasks from multiple deals, we will use the binding key `BINDINGS` in the `filter`. We will pass an array of objects, where each object contains:

-  `OWNER_TYPE_ID` — the identifier for the CRM object type. You can retrieve the identifiers using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. We will specify the value — `2`, which corresponds to a deal.

-  `OWNER_ID` — the identifier of the deal from the result of the previous request.

We will also filter only active tasks `COMPLETED: 'N'`.

We will output the following fields in the `select`:

-  `ID` — the identifier of the task,

-  `OWNER_ID` — the identifier of the deal,

-  `SUBJECT` — the description of the task,

-  `DEADLINE` — the date and time of the deadline,

-  `RESPONSIBLE_ID` — the identifier of the user responsible for the task.

{% list tabs %}

-  JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.activity.list',
        params: {
            filter: {
                BINDINGS: [
                    { OWNER_TYPE_ID: 2, OWNER_ID: 5111 },
                    { OWNER_TYPE_ID: 2, OWNER_ID: 5199 },
                    { OWNER_TYPE_ID: 2, OWNER_ID: 5257 }
                ],
                COMPLETED: 'N'
            },
            select: ['ID', 'OWNER_ID', 'SUBJECT', 'DEADLINE', 'RESPONSIBLE_ID']
        }
    });
    ```

-  PHP

    ```php
    $activities = $sb->getCRMScope()->activity()->list(
        [],
        [
            'BINDINGS' => [
                ['OWNER_TYPE_ID' => 2, 'OWNER_ID' => 5111],
                ['OWNER_TYPE_ID' => 2, 'OWNER_ID' => 5199],
                ['OWNER_TYPE_ID' => 2, 'OWNER_ID' => 5257]
            ],
            'COMPLETED' => 'N'
        ],
        ['ID', 'OWNER_ID', 'SUBJECT', 'DEADLINE', 'RESPONSIBLE_ID'],
        0
    )->getActivities();
    ```

- Python

    ```python
    result = client.crm.activity.list(
        filter={
            "BINDINGS": [
                {"OWNER_TYPE_ID": 2, "OWNER_ID": 5111},
                {"OWNER_TYPE_ID": 2, "OWNER_ID": 5199},
                {"OWNER_TYPE_ID": 2, "OWNER_ID": 5257},
            ],
            "COMPLETED": "N",
        },
        select=["ID", "OWNER_ID", "SUBJECT", "DEADLINE", "RESPONSIBLE_ID"],
    ).response.result
    ```

{% endlist %}

As a result, you will obtain a list of activities with a description for each activity.

```json
{
    "result": [
        {
            "ID": "10120",
            "OWNER_ID": "5111",
            "SUBJECT": "Call client",
            "DEADLINE": "2025-08-21T16:00:00+03:00",
            "RESPONSIBLE_ID": "29"
        },
        {
            "ID": "10131",
            "OWNER_ID": "5199",
            "SUBJECT": "Check contract",
            "DEADLINE": "2025-08-29T16:00:00+03:00",
            "RESPONSIBLE_ID": "47"
        },
        ...
    ],
    "total": 5
}
```

## 4. Retrieve User Data by RESPONSIBLE_ID

The individual responsible for a task in a deal can be any user, not just the one responsible for the deal. To display the name and surname of the person responsible for the task in the table, we will use the [user.get](../../../api-reference/user/user-get.md) method.

Pass the assigned user identifiers `ID: [29, 47, ...]` in the `filter` filter.

{% list tabs %}

-  JS

    ```javascript
    const result = await $b24.actions.v2.call.make({
        method: 'user.get',
        params: {
            filter: {
                ID: [29, 47]
            }
        }
    });
    ```

-  PHP

    ```php
    $users = $sb->getUserScope()->user()->get(
        [],
        ['ID' => [29, 47]]
    )->getUsers();
    ```

- Python

    ```python
    result = client.user.get(
        filter={
            "ID": [29, 47],
        }
    ).response.result
    ```

{% endlist %}

As a result, we will receive information about the users.

```json
{
    "result": [
        {
            "ID": "29",
            "XML_ID": "23699770",
            "ACTIVE": true,
            "NAME": "Klaus",
            "LAST_NAME": "Weber"
        },
        {
            "ID": "47",
            "XML_ID": "63726962",
            "ACTIVE": true,
            "NAME": "Peter",
            "LAST_NAME": "Schmidt"
        },
        ...
    ],
    "total": 3,
}
```

## Code Example

{% list tabs %}

-  JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Function for forming an array of links to deals
    // CRM object type OWNER_TYPE_ID — 2, i.e., deal
    function buildBindingsFromDealIds(dealIds) {
        return dealIds.map((id) => ({ OWNER_TYPE_ID: 2, OWNER_ID: id }));
    }

    // Function to get all elements using pagination
    // Required for list methods, as one request retrieves a maximum of 50 records
    async function fetchAllItems(method, params) {
        let allResults = [];
        let start = 0;
        const batchSize = 50;

        while (true) {
            const result = await $b24.actions.v2.call.make({
                method,
                params: { ...params, start }
            });
            if (!result.isSuccess) {
                throw new Error(`Error getting data from ${method}: ${result.getErrorMessages().join('; ')}`);
            }

            const data = result.getData().result;

            // Processing results depending on the method
            let pageItems;
            if (method === 'crm.item.list') {
                pageItems = data.items || [];
            } else if (Array.isArray(data)) {
                pageItems = data;
            } else {
                pageItems = data.result || [];
            }
            allResults = allResults.concat(pageItems);

            // Checking if there is more data
            if (pageItems.length < batchSize) {
                break;
            }
            start += batchSize;
        }

        return allResults;
    }

    // Step 1: Get information about the current user
    const userResult = await $b24.actions.v2.call.make({ method: 'user.current', params: {} });
    if (!userResult.isSuccess) {
        throw new Error('Error getting user: ' + userResult.getErrorMessages().join('; '));
    }
    const userId = Number(userResult.getData().result.ID);
    console.log('Current user ID:', userId);

    // Step 2: Get the list of all deals
    const allItems = await fetchAllItems('crm.item.list', {
        entityTypeId: 2,
        select: ['id', 'title'],
        filter: { assignedById: userId }
    });

    const dealIds = allItems.map(it => it.id);
    const dealMap = allItems.reduce((map, deal) => {
        map[deal.id] = deal.title;
        return map;
    }, {});

    console.log('Deals:', dealMap);

    if (dealIds.length === 0) {
        console.log('Employee has no deals');
    } else {
        // Forming links to search for tasks by deals
        const bindings = buildBindingsFromDealIds(dealIds);

        // Step 3: Get all tasks linked to these deals
        const allActivities = await fetchAllItems('crm.activity.list', {
            filter: { BINDINGS: bindings, COMPLETED: 'N' },
            select: ['ID', 'OWNER_ID', 'SUBJECT', 'DEADLINE', 'RESPONSIBLE_ID']
        });

        const userIds = [...new Set(allActivities.map(a => a.RESPONSIBLE_ID))];

        if (userIds.length === 0) {
            console.log('No incomplete tasks for these deals.');
            console.table([]);
        } else {
            // Step 4: Get user data
            const usersResult = await $b24.actions.v2.call.make({
                method: 'user.get',
                params: { filter: { ID: userIds } }
            });

            let userMap = {};
            if (usersResult.isSuccess) {
                userMap = usersResult.getData().result.reduce((map, user) => {
                    map[user.ID] = `${user.NAME || ''} ${user.LAST_NAME || ''}`.trim() || user.LOGIN;
                    return map;
                }, {});
            } else {
                console.error('Error getting users:', usersResult.getErrorMessages().join('; '));
            }

            const table = allActivities.map(a => ({
                activityId: a.ID,
                dealTitle: dealMap[a.OWNER_ID] || `Deal #${a.OWNER_ID}`,
                subject: a.SUBJECT,
                deadline: a.DEADLINE,
                responsibleId: a.RESPONSIBLE_ID,
                responsibleName: userMap[a.RESPONSIBLE_ID] || `User ${a.RESPONSIBLE_ID}`
            }));

            console.table(table);
        }
    }
    ```

-  PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Function for forming an array of links to deals
    // OWNER_TYPE_ID: 2 — CRM object type — deal
    function buildBindingsFromDealIds($dealIds) {
        $bindings = [];
        foreach ($dealIds as $id) {
            $bindings[] = [
                'OWNER_TYPE_ID' => 2,
                'OWNER_ID' => (int)$id,
            ];
        }
        return $bindings;
    }

    // Function to get all elements using pagination
    // Required for list methods, as one request retrieves a maximum of 50 records
    // $fetchPage($start) returns an array of elements for one page
    function fetchAllItems(callable $fetchPage) {
        $allResults = [];
        $start = 0;
        $batchSize = 50;

        do {
            $pageItems = $fetchPage($start);
            $allResults = array_merge($allResults, $pageItems);

            if (count($pageItems) < $batchSize) {
                break;
            }
            $start += $batchSize;
        } while (true);

        return $allResults;
    }

    $crm = $sb->getCRMScope();

    // Step 1: Get information about the current user
    $userId = $sb->getUserScope()->user()->current()->user()->ID;
    echo "Current user ID: $userId\n";

    // Step 2: Get the list of all deals
    $allItems = fetchAllItems(fn($start) => $crm->item()->list(
        2,
        [],
        ['assignedById' => $userId],
        ['id', 'title'],
        $start
    )->getItems());

    $dealMap = [];
    $dealIds = [];
    foreach ($allItems as $item) {
        $id = $item->id;
        $dealIds[] = $id;
        $dealMap[$id] = $item->title;
    }

    echo "Deals found: " . count($dealIds) . "\n";

    if (empty($dealIds)) {
        echo "Employee has no deals\n";
        exit;
    }

    // Forming links to search for tasks by deals
    $bindings = buildBindingsFromDealIds($dealIds);

    // Step 3: Get all tasks linked to these deals
    $allActivities = fetchAllItems(fn($start) => $crm->activity()->list(
        [],
        [
            'BINDINGS' => $bindings,
            'COMPLETED' => 'N',
        ],
        ['ID', 'OWNER_ID', 'SUBJECT', 'DEADLINE', 'RESPONSIBLE_ID'],
        $start
    )->getActivities());

    if (empty($allActivities)) {
        echo "No incomplete tasks for these deals.\n";
        echo implode("\t", ['Task ID', 'Deal', 'Subject', 'Deadline', 'Responsible person']) . "\n";
        exit;
    }

    // Collecting unique IDs of responsible persons
    $responsibleIds = [];
    foreach ($allActivities as $a) {
        $responsibleIds[$a->RESPONSIBLE_ID] = true;
    }
    $responsibleIds = array_keys($responsibleIds);
    $userMap = [];

    if (!empty($responsibleIds)) {
        // Step 4: Get user data
        $users = $sb->getUserScope()->user()->get([], ['ID' => $responsibleIds])->getUsers();
        foreach ($users as $user) {
            $fullName = trim(($user->NAME ?? '') . ' ' . ($user->LAST_NAME ?? ''));
            $userMap[$user->ID] = $fullName ?: ($user->LOGIN ?? "User {$user->ID}");
        }
    }

    // Forming and displaying the table
    $header = ['Task ID', 'Deal', 'Subject', 'Deadline', 'Responsible person'];
    echo implode("\t", $header) . "\n";

    foreach ($allActivities as $a) {
        $activityId = $a->ID;
        $ownerId = (int)$a->OWNER_ID;
        $dealTitle = $dealMap[$ownerId] ?? "Deal #{$ownerId}";
        $subject = $a->SUBJECT ?? '';
        $deadline = $a->DEADLINE;
        $responsibleId = $a->RESPONSIBLE_ID;
        $responsibleName = $userMap[$responsibleId] ?? "User {$responsibleId} (not found)";

        echo implode("\t", [
            $activityId,
            $dealTitle,
            $subject,
            $deadline,
            $responsibleName
        ]) . "\n";
    }
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    def build_bindings_from_deal_ids(deal_ids):
        return [{"OWNER_TYPE_ID": 2, "OWNER_ID": deal_id} for deal_id in deal_ids]

    def fetch_all_items(fetch_page, data_key=None):
        all_results = []
        start = 0
        batch_size = 50

        while True:
            response = fetch_page(start)
            if data_key is None:
                page_items = response.result or []
            else:
                page_items = response.result.get(data_key, [])

            all_results.extend(page_items)

            if len(page_items) < batch_size:
                break

            start += batch_size

        return all_results

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    current = client.user.current().response.result
    user_id = int(current["ID"])
    print(f"Current user ID: {user_id}")

    all_items = fetch_all_items(
        lambda start: client.crm.item.list(
            entity_type_id=2,
            select=["id", "title"],
            filter={"assignedById": user_id},
            start=start,
        ).response,
        data_key="items",
    )

    deal_ids = [int(item["id"]) for item in all_items]
    deal_map = {int(item["id"]): item["title"] for item in all_items}

    print(f"Deals found: {len(deal_ids)}")

    if not deal_ids:
        print("Employee has no deals")
    else:
        bindings = build_bindings_from_deal_ids(deal_ids)

        all_activities = fetch_all_items(
            lambda start: client.crm.activity.list(
                filter={
                    "BINDINGS": bindings,
                    "COMPLETED": "N",
                },
                select=["ID", "OWNER_ID", "SUBJECT", "DEADLINE", "RESPONSIBLE_ID"],
                start=start,
            ).response
        )

        if not all_activities:
            print("No incomplete tasks for these deals.")
            print("\t".join(["Task ID", "Deal", "Subject", "Deadline", "Responsible person"]))
        else:
            responsible_ids = sorted(
                {
                    int(item["RESPONSIBLE_ID"])
                    for item in all_activities
                    if item.get("RESPONSIBLE_ID")
                }
            )

            user_map = {}
            if responsible_ids:
                users = fetch_all_items(
                    lambda start: client.user.get(
                        filter={"ID": responsible_ids},
                        start=start,
                    ).response
                )
                for user in users:
                    full_name = f"{user.get('NAME', '')} {user.get('LAST_NAME', '')}".strip()
                    user_map[str(user["ID"])] = full_name or user.get("LOGIN", f"User {user['ID']}")

            print("\t".join(["Task ID", "Deal", "Subject", "Deadline", "Responsible person"]))
            for activity in all_activities:
                activity_id = activity.get("ID", "")
                owner_id = int(activity.get("OWNER_ID", 0))
                deal_title = deal_map.get(owner_id, f"Deal #{owner_id}")
                subject = activity.get("SUBJECT", "")
                deadline = activity.get("DEADLINE", "")
                responsible_id = activity.get("RESPONSIBLE_ID", "")
                responsible_name = user_map.get(str(responsible_id), f"User {responsible_id} (not found)")

                print(
                    "\t".join(
                        [
                            str(activity_id),
                            str(deal_title),
                            str(subject),
                            str(deadline),
                            str(responsible_name),
                        ]
                    )
                )
    ```

{% endlist %}
