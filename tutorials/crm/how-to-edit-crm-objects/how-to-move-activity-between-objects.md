# How to Move an Activity from One Object Type to Another

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to edit CRM items

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Activities associated with CRM items are stored in the item card's Timeline. Moving activities may be required between items of different types: [lead](../../../api-reference/crm/leads/index.md), [deal](../../../api-reference/crm/deals/index.md), [contact](../../../api-reference/crm/contacts/index.md), [company](../../../api-reference/crm/companies/index.md), [invoice](../../../api-reference/crm/universal/invoice.md), [SPA](../../../api-reference/crm/universal/index.md). For example, a customer has two e-mail addresses, but only one is saved in your Bitrix24 company card. When the customer writes an e-mail from the second, unknown address, Webmail will create a new lead instead of attaching the e-mail to the existing company card. To store customer information in one place, you can move an activity from a lead to a company card.

Moving between different object types consists of two operations: first, add the activity link to the new object, then delete the link to the old one. As a result of the scenario, the activity will appear in the company Timeline and disappear from the lead Timeline.

{% note warning "" %}

The [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md) method is not suitable here: it only moves an activity between items of the same type. If the types are different, the method will return error `SOURCE_AND_TARGET_ENTITY_TYPES_ARE_NOT_EQUAL_ERROR`. To move an activity between two leads or two deals, use the [How to Move an Activity Between Items of the Same Type](./how-to-move-activity.md) scenario.

{% endnote %}

To move an activity, we will sequentially execute four methods:

