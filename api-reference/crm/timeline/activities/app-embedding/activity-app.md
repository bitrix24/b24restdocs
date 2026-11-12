# How to Create an Activity from an Application

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the methods: a user with the permissions required for the specific action:
>
> - [crm.activity.add](../activity-base/crm-activity-add.md) and [crm.activity.update](../activity-base/crm-activity-update.md) — permission to modify the linked CRM item
> - [crm.activity.delete](../activity-base/crm-activity-delete.md) — permission to delete the linked CRM item

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

An installed application can add an activity to the timeline of a CRM card.

When an employee opens it, Bitrix24 displays the application page in the side panel. This way you can work with the application directly from the card: view linked data, perform the required actions, or address an external service.

## How the Scenario Works {#workflow}

Three participants are involved in the scenario: the user, Bitrix24, and the application:

- the user — selects a CRM item and works with the created activity in the timeline
- Bitrix24 — retains the activity and passes its opening parameters to the application page
- the application — creates, updates, or deletes the activity through the REST API

First, the user selects a CRM item on the application page — a lead in this scenario. The application creates an activity for it using the [crm.activity.add](../activity-base/crm-activity-add.md) method. The activity appears in the timeline of the lead.

When the user clicks this activity, Bitrix24 opens the application page and passes the activity identifier in `PLACEMENT_OPTIONS`. This is how the application knows which activity to work with and can update or delete it.

The scenario uses the following methods:

- [crm.activity.add](../activity-base/crm-activity-add.md) — creates the application activity
- [crm.activity.update](../activity-base/crm-activity-update.md) — updates or completes the activity
- [crm.activity.delete](../activity-base/crm-activity-delete.md) — deletes the activity

Next, build the application page step by step: prepare the file, determine the opening mode, add the interface, and set up the work with the activity. To call REST API methods and interface functions, use the [BX24.js](../../../../../sdk/bx24-js-sdk/index.md) JavaScript library.

## 1. Prepare the Application {#start}

Create an [application](../../../../../settings/app-installation/index.md) with the [`crm`](../../../../scopes/permissions.md) scope and prepare a server with an HTTPS address accessible from the external network. An application installation URL is not required for this scenario.

Before you begin, prepare:

- the identifier of an existing employee who will be responsible for the activity. Substitute it into the `responsibleId` variable in step four
- access to the leads you will use to test the scenario. The application user must have permissions to modify and delete the selected lead

Create the `index.php` file on the server. In the following steps, add the code blocks to it one after another. After the last step, you will have a complete application page.

## 2. Determine the Opening Mode {#placement-options}

The same application page can be opened in two situations:

- the user launches the application in Bitrix24 — on the page, the user selects a lead and creates an activity for it
- the user clicks the created activity in the timeline of the lead — Bitrix24 opens the same page with actions to complete or delete the activity

To determine the situation, the application checks the parameters passed by Bitrix24. When an activity is opened from the timeline, Bitrix24 passes `PLACEMENT_OPTIONS` in a POST request. The value contains a JSON string with two parameters:

- `action` — the action the page is opened with. For an application activity, the value is `view_activity`
- `activity_id` — the numeric identifier of the opened activity. Pass it to the [crm.activity.update](../activity-base/crm-activity-update.md) or [crm.activity.delete](../activity-base/crm-activity-delete.md) methods

Add the code that retrieves and checks these parameters to the beginning of the `index.php` file:

{% include [Note on examples](../../../../../_includes/examples.md) %}

```php
<?php
header('Content-Type: text/html; charset=UTF-8');

$placementOptions = [];

if (!empty($_POST['PLACEMENT_OPTIONS']))
{
    $decodedOptions = json_decode($_POST['PLACEMENT_OPTIONS'], true);

    if (is_array($decodedOptions))
    {
        $placementOptions = $decodedOptions;
    }
}

$activityId = isset($placementOptions['activity_id'])
    ? (int)$placementOptions['activity_id']
    : 0;

$isActivityView = ($placementOptions['action'] ?? '') === 'view_activity'
    && $activityId > 0;
?>
```

If the page is opened from an activity, the `$isActivityView` variable receives the value `true`, and `$activityId` receives the numeric identifier of the activity.

## 3. Add the Page Interface {#interface}

Right after the PHP block, add the HTML markup. It displays the required buttons depending on the value of `$isActivityView`:

