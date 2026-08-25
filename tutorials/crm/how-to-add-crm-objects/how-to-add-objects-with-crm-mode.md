# Add a CRM Activity to a New Lead or Deal Depending on the CRM Mode

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed rights is required — permission to modify leads and deals
>
> - [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) — a user with permission to create leads
> - [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md) — a user with permission to read leads
> - [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) — a user with permission to read deals
> - [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) — a user with permission to edit the CRM object to which the activity is being added

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can use the official REST documentation.

{% endnote %}

You can place a form on your website to collect potential client data. When a client fills out the form, their data will be sent to the CRM. You will be able to process the request and call the client.

As a result of the scenario, a new lead appears in the CRM, and an activity with a call reminder appears in the timeline. Which object the activity is linked to depends on the CRM mode: in simple mode it is the deal that resulted from the lead, in classic mode it is the lead itself.

The setup consists of two stages:

1. Prepare the fields and place the form on the page

2. Create a handler file that sequentially calls the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md), [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md), [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md), and [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) methods

## CRM Modes

Bitrix24 has two CRM modes.

1. Simple mode — operates without leads. The system automatically converts a new lead into a deal, and the lead receives the `CONVERTED` status

2. Classic mode — separates potential customers from existing customers. The lead remains in the system with the `NEW` status

The activity must be linked to the object that actually appeared in the CRM. That is why the handler checks the status of the lead after creating it and uses that status to choose where to add the call reminder.

You can find out which mode is configured in Bitrix24 with the [crm.settings.mode.get](../../../api-reference/crm/crm-settings-mode-get.md) method. It returns `1` for classic mode and `2` for simple mode. In the scenario, however, we rely not on this setting but on the status of the specific lead: in classic mode a lead can also be converted if this is done by robots or other automation tools.

{% note tip "User Documentation" %}