1. [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — retrieve the activity ID

2. [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) — retrieve the company ID to which the activity will be moved

3. [crm.activity.binding.add](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md) — add the activity link to the company

4. [crm.activity.binding.delete](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md) — delete the activity link to the lead

The order of steps 3 and 4 cannot be changed. If you delete the link to the lead first, the activity will be left without its only link, and the method will return error `LAST_BINDING_CANNOT_BE_DELETED`.

## 1. Retrieve the Activity ID {#first}

Use the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method with a filter:

- `OWNER_TYPE_ID` — [object type](../../../api-reference/crm/data-types.md#object_type), specify `1` for a lead,

- `OWNER_ID` — the ID of the item from which the activity will be moved.

In the example, we move an activity from lead `1000977`. Lead ID is visible in the address bar of its card, for example `/crm/lead/details/1000977/`, or it can be retrieved using the [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md) method.

Without the `select` parameter, the method returns all activity fields. To reduce the response size, specify only the fields required for the scenario: `ID`, `OWNER_TYPE_ID`, `OWNER_ID`, `SUBJECT`, and `DESCRIPTION`.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```JavaScript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const result = await $b24.actions.v2.call.make({
        method: "crm.activity.list",
        params: {
            filter:
            {
                "OWNER_TYPE_ID": 1,
                "OWNER_ID": 1000977
            },
            select: [ "ID", "OWNER_TYPE_ID", "OWNER_ID", "SUBJECT", "DESCRIPTION" ]
        }
    });
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
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    $activities = $serviceBuilder->getCRMScope()->activity()->list(
        [],
        [
            'OWNER_TYPE_ID' => 1,
            'OWNER_ID' => 1000977,
        ],
        [
            'ID', 'OWNER_TYPE_ID', 'OWNER_ID', 'SUBJECT', 'DESCRIPTION'
        ],
        0
    )->getActivities();
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token=os.environ["B24_HOOK_TOKEN"],
        )
    )
    # B24_HOOK_TOKEN = 'user_id/webhook_key'

    result = client.crm.activity.list(
        filter={
            "OWNER_TYPE_ID": 1,
            "OWNER_ID": 1000977,
        },
        select=["ID", "OWNER_TYPE_ID", "OWNER_ID", "SUBJECT", "DESCRIPTION"],
    ).response.result
    ```

{% endlist %}

As a result, we will retrieve all activities associated with the specified item.

```JSON
{
    "result": [
        {
            "ID": "7685",
            "OWNER_TYPE_ID": "1",
            "OWNER_ID": "1000977",
            "SUBJECT": "for leads",
            "DESCRIPTION": "<div>first email</div>\r\n"
        }
    ],
    "total": 1
}
```

We will save the activity `ID`: `7685`. We will pass this value into the parameter `activityId` in steps 3 and 4.

## 2. Retrieve the Company ID {#second}

Use the [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) method with a filter:

- `TITLE` — the company name.

To limit the returned fields, add the `select` parameter and specify only the `ID` and `TITLE` fields.

{% list tabs %}

- JS

    ```JavaScript
    const result = await $b24.actions.v2.call.make({
        method: "crm.company.list",
        params: {
            filter: { "TITLE": "Company_Name" },
            select: [ "ID", "TITLE" ]
        }
    });
    ```

- PHP

    ```php
    $companies = $serviceBuilder->getCRMScope()->company()->list(
        [],
        [
            'TITLE' => 'Company_Name'
        ],
        [
            'ID', 'TITLE'
        ],
        0
    )->getCompanies();
    ```

- Python

    ```python
    result = client.crm.company.list(
        filter={
            "TITLE": "Company_Name",
        },
        select=["ID", "TITLE"],
    ).response.result
    ```

{% endlist %}

As a result, you will obtain the company ID — `ID`: `173`. We will pass this value into the `entityId` parameter in step 3.

```JSON
{
    "result": [
        {
            "ID": "173",
            "TITLE": "Company_Name"
        }
    ],
    "total": 1
}
```

## 3. Add the Activity Link to the Company

To link the activity and the company, use the [crm.activity.binding.add](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md) method with the following parameters:

- `activityId` — the activity ID obtained in [step 1](#first) using the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method,

- `entityTypeId` — the [object type](../../../api-reference/crm/data-types.md#object_type) ID; specify `4` for the company,

- `entityId` — the company ID obtained in [step 2](#second) using the [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) method.

{% list tabs %}

- JS

    ```JavaScript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.activity.binding.add',
        params: {
            activityId: 7685,
            entityTypeId: 4,
            entityId: 173
        }
    });
    ```

- PHP

    ```php
    // crm.activity.binding.add does not have a typed wrapper — calling via core
    $result = $serviceBuilder->core->call(
        'crm.activity.binding.add',
        [
            'activityId' => 7685,
            'entityTypeId' => 4,
            'entityId' => 173
        ]
    );
    ```

- Python

    ```python
    result = client.crm.activity.binding.add(
        activity_id=7685,
        entity_type_id=4,
        entity_id=173,
    ).response.result
    ```

{% endlist %}

As a result, you will receive `true`, indicating that the activity link was added successfully. The activity is now linked to two elements simultaneously — the lead and the company.

```JSON
{
    "result": true
}
```

## 4. Delete the Activity Link to the Lead

Use the [crm.activity.binding.delete](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md) method with the following parameters:

- `activityId` — the activity ID obtained in [step 1](#first) using the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method,

- `entityTypeId` — the [object type](../../../api-reference/crm/data-types.md#object_type) ID; specify `1` for the lead,

- `entityId` — the lead ID from which the activity is being removed.

{% list tabs %}

- JS

    ```JavaScript
    const result = await $b24.actions.v2.call.make({
        method: 'crm.activity.binding.delete',
        params: {
            activityId: 7685,
            entityTypeId: 1,
            entityId: 1000977
        }
    });
    ```

- PHP

    ```php
    // crm.activity.binding.delete does not have a typed wrapper — calling via core
    $result = $serviceBuilder->core->call(
        'crm.activity.binding.delete',
        [
            'activityId' => 7685,
            'entityTypeId' => 1,
            'entityId' => 1000977
        ]
    );
    ```

- Python

    ```python
    result = client.crm.activity.binding.delete(
        activity_id=7685,
        entity_type_id=1,
        entity_id=1000977,
    ).response.result
    ```

{% endlist %}

As a result, you will receive `true`, indicating that the activity link to the lead was deleted successfully. The transfer is complete: the activity now has only one link — to the company.

```JSON
{
    "result": true
}
```

## Code Example

{% list tabs %}

- JS

    ```JavaScript
    import { B24Hook } from '@bitrix24/b24jssdk'
    import { createInterface } from 'node:readline/promises'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    async function call(method, params) {
        const result = await $b24.actions.v2.call.make({ method, params });
        if (!result.isSuccess) {
            throw new Error(result.getErrorMessages().join('; '));
        }
        return result.getData().result;
    }

    // Function to execute all steps
    async function transferActivityToCompany(leadId, companyName) {
        // Step 1: Get the list of tasks for the specified lead
        const activities = await call("crm.activity.list", {
            filter: {
                "OWNER_TYPE_ID": 1,
                "OWNER_ID": leadId
            },
            select: [ "ID", "OWNER_TYPE_ID", "OWNER_ID", "SUBJECT", "DESCRIPTION" ]
        });
        if (activities.length === 0) {
            console.log("Tasks for the specified lead were not found.");
            return;
        }

        const activityId = activities[0].ID;

        // Step 2: Search for the company by name
        const companies = await call("crm.company.list", {
            filter: { "TITLE": companyName },
            select: [ "ID", "TITLE" ]
        });
        if (companies.length === 0) {
            console.log("Company with the specified name was not found.");
            return;
        }

        const companyId = companies[0].ID;

        // Step 3: Create a link for the found task and company
        await call('crm.activity.binding.add', {
            activityId: activityId,
            entityTypeId: 4,
            entityId: companyId
        });

        console.log("Link between task and company successfully created.");

        // Step 4: Delete the link between task and lead
        await call('crm.activity.binding.delete', {
            activityId: activityId,
            entityTypeId: 1,
            entityId: leadId
        });

        console.log("Link between task and lead successfully deleted.");
    }

    // Request lead ID and company name from the user
    const rl = createInterface({ input: process.stdin, output: process.stdout });
    const leadId = await rl.question("Enter lead ID: ");
    const companyName = await rl.question("Enter company name: ");
    rl.close();

    // Run function
    try {
        await transferActivityToCompany(leadId, companyName);
    } catch (error) {
        console.error(error.message);
    }
    ```

- PHP

    ```php
    <?php
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $logger = new Logger('b24');
    $logger->pushHandler(new StreamHandler('php://stdout'));

    $serviceBuilder = (new ServiceBuilderFactory(new EventDispatcher(), $logger))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    // Function to execute all steps
    function transferActivityToCompany($serviceBuilder, $leadId, $companyName) {
        $crm = $serviceBuilder->getCRMScope();

        try {
            // Step 1: Get the list of tasks for the specified lead
            $activities = $crm->activity()->list(
                [],
                [
                    'OWNER_TYPE_ID' => 1,
                    'OWNER_ID' => $leadId
                ],
                [
                    'ID', 'OWNER_TYPE_ID', 'OWNER_ID', 'SUBJECT', 'DESCRIPTION'
                ],
                0
            )->getActivities();

            if (empty($activities)) {
                echo "Tasks for the specified lead were not found.";
                return;
            }

            $activityId = $activities[0]->ID;

            // Step 2: Search for the company by name
            $companies = $crm->company()->list(
                [],
                ['TITLE' => $companyName],
                ['ID', 'TITLE'],
                0
            )->getCompanies();

            if (empty($companies)) {
                echo "Company with the specified name was not found.";
                return;
            }

            $companyId = $companies[0]->ID;

            // Step 3: Create a link for the found task and company
            // crm.activity.binding.add does not have a typed wrapper — calling via core
            $serviceBuilder->core->call(
                'crm.activity.binding.add',
                [
                    'activityId' => $activityId,
                    'entityTypeId' => 4,
                    'entityId' => $companyId
                ]
            );

            echo "Link between task and company successfully created.";

            // Step 4: Delete the link between task and lead
            // crm.activity.binding.delete does not have a typed wrapper — calling via core
            $serviceBuilder->core->call(
                'crm.activity.binding.delete',
                [
                    'activityId' => $activityId,
                    'entityTypeId' => 1,
                    'entityId' => $leadId
                ]
            );

            echo "Link between task and lead successfully deleted.";
        } catch (\Throwable $e) {
            echo 'Error: ' . $e->getMessage();
        }
    }

    // Request lead ID and company name from the user
    $leadId = readline("Enter lead ID: ");
    $companyName = readline("Enter company name: ");

    // Run function
    transferActivityToCompany($serviceBuilder, $leadId, $companyName);
    ```

- Python

    ```python
    import os

    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def transfer_activity_to_company(client, lead_id, company_name):
        try:
            activity_result = client.crm.activity.list(
                filter={
                    "OWNER_TYPE_ID": 1,
                    "OWNER_ID": lead_id,
                },
                select=["ID", "OWNER_TYPE_ID", "OWNER_ID", "SUBJECT", "DESCRIPTION"],
            ).response.result
        except BitrixAPIError as error:
            print(f"Error: {error}")
            return

        if not activity_result:
            print("Tasks for the specified lead were not found.")
            return

        activity_id = activity_result[0]["ID"]

        try:
            company_result = client.crm.company.list(
                filter={"TITLE": company_name},
                select=["ID", "TITLE"],
            ).response.result
        except BitrixAPIError as error:
            print(f"Error: {error}")
            return

        if not company_result:
            print("Company with the specified name was not found.")
            return

        company_id = company_result[0]["ID"]

        try:
            add_result = client.crm.activity.binding.add(
                activity_id=activity_id,
                entity_type_id=4,
                entity_id=company_id,
            ).response.result
        except BitrixAPIError as error:
            print(f"Error: {error}")
            return

        if not add_result:
            return

        print("Link between task and company successfully created.")

        try:
            delete_result = client.crm.activity.binding.delete(
                activity_id=activity_id,
                entity_type_id=1,
                entity_id=lead_id,
            ).response.result
        except BitrixAPIError as error:
            print(f"Error: {error}")
        else:
            if delete_result:
                print("Link between task and lead successfully deleted.")

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token=os.environ["B24_HOOK_TOKEN"],
        )
    )
    # B24_HOOK_TOKEN = 'user_id/webhook_key'

    lead_id = int(input("Enter lead ID: "))
    company_name = input("Enter company name: ")

    transfer_activity_to_company(client, lead_id, company_name)
    ```

{% endlist %}

## Verify the Result

Open the company card — the transferred e-mail will appear in the Timeline. The activity will no longer be in the lead card: the scenario transfers the link rather than copying it.

You can verify the result via REST using the [crm.activity.binding.list](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-list.md) method. Pass the `activityId` of the transferred activity to it — the method will return all activity links. After a successful transfer, only one link should remain in the response: the object type `4` and the company ID. The link to the lead, object type `1`, should not be in the response.

```JSON
{
    "result": [
        {
            "entityTypeId": 4,
            "entityId": 173
        }
    ]
}
```

If both links remain in the response, step 4 was not executed — repeat it. If the link to the company does not appear, return to step 3.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `LAST_BINDING_CANNOT_BE_DELETED` | You are deleting the only connection of the activity. First, perform step 3 and link the activity to a company, only then delete the connection with the lead ||
|| `ACTIVITY_IS_ALREADY_BOUND` | The activity is already linked to a company. Step 3 is completed, proceed to step 4 ||
|| `BINDING_NOT_FOUND` | The activity is not linked to a lead from `entityId`. Check which item you are moving the activity from ||
|| `NOT_FOUND` | Activity or CRM item not found. Check `activityId` and `entityId` ||
|| `OWNER_NOT_FOUND` | Activity owner not found. Check `entityTypeId` and `entityId` ||
|| `ACCESS_DENIED` | The user does not have permission to modify CRM items ||
|| `100` | Mandatory parameters were not passed. Methods `binding.add` and `binding.delete` require all three: `activityId`, `entityTypeId`, and `entityId` ||
|#

## Key Considerations

- Activities between items of the same type are moved using a single method [crm.activity.binding.move](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-move.md); a two-step scenario is not required for this
- The order of steps 3 and 4 cannot be changed: an activity must always retain at least one link
- Between steps 3 and 4, the activity is visible in the timeline of both items — both the lead and the company
- The custom activity fields `OWNER_TYPE_ID` and `OWNER_ID` switch to the company only after step 4, when the activity has only one link remaining. As long as there are two links, the lead remains the owner. After step 4, [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) with a filter by lead will no longer return the moved activity; look for it by company with `OWNER_TYPE_ID` equal to `4`
- The company in the scenario is only an example of a target object. To move an activity to a deal, find it using the [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) method and pass `2` into `entityTypeId` of step 3. Values for other types can be found in the [object types](../../../api-reference/crm/data-types.md#object_type) reference
- The [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) method by filter `TITLE` may return several companies with the same name; verify that you have selected the correct company
- Rerunning the example on the same lead will not find the already moved activity: the link to the lead no longer exists, and the example will terminate with a message stating that no activities were found

## Continue Learning

- [{#T}](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md)
- [{#T}](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md)
- [{#T}](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-list.md)
- [{#T}](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)
- [{#T}](./how-to-move-activity.md)
- [{#T}](./how-to-change-date-in-activity.md)
