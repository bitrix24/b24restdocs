# Add a CRM Activity to a New Lead or Deal Depending on the CRM Mode

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method:
> -  Creating a lead — users with the permission to create leads,
> -  Adding an activity to a lead or deal — users with the permission to modify leads or deals in CRM.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../sdk/mcp.md) so that the assistant can use the official REST documentation.

{% endnote %}

You can place a form on your website to collect potential client data. When a client fills out the form, their data will be sent to the CRM. You will be able to process the request and call the client.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send the data to the handler.

2. Create a file to process the data. The handler will:

   -  accept and prepare the data,
   
   -  create a lead using the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add) method,
   
   -  check the CRM mode,
   
   -  add an activity with a reminder to call either the deal or the lead.

## CRM Modes

Bitrix24 has two CRM modes.

1. Simple mode — operates without leads. The system automatically converts a new lead into a deal.

2. Classic mode — separates potential and existing clients. The lead remains in the system after creation.

In the handler, we will determine which mode the CRM is operating in — simple or classic — and based on that, we will link the call reminder to either the deal or the lead.

{% note tip "User Documentation" %}

-  [How to choose the CRM operation mode](https://helpdesk.bitrix24.com/open/24207198/)

{% endnote %}

## 1. Creating a Web Form

In Bitrix24, you can automatically create a contact and a company from a lead. To make the form suitable for various cases, we will make it universal. For a contact, you need to specify the first name and last name, and for a company — the name. We will create a web form on the website page with five fields:

-  `NAME` — client's first name, required field,

-  `LAST_NAME` — last name,

-  `COMPANY_TITLE` — company name,

-  `PHONE` — phone number,

-  `EMAIL` — email address.

When submitted, the form sends data to the handler `form.php`.

```html
<form id="form_to_crm" method="POST" action="form.php">
    <!-- First Name (required field) -->
    <input type="text" name="NAME" placeholder="First Name" required>
    <!-- Last Name -->
    <input type="text" name="LAST_NAME" placeholder="Last Name">
    <!-- Company Name -->
    <input type="text" name="COMPANY_TITLE" placeholder="Company Name">
    <!-- Email -->
    <input type="text" name="EMAIL" placeholder="Email">
    <!-- Phone -->
    <input type="text" name="PHONE" placeholder="Phone">
    <!-- Submit Button -->
    <input type="submit" value="Submit">
</form>

<!-- Include jQuery for AJAX request -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script>
    $(document).ready(function() {
        $('#form_to_crm').on('submit', function(el) {
            el.preventDefault(); // Prevent default form submission
            var formData = $(this).serialize(); // Collect form data
            // Send data to the server
            $.ajax({
                'method': 'POST',
                'dataType': 'json',
                'url': 'form.php', // Handler file
                'data': formData,
                success: function(data) {
                    alert(data.message); // Show result
                }
            });
        });
    });
</script>
```

## 2. Creating the Form Handler

We will create a file `form.php` that will:

-  accept data from the form,

-  create a lead,

-  determine the CRM mode,

-  add an activity with a reminder to call either the lead or the deal.

### Preparing Data from the Form

We will retrieve and sanitize the data from the form.

```php
// Retrieve and sanitize data from the form
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);
```

The system stores phone numbers and emails as an array of objects [crm_multifield](../../../api-reference/crm/data-types#crm_multifield), so they need to be formatted as an array.

1. If a value exists, we add it as the first element `VALUE` in the array, and the second value specifies the type `VALUE_TYPE`, for example:

   -  `WORK` — for phone,

   -  `HOME` — for email.

2. If no values exist, we pass an empty array.

```php
// Format phone and email for Bitrix24 in crm_multifield format
$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
$arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();
```

We will form the lead title from the first name and last name. For companies, we will add the company name to the title.

```php
// Form the lead title from the first name and last name
$sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
// If there is a company name — add it after the first name and last name with a dash
if (!empty($sCompanyTitle)) {
    $sTitle .= ' — ' . $sCompanyTitle;
}
```

### Creating a Lead and Retrieving Lead Data

We will use the batch method [CRest::callBatch()](../../../settings/how-to-call-rest-api/batch.md) to sequentially execute two methods in one request: creating a lead and retrieving lead data. We will collect the methods in the array `$arData[]`.

To add a lead, we will use the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md). In the `fields` object, we will pass the fields:

-  `TITLE` — lead title,

-  `NAME` — lead first name,

-  `LAST_NAME` — lead last name,

-  `COMPANY_TITLE` — company name,

-  `PHONE` — phone number,

-  `EMAIL` — email address.

{% note warning "" %}

Check which required fields are set for leads in your Bitrix24. All required fields must be passed to the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md).

