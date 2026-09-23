# How to Reschedule a Planned Activity

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed permissions is required — permission to edit the CRM item to which the activity is linked
>
> - [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — any user; the method returns data according to the user's permissions
> - [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) — a user with permission to edit the CRM item to which the activity is linked

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A planned activity helps the responsible employee avoid missing the next step regarding a customer: making a call, sending an e-mail, or preparing documents. If the deadline changes, you must update the activity's deadline in the CRM timeline.

For example, let's reschedule a planned activity to tomorrow: we will change the deadline date but keep the same time. We will also specify a title and add reminders for 15 minutes before the deadline and at the moment the deadline occurs.

To reschedule an activity, use the [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) method. You must pass the activity identifier and the identifier of the CRM item to which the activity is linked.

Expected result: the deadline of an open universal activity in the deal timeline changes, and the update method returns the identifier of that activity.

The scenario consists of two steps.

1. Find an open universal activity using the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method
2. Pass the identifiers and the current activity deadline to the [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) method to calculate and retain the new deadline

## Before You Start

- A deal with an open universal activity where `PROVIDER_ID` is `CRM_TODO`
- The deal identifier, for example, `18`. It is displayed in the deal form URL and returned by the [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) and [crm.deal.add](../../../api-reference/crm/deals/crm-deal-add.md) methods
- An inbound webhook with the `crm` scope, created by a user who has permission to edit this deal
- The SDK for the selected language installed: `@bitrix24/b24jssdk`, `bitrix24/b24phpsdk:^3.0`, or `b24pysdk`

Store the webhook URL in an environment variable, not in the source code. Do not add a file containing a secret to version control or output the webhook value to a log.

The examples use these environment variables:

- `B24_WEBHOOK_URL` — full inbound webhook URL for JavaScript and PHP
- `B24_DOMAIN` — Bitrix24 domain without `https://` for Python, for example, `company.bitrix24.com`
- `B24_WEBHOOK_TOKEN` — the `user_id/webhook_key` part of the path for Python
- `CRM_DEAL_ID` — deal identifier

## 1. Find an Open Universal Activity

To update an activity, you need the following values:

- `id` — the activity identifier in the timeline
- `ownerTypeId` — the [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type) to which the activity is linked
- `ownerId` — the CRM item identifier to which the activity is linked
- `deadline` — the current activity deadline, from which we will take the time and time zone for the new deadline in [ISO 8601](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) format
- `title` — the current activity title
- `responsibleId` — identifier of the employee responsible for the activity

The [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) method updates only open universal activities. Therefore, pass the `COMPLETED: 'N'` and `PROVIDER_ID: 'CRM_TODO'` filters to [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md).

In the example, we retrieve the first open universal activity linked to deal `18`. For a deal, `OWNER_TYPE_ID` is `2`.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const webhookUrl = process.env.B24_WEBHOOK_URL
    const dealId = Number(process.env.CRM_DEAL_ID)

    if (!webhookUrl || !dealId) {
        throw new Error('Set B24_WEBHOOK_URL and CRM_DEAL_ID')
    }

    const $b24 = B24Hook.fromWebhookUrl(webhookUrl)

    const activityResponse = await $b24.actions.v2.call.make({
        method: 'crm.activity.list',
        params: {
            filter: {
                OWNER_TYPE_ID: 2,
                OWNER_ID: dealId,
                COMPLETED: 'N',
                PROVIDER_ID: 'CRM_TODO',
            },
            select: [
                'ID',
                'OWNER_TYPE_ID',
                'OWNER_ID',
                'SUBJECT',
                'DEADLINE',
                'COMPLETED',
                'RESPONSIBLE_ID',
                'PROVIDER_ID',
            ],
        },
        requestId: 'activity-list',
    })

    if (!activityResponse.isSuccess) {
        throw new Error(activityResponse.getErrorMessages().join('; '))
    }

    const activity = activityResponse.getData().result[0]

    if (!activity) {
        throw new Error('No open universal activities found')
    }

    const activityId = Number(activity.ID)
    const ownerTypeId = Number(activity.OWNER_TYPE_ID)
    const ownerId = Number(activity.OWNER_ID)
    const currentDeadline = activity.DEADLINE
    const responsibleId = Number(activity.RESPONSIBLE_ID)
    const title = activity.SUBJECT

    console.log(activityId, ownerTypeId, ownerId, currentDeadline, responsibleId, title)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $webhookUrl = getenv('B24_WEBHOOK_URL');
    $dealId = (int)getenv('CRM_DEAL_ID');

    if (!$webhookUrl || !$dealId)
    {
        throw new RuntimeException('Set B24_WEBHOOK_URL and CRM_DEAL_ID');
    }

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook($webhookUrl);

    $activities = $sb->getCRMScope()->activity()->list(
        [],
        [
            'OWNER_TYPE_ID' => 2,
            'OWNER_ID' => $dealId,
            'COMPLETED' => 'N',
            'PROVIDER_ID' => 'CRM_TODO'
        ],
        [
            'ID',
            'OWNER_TYPE_ID',
            'OWNER_ID',
            'SUBJECT',
            'DEADLINE',
            'COMPLETED',
            'RESPONSIBLE_ID',
            'PROVIDER_ID'
        ],
        0
    )->getActivities();

    $activity = $activities[0] ?? null;

    if ($activity === null)
    {
        throw new RuntimeException('No open universal activities found');
    }

    $activityId = $activity->ID;
    $ownerTypeId = $activity->OWNER_TYPE_ID;
    $ownerId = $activity->OWNER_ID;
    // DEADLINE is typed in CarbonImmutable
    $currentDeadline = $activity->DEADLINE;
    $responsibleId = $activity->RESPONSIBLE_ID;
    $title = $activity->SUBJECT;
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain=os.environ["B24_DOMAIN"],
            webhook_token=os.environ["B24_WEBHOOK_TOKEN"],
        )
    )

    deal_id = int(os.environ["CRM_DEAL_ID"])

    activities = client.crm.activity.list(
        filter={
            "OWNER_TYPE_ID": 2,
            "OWNER_ID": deal_id,
            "COMPLETED": "N",
            "PROVIDER_ID": "CRM_TODO",
        },
        select=[
            "ID",
            "OWNER_TYPE_ID",
            "OWNER_ID",
            "SUBJECT",
            "DEADLINE",
            "COMPLETED",
            "RESPONSIBLE_ID",
            "PROVIDER_ID",
        ],
    ).response.result

    if not activities:
        raise RuntimeError("No open universal activities found")

    activity = activities[0]
    activity_id = int(activity["ID"])
    owner_type_id = int(activity["OWNER_TYPE_ID"])
    owner_id = int(activity["OWNER_ID"])
    current_deadline = activity["DEADLINE"]
    responsible_id = int(activity["RESPONSIBLE_ID"])
    title = activity["SUBJECT"]
    ```
{% endlist %}

Take the values for updating the activity from the first item of the `result` array. The `ID` field is the identifier of the found activity. A shortened response item is shown below.

```json
{
    "ID": "555",
    "OWNER_TYPE_ID": "2",
    "OWNER_ID": "18",
    "SUBJECT": "Contact client",
    "DEADLINE": "2026-08-14T10:00:00+03:00",
    "COMPLETED": "N",
    "RESPONSIBLE_ID": "1",
    "PROVIDER_ID": "CRM_TODO"
}
```

## 2. Update the Activity Deadline

The [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) method updates a universal activity. To reschedule the activity to tomorrow, pass the following parameters:

- `id` — `555`, the identifier of the found activity from the `ID` field of the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) response
- `ownerTypeId` — `2`, the CRM object type identifier from the `OWNER_TYPE_ID` field in the previous step
- `ownerId` — `18`, the CRM item identifier from the `OWNER_ID` field in the previous step
- `deadline` — the new activity deadline. Take tomorrow's date and retain the time and time zone offset from `DEADLINE` in the previous step. For example, if the code runs on `2026-07-06`, `2026-08-14T10:00:00+03:00` becomes `2026-07-07T10:00:00+03:00`
- `title` — `Contact client`, the title from the `SUBJECT` field in the previous step
- `responsibleId` — `1`, the identifier of the responsible employee from the `RESPONSIBLE_ID` field in the previous step
- `pingOffsets` — `[0, 15]`, reminders at the deadline and 15 minutes before it
- `colorId` — `2`, the activity color in the timeline

{% list tabs %}

- JS

    ```js
    // Continuation of the example from step 1

    function getTomorrowDeadlineWithSameTime(isoDateTime) {
        const dateTimeParts = isoDateTime.match(
            /^\d{4}-\d{2}-\d{2}(T\d{2}:\d{2}:\d{2}(?:\.\d+)?)(Z|[+-]\d{2}:\d{2})$/
        )

        if (!dateTimeParts) {
            throw new Error('Invalid date format')
        }

        const offset = dateTimeParts[2]
        const offsetMinutes = offset === 'Z'
            ? 0
            : (offset.startsWith('-') ? -1 : 1)
                * (Number(offset.slice(1, 3)) * 60 + Number(offset.slice(4, 6)))
        const tomorrow = new Date(Date.now() + offsetMinutes * 60_000)
        tomorrow.setUTCDate(tomorrow.getUTCDate() + 1)

        const year = tomorrow.getUTCFullYear()
        const month = String(tomorrow.getUTCMonth() + 1).padStart(2, '0')
        const day = String(tomorrow.getUTCDate()).padStart(2, '0')

        return `${year}-${month}-${day}${dateTimeParts[1]}${offset}`
    }

    const deadline = getTomorrowDeadlineWithSameTime(currentDeadline)

    const updateResponse = await $b24.actions.v2.call.make({
        method: 'crm.activity.todo.update',
        params: {
            id: activityId,
            ownerTypeId,
            ownerId,
            deadline,
            title,
            responsibleId,
            pingOffsets: [0, 15],
            colorId: '2',
        },
        requestId: 'activity-todo-update',
    })

    if (!updateResponse.isSuccess) {
        console.error(updateResponse.getErrorMessages().join('; '))
    } else {
        console.log('Task updated: ' + updateResponse.getData().result.id)
    }
    ```

- PHP

    ```php
    // Continuation of the example from step 1

    $deadline = (new DateTimeImmutable('tomorrow', $currentDeadline->getTimezone()))
        ->setTime(
            (int)$currentDeadline->format('H'),
            (int)$currentDeadline->format('i'),
            (int)$currentDeadline->format('s')
        );

    try
    {
        // crm.activity.todo.update does not have a typed wrapper — calling via core
        $result = $sb->core->call(
            'crm.activity.todo.update',
            [
                'id' => $activityId,
                'ownerTypeId' => $ownerTypeId,
                'ownerId' => $ownerId,
                'deadline' => $deadline->format(DateTimeInterface::ATOM),
                'title' => $title,
                'responsibleId' => $responsibleId,
                'pingOffsets' => [0, 15],
                'colorId' => '2'
            ]
        )->getResponseData()->getResult();

        echo 'Task updated: ' . $result['id'];
    }
    catch (BaseException $exception)
    {
        echo 'Error: ' . $exception->getMessage();
    }
    ```

- Python

    ```python
    # Continuation of the example from step 1
    from datetime import datetime, timedelta

    from b24pysdk.errors import BitrixAPIError

    current_deadline_dt = datetime.fromisoformat(current_deadline)
    tomorrow = datetime.now(current_deadline_dt.tzinfo).date() + timedelta(days=1)
    deadline = datetime.combine(tomorrow, current_deadline_dt.timetz())

    try:
        response = client.crm.activity.todo.update(
            bitrix_id=activity_id,
            owner_type_id=owner_type_id,
            owner_id=owner_id,
            deadline=deadline,
            title=title,
            responsible_id=responsible_id,
            ping_offsets=[0, 15],
            color_id="2",
        ).response
        print(f"Activity updated: {response.result['id']}")
    except BitrixAPIError as error:
        print(f"Error: {error}")
    ```
{% endlist %}

If the activity is updated successfully, the method returns the activity identifier.

```json
{
    "result": {
        "id": 555
    }
}
```

## Verify the Result

The `result.id = 555` response confirms that the method updated activity `555`.

Verify the result in one of these ways:

1. Open the deal specified by `CRM_DEAL_ID`. The deadline, color, and reminders of the found activity must change in the timeline
2. Call [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) again with the `ID: 555` filter and select the `ID`, `DEADLINE`, `SUBJECT`, `RESPONSIBLE_ID`, and `COMPLETED` fields. The `DEADLINE` field must contain tomorrow's date with the previous time and time zone offset

## Errors and Troubleshooting

- The activity list is empty — check `CRM_DEAL_ID` and whether there is an open activity with `PROVIDER_ID = CRM_TODO`, then repeat step 1
- `CAN_NOT_UPDATE_COMPLETED_TODO` — the activity was closed after step 1. Find another open activity and repeat the scenario
- `WRONG_DATETIME_FORMAT` — the `deadline` value does not conform to ISO 8601. Check the source `DEADLINE` field and the new date calculation
- `NOT_FOUND` — the CRM item was not found or the selected activity was created by another provider. Check `id`, `ownerTypeId`, `ownerId`, and `PROVIDER_ID`, then repeat step 1
- `ACCESS_DENIED` — the webhook owner cannot edit the deal. Check the permissions of this user
- `OWNER_NOT_FOUND` — the CRM item linked to the activity was not found. Check `ownerTypeId` and `ownerId`

If the request fails authorization, check that the environment variables are set, the webhook is active, and it has the `crm` scope. Do not output the complete webhook URL during troubleshooting.

## Important Considerations

- The scenario selects the first activity from the `crm.activity.list` response. If the deal contains several universal activities, add a known `ID` to the filter or select the required activity by its response fields
- The date for "tomorrow" is calculated when the code runs, while the time and time zone offset are taken from the current `DEADLINE`
- Running the scenario again on the same day updates the same activity with the same values and does not create a new activity
- To apply the scenario to a lead, contact, company, or smart process, replace the object type and identifier in the step 1 filter. `OWNER_TYPE_ID` values are listed in [CRM Object Types](../../../api-reference/crm/data-types.md#object_type)

## Continue Learning

- [{#T}](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md)
- [{#T}](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)
- [{#T}](./how-to-move-activity.md)
