# How to Transfer an Activity from One Object Type to Another

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with permission to modify CRM entities

Activities related to CRM entities are stored in the timeline of the entity's card. Transferring activities may be necessary between different types of entities: [lead](../../../api-reference/crm/leads/index.md), [deal](../../../api-reference/crm/deals/index.md), [contact](../../../api-reference/crm/contacts/index.md), [company](../../../api-reference/crm/companies/index.md), [invoice](../../../api-reference/crm/universal/invoice.md), [SPA](../../../api-reference/crm/universal/index.md). For example, if a client has two email addresses but only one is saved in your Bitrix24 company card, when the client sends an email from the second, unknown address, the email will create a new lead instead of attaching to the existing company card. To keep client information in one place, you can transfer the activity from the lead to the company card.

To transfer the activity, we will sequentially execute four methods:

1. [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) — retrieve the activity ID

2. [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) — retrieve the company ID for transferring the activity

3. [crm.activity.binding.add](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md) — add the binding of the activity to the company

4. [crm.activity.binding.delete](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md) — remove the binding of the activity from the lead

## 1. Retrieve the Activity ID {#first}

Use the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) with the filter:

- `OWNER_TYPE_ID` — [object type](../../../api-reference/crm/data-types.md#object_type), specify `1` for lead

- `OWNER_ID` — ID of the entity from which we will transfer the activity

{% include [Example Notes](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```JavaScript
    BX24.callMethod(
        "crm.activity.list",
        {
            filter:
            {
                "OWNER_TYPE_ID": 1, 
                "OWNER_ID": 1000977 
            },
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.list',
        [
            'filter' => [
                'OWNER_TYPE_ID' => 1, 
                'OWNER_ID' => 1000977 
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive all activities related to the specified entity.

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
        }
    ],
    "total": 1
}
```

## 2. Retrieve the Company ID {#second}

Use the method [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md) with the filter:

- `TITLE` — company name

To limit the returned fields, add the `select` parameter and specify only the `ID` and `TITLE` fields.

{% list tabs %}

- JS

    ```JavaScript
    BX24.callMethod(
        "crm.company.list",
        {
            filter: { "TITLE": "Company_Name" },
            select: [ "ID", "TITLE" ]
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.company.list',
        [
            'filter' => [
                'TITLE' => 'Company_Name' 
            ],
            'select' => [
                'ID', 'TITLE'
            ]
        ]
    );
    ```

{% endlist %}

As a result, we will receive the company ID — `ID`: `173`.

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

## 3. Add the Binding of the Activity to the Company

To bind the activity and the company, use the method [crm.activity.binding.add](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md) with the parameters:

- `activityId` — ID of the activity, obtained in [step 1](#first) using the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)

- `entityTypeId` — ID of the [object type](../../../api-reference/crm/data-types.md#object_type), specify `4` for company

- `entityId` — ID of the company, obtained in [step 2](#second) using the method [crm.company.list](../../../api-reference/crm/companies/crm-company-list.md)

{% list tabs %}

- JS

    ```JavaScript
    BX24.callMethod(
        'crm.activity.binding.add',
        {
            activityId: 7685, 
            entityTypeId: 4, 
            entityId: 173 
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.binding.add',
        [
            'activityId' => 7685, 
            'entityTypeId' => 4, 
            'entityId' => 173 
        ]
    );
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the binding for the activity was successfully created. If you receive an `error` in the result, review the possible error descriptions in the documentation for the method [crm.activity.binding.add](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-add.md).

```JSON
{
    "result": true
}
```

## 4. Remove the Binding of the Activity from the Lead

Use the method [crm.activity.binding.delete](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md) with the parameters:

- `activityId` — ID of the activity, obtained in [step 1](#first) using the method [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md)

- `entityTypeId` — ID of the [object type](../../../api-reference/crm/data-types.md#object_type), specify `1` for lead

- `entityId` — ID of the lead from which we are removing the activity

{% list tabs %}

- JS

    ```JavaScript
    BX24.callMethod(
        'crm.activity.binding.delete',
        {
            activityId: 7685, 
            entityTypeId: 1, 
            entityId: 1000977 
        },
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.binding.delete',
        [
            'activityId' => 7685, 
            'entityTypeId' => 1, 
            'entityId' => 1000977 
        ]
    );
    ```

{% endlist %}

As a result, we will receive `true`, indicating that the binding of the activity from the lead was successfully removed. If you receive an `error` in the result, review the possible error descriptions in the documentation for the method [crm.activity.binding.delete](../../../api-reference/crm/timeline/activities/binding/crm-activity-binding-delete.md).

```JSON
{
    "result": true
}
```

## Code Example

{% list tabs %}

- JS

    ```JavaScript
    // Function to execute all steps
    function transferActivityToCompany() {
        // Prompt user for lead ID
        const leadId = prompt("Enter lead ID:");

        // Prompt user for company name
        const companyName = prompt("Enter company name:");

        // Step 1: Retrieve the list of activities for the specified lead
        BX24.callMethod(
            "crm.activity.list",
            {
                filter: {
                    "OWNER_TYPE_ID": 1,
                    "OWNER_ID": leadId
                },
            },
            function(result) {
                if (result.error()) {
                    console.error(result.error());
                    return;
                }

                const activities = result.data();
                if (activities.length === 0) {
                    console.log("No activities found for the specified lead.");
                    return;
                }

                const activityId = activities[0].ID;

                // Step 2: Search for the company by name
                BX24.callMethod(
                    "crm.company.list",
                    {
                        filter: { "TITLE": companyName },
                        select: [ "ID", "TITLE" ]
                    },
                    function(result) {
                        if (result.error()) {
                            console.error(result.error());
                            return;
                        }

                        const companies = result.data();
                        if (companies.length === 0) {
                            console.log("No company found with the specified name.");
                            return;
                        }

                        const companyId = companies[0].ID;

                        // Step 3: Create a binding for the found activity and company
                        BX24.callMethod(
                            'crm.activity.binding.add',
                            {
                                activityId: activityId,
                                entityTypeId: 4,
                                entityId: companyId
                            },
                            function(result) {
                                if (result.error()) {
                                    console.error(result.error());
                                    return;
                                }

                                console.log("Activity successfully bound to the company.");

                                // Step 4: Remove the binding of the activity from the lead
                                BX24.callMethod(
                                    'crm.activity.binding.delete',
                                    {
                                        activityId: activityId,
                                        entityTypeId: 1,
                                        entityId: leadId
                                    },
                                    function(result) {
                                        if (result.error()) {
                                            console.error(result.error());
                                        } else {
                                            console.log("Activity successfully unbound from the lead.");
                                        }
                                    }
                                );
                            }
                        );
                    }
                );
            }
        );
    }

    // Execute the function
    transferActivityToCompany();
    ```

- PHP

    ```php
    require_once('crest.php');

    // Function to execute all steps
    function transferActivityToCompany($leadId, $companyName) {
        // Step 1: Retrieve the list of activities for the specified lead
        $activityResult = CRest::call(
            'crm.activity.list',
            [
                'filter' => [
                    'OWNER_TYPE_ID' => 1,
                    'OWNER_ID' => $leadId
                ]
            ]
        );

        if (isset($activityResult['error'])) {
            echo 'Error: ' . $activityResult['error_description'];
            return;
        }

        $activities = $activityResult['result'];
        if (empty($activities)) {
            echo "No activities found for the specified lead.";
            return;
        }

        $activityId = $activities[0]['ID'];

        // Step 2: Search for the company by name
        $companyResult = CRest::call(
            'crm.company.list',
            [
                'filter' => ['TITLE' => $companyName],
                'select' => ['ID', 'TITLE']
            ]
        );

        if (isset($companyResult['error'])) {
            echo 'Error: ' . $companyResult['error_description'];
            return;
        }

        $companies = $companyResult['result'];
        if (empty($companies)) {
            echo "No company found with the specified name.";
            return;
        }

        $companyId = $companies[0]['ID'];

        // Step 3: Create a binding for the found activity and company
        $bindingAddResult = CRest::call(
            'crm.activity.binding.add',
            [
                'activityId' => $activityId,
                'entityTypeId' => 4,
                'entityId' => $companyId
            ]
        );

        if (isset($bindingAddResult['error'])) {
            echo 'Error: ' . $bindingAddResult['error_description'];
            return;
        }

        echo "Activity successfully bound to the company.";

        // Step 4: Remove the binding of the activity from the lead
        $bindingDeleteResult = CRest::call(
            'crm.activity.binding.delete',
            [
                'activityId' => $activityId,
                'entityTypeId' => 1,
                'entityId' => $leadId
            ]
        );

        if (isset($bindingDeleteResult['error'])) {
            echo 'Error: ' . $bindingDeleteResult['error_description'];
        } else {
            echo "Activity successfully unbound from the lead.";
        }
    }

    // Prompt user for lead ID and company name
    $leadId = readline("Enter lead ID: ");
    $companyName = readline("Enter company name: ");

    // Execute the function
    transferActivityToCompany($leadId, $companyName);
    ```

{% endlist %}