```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Application activity</title>
</head>
<body hidden>
<script src="//api.bitrix24.com/api/v1/"></script>

<?php if ($isActivityView): ?>
    <p>Activity identifier: <?= $activityId ?></p>
    <button type="button" onclick="updateActivity(<?= $activityId ?>)">
        Complete activity
    </button>
    <button type="button" onclick="deleteActivity(<?= $activityId ?>)">
        Delete activity
    </button>
<?php else: ?>
    <button type="button" onclick="selectCRMEntity()">Select lead</button>
    <span id="selected-entity">Lead not selected</span>
    <button type="button" onclick="addActivity()">Create activity</button>
<?php endif; ?>

<p id="status" role="status"></p>
```

The `$isActivityView` branch displays actions for the opened activity, while the alternative branch displays lead selection and activity creation. The `#status` element displays result messages in both modes.

## 4. Initialize BX24.js {#initialize}

After the HTML markup, add the shared variables and helper functions. Replace the value of `responsibleId` with the identifier of the employee responsible for the activity.

The [BX24.js](../../../../../sdk/bx24-js-sdk/index.md) library is already included in the previous block. Wait for it to initialize using [BX24.init](../../../../../sdk/bx24-js-sdk/system-functions/bx24-init.md) before calling Bitrix24 methods.

```html
<script>
    const responsibleId = 1;
    let selectedEntityId = null;

    BX24.init(() => {
        document.body.hidden = false;
    });

    function showStatus(message, isError = false)
    {
        const status = document.getElementById('status');
        status.textContent = message;
        status.style.color = isError ? 'red' : 'green';
    }

    function showError(result)
    {
        showStatus(
            `Error: ${result.error()} — ${result.error_description()}`,
            true
        );
    }
</script>
```

After `BX24.init` runs, the page becomes visible. The `showStatus` and `showError` functions display the result of REST API calls.

## 5. Add Activity Creation {#create}

To create an application activity, call the [crm.activity.add](../activity-base/crm-activity-add.md) method with the `PROVIDER_ID=REST_APP` field. An activity with `PROVIDER_ID=REST_APP` can be created only from an installed application. It can be updated or deleted only by the application that created it. When called through a webhook, Bitrix24 returns the error `Application context required.`, and when called from another application, it returns `Access denied.`

The [crm.activity.add](../activity-base/crm-activity-add.md) method is marked as deprecated, but in this scenario it cannot be replaced with [crm.activity.todo.add](../todo/crm-activity-todo-add.md): that method does not accept the `PROVIDER_ID` field.

In the example, the user selects a lead through [BX24.selectCRM](../../../../../sdk/bx24-js-sdk/system-dialogues/bx24-select-crm.md). For a lead, the function returns an identifier with a prefix, for example `L_123`. The application removes the `L_` prefix and passes the numeric part to `OWNER_ID` of the [crm.activity.add](../activity-base/crm-activity-add.md) method.

For a different CRM object, change `entityType`, the identifier prefix, and `OWNER_TYPE_ID`.

The main fields for creating an activity:

{% include [Note on required parameters](../../../../../_includes/required.md) %}

