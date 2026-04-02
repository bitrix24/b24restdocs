# How to Retrieve a List of Tasks from Deals

> Scope: [`crm, user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: a user with read access to deals in CRM

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

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
    BX24.callMethod(
        'user.current',
        {}
    );
    ```

-  PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.current',
        []
    );
    ```

{% endlist %}

As a result, we will receive the user identifier `"ID": "29"`.

```json
{
    "result": {
        "ID": "29",
        "ACTIVE": true,
        "NAME": "John",
        "LAST_NAME": "Doe",
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
    BX24.callMethod(
        'crm.item.list',
        {
            entityTypeId: 2,
            select: ['id','title'],
            filter: {
                assignedById: 29
            }
        }
    );
    ```

-  PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.item.list',
        [
            'entityTypeId' => 2,
            'select' => ['id','title'],
            'filter' => [
                'assignedById' => 29
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive an array `items` with deal identifiers like `"id": 5111`.

```json
{
    "result": {
        "items": [
            { "id": 5111, "title": "Deal #1" },
            { "id": 5199, "title": "Deal #2" },
            { "id": 5257, "title": "Deal #3" }
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
    BX24.callMethod(
        'crm.activity.list',
        {
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
    );
    ```

-  PHP

    ```php
    require_once('crest.php');
    
    $result = CRest::call(
        'crm.activity.list',
        [
            'filter' => [
                'BINDINGS' => [
                    ['OWNER_TYPE_ID' => 2, 'OWNER_ID' => 5111],
                    ['OWNER_TYPE_ID' => 2, 'OWNER_ID' => 5199],
                    ['OWNER_TYPE_ID' => 2, 'OWNER_ID' => 5257]
                ],
                'COMPLETED' => 'N'
            ],
            'select' => ['ID', 'OWNER_ID', 'SUBJECT', 'DEADLINE', 'RESPONSIBLE_ID']
        ]
    );
    ```

{% endlist %}

As a result, we will receive a list of tasks with descriptions for each task.

```json
{
    "result": [
        {
            "ID": "10120",
            "OWNER_ID": "5111",
            "SUBJECT": "Call the client",
            "DEADLINE": "2025-08-21T16:00:00+02:00",
            "RESPONSIBLE_ID": "29"
        },
        {
            "ID": "10131",
            "OWNER_ID": "5199",
            "SUBJECT": "Check the contract",
            "DEADLINE": "2025-08-29T16:00:00+02:00",
            "RESPONSIBLE_ID": "47"
        },
        ...
    ],
    "total": 5
}
```

## 4. Retrieve User Data by RESPONSIBLE_ID

The individual responsible for a task in a deal can be any user, not just the one responsible for the deal. To display the name and surname of the person responsible for the task in the table, we will use the [user.get](../../../api-reference/user/user-get.md) method.

In the `filter`, we will pass the identifiers of the responsible users `ID: [29, 47, ...]`.

{% list tabs %}

-  JS

    ```javascript
    BX24.callMethod(
        'user.get',
        {
            filter: {
                ID: [29, 47, ...]
            }
        }
    );
    ```

-  PHP

    ```php
    require_once('crest.php');
    
    $result = CRest::call(
        'user.get',
        [
            'filter' => [
                'ID' => [29, 47, ...]
            ]
        }
    );
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
            "NAME": "John",
            "LAST_NAME": "Doe"
        },
        {
            "ID": "47",
            "XML_ID": "63726962",
            "ACTIVE": true,
            "NAME": "Peter",
            "LAST_NAME": "Smith"
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
    // Function to build an array of bindings to deals
    // CRM object type OWNER_TYPE_ID — 2, which means deal
    function buildBindingsFromDealIds(dealIds) {
        return dealIds.map((id) => ({ OWNER_TYPE_ID: 2, OWNER_ID: id }));
    }
    
    // Function to retrieve all items using pagination
    // Necessary for list methods, as one request retrieves a maximum of 50 records
    function fetchAllItems(method, params, callback) {
        let allResults = [];
        
        function processResult(result) {
            if (result.error()) {
                console.error(`Error retrieving data from ${method}:`, result.error().ex);
                callback(result.error(), null);
                return;
            }
    
            const data = result.data();
            
            // Process results based on the method
            if (method === 'crm.item.list') {
                allResults = allResults.concat(data.items || []);
            } else if (method === 'crm.activity.list') {
                allResults = allResults.concat(data || []);
            } else {
                if (Array.isArray(data)) {
                    allResults = allResults.concat(data);
                } else if (data && Array.isArray(data.result)) {
                    allResults = allResults.concat(data.result);
                }
            }
    
            // Check if there are more data
            if (result.more && result.more()) {
                // Use result.next() to get the next page
                result.next(processResult);
            } else {
                // If there are no more pages, finish
                callback(null, allResults);
            }
        }
    
        // First request
        BX24.callMethod(method, params, processResult);
    }
    
    // Step 1: Retrieve information about the current user
    BX24.callMethod('user.current', {}, function(userResult) {
        if (userResult.error()) {
            console.error('Error retrieving user:', userResult.error().ex);
            return;
        }
    
        const current = userResult.data();
        const userId = Number(current.ID);
        console.log('Current user ID:', userId);
    
        // Step 2: Retrieve the list of all deals
        fetchAllItems('crm.item.list', {
            entityTypeId: 2,
            select: ['id', 'title'],
            filter: { assignedById: userId }
        }, function(error, allItems) {
            if (error) {
                console.error('Error retrieving all deals:', error.ex);
                return;
            }
    
            const dealIds = allItems.map(it => it.id);
            const dealMap = allItems.reduce((map, deal) => {
                map[deal.id] = deal.title;
                return map;
            }, {});
    
            console.log('Deals:', dealMap);
    
            if (dealIds.length === 0) {
                alert('The employee has no deals');
                return;
            }
    
            // Build bindings to search for tasks by deals
            const bindings = buildBindingsFromDealIds(dealIds);
    
            // Step 3: Retrieve all tasks linked to these deals
            fetchAllItems('crm.activity.list', {
                filter: { BINDINGS: bindings, COMPLETED: 'N' },
                select: ['ID', 'OWNER_ID', 'SUBJECT', 'DEADLINE', 'RESPONSIBLE_ID']
            }, function(error, allActivities) {
                if (error) {
                    console.error('Error retrieving all tasks:', error.ex);
                    return;
                }
    
                const userIds = [...new Set(allActivities.map(a => a.RESPONSIBLE_ID))];
    
                if (userIds.length === 0) {
                    console.log('No incomplete tasks for the deals.');
                    console.table([]);
                    return;
                }
    
                // Step 4: Retrieve user data
                BX24.callMethod('user.get', {
                    filter: { ID: userIds }
                }, function(userResult) {
                    if (userResult.error()) {
                        console.error('Error retrieving users:', userResult.error().ex);
                        const tableFallback = allActivities.map(a => ({
                            activityId: a.ID,
                            dealTitle: dealMap[a.OWNER_ID] || `Deal #${a.OWNER_ID}`,
                            subject: a.SUBJECT,
                            deadline: a.DEADLINE,
                            responsibleId: a.RESPONSIBLE_ID,
                            responsibleName: `User ${a.RESPONSIBLE_ID} (not found)`
                        }));
                        console.table(tableFallback);
                        return;
                    }
    
                    const users = userResult.data();
                    const userMap = users.reduce((map, user) => {
                        map[user.ID] = `${user.NAME || ''} ${user.LAST_NAME || ''}`.trim() || user.LOGIN;
                        return map;
                    }, {});
    
                    const table = allActivities.map(a => ({
                        activityId: a.ID,
                        dealTitle: dealMap[a.OWNER_ID] || `Deal #${a.OWNER_ID}`,
                        subject: a.SUBJECT,
                        deadline: a.DEADLINE,
                        responsibleId: a.RESPONSIBLE_ID,
                        responsibleName: userMap[a.RESPONSIBLE_ID] || `User ${a.RESPONSIBLE_ID}`
                    }));
    
                    console.table(table);
                });
            });
        });
    });
    ```

-  PHP

    ```php
    <?php
    
    require_once('crest.php');
    
    // Function to build an array of bindings to deals
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
    
    // Function to retrieve all items using pagination
    // Necessary for list methods, as one request retrieves a maximum of 50 records
    function fetchAllItems($method, $params) {
        $allResults = [];
        
        $start = 0;
        $batchSize = 50;
        
        do {
            $params['start'] = $start;
            $result = CRest::call($method, $params);
            
            if (!empty($result['error'])) {
                return ['error' => $result['error'], 'error_description' => $result['error_description']];
            }
            
            $data = $result['result'] ?? [];
            
            // Process results based on the method
            if ($method === 'crm.item.list') {
                if (!empty($data['items']) && is_array($data['items'])) {
                    $allResults = array_merge($allResults, $data['items']);
                }
            } elseif ($method === 'crm.activity.list') {
                if (is_array($data)) {
                    $allResults = array_merge($allResults, $data);
                }
            } else {
                if (is_array($data)) {
                    $allResults = array_merge($allResults, $data);
                }
            }
            
            // Check if there are more data
            if (count($data) >= $batchSize) {
                $start += $batchSize;
            } else {
                break;
            }
            
        } while (true);
        
        return ['result' => $allResults];
    }
    
    // Step 1: Retrieve information about the current user
    $userResult = CRest::call('user.current', []);
    
    if (!empty($userResult['error'])) {
        echo 'Error retrieving user: ' . $userResult['error_description'] . "\n";
        exit;
    }
    
    $current = $userResult['result'] ?? null;
    if (!$current || empty($current['ID'])) {
        echo "Failed to retrieve the current user\n";
        exit;
    }
    
    $userId = (int)$current['ID'];
    echo "Current user ID: $userId\n";
    
    // Step 2: Retrieve the list of all deals
    $itemsResult = fetchAllItems('crm.item.list', [
        'entityTypeId' => 2,
        'select' => ['id', 'title'],
        'filter' => ['assignedById' => $userId]
    ]);
    
    if (isset($itemsResult['error'])) {
        echo 'Error retrieving all deals: ' . $itemsResult['error_description'] . "\n";
        exit;
    }
    
    $allItems = $itemsResult['result'];
    
    $dealMap = [];
    $dealIds = [];
    foreach ($allItems as $item) {
        $id = (int)$item['id'];
        $dealIds[] = $id;
        $dealMap[$id] = $item['title'];
    }
    
    echo "Deals found: " . count($dealIds) . "\n";
    
    if (empty($dealIds)) {
        echo "The employee has no deals\n";
        exit;
    }
    
    // Build bindings to search for tasks by deals
    $bindings = buildBindingsFromDealIds($dealIds);
    
    // Step 3: Retrieve all tasks linked to these deals
    $activitiesResult = fetchAllItems('crm.activity.list', [
        'filter' => [
            'BINDINGS' => $bindings,
            'COMPLETED' => 'N',
        ],
        'select' => ['ID', 'OWNER_ID', 'SUBJECT', 'DEADLINE', 'RESPONSIBLE_ID']
    ]);
    
    if (isset($activitiesResult['error'])) {
        echo 'Error retrieving all tasks: ' . $activitiesResult['error_description'] . "\n";
        exit;
    }
    
    $allActivities = $activitiesResult['result'];
    
    if (empty($allActivities)) {
        echo "No incomplete tasks for the deals.\n";
        echo implode("\t", ['Task ID', 'Deal', 'Subject', 'Deadline', 'Responsible']) . "\n";
        exit;
    }
    
    // Collect unique responsible IDs
    $responsibleIds = array_unique(array_column($allActivities, 'RESPONSIBLE_ID'));
    $userMap = [];
    
    if (!empty($responsibleIds)) {
        // Step 4: Retrieve user data
        $usersResult = fetchAllItems('user.get', [
            'filter' => ['ID' => array_values($responsibleIds)]
        ]);
    
        if (isset($usersResult['error'])) {
            echo 'Error retrieving users: ' . $usersResult['error_description'] . "\n";
            $userMap = [];
        } else {
            foreach ($usersResult['result'] as $user) {
                $fullName = trim(($user['NAME'] ?? '') . ' ' . ($user['LAST_NAME'] ?? ''));
                $userMap[$user['ID']] = $fullName ?: ($user['LOGIN'] ?? "User {$user['ID']}");
            }
        }
    }
    
    // Build and display the table
    $header = ['Task ID', 'Deal', 'Subject', 'Deadline', 'Responsible'];
    echo implode("\t", $header) . "\n";
    
    foreach ($allActivities as $a) {
        $activityId = $a['ID'] ?? '';
        $ownerId = (int)($a['OWNER_ID'] ?? 0);
        $dealTitle = $dealMap[$ownerId] ?? "Deal #{$ownerId}";
        $subject = $a['SUBJECT'] ?? '';
        $deadline = $a['DEADLINE'] ?? '';
        $responsibleId = $a['RESPONSIBLE_ID'] ?? '';
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

{% endlist %}