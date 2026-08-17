# Add a CRM Activity to a New Lead or Deal Depending on the CRM Mode

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method:
> -  Creating a lead — users with the permission to create leads,
> -  Adding an activity to a lead or deal — users with the permission to modify leads or deals in CRM.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can use the official REST documentation.

{% endnote %}

You can place a form on your website to collect potential client data. When a client fills out the form, their data will be sent to the CRM. You will be able to process the request and call the client.

Setting up the form consists of two steps.

1. Place the form on an HTML page. It will send the data to the handler.

2. Create a file to process the data. The handler:

   -  receives and prepares the data,

   -  creates a lead using the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method,

   -  checks the CRM mode,

   -  adds an activity with a call reminder to either a deal or a lead.

## CRM Modes

Bitrix24 has two CRM modes.

1. Simple mode — operates without leads. The system automatically converts a new lead into a deal.

2. Classic mode — separates potential customers from existing customers. The lead remains in the system after creation.

In the handler, we will determine which mode the CRM is operating in — simple or classic — and based on this, attach the call reminder to either a deal or a lead.

{% note tip "User Documentation" %}

-  [How to choose the CRM operation mode](https://helpdesk.bitrix24.com/open/24207198/)

{% endnote %}

## 1. Creating a Web Form

In Bitrix24, a contact and a company can be automatically created from a lead. To make the form suitable for different scenarios, we will make it universal. For a contact, a first name and last name are required, and for a company, a name is required. We will create a web form on a website page with five fields:

-  `NAME` — customer first name, a required field,

-  `LAST_NAME` — last name,

-  `COMPANY_TITLE` — company name,

-  `PHONE` — phone,

-  `EMAIL` — Email.

When submitted, the form passes the data to the handler.

{% list tabs %}

- JS

    ```html
    <form id="form_to_crm">
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

    <script>
        document.getElementById('form_to_crm').addEventListener('submit', async (el) => {
            el.preventDefault(); // Preventing default form submission
            // Collecting form data into JSON
            const formData = Object.fromEntries(new FormData(el.currentTarget).entries());
            // Sending data to the server (Node.js handler endpoint)
            const response = await fetch('/form', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(formData),
            });
            const data = await response.json();
            alert(data.message); // Showing the result
        });
    </script>
    ```

- PHP

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

    <!-- Including jQuery for the AJAX request -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault(); // Preventing default form submission
                var formData = $(this).serialize(); // Collecting form data
                // Sending data to the server
                $.ajax({
                    'method': 'POST',
                    'dataType': 'json',
                    'url': 'form.php', // Handler file
                    'data': formData,
                    success: function(data) {
                        alert(data.message); // Showing the result
                    }
                });
            });
        });
    </script>
    ```

- Python

    ```html
    <form id="form_to_crm">
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

    <!-- Including jQuery for the AJAX request -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault(); // Preventing default form submission
                var formData = $(this).serialize(); // Collecting form data
                // Sending data to the server (Flask handler route)
                $.ajax({
                    'method': 'POST',
                    'dataType': 'json',
                    'url': '/form', // Handler route
                    'data': formData,
                    success: function(data) {
                        alert(data.message); // Showing the result
                    }
                });
            });
        });
    </script>
    ```

{% endlist %}

## 2. Create a Form Handler

We will create a handler that will:

-  receive data from a form,

-  create a lead,

-  determine the CRM mode,

-  add an activity with a call reminder to a lead or a deal.

### Prepare Data from the Form

Retrieve the form data and strip HTML tags.

{% list tabs %}

- JS

    ```javascript
    // Getting data from the form
    const sName = String(req.body.NAME ?? '')
    const sLastName = String(req.body.LAST_NAME ?? '')
    const sCompanyTitle = String(req.body.COMPANY_TITLE ?? '')
    const sPhone = String(req.body.PHONE ?? '')
    const sEmail = String(req.body.EMAIL ?? '')
    ```

- PHP

    ```php
    // Getting and sanitizing data from the form
    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $sEmail = htmlspecialchars($_POST["EMAIL"]);
    ```

- Python

    ```python
    # Getting data from the form
    s_name = request.form.get("NAME", "")
    s_last_name = request.form.get("LAST_NAME", "")
    s_company_title = request.form.get("COMPANY_TITLE", "")
    s_phone = request.form.get("PHONE", "")
    s_email = request.form.get("EMAIL", "")
    ```

{% endlist %}

The system stores phone and email as an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) objects, so they must be converted to an array format.

1. If a value exists, add it as the first item `VALUE` in the array, and specify the type `VALUE_TYPE` as the second value, for example:

   -  `WORK` — for a phone number,

   -  `HOME` — for an email.

2. If no value exists, pass an empty array.

{% list tabs %}

- JS

    ```javascript
    // Formatting phone and email for Bitrix24 into crm_multifield format
    const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
    const arEmail = sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : []
    ```

- PHP

    ```php
    // Formatting phone and email for Bitrix24 into crm_multifield format
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    $arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();
    ```

- Python

    ```python
    # Formatting phone and email for Bitrix24 into crm_multifield format
    ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
    ar_email = [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else []
    ```

{% endlist %}

Form the lead heading from the first and last name. For companies, add the company name to the heading.

{% list tabs %}

- JS

    ```javascript
    // Creating the lead title from first and last name
    let sTitle = 'From website: ' + `${sName} ${sLastName}`.trim()
    // If company name exists — add it via a dash after the first and last name
    if (sCompanyTitle) {
        sTitle += ' — ' + sCompanyTitle
    }
    ```

- PHP

    ```php
    // Creating the lead title from first and last name
    $sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
    // If company name exists — add it via a dash after the first and last name
    if (!empty($sCompanyTitle)) {
        $sTitle .= ' — ' . $sCompanyTitle;
    }
    ```

- Python

    ```python
    # Creating the lead title from first and last name
    s_title = "From website: " + f"{s_name} {s_last_name}".strip()
    # If company name exists — add it via a dash after the first and last name
    if s_company_title:
        s_title += " — " + s_company_title
    ```

{% endlist %}

### Create a Lead and Retrieve Lead Data

Execute two methods sequentially: create a lead and retrieve its data.

To add a lead, use the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method. Pass the following fields in the `fields` object:

-  `TITLE` — lead heading,

-  `NAME` — lead first name,

-  `LAST_NAME` — last name,

-  `COMPANY_TITLE` — company name,

-  `PHONE` — phone number,

-  `EMAIL` — Email.

{% note warning "" %}

Check which mandatory fields are configured for leads in your Bitrix24. All mandatory fields must be passed to the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method.

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    const addLead = await $b24.actions.v2.call.make({
        method: 'crm.lead.add',
        params: {
            fields: {
                TITLE: sTitle, // Lead title
                NAME: sName, // First Name
                LAST_NAME: sLastName, // Last Name
                COMPANY_TITLE: sCompanyTitle, // Company Name
                PHONE: arPhone, // Phone
                EMAIL: arEmail, // Email
            }
        },
        requestId: 'add-lead'
    })
    const leadId = addLead.getData().result
    ```

- PHP

    ```php
    $leadId = $sb->getCRMScope()->lead()->add([
        'TITLE' => $sTitle, // Lead title
        'NAME' => $sName, // First Name
        'LAST_NAME' => $sLastName, // Last Name
        'COMPANY_TITLE' => $sCompanyTitle, // Company Name
        'PHONE' => $arPhone, // Phone
        'EMAIL' => $arEmail, // Email
    ])->getId();
    ```

- Python

    ```python
    lead_id = client.crm.lead.add(fields={
        "TITLE": s_title,  # Lead title
        "NAME": s_name,  # First Name
        "LAST_NAME": s_last_name,  # Last Name
        "COMPANY_TITLE": s_company_title,  # Company Name
        "PHONE": ar_phone,  # Phone
        "EMAIL": ar_email,  # Email
    }).result
    ```

{% endlist %}

To retrieve lead data, use the [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md) method. Pass the lead identifier obtained from the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method result into the `ID` parameter.

{% list tabs %}

- JS

    ```javascript
    const getLead = await $b24.actions.v2.call.make({
        method: 'crm.lead.get',
        params: { id: leadId }, // ID from the crm.lead.add method result
        requestId: 'get-lead'
    })
    const leadStatus = getLead.getData().result.STATUS_ID
    ```

- PHP

    ```php
    $lead = $sb->getCRMScope()->lead()->get($leadId)->lead(); // ID from the crm.lead.add result
    $leadStatus = $lead->STATUS_ID;
    ```

- Python

    ```python
    lead = client.crm.lead.get(bitrix_id=lead_id).result  # ID from the crm.lead.add result
    lead_status = lead["STATUS_ID"]
    ```

{% endlist %}

As a result, the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method will return the identifier of the new lead, and the [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md) method will return the lead data, including the `STATUS_ID` field.

```json
{
    "result": {
        "ID": "22",
        "TITLE": "Klaus Weber",
        "HONORIFIC": null,
        "NAME": "Klaus",
        "SECOND_NAME": null,
        "LAST_NAME": "Weber",
        "COMPANY_TITLE": null,
        ...,
        "STATUS_ID": "CONVERTED",
        ...
    }
}
```

### Determine CRM Mode and Create an Activity

If the system successfully creates a lead, save the following to variables:

-  `$leadId` — lead identifier,

-  `$leadStatus` — lead status `STATUS_ID`.

{% list tabs %}

- JS

    ```javascript
    const leadId = addLead.getData().result
    const leadStatus = getLead.getData().result.STATUS_ID
    // ...
    ```

- PHP

    ```php
    $leadId = $crm->lead()->add([/* ... */])->getId();
    $leadStatus = $crm->lead()->get($leadId)->lead()->STATUS_ID;
    // ...
    ```

- Python

    ```python
    lead_id = client.crm.lead.add(fields={...}).result
    lead_status = client.crm.lead.get(bitrix_id=lead_id).result["STATUS_ID"]
    # ...
    ```

{% endlist %}

#### Simple Mode

In simple mode, when creating a lead with a filled-in name, the system automatically converts it into a deal. The lead field `STATUS_ID` takes the value `CONVERTED`.

Check the value of the `$leadStatus` variable. If the value is equal to `'CONVERTED'`, the CRM is operating in simple mode and the lead has already been converted into a deal.

{% note warning "" %}

In classic mode, a new lead can also be automatically converted into a deal using automation tools.

You can determine the exact CRM mode using the special [crm.settings.mode.get](../../../api-reference/crm/crm-settings-mode-get.md) method.

{% endnote %}

To retrieve the deal identifier, use the [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) method. Specify the `ID` field in `select`, and in the filter `filter`, pass the `LEAD_ID` field with the lead identifier from the `$leadId` variable.

{% list tabs %}

- JS

    ```javascript
    if (leadStatus === 'CONVERTED') {
        // Simple mode: looking for a deal created from a lead
        const resultDeal = await $b24.actions.v2.callList.make({
            method: 'crm.deal.list',
            params: { select: ['ID'], filter: { LEAD_ID: leadId } },
            requestId: 'deal-list'
        })
    ```

- PHP

    ```php
    if ($leadStatus == 'CONVERTED') {
        // Simple mode: looking for a deal created from a lead
        $deals = $sb->getCRMScope()->deal()->list(
            order: [],
            filter: ['LEAD_ID' => $leadId],
            select: ['ID']
        )->getDeals();
    ```

- Python

    ```python
    if lead_status == "CONVERTED":
        # Simple mode: looking for a deal created from a lead
        deals = client.crm.deal.list(
            filter={"LEAD_ID": lead_id}, select=["ID"],
        ).as_list().result
    ```

{% endlist %}

As a result, you will obtain the deal identifier.

```json
"result": [
    {
        "ID": "1811"
    }
],
```

To add an activity to a deal, use the [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) method. Pass the following fields:

- `ownerTypeId` — CRM object type identifier. You can retrieve identifiers using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. If we specify the value `2`, it refers to a deal,

- `ownerId` — CRM item identifier. We specify the deal identifier obtained in the previous request,

- `deadline` — activity deadline,

- `title` — activity title,

- `description` — activity description.

{% list tabs %}

- JS

    ```javascript
    const deals = resultDeal.getData().result
    if (deals.length && deals[0].ID) {
        const dealId = deals[0].ID
        // Linking the activity to the deal
        await $b24.actions.v2.call.make({
            method: 'crm.activity.todo.add',
            params: {
                ownerTypeId: 2, // object type — deal
                ownerId: dealId, // deal identifier
                deadline: new Date(Date.now() + 3600 * 1000).toISOString(), // current time + 1 hour
                title: 'Call client',
                description: 'Filled out a request on the website',
            },
            requestId: 'todo-deal'
        })
    }
    ```

- PHP

    ```php
    if (!empty($deals)) {
        $dealId = $deals[0]->ID;
        // Linking a task to a deal — there is no wrapper for crm.activity.todo.add in the SDK, so we call it directly
        $sb->core->call('crm.activity.todo.add', [
            'ownerTypeId' => 2, // object type — deal
            'ownerId' => $dealId, // deal identifier
            'deadline' => date("Y-m-d H:i:s", time() + 3600), // current time + 1 hour
            'title' => 'Call client',
            'description' => 'Filled out a request on the website',
        ]);
    }
    ```

- Python

    ```python
    from datetime import datetime, timedelta

    if deals:
        deal_id = deals[0]["ID"]
        deadline = (datetime.now() + timedelta(hours=1)).strftime("%Y-%m-%d %H:%M:%S")  # +1 hour
        # Linking a task to a deal — we call the crm.activity.todo.add method directly
        token.call_method("crm.activity.todo.add", {
            "ownerTypeId": 2,  # object type — deal
            "ownerId": int(deal_id),  # deal identifier
            "deadline": deadline,
            "title": "Call client",
            "description": "Filled out a request on the website",
        })
    ```

{% endlist %}

#### Classic Mode

In classic mode, the system does not convert the lead, so we link the activity to the created lead.

To add an activity to a lead, use the [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) method. Pass the following fields:

- `ownerTypeId` — CRM object type identifier. You can retrieve identifiers using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. If we specify the value `1`, it refers to a lead,

- `ownerId` — CRM item identifier. We specify the new lead identifier,

- `deadline` — activity deadline,

- `title` — activity title,

- `description` — activity description.

{% list tabs %}

- JS

    ```javascript
    // Classic mode: linking a task to a lead
    await $b24.actions.v2.call.make({
        method: 'crm.activity.todo.add',
        params: {
            ownerTypeId: 1, // object type — lead
            ownerId: leadId, // lead identifier
            deadline: new Date(Date.now() + 3600 * 1000).toISOString(), // current time + 1 hour
            title: 'Call client',
            description: 'Filled out a request on the website',
        },
        requestId: 'todo-lead'
    })
    ```

- PHP

    ```php
    // Classic mode: linking a task to a lead
    $sb->core->call('crm.activity.todo.add', [
        'ownerTypeId' => 1, // object type — lead
        'ownerId' => $leadId, // lead identifier
        'deadline' => date("Y-m-d H:i:s", time() + 3600), // current time + 1 hour
        'title' => 'Call client',
        'description' => 'Filled out a request on the website',
    ]);
    ```

- Python

    ```python
    from datetime import datetime, timedelta

    deadline = (datetime.now() + timedelta(hours=1)).strftime("%Y-%m-%d %H:%M:%S")  # +1 hour
    # Classic mode: linking a task to a lead — we call the crm.activity.todo.add method directly
    token.call_method("crm.activity.todo.add", {
        "ownerTypeId": 1,  # object type — lead
        "ownerId": lead_id,  # lead identifier
        "deadline": deadline,
        "title": "Call client",
        "description": "Filled out a request on the website",
    })
    ```

{% endlist %}

## Full Handler Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const app = express()
    app.use(express.json())

    // The handler accepts form data via the /form route
    app.post('/form', async (req, res) => {
        // Getting and sanitizing data from the form
        const sName = String(req.body.NAME ?? '')
        const sLastName = String(req.body.LAST_NAME ?? '')
        const sCompanyTitle = String(req.body.COMPANY_TITLE ?? '')
        const sPhone = String(req.body.PHONE ?? '')
        const sEmail = String(req.body.EMAIL ?? '')

        // Formatting phone and email for Bitrix24 into crm_multifield format
        const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
        const arEmail = sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : []

        // Creating the lead title from first and last name
        let sTitle = 'From website: ' + `${sName} ${sLastName}`.trim()
        if (sCompanyTitle) {
            sTitle += ' — ' + sCompanyTitle
        }

        try {
            // Creating a lead
            const addLead = await $b24.actions.v2.call.make({
                method: 'crm.lead.add',
                params: {
                    fields: {
                        TITLE: sTitle, NAME: sName, LAST_NAME: sLastName,
                        COMPANY_TITLE: sCompanyTitle, PHONE: arPhone, EMAIL: arEmail,
                    }
                },
                requestId: 'add-lead'
            })
            const leadId = addLead.getData().result

            // Getting lead data
            const getLead = await $b24.actions.v2.call.make({
                method: 'crm.lead.get', params: { id: leadId }, requestId: 'get-lead'
            })
            const leadStatus = getLead.getData().result.STATUS_ID

            const deadline = new Date(Date.now() + 3600 * 1000).toISOString() // current time + 1 hour

            if (leadStatus === 'CONVERTED') {
                // Simple mode: looking for a deal created from a lead
                const resultDeal = await $b24.actions.v2.callList.make({
                    method: 'crm.deal.list',
                    params: { select: ['ID'], filter: { LEAD_ID: leadId } },
                    requestId: 'deal-list'
                })
                const deals = resultDeal.getData().result
                if (deals.length && deals[0].ID) {
                    // Adding a task to a deal
                    await $b24.actions.v2.call.make({
                        method: 'crm.activity.todo.add',
                        params: {
                            ownerTypeId: 2, ownerId: deals[0].ID, deadline,
                            title: 'Call client', description: 'Filled out a request on the website',
                        },
                        requestId: 'todo-deal'
                    })
                }
            } else {
                // Classic mode: adding a task to a new lead
                await $b24.actions.v2.call.make({
                    method: 'crm.activity.todo.add',
                    params: {
                        ownerTypeId: 1, ownerId: leadId, deadline,
                        title: 'Call client', description: 'Filled out a request on the website',
                    },
                    requestId: 'todo-lead'
                })
            }

            res.json({ message: 'Task added to lead or deal' })
        } catch (e) {
            res.json({ message: e.message })
        }
    })

    app.listen(3000)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');
    $crm = $sb->getCRMScope();

    // Getting and sanitizing data from the form
    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sCompanyTitle = htmlspecialchars($_POST["COMPANY_TITLE"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $sEmail = htmlspecialchars($_POST["EMAIL"]);

    // Formatting phone and email for Bitrix24 into crm_multifield format
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    $arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();

    // Creating the lead title from first and last name
    $sTitle = 'From website: ' . trim($sName . ' ' . $sLastName);
    if (!empty($sCompanyTitle)) {
        $sTitle .= ' — ' . $sCompanyTitle;
    }

    try {
        // Creating a lead
        $leadId = $crm->lead()->add([
            'TITLE' => $sTitle, 'NAME' => $sName, 'LAST_NAME' => $sLastName,
            'COMPANY_TITLE' => $sCompanyTitle, 'PHONE' => $arPhone, 'EMAIL' => $arEmail,
        ])->getId();

        // Getting lead data
        $leadStatus = $crm->lead()->get($leadId)->lead()->STATUS_ID;

        $deadline = date("Y-m-d H:i:s", time() + 3600); // current time + 1 hour

        if ($leadStatus == 'CONVERTED') {
            // Simple mode: looking for a deal created from a lead
            $deals = $crm->deal()->list(order: [], filter: ['LEAD_ID' => $leadId], select: ['ID'])->getDeals();
            if (!empty($deals)) {
                // Adding a task to a deal — crm.activity.todo.add has no wrapper, so we call it directly
                $sb->core->call('crm.activity.todo.add', [
                    'ownerTypeId' => 2, 'ownerId' => $deals[0]->ID, 'deadline' => $deadline,
                    'title' => 'Call client', 'description' => 'Filled out a request on the website',
                ]);
            }
        } else {
            // Classic mode: adding a task to a new lead
            $sb->core->call('crm.activity.todo.add', [
                'ownerTypeId' => 1, 'ownerId' => $leadId, 'deadline' => $deadline,
                'title' => 'Call client', 'description' => 'Filled out a request on the website',
            ]);
        }

        echo json_encode(['message' => 'Task added to lead or deal']);
    } catch (\Throwable $e) {
        echo json_encode(['message' => $e->getMessage()]);
    }
    ```

- Python

    ```python
    # pip install b24pysdk
    from datetime import datetime, timedelta
    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
    )
    client = Client(token)

    @app.route("/form", methods=["POST"])
    def handle_form():
        # Getting and clearing data from the form
        s_name = request.form.get("NAME", "")
        s_last_name = request.form.get("LAST_NAME", "")
        s_company_title = request.form.get("COMPANY_TITLE", "")
        s_phone = request.form.get("PHONE", "")
        s_email = request.form.get("EMAIL", "")

        # Formatting phone and email for Bitrix24 into crm_multifield format
        ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
        ar_email = [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else []

        # Creating the lead title from first and last name
        s_title = "From website: " + f"{s_name} {s_last_name}".strip()
        if s_company_title:
            s_title += " — " + s_company_title

        try:
            # Creating a lead
            lead_id = client.crm.lead.add(fields={
                "TITLE": s_title, "NAME": s_name, "LAST_NAME": s_last_name,
                "COMPANY_TITLE": s_company_title, "PHONE": ar_phone, "EMAIL": ar_email,
            }).result

            # Getting lead data
            lead_status = client.crm.lead.get(bitrix_id=lead_id).result["STATUS_ID"]

            deadline = (datetime.now() + timedelta(hours=1)).strftime("%Y-%m-%d %H:%M:%S")  # +1 hour

            if lead_status == "CONVERTED":
                # Simple mode: looking for a deal created from a lead
                deals = client.crm.deal.list(filter={"LEAD_ID": lead_id}, select=["ID"]).as_list().result
                if deals:
                    # Adding a task to a deal (crm.activity.todo.add — calling directly)
                    token.call_method("crm.activity.todo.add", {
                        "ownerTypeId": 2, "ownerId": int(deals[0]["ID"]), "deadline": deadline,
                        "title": "Call client", "description": "Filled out a request on the website",
                    })
            else:
                # Classic mode: adding a task to a new lead
                token.call_method("crm.activity.todo.add", {
                    "ownerTypeId": 1, "ownerId": lead_id, "deadline": deadline,
                    "title": "Call client", "description": "Filled out a request on the website",
                })

            return jsonify({"message": "Task added to lead or deal"})
        except Exception as e:
            return jsonify({"message": str(e)})
    ```

{% endlist %}
