# How to Transfer a Deal from One Object Type to Another

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify CRM entities

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Activities associated with CRM items are stored in the item card timeline. Moving activities may be required between items of different types: [lead](../../../api-reference/crm/leads/index.md), [deal](../../../api-reference/crm/deals/index.md), [contact](../../../api-reference/crm/contacts/index.md), [company](../../../api-reference/crm/companies/index.md), [invoice](../../../api-reference/crm/universal/invoice.md), [SPA](../../../api-reference/crm/universal/index.md). For example, a customer has two e-mail addresses, but only one is saved in the company card of your Bitrix24. When the customer writes an e-mail from the second, unknown address, Webmail will create a new lead instead of attaching the e-mail to the existing company card. To store customer information in one place, you can move an activity from a lead to a company card.

To move an activity, we will sequentially execute four methods:

1. [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — retrieve the activity ID

2. [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) — retrieve the company ID for transferring the activity

3. [crm.activity.binding.add](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md) — add the binding of the activity to the company

4. [crm.activity.binding.delete](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md) — remove the binding of the activity from the lead

## 1. Retrieving the Activity ID {#first}

We will use the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) with the following filter:

- `OWNER_TYPE_ID` — [object type](../../../api-reference/crm/data-types.md#object_type), specify `1` for the lead

- `OWNER_ID` — the ID of the item from which the activity will be moved

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
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $result = $serviceBuilder->getCRMScope()->activity()->list(
        [],
        [
            'OWNER_TYPE_ID' => 1,
            'OWNER_ID' => 1000977,
        ],
        [],
        0
    );
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

    result = client.crm.activity.list(
        filter={
            "OWNER_TYPE_ID": 1,
            "OWNER_ID": 1000977,
        }
    ).response.result
    ```

{% endlist %}

As a result, you will retrieve all activities associated with the specified item.

```JSON
{
    "result": [
        {
            "ID": "7685",
            "OWNER_ID": "1000977",
            "OWNER_TYPE_ID": "1",
            "TYPE_ID": "4",
            "PROVIDER_ID": "CRM_EMAIL",
            "PROVIDER_TYPE_ID": "EMAIL",
            "PROVIDER_GROUP_ID": null,
            "ASSOCIATED_ENTITY_ID": "0",
            "SUBJECT": "for leads",
            "CREATED": "2025-03-10T10:57:41+03:00",
            "LAST_UPDATED": "2025-03-10T10:57:41+03:00",
            "START_TIME": "2025-03-10T10:57:34+03:00",
            "END_TIME": "2025-03-10T20:00:00+03:00",
            "DEADLINE": "9999-12-31T00:00:00+03:00",
            "COMPLETED": "N",
            "STATUS": "1",
            "RESPONSIBLE_ID": "29",
            "PRIORITY": "2",
            "NOTIFY_TYPE": "0",
            "NOTIFY_VALUE": "0",
            "DESCRIPTION": "<div>first email</div>\r\n",
            "DESCRIPTION_TYPE": "3",
            "DIRECTION": "1",
            "LOCATION": "",
            "SETTINGS": {
                "EMAIL_META": {
                    "__email": "some_email@gmail.com",
                    "from": "Some client <some_client@gmail.com>",
                    "replyTo": "",
                    "to": "\"some_email@gmail.com\" <some_email@gmail.com>",
                    "cc": "",
                    "bcc": ""
                },
                "SANITIZE_ON_VIEW": 1
            },
            "ORIGINATOR_ID": null,
            "ORIGIN_ID": null,
            "AUTHOR_ID": "1",
            "EDITOR_ID": "29",
            "PROVIDER_PARAMS": [],
            "PROVIDER_DATA": null,
            "RESULT_MARK": "0",
            "RESULT_VALUE": null,
            "RESULT_SUM": null,
            "RESULT_CURRENCY_ID": null,
            "RESULT_STATUS": "0",
            "RESULT_STREAM": "0",
            "RESULT_SOURCE_ID": null,
            "AUTOCOMPLETE_RULE": "0"
        },
    ],
    "total": 1,
}
```

## 2. Retrieving the Company ID {#second}

We will use the method [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) with the following filter:

- `TITLE` — the company name

To limit the returned fields, we will add the `select` parameter and specify only the `ID` and `TITLE` fields.

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
    $result = $serviceBuilder->getCRMScope()->company()->list(
        [],
        [
            'TITLE' => 'Company_Name'
        ],
        [
            'ID', 'TITLE'
        ],
        0
    );
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

As a result, you will retrieve the company ID — `ID`: `173`.

```JSON
{
    "result": [
        {
            "ID": "173",
            "TITLE": "Company_Name"
        }
    ],
    "total": 1,
}
```

## 3. Adding the Binding of the Activity to the Company

To bind the activity and the company, we will use the method [crm.activity.binding.add](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md) with the following parameters:

- `activityId` — the activity ID retrieved in [step 1](#first) using the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method

- `entityTypeId` — the [object type](../../../api-reference/crm/data-types.md#object_type) ID, specify `4` for the company

- `entityId` — the company ID retrieved in [step 2](#second) using the [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) method

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

As a result, we will receive `true`, indicating that the binding for the activity was successfully created. If you receive an `error` in the result, refer to the documentation for the method [crm.activity.binding.add](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md) to understand possible errors.

```JSON
{
    "result": true,
}
```

## 4. Removing the Binding of the Activity from the Lead

We will use the method [crm.activity.binding.delete](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md) with the following parameters:

- `activityId` — The activity ID obtained in [Step 1](#first) using the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method

- `entityTypeId` — The [object type](../../../api-reference/crm/data-types.md#object_type) ID; specify `1` for a lead

- `entityId` — The lead ID from which the activity is being removed

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

As a result, we will receive `true`, indicating that the binding of the activity from the lead was successfully removed. If you receive an `error` in the result, refer to the documentation for the method [crm.activity.binding.delete](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md) to understand possible errors.

```JSON
{
    "result": true,
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

    // Function to perform all steps
    async function transferActivityToCompany(leadId, companyName) {
        // Step 1: Get the task list for the specified lead
        const activities = await call("crm.activity.list", {
            filter: {
                "OWNER_TYPE_ID": 1,
                "OWNER_ID": leadId
            }
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

        // Step 3: Create a link between the found task and the company
        await call('crm.activity.binding.add', {
            activityId: activityId,
            entityTypeId: 4,
            entityId: companyId
        });

        console.log("Task-company link created successfully.");

        // Step 4: Delete the link between the task and the lead
        await call('crm.activity.binding.delete', {
            activityId: activityId,
            entityTypeId: 1,
            entityId: leadId
        });

        console.log("Task-lead link deleted successfully.");
    }

    // Request lead ID and company name from the user
    const rl = createInterface({ input: process.stdin, output: process.stdout });
    const leadId = await rl.question("Enter lead ID: ");
    const companyName = await rl.question("Enter company name: ");
    rl.close();

    // Running function
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
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    // Function to perform all steps
    function transferActivityToCompany($serviceBuilder, $leadId, $companyName) {
        $crm = $serviceBuilder->getCRMScope();

        try {
            // Step 1: Get the task list for the specified lead
            $activities = $crm->activity()->list(
                [],
                [
                    'OWNER_TYPE_ID' => 1,
                    'OWNER_ID' => $leadId
                ],
                [],
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

            // Step 3: Create a link between the found task and the company
            $serviceBuilder->core->call(
                'crm.activity.binding.add',
                [
                    'activityId' => $activityId,
                    'entityTypeId' => 4,
                    'entityId' => $companyId
                ]
            );

            echo "Task-company link created successfully.";

            // Step 4: Delete the link between the task and the lead
            $serviceBuilder->core->call(
                'crm.activity.binding.delete',
                [
                    'activityId' => $activityId,
                    'entityTypeId' => 1,
                    'entityId' => $leadId
                ]
            );

            echo "Task-lead link deleted successfully.";
        } catch (\Throwable $e) {
            echo 'Error: ' . $e->getMessage();
        }
    }

    // Request lead ID and company name from the user
    $leadId = readline("Enter lead ID: ");
    $companyName = readline("Enter company name: ");

    // Running function
    transferActivityToCompany($serviceBuilder, $leadId, $companyName);
    ``` 

- Python

    ```python
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError

    def transfer_activity_to_company(client, lead_id, company_name):
        try:
            activity_result = client.crm.activity.list(
                filter={
                    "OWNER_TYPE_ID": 1,
                    "OWNER_ID": lead_id,
                }
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

        print("Task-company link created successfully.")

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
                print("Task-lead link deleted successfully.")

    client = Client(
        BitrixWebhook(
            domain="your-domain.bitrix24.com",
            webhook_token="user_id/webhook_key",
        )
    )

    lead_id = int(input("Enter lead ID: "))
    company_name = input("Enter company name: ")

    transfer_activity_to_company(client, lead_id, company_name)
    ```

{% endlist %}
