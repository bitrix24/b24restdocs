# How to Retrieve a List of Activities from Deals

> Scope: [`crm`, `user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — read access to deals in CRM
>
> - [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — a user with permission to read items of a CRM object
> - [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — any user
> - [user.current](../../../api-reference/user/user-current.md) and [user.get](../../../api-reference/user/user-get.md) — any user

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Activities are calls, meetings, emails, and other actions on a CRM item. A list of incomplete activities shows what is left to do on an employee's deals, by when, and who is responsible for it.

There is no method that returns the activities for all of an employee's deals at once: activities are bound to CRM items, and they can be filtered only by those items or by the user responsible for the activity itself. The user responsible for an activity and the user responsible for the deal are different roles, so we will first find the employee's deals and then request the activities by the identifiers of those deals.

As a result of the scenario, we get a table of incomplete activities: the activity identifier, the deal title, the activity description, the deadline, and the name of the responsible user.

The scenario consists of four steps.

1. Find the `ID` of the current user using the [user.current](../../../api-reference/user/user-current.md) method
2. Retrieve the `ID` of the deals the employee is responsible for using the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method
3. Retrieve the activities for those deals using the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method
4. Retrieve the names of the users responsible for the activities using the [user.get](../../../api-reference/user/user-get.md) method

## Before You Start

- The webhook is created on behalf of the employee whose deals and activities need to be retrieved. The methods return data according to that user's permissions: whatever they do not see in CRM does not get into the selection

- The `crm` and `user_brief` scopes are selected in the webhook permissions

- The employee has at least one deal with an incomplete activity, otherwise steps 3 and 4 return an empty result

- The webhook URL grants full access within its scope. Retain the URL in an environment variable and never publish it in open code

The identifiers in the examples — `29` for the user and `5111`, `5199`, and `5257` for the deals — are taken from one Bitrix24. In your Bitrix24 they will be different. Each subsequent step substitutes the values from the response of the previous one.

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

- Go

    ```go
    res, err := core.Call(ctx, "user.current", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("user.current: %w", err)
    }

    // The ID arrives AS A STRING ("29"): b24.ID parses both a number and a string
    // containing a number, a plain int fails here.
    var me struct {
    	ID       b24.ID `json:"ID"`
    	Name     string `json:"NAME"`
    	LastName string `json:"LAST_NAME"`
    }
    if err := json.Unmarshal(res.Result, &me); err != nil {
    	return fmt.Errorf("parse the current user: %w", err)
    }
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
        "EMAIL": "k.weber@example.com",
        "WORK_POSITION": "Manager",
        "USER_TYPE": "employee"
    }
}
```

The response is abridged: the method also returns the remaining profile fields. The identifier comes as a string, not as a number. Retain the value of the `ID` field — in step 2 it becomes the value of the `assignedById` filter.

## 2. Retrieve the List of Deal IDs for the Employee

To obtain the identifiers of the deals assigned to the employee, we will call the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method. We will pass the following parameters:

-  `entityTypeId` — the identifier of the CRM object type, a required parameter. We will specify `2` — a deal. The values for other objects are returned by the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method

-  `select` — the fields to retrieve. We will specify `id` and `title`: the identifier is needed for step 3, the title — for the final table

-  `filter` — the selection conditions. To select deals by the responsible user, we will specify `assignedById` with the `ID` value from step 1. In the example, `29`

{% note info "" %}

To narrow the selection, add a filter by stages `stageId`. For example, you can select only deals in the "In Progress" stage.

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

- Go

    ```go
    // A list method returns at most 50 records per request. Pages walks
    // the cursor itself: the ABSENCE of next ends the list, because next: 0 is
    // a legitimate first offset rather than a sign of the end.
    pager, err := core.Pages("crm.item.list", b24.Params{
    	"entityTypeId": entityTypeDeal,
    	"select":       []string{"id", "title"},
    	"filter":       b24.Params{"assignedById": me.ID},
    }, b24.WithCallOptions(b24.WithIdempotent()))
    if err != nil {
    	return fmt.Errorf("crm.item.list: %w", err)
    }

    var deals []struct {
    	ID    int    `json:"id"`
    	Title string `json:"title"`
    }
    for pager.Next(ctx) {
    	for _, row := range pager.Rows() {
    		var d struct {
    			ID    int    `json:"id"`
    			Title string `json:"title"`
    		}
    		if err := json.Unmarshal(row, &d); err != nil {
    			return fmt.Errorf("parse deal: %w", err)
    		}
    		deals = append(deals, d)
    	}
    	if len(deals) >= maxDeals {
    		break
    	}
    }
    // The error surfaces HERE, after the loop: Next returns false both at the end
    // of the list and on an error.
    if err := pager.Err(); err != nil {
    	return fmt.Errorf("traverse deals: %w", err)
    }
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