- [How to choose the CRM operation mode](https://helpdesk.bitrix24.com/open/24207198/)

{% endnote %}

## Before You Start

- The webhook is created on behalf of a user with permission to create leads, to read deals, and to edit leads and deals

- You have a server that serves the page with the form and accepts the form data using the `POST` method. In the examples, this is Express for JS, a PHP script, and Flask for Python

- The webhook URL is stored in the environment, not in the page code. The form is on a public page, and the secret must not end up in it

- The `NAME` field in the form is required. In simple mode, the system converts a lead into a deal when the first name is filled in

## 1. Creating a Web Form

In Bitrix24, a contact and a company can be automatically created from a lead. To make the form suitable for different scenarios, we will make it universal. For a contact, a first name and last name are required, and for a company, a name is required. We will create a web form on a website page with five fields:

- `NAME` — customer first name, a required field

- `LAST_NAME` — last name

- `COMPANY_TITLE` — company name

- `PHONE` — phone

- `EMAIL` — Email

The form passes the data to the handler using the `POST` method.

{% include [Note on examples](../../../_includes/examples.md) %}

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
{% endlist %}

## 2. Create a Form Handler

The handler accepts the form field values, creates a lead, checks its status, and adds an activity with a call reminder to the lead or to the deal.

### Prepare Data from the Form

Read the `NAME`, `LAST_NAME`, `COMPANY_TITLE`, `PHONE`, and `EMAIL` fields and cast them to a string. If a field is empty, you get an empty string rather than `undefined` or `None`.

The form is filled out by a site visitor, so the values cannot be considered safe. In the PHP example, they are additionally passed through `htmlspecialchars`. If you return these values back to the page, escape them in the other examples as well.

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

- Python

    ```python
    # Getting data from the form
    s_name = request.form.get("NAME", "")
    s_last_name = request.form.get("LAST_NAME", "")
    s_company_title = request.form.get("COMPANY_TITLE", "")
    s_phone = request.form.get("PHONE", "")
    s_email = request.form.get("EMAIL", "")
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
{% endlist %}

The system stores phone and email as an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) objects, so they must be converted to an array format.

1. If a value exists, write it to the `VALUE` field, and pass the [type](../../../api-reference/crm/data-types.md#crm_multifield) in the `VALUE_TYPE` field, for example `WORK` for a phone number and `HOME` for an email address

2. If no value exists, pass an empty array

{% list tabs %}

- JS

    ```javascript
    // Formatting phone and email for Bitrix24 into crm_multifield format
    const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
    const arEmail = sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : []
    ```

- Python

    ```python
    # Formatting phone and email for Bitrix24 into crm_multifield format
    ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
    ar_email = [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else []
    ```


- PHP

    ```php
    // Formatting phone and email for Bitrix24 into crm_multifield format
    $arPhone = (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK')) : array();
    $arEmail = (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array();
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

- Python

    ```python
    # Creating the lead title from first and last name
    s_title = "From website: " + f"{s_name} {s_last_name}".strip()
    # If company name exists — add it via a dash after the first and last name
    if s_company_title:
        s_title += " — " + s_company_title
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
{% endlist %}

### Create a Lead and Retrieve Lead Data

Execute two methods sequentially: create a lead and retrieve its data.

To add a lead, use the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method. Pass the following fields in the `fields` object:

- `TITLE` — lead heading from the `$sTitle` variable

- `NAME` — first name from the `NAME` form field

- `LAST_NAME` — last name from the `LAST_NAME` form field

- `COMPANY_TITLE` — company name from the `COMPANY_TITLE` form field

- `PHONE` — phone number in the `crm_multifield` format from the `$arPhone` variable

- `EMAIL` — email address in the `crm_multifield` format from the `$arEmail` variable

The method returns the identifier of the new lead — we retain it in the `$leadId` variable. It is needed in the next steps: to retrieve the lead status and to find the deal if the lead has been converted.

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

- Python

    ```python
    lead = client.crm.lead.get(bitrix_id=lead_id).result  # ID from the crm.lead.add result
    lead_status = lead["STATUS_ID"]
    ```


- PHP

    ```php
    $lead = $sb->getCRMScope()->lead()->get($leadId)->lead(); // ID from the crm.lead.add result
    $leadStatus = $lead->STATUS_ID;
    ```
{% endlist %}

As a result, the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method will return the identifier of the new lead, and the [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md) method will return the lead data, including the `STATUS_ID` field.

```json
{
    "result": {
        "ID": "22",
        "TITLE": "From website: Klaus Weber",
        "HONORIFIC": null,
        "NAME": "Klaus",
        "SECOND_NAME": null,
        "LAST_NAME": "Weber",
        "COMPANY_TITLE": null,
        "STATUS_ID": "CONVERTED"
    }
}
```

The response is abridged: the method returns all lead fields. Only `STATUS_ID` matters for the scenario.

### Determine Where to Add the Activity

The value of the `$leadStatus` variable selects the branch of the scenario.

- `CONVERTED` — the lead has already been converted into a deal. Find the deal and add the activity to it

- Any other value, for example `NEW` — the lead has remained a lead. Add the activity directly to it

In both branches, the activity is added by the [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) method. Pass the following fields:

- `ownerTypeId` — CRM object type identifier. You can retrieve identifiers using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method. The value depends on the branch: `1` for a lead, `2` for a deal

- `ownerId` — CRM item identifier. Depends on the branch: the identifier of the lead or of the deal

- `deadline` — activity deadline. Pass the time in the `2026-08-19 15:00:00` or `2026-08-19T15:00:00` format, the method accepts both

- `title` — activity title

- `description` — activity description

#### Simple Mode

The lead itself is no longer needed — we will add the activity to the deal. To retrieve its identifier, use the [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) method. Specify the `ID` field in `select`, and in the filter `filter`, pass the `LEAD_ID` field with the lead identifier from the `$leadId` variable.

{% list tabs %}

- JS

    ```javascript
    // Simple mode: looking for a deal created from a lead
    const resultDeal = await $b24.actions.v2.callList.make({
        method: 'crm.deal.list',
        params: { select: ['ID'], filter: { LEAD_ID: leadId } },
        requestId: 'deal-list'
    })
    const deals = resultDeal.getData().result
    ```

- Python

    ```python
    # Simple mode: looking for a deal created from a lead
    deals = client.crm.deal.list(
        filter={"LEAD_ID": lead_id}, select=["ID"],
    ).as_list().result
    ```


- PHP

    ```php
    // Simple mode: looking for a deal created from a lead
    $deals = $sb->getCRMScope()->deal()->list(
        order: [],
        filter: ['LEAD_ID' => $leadId],
        select: ['ID']
    )->getDeals();
    ```
{% endlist %}

As a result, you will obtain an array of deals. One lead produces one deal, so we take the first item.

```json
{
    "result": [
        {
            "ID": "1811"
        }
    ],
    "total": 1
}
```

We add the activity to the deal: pass `2` in `ownerTypeId` and the deal identifier from the `$deals` variable in `ownerId`.

{% list tabs %}

- JS

    ```javascript
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
{% endlist %}

#### Classic Mode

There is no deal, so we link the activity to the lead itself. No additional request is needed: the lead identifier is already in the `$leadId` variable.

We add the activity to the lead: pass `1` in `ownerTypeId` and the identifier of the new lead from the `$leadId` variable in `ownerId`.

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
{% endlist %}

The method returns the identifier of the created activity.

```json
{
    "result": {
        "id": 999
    }
}
```

### Full Handler Code Example

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

- Python

    ```python
    # pip install b24pysdk flask
    import os
    from datetime import datetime, timedelta
    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    token = BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token=os.environ["B24_HOOK_TOKEN"],
    )
    # B24_HOOK_TOKEN = 'USER_ID/TOKEN' — user_id and token only, without https://
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


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Psr\Log\NullLogger;

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))
        ->initFromWebhook(getenv('B24_HOOK'));
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'
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
{% endlist %}

## Verify the Result

Open the created lead in Bitrix24. If the CRM is operating in classic mode, the "Call client" activity with a deadline one hour from now appears in the lead timeline. In simple mode, the lead is converted, and the activity ends up in the deal timeline.

Through REST, the activities of an object are checked with the [crm.activity.list](../../../api-reference/crm/timeline/activities/activity-base/crm-activity-list.md) method using a filter by owner: `OWNER_TYPE_ID` is `1` for a lead and `2` for a deal, and `OWNER_ID` is the object identifier.

{% list tabs %}

- JS

    ```javascript
    const checkResponse = await $b24.actions.v2.callList.make({
        method: 'crm.activity.list',
        params: {
            filter: { OWNER_TYPE_ID: 1, OWNER_ID: leadId },
            select: ['ID', 'SUBJECT', 'OWNER_TYPE_ID', 'OWNER_ID']
        },
        requestId: 'activity-list'
    })

    console.dir(checkResponse.getData().result)
    ```

- Python

    ```python
    activities = client.crm.activity.list(
        filter={"OWNER_TYPE_ID": 1, "OWNER_ID": lead_id},
        select=["ID", "SUBJECT", "OWNER_TYPE_ID", "OWNER_ID"],
    ).response.result
    ```


- PHP

    ```php
    $activities = $sb->getCRMScope()->activity()->list(
        [],
        ['OWNER_TYPE_ID' => 1, 'OWNER_ID' => $leadId],
        ['ID', 'SUBJECT', 'OWNER_TYPE_ID', 'OWNER_ID'],
        0
    )->getActivities();
    ```
{% endlist %}

The scenario is complete if:

- The [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method returned the lead identifier

- The [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) method returned an object with the `id` of the activity

- The object's activity list contains an activity with the "Call client" subject, and its `OWNER_TYPE_ID` and `OWNER_ID` point to the lead or to the deal — depending on which branch the scenario followed

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| Empty value `Access denied` | The user does not have permission to create leads. Check which user the webhook was created on behalf of ||
|| `ACCESS_DENIED` | The user does not have permission to edit the object to which the activity is being added. The permission is needed both for the lead and for the deal ||
|| `100` | The required `ownerTypeId`, `ownerId`, or `deadline` fields were not passed to [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) ||
|| `OWNER_NOT_FOUND` | The object specified in `ownerId` was not found. Most often this means that a lead identifier ended up in `ownerId` while `ownerTypeId` holds the value `2` ||
|| `WRONG_DATETIME_FORMAT` | The `deadline` value was not recognized as a date. Pass the time in the `2026-08-19 15:00:00` or `2026-08-19T15:00:00` format ||
|#

The lead may be created while the activity does not appear where you expect it. Check the following in order:

- [crm.lead.get](../../../api-reference/crm/leads/crm-lead-get.md) returned `STATUS_ID` with the `NEW` value although the CRM is operating in simple mode. The lead has not been converted — check that the `NAME` field is filled in

- The status is `CONVERTED`, but [crm.deal.list](../../../api-reference/crm/deals/crm-deal-list.md) returned an empty list. The webhook user does not have permission to read deals. The deal has been created but did not make it into the selection

- The activity was added to the lead although you expected it in the deal. This means that the lead had not yet been converted at the moment of the check

Repeat the scenario from the step that returned the error. Retrieving the lead and the list of deals do not create anything, so they can be executed any number of times. If [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) returned the error, the lead was not created: fix the `fields` and repeat only that call. If [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) returned the error, the lead already exists — repeat only the activity creation, otherwise you get a duplicate lead.

## Key Considerations

- The webhook needs permissions for two object types at once. The branch is selected by the system, and it is not known in advance whether the activity ends up in the lead or in the deal

- The [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md) method has no typed wrapper in B24PhpSDK and b24pysdk, so we call it through the SDK core

- Submitting the form again with the same data creates a new lead every time. Duplicates are not filtered out. To link repeat requests, use the [{#T}](./how-to-add-repeat-lead.md) scenario

- The call reminder can be configured more precisely: pass the `pingOffsets` parameter to [crm.activity.todo.add](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md), for example `[0, 15]` — notifications arrive 15 minutes before the deadline and at the moment it comes

## Continue Learning

- [{#T}](../../../api-reference/crm/leads/crm-lead-add.md)
- [{#T}](../../../api-reference/crm/leads/crm-lead-get.md)
- [{#T}](../../../api-reference/crm/deals/crm-deal-list.md)
- [{#T}](../../../api-reference/crm/timeline/activities/todo/crm-activity-todo-add.md)
- [{#T}](../../../api-reference/crm/crm-settings-mode-get.md)
- [{#T}](../../../api-reference/crm/data-types.md)
