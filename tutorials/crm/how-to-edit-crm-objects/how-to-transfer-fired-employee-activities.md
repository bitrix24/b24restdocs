# How to Transfer or Complete Activities of a Terminated Employee

> Scope: [`crm`, `user_basic`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: for the webhook scenario, you need permission to read and modify CRM activities. Configurable activities can be updated only by the application that created them
>
> - [user.get](../../../api-reference/user/user-get.md) - any user
> - [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) - any user
> - [crm.activity.update](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md) - a user with permission to update the activity
> - [crm.activity.todo.updateResponsibleUser](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update-responsible-user.md) - a user with permission to edit the CRM item for which the activity is updated
> - [crm.activity.configurable.get](../../../api-reference/crm/timeline/activities/configurable/crm-activity-configurable-get.md) - the application that created the configurable activity
> - [crm.activity.configurable.update](../../../api-reference/crm/timeline/activities/configurable/crm-activity-configurable-update.md) - the application that created the configurable activity

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

After an employee is terminated, their incomplete CRM activities can be transferred to another responsible person or completed. To do this, find the employee identifier, retrieve the activities where this employee is responsible, and perform an action for each activity.

The update method depends on the activity type:

- regular and system activities are updated using [crm.activity.update](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md)
- universal activities with `PROVIDER_ID: CRM_TODO` are transferred using [crm.activity.todo.updateResponsibleUser](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update-responsible-user.md)
- configurable activities with `PROVIDER_ID: CONFIGURABLE_REST_APP` are updated only in the OAuth context of the application that created them

The scenario consists of four steps.

1. Find the terminated employee and, if the activities must be transferred, the new responsible person using [user.get](../../../api-reference/user/user-get.md)
2. Retrieve the terminated employee's incomplete activities using [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)
3. Split the activities by the `PROVIDER_ID` field
4. Transfer or complete regular, system, and universal activities using the appropriate method

As a result, the responsible person will change for regular, system, and universal activities available to the webhook user. If completion mode is selected, incomplete regular, system, and universal activities will be completed. The scenario stores app activities separately and does not update them through a webhook.

## Before You Start

Check access conditions and prepare the input data:

- an inbound webhook is created for a user who can see the terminated employee's activities and can modify the CRM items to which these activities are linked
- the webhook permissions include the `crm` and `user_basic` scopes
- you know the name, last name, e-mail, or another attribute of the terminated employee
- if the activities must be transferred, you know the name, last name, e-mail, or identifier of the new responsible person

The webhook runs requests with the permissions of the user who created it. The user will see and modify only the activities and CRM items to which they have access.

Store the webhook URL in an environment variable and do not publish it in open code.

In the examples below, we search for the terminated employee by the last name `Weber` and for the new responsible person by the e-mail `new.responsible@example.com`. Replace these values with your own values in your Bitrix24.

Server-side JS examples with `B24Hook` require Node.js 18, 20, 22, or later. For new projects, use 22 or later. B24JsSDK is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`.

## 1. Find Employees

The [user.get](../../../api-reference/user/user-get.md) method retrieves users by filter. To find the terminated employee, pass `ACTIVE: false` and an additional attribute to the filter, such as a last name or e-mail. If the activities must be transferred, find the new responsible person with the `ACTIVE: true` filter.

Save these values from the response:

- `ID` of the terminated employee - we will pass it to the `RESPONSIBLE_ID` activity filter
- `ID` of the new responsible person - we will pass it to responsible-person update methods if the activity must be transferred to another user instead of completed

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const action = 'transfer'

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

    async function findOneUser(filter, requestId) {
        const result = await callMethod(
            'user.get',
            {
                FILTER: filter,
                SELECT: ['ID', 'ACTIVE', 'NAME', 'LAST_NAME', 'EMAIL']
            },
            requestId
        )

        if (!Array.isArray(result) || result.length !== 1) {
            throw new Error(`Expected one user, received: ${Array.isArray(result) ? result.length : 0}`)
        }

        return result[0]
    }

    const firedUser = await findOneUser(
        {
            ACTIVE: false,
            LAST_NAME: 'Weber'
        },
        'user-get-fired'
    )

    const newResponsible = action === 'transfer'
        ? await findOneUser(
            {
                ACTIVE: true,
                EMAIL: 'new.responsible@example.com'
            },
            'user-get-new-responsible'
        )
        : null
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

    $action = 'transfer';

    function callMethod($serviceBuilder, string $method, array $params)
    {
        return $serviceBuilder
            ->core
            ->call($method, $params)
            ->getResponseData()
            ->getResult();
    }

    function findOneUser($serviceBuilder, array $filter): array
    {
        $result = callMethod(
            $serviceBuilder,
            'user.get',
            [
                'FILTER' => $filter,
                'SELECT' => ['ID', 'ACTIVE', 'NAME', 'LAST_NAME', 'EMAIL'],
            ]
        );

        if (count($result) !== 1) {
            throw new RuntimeException('Expected one user, received: ' . count($result));
        }

        return $result[0];
    }

    $firedUser = findOneUser(
        $serviceBuilder,
        [
            'ACTIVE' => false,
            'LAST_NAME' => 'Weber',
        ]
    );

    $newResponsible = $action === 'transfer'
        ? findOneUser(
            $serviceBuilder,
            [
                'ACTIVE' => true,
                'EMAIL' => 'new.responsible@example.com',
            ]
        )
        : null;
    ```

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client

    action = "transfer"

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="user_id/webhook_key",
    )
    client = Client(token)

    def call_method(method, params):
        # Call through the SDK core: the scenario includes methods without a typed wrapper
        payload = token.call_method(method, params)

        if "error" in payload:
            raise RuntimeError(f"{payload['error']}: {payload.get('error_description', '')}")

        return payload["result"]

    def find_one_user(filter):
        result = call_method(
            "user.get",
            {
                "FILTER": filter,
                "SELECT": ["ID", "ACTIVE", "NAME", "LAST_NAME", "EMAIL"],
            },
        )

        if len(result) != 1:
            raise RuntimeError(f"Expected one user, received: {len(result)}")

        return result[0]

    fired_user = find_one_user(
        {
            "ACTIVE": False,
            "LAST_NAME": "Weber",
        }
    )

    new_responsible = (
        find_one_user(
            {
                "ACTIVE": True,
                "EMAIL": "new.responsible@example.com",
            }
        )
        if action == "transfer"
        else None
    )
    ```

{% endlist %}

Short response for the terminated employee:

```json
{
    "result": [
        {
            "ID": "37",
            "ACTIVE": false,
            "NAME": "Klaus",
            "LAST_NAME": "Weber",
            "EMAIL": "k.weber@example.com"
        }
    ],
    "total": 1
}
```

Short response for the new responsible person:

```json
{
    "result": [
        {
            "ID": "547",
            "ACTIVE": true,
            "NAME": "Anna",
            "LAST_NAME": "Schmidt",
            "EMAIL": "new.responsible@example.com"
        }
    ],
    "total": 1
}
```

As a result, we retrieved the terminated employee `ID` `37` and the new responsible person `ID` `547`. These values are required in the next steps.

## 2. Retrieve the Terminated Employee's Activities

The [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method retrieves activities with page navigation. Pass these values to the filter:

- `RESPONSIBLE_ID` - terminated employee identifier
- `COMPLETED: N` - only incomplete activities

To retrieve only universal activities, add `PROVIDER_ID: CRM_TODO` to the filter. To retrieve all activities of the terminated employee, leave the filter without `PROVIDER_ID` and split records in code.

Add the fields required to select the update method to `select`:

- `ID` - activity identifier
- `OWNER_TYPE_ID` - identifier of the CRM object type to which the activity is linked
- `OWNER_ID` - identifier of the CRM item to which the activity is linked
- `PROVIDER_ID` - activity provider identifier. We will use it to split activities into universal activities, configurable app activities, regular activities, and system activities
- `PROVIDER_TYPE_ID` - provider type identifier
- `SUBJECT` - activity title, required to verify the result
- `COMPLETED` - activity completion flag
- `RESPONSIBLE_ID` - responsible person identifier

{% list tabs %}

- JS

    ```javascript
    async function getEmployeeActivities(employeeId) {
        const activities = []
        let start = 0

        while (true) {
            const page = await callMethod(
                'crm.activity.list',
                {
                    order: { ID: 'asc' },
                    filter: {
                        RESPONSIBLE_ID: Number(employeeId),
                        COMPLETED: 'N'
                    },
                    select: [
                        'ID',
                        'OWNER_TYPE_ID',
                        'OWNER_ID',
                        'PROVIDER_ID',
                        'PROVIDER_TYPE_ID',
                        'SUBJECT',
                        'COMPLETED',
                        'RESPONSIBLE_ID'
                    ],
                    start
                },
                `crm-activity-list-${start}`
            )

            activities.push(...page)

            if (page.length < 50) {
                break
            }

            start += 50
        }

        return activities
    }

    const activities = await getEmployeeActivities(firedUser.ID)
    ```

- PHP

    ```php
    function getEmployeeActivities($serviceBuilder, int $employeeId): array
    {
        $activities = [];
        $start = 0;

        do {
            $page = callMethod(
                $serviceBuilder,
                'crm.activity.list',
                [
                    'order' => ['ID' => 'asc'],
                    'filter' => [
                        'RESPONSIBLE_ID' => $employeeId,
                        'COMPLETED' => 'N',
                    ],
                    'select' => [
                        'ID',
                        'OWNER_TYPE_ID',
                        'OWNER_ID',
                        'PROVIDER_ID',
                        'PROVIDER_TYPE_ID',
                        'SUBJECT',
                        'COMPLETED',
                        'RESPONSIBLE_ID',
                    ],
                    'start' => $start,
                ]
            );

            $activities = array_merge($activities, $page);
            $start += 50;
        } while (count($page) === 50);

        return $activities;
    }

    $activities = getEmployeeActivities($serviceBuilder, (int)$firedUser['ID']);
    ```

- Python

    ```python
    def get_employee_activities(employee_id):
        activities = []
        start = 0

        while True:
            page = call_method(
                "crm.activity.list",
                {
                    "order": {"ID": "asc"},
                    "filter": {
                        "RESPONSIBLE_ID": int(employee_id),
                        "COMPLETED": "N",
                    },
                    "select": [
                        "ID",
                        "OWNER_TYPE_ID",
                        "OWNER_ID",
                        "PROVIDER_ID",
                        "PROVIDER_TYPE_ID",
                        "SUBJECT",
                        "COMPLETED",
                        "RESPONSIBLE_ID",
                    ],
                    "start": start,
                },
            )

            activities.extend(page)

            if len(page) < 50:
                break

            start += 50

        return activities

    activities = get_employee_activities(fired_user["ID"])
    ```

{% endlist %}

Short response:

```json
{
    "result": [
        {
            "ID": "1501",
            "OWNER_TYPE_ID": "2",
            "OWNER_ID": "18",
            "PROVIDER_ID": "CRM_TODO",
            "PROVIDER_TYPE_ID": "TODO",
            "SUBJECT": "Contact the customer",
            "COMPLETED": "N",
            "RESPONSIBLE_ID": "37"
        }
    ],
    "total": 1
}
```

The method returns an array of activities. For the next steps, you need the `ID`, `OWNER_TYPE_ID`, `OWNER_ID`, `PROVIDER_ID`, and `PROVIDER_TYPE_ID` fields. If the method returns 50 activities, request the next page with `start: 50`.

## 3. Split Activities by Type

The `PROVIDER_ID` field shows which method should be used to update the activity. Split the array from step 2 into groups:

- `CRM_TODO` - universal activities
- `CONFIGURABLE_REST_APP` - configurable app activities. In the example, we store them in a separate list without changes
- other values - regular and system activities

{% list tabs %}

- JS

    ```javascript
    function splitActivitiesByProvider(activities) {
        return activities.reduce(
            (groups, activity) => {
                if (activity.PROVIDER_ID === 'CRM_TODO') {
                    groups.todos.push(activity)
                } else if (activity.PROVIDER_ID === 'CONFIGURABLE_REST_APP') {
                    groups.configurable.push(activity)
                } else {
                    groups.base.push(activity)
                }

                return groups
            },
            {
                todos: [],
                configurable: [],
                base: []
            }
        )
    }

    const groupedActivities = splitActivitiesByProvider(activities)
    ```

- PHP

    ```php
    function splitActivitiesByProvider(array $activities): array
    {
        $groups = [
            'todos' => [],
            'configurable' => [],
            'base' => [],
        ];

        foreach ($activities as $activity) {
            if (($activity['PROVIDER_ID'] ?? '') === 'CRM_TODO') {
                $groups['todos'][] = $activity;
            } elseif (($activity['PROVIDER_ID'] ?? '') === 'CONFIGURABLE_REST_APP') {
                $groups['configurable'][] = $activity;
            } else {
                $groups['base'][] = $activity;
            }
        }

        return $groups;
    }

    $groupedActivities = splitActivitiesByProvider($activities);
    ```

- Python

    ```python
    def split_activities_by_provider(activities):
        groups = {
            "todos": [],
            "configurable": [],
            "base": [],
        }

        for activity in activities:
            provider_id = activity.get("PROVIDER_ID", "")

            if provider_id == "CRM_TODO":
                groups["todos"].append(activity)
            elif provider_id == "CONFIGURABLE_REST_APP":
                groups["configurable"].append(activity)
            else:
                groups["base"].append(activity)

        return groups

    grouped_activities = split_activities_by_provider(activities)
    ```

{% endlist %}

After splitting, the code has three local arrays: `base`, `todos`, and `configurable`. These arrays are not Bitrix24 response fields. They are needed only to select the next method.

### If the List Contains App Activities

Do not pass activities from the `configurable` array to the update methods in this scenario. An inbound webhook cannot update activities with `PROVIDER_ID: CONFIGURABLE_REST_APP`, because such activities are modified only by the application that created them.

Store these activities separately:

{% list tabs %}

- JS

    ```javascript
    const skippedConfigurableActivities = groupedActivities.configurable
    ```

- PHP

    ```php
    $skippedConfigurableActivities = $groupedActivities['configurable'];
    ```

- Python

    ```python
    skipped_configurable_activities = grouped_activities["configurable"]
    ```

{% endlist %}

If you need to transfer these activities, do it in the application that created them:

1. Authorize requests in the OAuth context of the application
2. Retrieve the activity using [crm.activity.configurable.get](../../../api-reference/crm/timeline/activities/configurable/crm-activity-configurable-get.md)
3. Pass the current `layout` and the new `fields.responsibleId` value to [crm.activity.configurable.update](../../../api-reference/crm/timeline/activities/configurable/crm-activity-configurable-update.md)

If configurable activity methods are called outside the application, Bitrix24 returns the `ERROR_WRONG_CONTEXT` error. If the application did not create the activity, Bitrix24 returns the `ERROR_WRONG_APPLICATION` error.

## 4. Transfer or Complete Activities

Select the processing mode before running the update:

- `transfer` - transfer activities to the new responsible person
- `complete` - complete incomplete activities

Update regular and system activities using [crm.activity.update](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md). Method development has stopped, but the documentation does not specify a current replacement for changing `RESPONSIBLE_ID` in regular and system activities.

In `complete` mode, complete universal activities using the same method. The `crm.activity.todo.*` methods do not have a separate method for completing an activity. To transfer an activity, send the `RESPONSIBLE_ID` field. To complete an activity, send the `COMPLETED: Y` field.

{% list tabs %}

- JS

    ```javascript
    async function updateBaseActivities(activities, action, newResponsibleId) {
        const updated = []

        for (const activity of activities) {
            const fields = action === 'transfer'
                ? { RESPONSIBLE_ID: Number(newResponsibleId) }
                : { COMPLETED: 'Y' }

            const result = await callMethod(
                'crm.activity.update',
                {
                    id: Number(activity.ID),
                    fields
                },
                `crm-activity-update-${activity.ID}`
            )

            updated.push({
                id: activity.ID,
                method: 'crm.activity.update',
                result
            })
        }

        return updated
    }

    const activitiesForBaseUpdate = action === 'complete'
        ? [...groupedActivities.base, ...groupedActivities.todos]
        : groupedActivities.base

    const updatedBaseActivities = await updateBaseActivities(
        activitiesForBaseUpdate,
        action,
        action === 'transfer' ? newResponsible.ID : 0
    )
    ```

- PHP

    ```php
    function updateBaseActivities($serviceBuilder, array $activities, string $action, int $newResponsibleId): array
    {
        $updated = [];

        foreach ($activities as $activity) {
            $fields = $action === 'transfer'
                ? ['RESPONSIBLE_ID' => $newResponsibleId]
                : ['COMPLETED' => 'Y'];

            $result = callMethod(
                $serviceBuilder,
                'crm.activity.update',
                [
                    'id' => (int)$activity['ID'],
                    'fields' => $fields,
                ]
            );

            $updated[] = [
                'id' => $activity['ID'],
                'method' => 'crm.activity.update',
                'result' => $result,
            ];
        }

        return $updated;
    }

    $activitiesForBaseUpdate = $action === 'complete'
        ? array_merge($groupedActivities['base'], $groupedActivities['todos'])
        : $groupedActivities['base'];

    $updatedBaseActivities = updateBaseActivities(
        $serviceBuilder,
        $activitiesForBaseUpdate,
        $action,
        $newResponsible !== null ? (int)$newResponsible['ID'] : 0
    );
    ```

- Python

    ```python
    def update_base_activities(activities, action, new_responsible_id):
        updated = []

        for activity in activities:
            fields = (
                {"RESPONSIBLE_ID": int(new_responsible_id)}
                if action == "transfer"
                else {"COMPLETED": "Y"}
            )

            result = call_method(
                "crm.activity.update",
                {
                    "id": int(activity["ID"]),
                    "fields": fields,
                },
            )

            updated.append(
                {
                    "id": activity["ID"],
                    "method": "crm.activity.update",
                    "result": result,
                }
            )

        return updated

    activities_for_base_update = (
        grouped_activities["base"] + grouped_activities["todos"]
        if action == "complete"
        else grouped_activities["base"]
    )

    updated_base_activities = update_base_activities(
        activities_for_base_update,
        action,
        new_responsible["ID"] if action == "transfer" else 0,
    )
    ```

{% endlist %}

Transfer universal activities using [crm.activity.todo.updateResponsibleUser](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update-responsible-user.md). The method changes only the responsible person, so call it in `transfer` mode. In `complete` mode, universal activities are already included in the array for [crm.activity.update](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md).

{% list tabs %}

- JS

    ```javascript
    async function transferTodoActivities(activities, newResponsibleId) {
        const updated = []

        for (const activity of activities) {
            const result = await callMethod(
                'crm.activity.todo.updateResponsibleUser',
                {
                    id: Number(activity.ID),
                    ownerTypeId: Number(activity.OWNER_TYPE_ID),
                    ownerId: Number(activity.OWNER_ID),
                    responsibleId: Number(newResponsibleId)
                },
                `crm-activity-todo-update-responsible-${activity.ID}`
            )

            updated.push({
                id: activity.ID,
                method: 'crm.activity.todo.updateResponsibleUser',
                result
            })
        }

        return updated
    }

    const updatedTodoActivities = action === 'transfer'
        ? await transferTodoActivities(groupedActivities.todos, newResponsible.ID)
        : []
    ```

- PHP

    ```php
    function transferTodoActivities($serviceBuilder, array $activities, int $newResponsibleId): array
    {
        $updated = [];

        foreach ($activities as $activity) {
            $result = callMethod(
                $serviceBuilder,
                'crm.activity.todo.updateResponsibleUser',
                [
                    'id' => (int)$activity['ID'],
                    'ownerTypeId' => (int)$activity['OWNER_TYPE_ID'],
                    'ownerId' => (int)$activity['OWNER_ID'],
                    'responsibleId' => $newResponsibleId,
                ]
            );

            $updated[] = [
                'id' => $activity['ID'],
                'method' => 'crm.activity.todo.updateResponsibleUser',
                'result' => $result,
            ];
        }

        return $updated;
    }

    $updatedTodoActivities = $action === 'transfer'
        ? transferTodoActivities($serviceBuilder, $groupedActivities['todos'], (int)$newResponsible['ID'])
        : [];
    ```

- Python

    ```python
    def transfer_todo_activities(activities, new_responsible_id):
        updated = []

        for activity in activities:
            result = call_method(
                "crm.activity.todo.updateResponsibleUser",
                {
                    "id": int(activity["ID"]),
                    "ownerTypeId": int(activity["OWNER_TYPE_ID"]),
                    "ownerId": int(activity["OWNER_ID"]),
                    "responsibleId": int(new_responsible_id),
                },
            )

            updated.append(
                {
                    "id": activity["ID"],
                    "method": "crm.activity.todo.updateResponsibleUser",
                    "result": result,
                }
            )

        return updated

    updated_todo_activities = (
        transfer_todo_activities(grouped_activities["todos"], new_responsible["ID"])
        if action == "transfer"
        else []
    )
    ```

{% endlist %}

Short successful [crm.activity.update](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md) response:

```json
{
    "result": true
}
```

Short successful [crm.activity.todo.updateResponsibleUser](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update-responsible-user.md) response:

```json
{
    "result": {
        "id": 1501
    }
}
```

In the example, the code stores update results in the local `updatedBaseActivities` and `updatedTodoActivities` arrays. These are not Bitrix24 response fields, but data for verifying the result.

Activities from the `configurable` group are stored in the local `skippedConfigurableActivities` array.

If `complete` mode is selected, the `updatedTodoActivities` array will be empty. Universal activities in this mode are updated by [crm.activity.update](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md), so their results will be added to `updatedBaseActivities`.

## Verify the Result

Verify the result by calling [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) again. For transfer, select activities by the `RESPONSIBLE_ID` of the new responsible person and the identifiers of the updated activities. For completion, select activities by `COMPLETED: Y`.

{% list tabs %}

- JS

    ```javascript
    const updatedActivityIds = [
        ...updatedBaseActivities,
        ...updatedTodoActivities
    ].map((activity) => activity.id)

    if (updatedActivityIds.length > 0) {
        const checkResult = await callMethod(
            'crm.activity.list',
            {
                filter: {
                    '@ID': updatedActivityIds,
                    ...(action === 'transfer'
                        ? { RESPONSIBLE_ID: Number(newResponsible.ID) }
                        : { COMPLETED: 'Y' })
                },
                select: ['ID', 'SUBJECT', 'COMPLETED', 'RESPONSIBLE_ID', 'PROVIDER_ID']
            },
            'crm-activity-list-check'
        )

        console.table(checkResult)
    }
    ```

- PHP

    ```php
    $updatedActivityIds = array_map(
        static fn(array $activity): string => (string)$activity['id'],
        array_merge($updatedBaseActivities, $updatedTodoActivities)
    );

    if (!empty($updatedActivityIds)) {
        $filter = [
            '@ID' => $updatedActivityIds,
        ];

        if ($action === 'transfer') {
            $filter['RESPONSIBLE_ID'] = (int)$newResponsible['ID'];
        } else {
            $filter['COMPLETED'] = 'Y';
        }

        $checkResult = callMethod(
            $serviceBuilder,
            'crm.activity.list',
            [
                'filter' => $filter,
                'select' => ['ID', 'SUBJECT', 'COMPLETED', 'RESPONSIBLE_ID', 'PROVIDER_ID'],
            ]
        );

        print_r($checkResult);
    }
    ```

- Python

    ```python
    updated_activity_ids = [
        activity["id"]
        for activity in (
            updated_base_activities
            + updated_todo_activities
        )
    ]

    if updated_activity_ids:
        filter_params = {
            "@ID": updated_activity_ids,
        }

        if action == "transfer":
            filter_params["RESPONSIBLE_ID"] = int(new_responsible["ID"])
        else:
            filter_params["COMPLETED"] = "Y"

        check_result = call_method(
            "crm.activity.list",
            {
                "filter": filter_params,
                "select": ["ID", "SUBJECT", "COMPLETED", "RESPONSIBLE_ID", "PROVIDER_ID"],
            },
        )

        print(check_result)
    ```

{% endlist %}

The scenario is successful if the number of activities in the verification response matches the number of updated activities.

Check these values in the responses:

- when transferring, the `RESPONSIBLE_ID` field equals the new responsible person `ID`
- when completing, the `COMPLETED` field equals `Y`
- activities from the `configurable` group are added to `skippedConfigurableActivities` and remain unchanged
- universal activities in `complete` mode are added to the `updatedBaseActivities` array

In the CRM interface, transferred activities will appear for the new responsible person, and completed activities will no longer be displayed as open.

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code or Error Text** | **Reason and Action** ||
|| `insufficient_scope` | The webhook or application does not have the required scope. CRM activities require the `crm` scope. [user.get](../../../api-reference/user/user-get.md) with e-mail search requires `user_basic` or `user` ||
|| `Access denied` or `ACCESS_DENIED` | The user who created the webhook does not have permission to modify the activity or the CRM item to which it is linked ||
|| `Activity is not found` or `NOT_FOUND` | The activity was not found. Check the activity `ID` and user permissions ||
|| `OWNER_NOT_FOUND` | The CRM item to which the activity is linked was not found. Check `OWNER_TYPE_ID` and `OWNER_ID` ||
|| `CAN_NOT_UPDATE_RESPONSIBLE_USER_COMPLETED_TODO` | The responsible person cannot be changed in a completed universal activity. Retrieve only incomplete activities using the `COMPLETED: N` filter ||
|#

If the activity list is empty, check:

- the terminated employee was found using [user.get](../../../api-reference/user/user-get.md), and their `ID` was passed to `RESPONSIBLE_ID`
- the user who created the webhook can see the terminated employee's activities
- the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) filter does not contain extra conditions
- the terminated employee has incomplete CRM activities

If some activities have already been updated, restart the scenario from step 2. The [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method will no longer return transferred activities by the terminated employee `RESPONSIBLE_ID` filter, or completed activities by the `COMPLETED: N` filter.

## Key Points

Before running the scenario, consider method and access permission limitations.

- [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) returns activities in pages of 50 items. To retrieve all activities, iterate over `start`: `0`, `50`, `100`
- [crm.activity.update](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md) and [crm.activity.todo.updateResponsibleUser](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update-responsible-user.md) update one activity per call
- [crm.activity.update](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md) is required for regular and system activities, and for completing universal activities. The `crm.activity.todo.*` methods do not have a separate method for completing an activity
- Universal activities are identified by `PROVIDER_ID: CRM_TODO`. Configurable activities with `PROVIDER_ID: CONFIGURABLE_REST_APP` are not updated through a webhook. They are transferred in the OAuth context of the application that created them
- if several employees are found by the [user.get](../../../api-reference/user/user-get.md) filter, refine the filter: add `EMAIL`, `ID`, or another exact attribute

## Continue Learning

- [{#T}](../../../api-reference/user/user-get.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-update.md)
- [{#T}](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update-responsible-user.md)
- [{#T}](../../../api-reference/crm/timeline/activities/index.md)