Here `id` comes as a number, unlike the `ID` from step 1. Retain two fields: `id` — the filter for step 3 is assembled from it, and `title` — it labels the rows of the final table.

The list methods of the scenario — this one and [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) in step 3 — return no more than 50 records per call. If the `total` field is greater than 50, retrieve the remaining pages with repeated calls using the `start` parameter: `50`, `100`, and so on. In the code example below, the pages are iterated automatically.

## 3. Retrieve the List of Activities for the Found Deals

To retrieve the activities for the found deals, we will use the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method.

To select activities from several deals at once, we will use the `BINDINGS` key in the `filter` — it sets the binding to CRM items. We will pass an array of objects, where each object contains:

-  `OWNER_TYPE_ID` — the identifier of the CRM object type. We will specify `2` — the same deal type as in step 2

-  `OWNER_ID` — the identifier of the deal from the `id` field of step 2

We will also filter only incomplete activities `COMPLETED: 'N'`.

In the `select` parameter, we will specify the following fields:

-  `ID` — the identifier of the activity

-  `OWNER_ID` — the identifier of the deal

-  `SUBJECT` — the description of the activity

-  `DEADLINE` — the date and time of the deadline

-  `RESPONSIBLE_ID` — the identifier of the user responsible for the activity

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

- Go

    ```go
    // BINDINGS is an array of bindings: one object per deal.
    bindings := make([]b24.Params, 0, len(deals))
    for _, d := range deals {
    	bindings = append(bindings, b24.Params{"OWNER_TYPE_ID": entityTypeDeal, "OWNER_ID": d.ID})
    }

    pager, err = core.Pages("crm.activity.list", b24.Params{
    	"filter": b24.Params{"BINDINGS": bindings, "COMPLETED": "N"},
    	"select": []string{"ID", "OWNER_ID", "SUBJECT", "DEADLINE", "RESPONSIBLE_ID"},
    }, b24.WithCallOptions(b24.WithIdempotent()))
    if err != nil {
    	return fmt.Errorf("crm.activity.list: %w", err)
    }

    // Activity fields are in UPPERCASE, whereas crm.item.list returns camelCase.
    var activities []activity
    for pager.Next(ctx) {
    	for _, row := range pager.Rows() {
    		var a activity
    		if err := json.Unmarshal(row, &a); err != nil {
    			return fmt.Errorf("parse activity: %w", err)
    		}
    		activities = append(activities, a)
    	}
    }
    if err := pager.Err(); err != nil {
    	return fmt.Errorf("traverse activities: %w", err)
    }
    ```

{% endlist %}