- `OWNER_TYPE_ID*` — the numeric identifier of the [CRM object type](../../../data-types.md#object_type). For a lead, pass `1`
- `OWNER_ID*` — the numeric identifier of the selected lead
- `PROVIDER_ID*` — the provider identifier. For an application activity, pass `REST_APP`
- `PROVIDER_TYPE_ID` — the type of the application activity. If the field is not passed, Bitrix24 uses the value `LINK`
- `SUBJECT*` — the name of the activity in the timeline
- `RESPONSIBLE_ID` — the identifier of the employee responsible for the activity. Pass it explicitly in this scenario

The `TYPE_ID` field is normally required for [crm.activity.add](../activity-base/crm-activity-add.md). For `REST_APP`, it can be omitted: the method automatically sets the activity type to "Provider".

After the previous block, add the functions for selecting a lead and creating an activity:

```html
<script>
    function selectCRMEntity()
    {
        BX24.selectCRM(
            { entityType: ['lead'] },
            (selected) => {
                const lead = selected.lead && selected.lead[0];

                if (!lead)
                {
                    return;
                }

                const id = Number(lead.id.replace(/^L_/, ''));

                if (!Number.isInteger(id) || id <= 0)
                {
                    showStatus('Failed to determine the lead identifier', true);
                    return;
                }

                selectedEntityId = id;
                document.getElementById('selected-entity').textContent = lead.title;
                showStatus(`Selected the lead with identifier ${id}`);
            }
        );
    }

    function addActivity()
    {
        if (!selectedEntityId)
        {
            showStatus('Select a lead first', true);
            return;
        }

        BX24.callMethod(
            'crm.activity.add',
            {
                fields: {
                    OWNER_TYPE_ID: 1,
                    OWNER_ID: selectedEntityId,
                    PROVIDER_ID: 'REST_APP',
                    PROVIDER_TYPE_ID: 'LINK',
                    SUBJECT: 'New application activity',
                    COMPLETED: 'N',
                    RESPONSIBLE_ID: responsibleId,
                    DESCRIPTION: 'Description of the new activity'
                }
            },
            (result) => {
                if (result.error())
                {
                    showError(result);
                    return;
                }

                showStatus(`Activity created. Identifier: ${result.data()}`);
            }
        );
    }
</script>
```

After a successful call to [crm.activity.add](../activity-base/crm-activity-add.md), the page displays the numeric identifier of the created activity.

Example of a successful response with HTTP status `200`:

```json
{
    "result": 123,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

The `result` field contains the activity identifier, which is `123` in this example. The `time` field contains request timing information. When the user opens this activity in the timeline, Bitrix24 passes the same identifier to the application page in `PLACEMENT_OPTIONS.activity_id`.

## 6. Add Activity Update and Deletion {#manage}

To complete an activity, pass its identifier to the [crm.activity.update](../activity-base/crm-activity-update.md) method and set the `COMPLETED=Y` field.

To delete an activity, pass the same identifier to the [crm.activity.delete](../activity-base/crm-activity-delete.md) method.

{% note warning "" %}

The `crm.activity.delete` source code has a defect: when checking ownership of a `REST_APP` activity, the application identifier is read as a string and compared strictly to a numeric identifier without type casting. Therefore, in normal operation, when the activity is loaded from the database, the method returns `Access denied.` even for the application that created it. Until the method is fixed, deleting such an activity through this scenario does not work, while updating the activity remains available.

{% endnote %}

Add the following block after the activity creation functions. It also closes the `body` and `html` elements, so place it at the end of the file:

```html
<script>
    function updateActivity(id)
    {
        BX24.callMethod(
            'crm.activity.update',
            {
                id,
                fields: {
                    COMPLETED: 'Y',
                    SUBJECT: 'Activity completed',
                    DESCRIPTION: 'Description of the completed activity'
                }
            },
            (result) => {
                if (result.error())
                {
                    showError(result);
                    return;
                }

                showStatus('Activity updated');
            }
        );
    }

    function deleteActivity(id)
    {
        if (!window.confirm('Delete the activity?'))
        {
            return;
        }

        BX24.callMethod(
            'crm.activity.delete',
            { id },
            (result) => {
                if (result.error())
                {
                    showError(result);
                    return;
                }

                showStatus('Activity deleted');
            }
        );
    }
</script>
</body>
</html>
```

After a successful update or deletion, the [crm.activity.update](../activity-base/crm-activity-update.md) and [crm.activity.delete](../activity-base/crm-activity-delete.md) methods return a response with HTTP status `200` and the same structure:

```json
{
    "result": true,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

The `result=true` value confirms a successful update or deletion. The `time` field contains request timing information. The page displays a message about the completed action.

## 7. Deploy and Check the Application {#check}

Save `index.php` and place it on a server accessible from the external network. Specify the resulting HTTPS address as the address of the main page of the application.

Check the scenario:

1. Open the application, select a lead, and create an activity
2. Check that the application displayed the numeric identifier of the new activity
3. Open the card of the selected lead and find the activity in the timeline
4. Click the activity and check that Bitrix24 opened the application with the same identifier
5. Complete the activity and check its status in the timeline

## Errors and Diagnostics {#errors}

The `showError` function displays the error code and description returned by the method. Resolve the issue based on the message:

- `Application context required.` — the call was made outside the application context, for example, through an inbound webhook. Open the installed application page and repeat the action from step five or six
- `Access denied.` — the user lacks permissions for the action: creating and updating an activity requires permission to modify the linked CRM item, and deleting an activity requires permission to delete this item. This error also occurs if another application created the activity. Check the permissions and activity ownership, then repeat the action
- `Activity is not found.` — the activity with the identifier from `activity_id` was not found or has already been deleted. Open the current activity from the timeline and repeat step six
- `Could not build binding. Please ensure that owner info and communications are defined correctly.` — the activity owner is not specified. Check that `OWNER_TYPE_ID=1` and `OWNER_ID` contains a positive numeric identifier without the `L_` prefix, then repeat step five
- `Could not find 'LEAD' with ID: N` — no lead with the identifier `N` was found. Select an existing lead and repeat step five

If the page remains hidden and the buttons are unavailable, check the application's HTTPS address and make sure that BX24.js loads and `BX24.init` runs. After correcting the issue, reload the application page. Until you receive an identifier in the `crm.activity.add` response, do not consider the activity created and do not blindly repeat the call: check the timeline of the selected lead first.
