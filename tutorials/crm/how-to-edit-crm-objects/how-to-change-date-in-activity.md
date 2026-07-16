# How to Reschedule a Planned Activity

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: a user with permission to edit the CRM item to which the activity is linked

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A planned activity helps the responsible employee avoid missing the next step regarding a customer: making a call, sending an e-mail, or preparing documents. If the deadline changes, you must update the activity's deadline in the CRM timeline.

For example, let's reschedule a planned activity to tomorrow: we will change the deadline date but keep the same time. We will also specify a title and add reminders for 15 minutes before the deadline and at the moment the deadline occurs.

To reschedule an activity, use the [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) method. You must pass the activity identifier and the identifier of the CRM item to which the activity is linked.

The scenario consists of two steps.

1. Find an open activity using the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method.
2. Pass the activity data to the [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) method and update the deadline.

## 1. Prepare the Data

To update an activity, you need the following values:

- `id` — the activity identifier in the timeline,
- `ownerTypeId` — the [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type) to which the activity is linked,
- `ownerId` — the CRM item identifier to which the activity is linked,
- `deadline` — the current activity deadline, from which we will take the time and time zone for the new deadline in [ISO 8601](https://www.php.net/manual/en/class.datetimeinterface.php#datetimeinterface.constants.atom) format.

The [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) method does not update closed activities. To retrieve an open activity, call the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method with the `COMPLETED: 'N'` filter.

In the example, we retrieve the first open activity linked to deal `18`. For the Deal, the value `OWNER_TYPE_ID` is `2`.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    const dealId = 18

    const activityResponse = await $b24.actions.v2.call.make({
        method: 'crm.activity.list',
        params: {
            filter: {
                OWNER_TYPE_ID: 2,
                OWNER_ID: dealId,
                COMPLETED: 'N',
            },
            select: [
                'ID',
                'OWNER_TYPE_ID',
                'OWNER_ID',
                'SUBJECT',
                'DEADLINE',
                'COMPLETED',
                'RESPONSIBLE_ID',
            ],
        },
        requestId: 'activity-list',
    })

    if (!activityResponse.isSuccess) {
        throw new Error(activityResponse.getErrorMessages().join('; '))
    }

    const activity = activityResponse.getData().result[0]

    if (!activity) {
        console.log('Unclosed tasks not found')
    } else {
        const activityId = Number(activity.ID)
        const ownerTypeId = Number(activity.OWNER_TYPE_ID)
        const ownerId = Number(activity.OWNER_ID)
        const currentDeadline = activity.DEADLINE
        const responsibleId = Number(activity.RESPONSIBLE_ID)

        console.log(activityId, ownerTypeId, ownerId, currentDeadline, responsibleId)
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

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $dealId = 18;

    $activities = $sb->getCRMScope()->activity()->list(
        [],
        [
            'OWNER_TYPE_ID' => 2,
            'OWNER_ID' => $dealId,
            'COMPLETED' => 'N'
        ],
        [
            'ID',
            'OWNER_TYPE_ID',
            'OWNER_ID',
            'SUBJECT',
            'DEADLINE',
            'COMPLETED',
            'RESPONSIBLE_ID'
        ],
        0
    )->getActivities();

    $activity = $activities[0] ?? null;

    if ($activity === null)
    {
        echo 'Unclosed tasks not found';
        return;
    }

    $activityId = $activity->ID;
    $ownerTypeId = $activity->OWNER_TYPE_ID;
    $ownerId = $activity->OWNER_ID;
    // DEADLINE is typed in CarbonImmutable
    $currentDeadline = $activity->DEADLINE;
    $responsibleId = $activity->RESPONSIBLE_ID;
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

    deal_id = 18

    activities = client.crm.activity.list(
        filter={
            "OWNER_TYPE_ID": 2,
            "OWNER_ID": deal_id,
            "COMPLETED": "N",
        },
        select=[
            "ID",
            "OWNER_TYPE_ID",
            "OWNER_ID",
            "SUBJECT",
            "DEADLINE",
            "COMPLETED",
            "RESPONSIBLE_ID",
        ],
    ).response.result

    if not activities:
        print("Unclosed tasks not found")
        raise SystemExit

    activity = activities[0]
    activity_id = int(activity["ID"])
    owner_type_id = int(activity["OWNER_TYPE_ID"])
    owner_id = int(activity["OWNER_ID"])
    current_deadline = activity["DEADLINE"]
    responsible_id = int(activity["RESPONSIBLE_ID"])
    ```

{% endlist %}

Take the values for updating the activity from the first item of the `result` array. The `ID` field is the identifier of the found activity.

```json
{
    "ID": "555",
    "OWNER_TYPE_ID": "2",
    "OWNER_ID": "18",
    "SUBJECT": "Contact client",
    "DEADLINE": "2026-08-14T10:00:00+03:00",
    "COMPLETED": "N",
    "RESPONSIBLE_ID": "1"
}
```

## 2. Update the Activity Deadline

The [crm.activity.todo.update](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md) method updates a universal activity. To reschedule the activity to tomorrow, pass the following parameters:

- `id` — `555`, the ID of the found activity from the `ID` field of the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) response.
- `ownerTypeId` — `2`, the ID of the CRM object type from the `OWNER_TYPE_ID` field of the previous step.
- `ownerId` — `18`, the ID of the CRM item from the `OWNER_ID` field of the previous step.
- `deadline` — the new activity deadline. Take the date for tomorrow, and transfer the time and time zone from `DEADLINE` of the previous step. For example, if the code is executed on `2026-07-06`, the value `2026-08-14T10:00:00+03:00` will become `2026-07-07T10:00:00+03:00`.
- `title` — Contact the customer, the activity title.
- `responsibleId` — `1`, the ID of the responsible employee from the `RESPONSIBLE_ID` field of the previous step.
- `pingOffsets` — `[0, 15]`, reminders at the moment the deadline occurs and 15 minutes before it.
- `colorId` — `2`, the activity color in the timeline.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/')

    const activityId = 555;
    const ownerTypeId = 2;
    const ownerId = 18;
    const responsibleId = 1;
    const currentDeadline = '2026-08-14T10:00:00+03:00';

    function getTomorrowDeadlineWithSameTime(isoDateTime) {
        const dateTimeParts = isoDateTime.match(/^\d{4}-\d{2}-\d{2}(T.+)$/);

        if (!dateTimeParts) {
            throw new Error('Incorrect date format');
        }

        const tomorrow = new Date();
        tomorrow.setDate(tomorrow.getDate() + 1);

        const year = tomorrow.getFullYear();
        const month = String(tomorrow.getMonth() + 1).padStart(2, '0');
        const day = String(tomorrow.getDate()).padStart(2, '0');

        return `${year}-${month}-${day}${dateTimeParts[1]}`;
    }

    const deadline = getTomorrowDeadlineWithSameTime(currentDeadline);

    const updateResponse = await $b24.actions.v2.call.make({
        method: 'crm.activity.todo.update',
        params: {
            id: activityId,
            ownerTypeId: ownerTypeId,
            ownerId: ownerId,
            deadline: deadline,
            title: 'Contact client',
            responsibleId: responsibleId,
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
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $activityId = 555;
    $ownerTypeId = 2;
    $ownerId = 18;
    $responsibleId = 1;
    $currentDeadline = '2026-08-14T10:00:00+03:00';

    $currentDeadlineDate = new DateTime($currentDeadline);
    $deadline = new DateTime('tomorrow', $currentDeadlineDate->getTimezone());
    $deadline->setTime(
        (int)$currentDeadlineDate->format('H'),
        (int)$currentDeadlineDate->format('i'),
        (int)$currentDeadlineDate->format('s')
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
                'title' => 'Contact client',
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
    from datetime import datetime, timedelta

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    activity_id = 555
    owner_type_id = 2
    owner_id = 18
    responsible_id = 1
    current_deadline = datetime.fromisoformat("2026-08-14T10:00:00+03:00")
    tomorrow = datetime.now(current_deadline.tzinfo).date() + timedelta(days=1)
    deadline = datetime.combine(tomorrow, current_deadline.timetz())

    try:
        response = client.crm.activity.todo.update(
            bitrix_id=activity_id,
            owner_type_id=owner_type_id,
            owner_id=owner_id,
            deadline=deadline,
            title="Contact client",
            responsible_id=responsible_id,
            ping_offsets=[0, 15],
            color_id="2",
        ).response
        print(f"Task updated: {response.result['id']}")
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

Open the CRM item to which the activity is linked. The activity deadline will change in the timeline. If `pingOffsets` are specified, Bitrix24 will create reminders regarding the new deadline.

If the method returns an error, check the request data.

- `CAN_NOT_UPDATE_COMPLETED_TODO` — the activity is already closed. Closed activities cannot be modified.
- `WRONG_DATETIME_FORMAT` — the `deadline` value was passed in an incorrect format.
- `NOT_FOUND` — the CRM item or activity was not found. Check `id`, `ownerTypeId`, and `ownerId`.
- `ACCESS_DENIED` — the user does not have permission to modify the CRM item.
- `OWNER_NOT_FOUND` — the CRM item to which the activity is linked was not found.

## Continue Learning

- [{#T}](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-update.md)
- [{#T}](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)
- [{#T}](./how-to-move-activity.md)