As a result, we get a list of incomplete activities for the specified deals.

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
        {
            "ID": "10145",
            "OWNER_ID": "5257",
            "SUBJECT": "Approve delivery",
            "DEADLINE": "9999-12-31T00:00:00+03:00",
            "RESPONSIBLE_ID": "29"
        }
    ],
    "total": 3
}
```

The activity fields come in uppercase, and the values come as strings. This differs from step 2: the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method returns fields in camelCase and identifiers as numbers.

What we take from the response next:

-  `OWNER_ID` — links the activity to the deal from step 2 and substitutes the deal title into the table

-  `RESPONSIBLE_ID` — the unique values are collected and passed to the filter in step 4

An activity with no deadline comes with the date `9999-12-31`. This is not an error: this is how Bitrix24 retains the "no deadline set" flag. In the table, it is better to show such activities without a date.

## 4. Retrieve User Data by RESPONSIBLE_ID

To display the name and surname of the user responsible for the activity in the table, we will use the [user.get](../../../api-reference/user/user-get.md) method.

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

- Go

    ```go
    res, err := core.Call(ctx, "user.get", b24.Params{
    	"filter": b24.Params{"ID": ids},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("user.get: %w", err)
    }
    var rows []struct {
    	ID       b24.ID `json:"ID"`
    	Name     string `json:"NAME"`
    	LastName string `json:"LAST_NAME"`
    }
    if err := json.Unmarshal(res.Result, &rows); err != nil {
    	return fmt.Errorf("parse employees: %w", err)
    }
    for _, u := range rows {
    	users[u.ID] = u.Name + " " + u.LastName
    }
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
        }
    ],
    "total": 2
}
```

The method returns only the users it found by the identifiers from the filter. Using the `ID` field, build an "identifier — first and last name" mapping and substitute the names into the table instead of the numbers from `RESPONSIBLE_ID`.

## Verify the Result

The scenario is complete if the table has a row with the deal, the deadline, and the name of the responsible user for every activity from step 3.

What to check in the responses:

-  The number of table rows matches the number of activities from step 3 after duplicates by the `ID` field are removed

-  Every `OWNER_ID` value from step 3 is present among the `id` values of the deals from step 2. If a deal is not found, the activity is bound to something other than a deal — check that only `OWNER_TYPE_ID`: `2` is passed in `BINDINGS`

-  Every `RESPONSIBLE_ID` value from step 3 is present among the `ID` values of the users from step 4. If a user is not found, they have been dismissed or deleted — show such activities with the identifier instead of the name

You can check the data in the interface in the deal card: open the deal with the identifier from `OWNER_ID` and compare the activities in its timeline with the rows of the table. The table contains only incomplete activities — the same ones Bitrix24 shows in the card as pending.

## Errors and Diagnostics

If the method returned an error, check the request data.

#|
|| **Code** | **Cause and Action** ||
|| `INVALID_ARG_VALUE` `Invalid filter: field 'field' is not allowed in filter` | In step 2, the `filter` holds a field that cannot be filtered by. The list of available fields is returned by the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method with the same `entityTypeId` ||
|| `NOT_FOUND` | In step 2, `entityTypeId` holds a value that matches no CRM object. A deal requires `2` ||
|| `allowed_only_intranet_user` | The webhook is created on behalf of an external user. The scenario is available to Bitrix24 employees only ||
|| `INVALID_REQUEST` `Https required` | The request was sent over HTTP. Address Bitrix24 over HTTPS ||
|#

An empty result is not an error, but it means different things at different steps.

- An empty `items` in step 2 — the employee has no deals, or they do not see their deals due to permissions. Check `assignedById`: in step 1 the identifier comes as a string, so cast it to a number, as in the code examples

- An empty `result` in step 3 with a non-empty step 2 — there are no incomplete activities for the deals. Remove `COMPLETED`: `N` from the filter and repeat the call. If activities appear, they have all been completed already

All four methods only read data, so after an error the scenario can be repeated from any step.

## Key Considerations

- `BINDINGS` is a filter, not a batch request. The entire array of bindings goes into a single call, so it is worth narrowing the deal selection in step 2 in advance

- If an activity is bound to two deals from the selection at once, the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method returns it twice — one row per binding. Discard duplicates by the activity `ID` field

- To retrieve activities for another CRM object, replace `entityTypeId` in step 2 and `OWNER_TYPE_ID` in step 3. The values are returned by the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method: `1` — lead, `3` — contact, `4` — company

## Code Example

The code goes through all four steps and prints the activity table. No identifiers are hardcoded: the user comes from [user.current](../../../api-reference/user/user-current.md), and the deals and responsible users come from the responses of the previous steps. The only thing to replace is the webhook URL in the environment variable.

The Go example is self-contained: before step 2 it creates two deals of its own with activities, and deletes them at the end. The other examples change nothing.

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
        // Forming links to search for activities by deals
        const bindings = buildBindingsFromDealIds(dealIds);

        // Step 3: Get all activities linked to these deals
        const allActivities = await fetchAllItems('crm.activity.list', {
            filter: { BINDINGS: bindings, COMPLETED: 'N' },
            select: ['ID', 'OWNER_ID', 'SUBJECT', 'DEADLINE', 'RESPONSIBLE_ID']
        });

        const userIds = [...new Set(allActivities.map(a => a.RESPONSIBLE_ID))];

        if (userIds.length === 0) {
            console.log('No incomplete activities for these deals.');
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

    // Forming links to search for activities by deals
    $bindings = buildBindingsFromDealIds($dealIds);

    // Step 3: Get all activities linked to these deals
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
        echo "No incomplete activities for these deals.\n";
        echo implode("\t", ['Activity ID', 'Deal', 'Subject', 'Deadline', 'Responsible person']) . "\n";
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
    $header = ['Activity ID', 'Deal', 'Subject', 'Deadline', 'Responsible person'];
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
            print("No incomplete activities for these deals.")
            print("\t".join(["Activity ID", "Deal", "Subject", "Deadline", "Responsible person"]))
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

            print("\t".join(["Activity ID", "Deal", "Subject", "Deadline", "Responsible person"]))
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

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // The example is self-contained: it creates two deals of its own with activities, collects for them
    // a table of activities with the responsible persons and cleans up after itself. It runs on any
    // portal, nothing needs to be edited.
    package main

    import (
    	"context"
    	"encoding/json"
    	"fmt"
    	"log"
    	"os"
    	"sort"
    	"time"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // entityTypeDeal is the ID of the "deal" object type from crm.enum.ownertype.
    const entityTypeDeal = 2

    // maxDeals limits the deal selection. BINDINGS is a FILTER rather than a batch:
    // an array of a thousand bindings goes in a single request and weighs it down. On a production
    // portal, the deals should be narrowed down by a stage filter as well.
    const maxDeals = 50

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook path is a secret, so it comes from the environment, not from the code.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- step 1: the current user ID
    	res, err := core.Call(ctx, "user.current", nil, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("user.current: %w", err)
    	}

    	// The ID arrives AS A STRING ("29"): b24.ID parses both a number and a string
    	// containing a number, a plain int fails here.
    	var me struct {
    		ID       b24.ID `json:"ID"`
    		Name     string `json:"NAME"`
    		LastName string `json:"LAST_NAME"`
    	}
    	if err := json.Unmarshal(res.Result, &me); err != nil {
    		return fmt.Errorf("parse the current user: %w", err)
    	}
    	fmt.Printf("current user %d: %s %s\n", me.ID, me.Name, me.LastName)

    	// --- setup: our own deals with activities, so the table has something to show

    	cleanup, err := createDealsWithActivities(ctx, core, me.ID)
    	defer cleanup()
    	if err != nil {
    		return err
    	}

    	// --- step 2: the deals the employee is responsible for
    	// A list method returns at most 50 records per request. Pages walks
    	// the cursor itself: the ABSENCE of next ends the list, because next: 0 is
    	// a legitimate first offset rather than a sign of the end.
    	pager, err := core.Pages("crm.item.list", b24.Params{
    		"entityTypeId": entityTypeDeal,
    		"select":       []string{"id", "title"},
    		"filter":       b24.Params{"assignedById": me.ID},
    	}, b24.WithCallOptions(b24.WithIdempotent()))
    	if err != nil {
    		return fmt.Errorf("crm.item.list: %w", err)
    	}

    	var deals []struct {
    		ID    int    `json:"id"`
    		Title string `json:"title"`
    	}
    	for pager.Next(ctx) {
    		for _, row := range pager.Rows() {
    			var d struct {
    				ID    int    `json:"id"`
    				Title string `json:"title"`
    			}
    			if err := json.Unmarshal(row, &d); err != nil {
    				return fmt.Errorf("parse deal: %w", err)
    			}
    			deals = append(deals, d)
    		}
    		if len(deals) >= maxDeals {
    			break
    		}
    	}
    	// The error surfaces HERE, after the loop: Next returns false both at the end
    	// of the list and on an error.
    	if err := pager.Err(); err != nil {
    		return fmt.Errorf("traverse deals: %w", err)
    	}
    	fmt.Printf("deals of the employee taken: %d\n", len(deals))
    	if len(deals) == 0 {
    		return nil
    	}

    	// --- step 3: the activities of these deals
    	// BINDINGS is an array of bindings: one object per deal.
    	bindings := make([]b24.Params, 0, len(deals))
    	for _, d := range deals {
    		bindings = append(bindings, b24.Params{"OWNER_TYPE_ID": entityTypeDeal, "OWNER_ID": d.ID})
    	}

    	pager, err = core.Pages("crm.activity.list", b24.Params{
    		"filter": b24.Params{"BINDINGS": bindings, "COMPLETED": "N"},
    		"select": []string{"ID", "OWNER_ID", "SUBJECT", "DEADLINE", "RESPONSIBLE_ID"},
    	}, b24.WithCallOptions(b24.WithIdempotent()))
    	if err != nil {
    		return fmt.Errorf("crm.activity.list: %w", err)
    	}

    	// Activity fields are in UPPERCASE, whereas crm.item.list returns camelCase.
    	var activities []activity
    	for pager.Next(ctx) {
    		for _, row := range pager.Rows() {
    			var a activity
    			if err := json.Unmarshal(row, &a); err != nil {
    				return fmt.Errorf("parse activity: %w", err)
    			}
    			activities = append(activities, a)
    		}
    	}
    	if err := pager.Err(); err != nil {
    		return fmt.Errorf("traverse activities: %w", err)
    	}
    	fmt.Printf("active activities: %d\n", len(activities))

    	// --- step 4: the persons responsible for the activities

    	// The person responsible for an activity may differ from the one responsible for the deal, so
    	// the IDs are taken from the activities themselves.
    	ids := uniqueIDs(activities)
    	users := map[b24.ID]string{}
    	if len(ids) > 0 {
    		res, err := core.Call(ctx, "user.get", b24.Params{
    			"filter": b24.Params{"ID": ids},
    		}, b24.WithIdempotent())
    		if err != nil {
    			return fmt.Errorf("user.get: %w", err)
    		}
    		var rows []struct {
    			ID       b24.ID `json:"ID"`
    			Name     string `json:"NAME"`
    			LastName string `json:"LAST_NAME"`
    		}
    		if err := json.Unmarshal(res.Result, &rows); err != nil {
    			return fmt.Errorf("parse employees: %w", err)
    		}
    		for _, u := range rows {
    			users[u.ID] = u.Name + " " + u.LastName
    		}
    	}

    	titles := map[int]string{}
    	for _, d := range deals {
    		titles[d.ID] = d.Title
    	}
    	fmt.Println("Activity\tDeal\tDescription\tDeadline\tResponsible")
    	for _, a := range activities {
    		who := users[a.ResponsibleID]
    		if who == "" {
    			who = "Unknown"
    		}
    		fmt.Printf("%d\t%s\t%s\t%s\t%s\n", a.ID, titles[int(a.OwnerID)], a.Subject, a.Deadline, who)
    	}
    	return nil
    }

    // activity is a single row of the crm.activity.list response.
    type activity struct {
    	ID            b24.ID `json:"ID"`
    	OwnerID       b24.ID `json:"OWNER_ID"`
    	Subject       string `json:"SUBJECT"`
    	Deadline      string `json:"DEADLINE"`
    	ResponsibleID b24.ID `json:"RESPONSIBLE_ID"`
    }

    func uniqueIDs(activities []activity) []b24.ID {
    	seen := map[b24.ID]bool{}
    	out := make([]b24.ID, 0, len(activities))
    	for _, a := range activities {
    		if a.ResponsibleID > 0 && !seen[a.ResponsibleID] {
    			seen[a.ResponsibleID] = true
    			out = append(out, a.ResponsibleID)
    		}
    	}
    	sort.Slice(out, func(i, j int) bool { return out[i] < out[j] })
    	return out
    }

    // --- helpers: data setup and cleanup

    // createDealsWithActivities creates two deals and one activity in each. It returns
    // a cleanup function — it is called even if the setup broke off halfway.
    func createDealsWithActivities(ctx context.Context, core *b24.Core, userID b24.ID) (func(), error) {
    	var dealIDs []b24.ID
    	cleanup := func() {
    		// Deleting a deal also removes its activities.
    		for _, id := range dealIDs {
    			del(ctx, core, "crm.item.delete", b24.Params{"entityTypeId": entityTypeDeal, "id": id})
    		}
    	}

    	for i, spec := range []struct {
    		title string
    		task  string
    		in    time.Duration
    	}{
    		{"Oven procurement (b24gosdk example)", "Call client", 24 * time.Hour},
    		{"Blender procurement (b24gosdk example)", "Send the invoice", 48 * time.Hour},
    	} {
    		res, err := core.Call(ctx, "crm.item.add", b24.Params{
    			"entityTypeId": entityTypeDeal,
    			"fields": b24.Params{
    				"title":        spec.title,
    				"assignedById": userID,
    				"opportunity":  (i + 1) * 100,
    			},
    		})
    		if err != nil {
    			return cleanup, fmt.Errorf("crm.item.add: %w", err)
    		}
    		raw, ok := b24.Unwrap(res.Result, "item", "id")
    		if !ok {
    			return cleanup, fmt.Errorf("no item.id in %s", res.Result)
    		}
    		var dealID b24.ID
    		if err := json.Unmarshal(raw, &dealID); err != nil {
    			return cleanup, err
    		}
    		dealIDs = append(dealIDs, dealID)

    		if _, err := core.Call(ctx, "crm.activity.todo.add", b24.Params{
    			"ownerTypeId":   entityTypeDeal,
    			"ownerId":       dealID,
    			"title":         spec.task,
    			"description":   "Activity created by the b24gosdk example",
    			"deadline":      time.Now().Add(spec.in).Format(time.RFC3339),
    			"responsibleId": userID,
    		}); err != nil {
    			return cleanup, fmt.Errorf("crm.activity.todo.add: %w", err)
    		}
    	}
    	return cleanup, nil
    }

    // del removes what was created. A cleanup error is printed but not returned: it must not
    // mask the real error of the scenario.
    func del(ctx context.Context, core *b24.Core, method string, params b24.Params) {
    	if _, err := core.Call(ctx, method, params); err != nil {
    		fmt.Fprintf(os.Stderr, "cleanup, %s: %v
", method, err)
    	}
    }
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-list.md)
- [{#T}](../../../api-reference/user/user-get.md)
- [{#T}](./how-to-get-elements-by-stage-filter.md)
- [{#T}](../how-to-edit-crm-objects/how-to-move-activity.md)