{% endnote %}

```php
'add_lead' => [
    'method' => 'crm.lead.add',
    'params' => [
        'fields' => [
            'TITLE' => $sTitle, // Lead title
            'NAME' => $sName, // First Name
            'LAST_NAME' => $sLastName, // Last Name
            'COMPANY_TITLE' => $sCompanyTitle, // Company Name
            'PHONE' => $arPhone, // Phone
            'EMAIL' => $arEmail, // Email
        ]
    ]
],
```

To retrieve lead data, we will use the method [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md). In the `ID` parameter, we will pass the identifier of the created lead `$result[add_lead]` from the result of the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method.

```php
'get_lead' => [
    'method' => 'crm.lead.get',
    'params' => [
        'id' => $result[add_lead] // ID from the result of the crm.lead.add method
    ]
]
```

We will pass the array of requests `$arData` to the `CRest::callBatch` method. The result of the method execution will be saved in the variable `$result`.

```php
$result = CRest::callBatch($arData);
```

As a result, we will obtain the identifier of the new lead `add_lead` and the lead data `get_lead`.

```json
{
    "result": {
        "result": {
            "add_lead": 22, // result of the crm.lead.add method
            "get_lead": { // result of the crm.lead.get method
                "ID": "22",
                "TITLE": "John Doe",
                "HONORIFIC": null,
                "NAME": "John",
                "SECOND_NAME": null,
                "LAST_NAME": "Doe",
                "COMPANY_TITLE": null,
                ...,
                "STATUS_ID": "CONVERTED",
                ...
            }
        },
        "result_error": [],
        ...
}
```

### Determining the CRM Mode and Creating an Activity

If the system successfully created the lead, we will save the following in variables:

-  `$leadId` — lead identifier,

-  `$leadStatus` — lead status `STATUS_ID`.

```php
if (empty($result['result']['result_error']['add_lead']) && !empty($result['result']['result']['get_lead'])) {
    $leadId = $result['result']['result']['add_lead'];
    $leadStatus = $result['result']['result']['get_lead']['STATUS_ID'];
    ...
}
```

#### Simple Mode

In simple mode, when a lead is created with a filled first name, the system automatically converts it into a deal. The lead field `STATUS_ID` takes the value `CONVERTED`.

We check the value of the variable `$leadStatus`. If the value is equal to `'CONVERTED'` — the CRM is operating in simple mode and the lead has already been converted into a deal.

{% note warning "" %}

In classic mode, a new lead can also be automatically converted into a deal using automation tools.

You can accurately determine the CRM operating mode using the special method [crm.settings.mode.get](../../../api-reference/crm/crm-settings-mode-get.md).

{% endnote %}

To obtain the deal identifier, we will use the method [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md). We will specify the `select` field as `ID`, and in the filter `filter`, we will pass the field `LEAD_ID` with the lead identifier from the variable `$leadId`.

```php
if ($leadStatus == 'CONVERTED') {
    // Simple mode: find the deal created from the lead
    $resultDeal = CRest::call('crm.deal.list', [
        'select' => [
            'ID'
        ],
        'filter' => [
            'LEAD_ID' => $leadId
        ]
    ]);
```

As a result, we will obtain the deal identifier.

```json
"result": [
    {
        "ID": "1811"
    }
],
```

To add an activity to the deal, we will use the method [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md). We will pass the fields:

-  `ownerTypeId` — identifier of the CRM object type. You can obtain identifiers using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md). We will specify the value — `2`, which means a deal,

-  `ownerId` — identifier of the CRM entity. We will specify the deal identifier obtained in the previous request,

-  `deadline` — deadline for the activity,

-  `title` — activity title,

-  `description` — activity description.

```php
if (!empty($resultDeal['result'][0]['ID'])) {
    $dealId = $resultDeal['result'][0]['ID'];
    // Link the activity to the deal
    CRest::call(
        'crm.activity.todo.add',
        [
            'ownerTypeId' => 2, // object type — deal
            'ownerId' => $dealId, // deal identifier
            'deadline' => date("Y-m-d H:i:s", time() + 3600), // current time + 1 hour
            'title' => 'Call the client',
            'description' => 'Filled out the request on the website',
        ]
    );
}
```

