# Add a lead or deal with an activity considering the CRM mode

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method:
> -  creating a lead — users with the permission to create a lead,
> -  adding an activity to a lead or deal — users with the permission to modify a lead or deal in CRM

You can place a form on the site to collect potential client data. When a client fills out the form, their data will be sent to the CRM. You will be able to process the request and call the client.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send data to the handler.

2. Create a file to process the data. The handler will:

   -  accept and prepare the data,

   -  create a lead using the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add) method,

   -  check the CRM mode,

   -  add an activity with a reminder to call either the deal or the lead.

## CRM Modes

Bitrix24 has two CRM operation modes.

1. Simple mode — operates without leads. The system automatically converts a new lead into a deal.

2. Classic mode — separates potential and existing clients. The lead remains in the system after creation.

In the handler, we will determine which mode the CRM is operating in — simple or classic — and based on that, we will link the call reminder to either the deal or the lead.

{% note tip "User documentation" %}

-  [How to choose the CRM operation mode](https://helpdesk.bitrix24.com/open/24207198/)

{% endnote %}

## 1. Create a web form

In Bitrix24, you can automatically create a contact and company from a lead. To make the form suitable for various cases, we will make it universal. For a contact, you need to specify the first name and last name, and for a company — the name. We will create a web form on the site page with five fields:

-  `NAME` — client's first name, required field,

-  `LAST_NAME` — last name,

-  `COMPANY_TITLE` — company name,

-  `PHONE` — phone,

-  `EMAIL` — email.

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

## 2. Create the form handler

We will create a file `form.php`, which will:

-  accept data from the form,

-  create a lead,

-  determine the CRM mode,

-  add an activity with a reminder to call either the lead or the deal.

### Prepare data from the form

We will retrieve and sanitize the data from the form.

```php
// Retrieve and sanitize data from the form
$sName = htmlspecialchars($_POST["NAME"]);
$sLastName = htmlspecialchars($_POST["LAST_NAME"]);
$sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
$sPhone = htmlspecialchars($_POST["PHONE"]);
$sEmail = htmlspecialchars($_POST["EMAIL"]);
```

The system stores phone and email as an array of objects [crm_multifield](../../../api-reference/crm/data-types#crm_multifield), so they need to be formatted as an array.

1. If a value exists, we add it as the first element `VALUE` in the array, and the second value specifies the type `VALUE_TYPE`, for example:

   -  `WORK` — for phone,

   -  `HOME` — for email.

2. If there is no value, we pass an empty array.

```php
// Format phone and email for Bitrix24 in crm_multifield format
$arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
$arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();
```

We will form the lead title from the first name and last name. For companies, we will add the company name to the title.

```php
// Form the lead title from the first name and last name
$sTitle = 'From the website: ' . trim($sName . ' ' . $sLastName);
// If there is a company name — add it with a dash after the first name and last name
if (!empty($sCompanyTitle)) {
    $sTitle .= ' — ' . $sCompanyTitle;
}
```

### Create a lead and retrieve lead data

We will use the batch method [CRest::callBatch()](../../../api-reference/how-to-call-rest-api/batch.md) to sequentially execute two methods in one request: creating a lead and retrieving lead data. We will collect the methods in the array `$arData[]`.

To add a lead, we will use the method [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md). In the `fields` object, we will pass the fields:

-  `TITLE` — lead title,

-  `NAME` — lead first name,

-  `LAST_NAME` — lead last name,

-  `COMPANY_TITLE` — company name,

-  `PHONE` — phone number,

-  `EMAIL` — email.

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

We will pass the array of requests `$arData` to the `CRest::callBatch` method. The result of the method execution will be stored in the variable `$result`.

```php
$result = CRest::callBatch($arData);
```

As a result, we will obtain the identifier of the new lead `add_lead` and lead data `get_lead`.

```json
{
    "result": {
        "result": {
            "add_lead": 22, // result of the crm.lead.add method execution
            "get_lead": { // result of the crm.lead.get method execution
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

### Determine the CRM mode and create an activity

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

#### Simple mode

In simple mode, when a lead with a filled first name is created, the system automatically converts it into a deal. The lead field `STATUS_ID` takes the value `CONVERTED`.

We check the value of the variable `$leadStatus`. If the value is equal to `'CONVERTED'` — the CRM is operating in simple mode, and the lead has already been converted into a deal.

{% note warning "" %}

In classic mode, a new lead can also be automatically converted into a deal using automation tools.

You can accurately determine the CRM operation mode using the special method [crm.settings.mode.get](../../../api-reference/crm/crm-settings-mode-get.md).

{% endnote %}

To obtain the deal identifier, we will use the method [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md). We will specify the `select` field as `ID`, and in the filter `filter`, we will pass the `LEAD_ID` field with the lead identifier from the variable `$leadId`.

```php
if ($leadStatus == 'CONVERTED') {
    // Simple mode: searching for the deal created from the lead
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

-  `ownerTypeId` — identifier of the CRM object type. You can obtain identifiers using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md). We will specify the value — `2`, which means deal,

-  `ownerId` — identifier of the CRM entity. We will specify the deal identifier obtained in the previous request,

-  `deadline` — deadline for the activity,

-  `title` — activity title,

-  `description` — activity description.

```php
if (!empty($resultDeal['result'][0]['ID'])) {
    $dealId = $resultDeal['result'][0]['ID'];
    // Linking the activity to the deal
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

#### Classic mode

In classic mode, the system does not convert the lead, so we link the activity to the created lead.

To add an activity to the lead, we will use the method [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md). We will pass the fields:

-  `ownerTypeId` — identifier of the CRM object type. You can obtain identifiers using the method [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md). We will specify the value — `1`, which means lead,

-  `ownerId` — identifier of the CRM entity. We will specify the identifier of the new lead,

-  `deadline` — deadline for the activity,

-  `title` — activity title,

-  `description` — activity description.

```php
// Classic mode: linking the activity to the lead
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

## Complete example of the handler code

{% include [Example notes](../../../_includes/examples.md) %}

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
$sTitle = 'From the website: ' . trim($sName . ' ' . $sLastName);
// If there is a company name — add it with a dash after the first name and last name
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
    $leadId = $result['result']['result']['add_lead']; // save the lead identifier in the variable
    $leadStatus = $result['result']['result']['get_lead']['STATUS_ID']; // save the lead status in the variable

    if ($leadStatus == 'CONVERTED') {
        // Simple mode: searching for the deal created from the lead
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
                // Adding activity to the deal
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
                // Classic mode: adding activity to the new lead
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

    echo json_encode(['message' => 'Activity added to the lead or deal']);
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