#### Classic Mode

In classic mode, the system does not convert the lead, so we link the activity to the created lead.

To add an activity to the lead, we will use the method [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md). We will pass the fields:

-  `ownerTypeId` — identifier of the CRM object type. You can obtain identifiers using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md). We will specify the value — `1`, which means a lead,

-  `ownerId` — identifier of the CRM entity. We will specify the identifier of the new lead,

-  `deadline` — deadline for the activity,

-  `title` — activity title,

-  `description` — activity description.

```php
// Classic mode: link the activity to the lead
CRest::call(
    'crm.activity.todo.add',
    [
        'ownerTypeId' => 1, // object type — lead
        'ownerId' => $leadId, // lead identifier
        'deadline' => date("Y-m-d H:i:s", time() + 3600), // current time + 1 hour
        'title' => 'Call the client',
        'description' => 'Filled out the request on the website',
    ]
);
```

## Complete Example of the Handler Code

{% include [Example Note](../../../_includes/examples.md) %}

```php
<?php
// Retrieve and sanitize data from the form
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);

// Format phone and email for Bitrix24 in crm_multifield format
$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
$arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();

// Form the lead title from the first name and last name
$sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
// If there is a company name — add it after the first name and last name with a dash
if (!empty($sCompanyTitle)) {
    $sTitle .= ' — ' . $sCompanyTitle;
}

$arData = [
    'add_lead' => [
        'method' => 'crm.lead.add',
        'params' => [
            'fields' => [
                'TITLE' => $sTitle, // Lead title
                'NAME' => $sName, // First Name
                'LAST_NAME' => $sLastName, // Last Name
                'COMPANY_TITLE' => $sCompanyTitle, // Company Name
                'PHONE' => $arPhone, // Phone
                'EMAIL' => $arEmail, // Email
            ]
        ]
    ],
    'get_lead' => [
        'method' => 'crm.lead.get',
        'params' => [
            'id' => '$result[add_lead]' // ID from the result of the crm.lead.add method
        ]
    ]
];

$result = CRest::callBatch($arData);

if (empty($result['result']['result_error']['add_lead']) && !empty($result['result']['result']['get_lead'])) {
    $leadId = $result['result']['result']['add_lead']; // save the lead identifier in a variable
    $leadStatus = $result['result']['result']['get_lead']['STATUS_ID']; // save the lead status in a variable

    if ($leadStatus == 'CONVERTED') {
        // Simple mode: find the deal created from the lead
        $resultDeal = CRest::call('crm.deal.list', [
            'select' => [
                'ID'
            ],
            'filter' => [
                'LEAD_ID' => $leadId
            ]
        ]);

        if (!empty($resultDeal['result'][0]['ID'])) {
            $dealId = $resultDeal['result'][0]['ID'];
            CRest::call(
                // Add activity to the deal
                'crm.activity.todo.add',
                [
                    'ownerTypeId' => 2, // object type — deal
                    'ownerId' => $dealId, // deal identifier
                    'deadline' => date("Y-m-d H:i:s", time() + 3600), // current time + 1 hour
                    'title' => 'Call the client',
                    'description' => 'Filled out the request on the website',
                ]
            );
        }
    } else {
        CRest::call(
            // Classic mode: add activity to the new lead
            'crm.activity.todo.add',
            [
                'ownerTypeId' => 1, // object type — lead
                'ownerId' => $leadId, // lead identifier
                'deadline' => date("Y-m-d H:i:s", time() + 3600), // current time + 1 hour
                'title' => 'Call the client',
                'description' => 'Filled out the request on the website',
            ]
        );
    }

    echo json_encode(['message' => 'Activity added to lead or deal']);
} else {
    $errors = [];
    if (!empty($result['result']['result_error']['add_lead'])) {
        $errors[] = 'Error adding lead: ' . $result['result']['result_error']['add_lead']['error_description'];
    }
    if (!empty($result['result']['result_error']['get_lead'])) {
        $errors[] = 'Error getting lead: ' . $result['result']['result_error']['get_lead']['error_description'];
    }
    echo json_encode(['message' => !empty($errors) ? implode('; ', $errors) : 'Unknown error']);
}
?>
```
