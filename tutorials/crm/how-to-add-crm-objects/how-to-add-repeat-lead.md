# Add Repeat Lead

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the method: users with read access permission for leads, contacts, companies, and permission to create leads.

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

When a customer fills out a form on a site, their data is sent to a handler. The script searches the CRM for matches by phone number or email address among leads, contacts, and companies. If matches are found, the lead is marked as a repeat lead and linked to the existing record. This approach helps avoid duplicates and increases manager efficiency.

{% note info "" %}

Repeat lead mode must be enabled in Bitrix24. For more details, read the article [Repeat Leads and Deals](https://helpdesk.bitrix24.com/open/24147842/).

{% endnote %}

The setup consists of two stages:

1. Prepare the fields and place the form on the page.

2. Create a handler file that sequentially calls the [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md), [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md), and [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) methods.

## 1. Create a Web Form

Create an HTML form with the following fields:

- `NAME` — customer name, required field,

- `LAST_NAME` — surname,

- `PHONE` — phone,

- `EMAIL` — Email.

The form sends data to the handler using the `POST` method.

{% list tabs %}

- JS

    ```html
    <form id="form_to_crm">
        <input type="text" name="NAME" placeholder="Name" required>
        <input type="text" name="LAST_NAME" placeholder="Last name">
        <input type="text" name="PHONE" placeholder="Phone">
        <input type="text" name="EMAIL" placeholder="E-mail">
        <input type="submit" value="Submit">
    </form>

    <script>
        document.getElementById('form_to_crm').addEventListener('submit', async (el) => {
            el.preventDefault();
            const formData = Object.fromEntries(new FormData(el.currentTarget).entries());
            const response = await fetch('/form', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(formData),
            });
            const data = await response.json();
            alert(data.message);
        });
    </script>
    ```

- PHP

    ```html
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
    $(document).ready(function() {
        $('#form_to_crm').on( 'submit', function(el) {//event submit form
            el.preventDefault();//the default action of the event will not be triggered
            var formData = $(this).serialize();
            $.ajax({
                'method': 'POST',
                'dataType': 'json',
                'url': 'form.php', // file for saving filled forms
                'data': formData,
                success: function(data){//success callback
                    alert(data.message);
                }
            });
        });
    });
    </script>

    <form id="form_to_crm">
        <input type="text" name="NAME" placeholder="Name" required>
        <input type="text" name="LAST_NAME" placeholder="Last name">
        <input type="text" name="PHONE" placeholder="Phone">
        <input type="text" name="EMAIL" placeholder="E-mail">
        <input type="submit" value="Submit">
    </form>
    ```

- Python

    ```html
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
    $(document).ready(function() {
        $('#form_to_crm').on( 'submit', function(el) {//event submit form
            el.preventDefault();//the default action of the event will not be triggered
            var formData = $(this).serialize();
            $.ajax({
                'method': 'POST',
                'dataType': 'json',
                'url': '/form', // Flask handler route
                'data': formData,
                success: function(data){//success callback
                    alert(data.message);
                }
            });
        });
    });
    </script>

    <form id="form_to_crm">
        <input type="text" name="NAME" placeholder="Name" required>
        <input type="text" name="LAST_NAME" placeholder="Last name">
        <input type="text" name="PHONE" placeholder="Phone">
        <input type="text" name="EMAIL" placeholder="E-mail">
        <input type="submit" value="Submit">
    </form>
    ```

{% endlist %}

## 2. Creating a Form Handler

Create a handler. It will process the data, check for duplicates, and create a lead.

### Retrieving Data from the Form

Retrieve and safely process data from the `NAME`, `LAST_NAME`, `PHONE`, and `EMAIL` fields to prevent XSS attacks.

{% list tabs %}

- JS

    ```javascript
    const sName = String(req.body.NAME ?? '')
    const sLastName = String(req.body.LAST_NAME ?? '')
    const sPhone = String(req.body.PHONE ?? '')
    const sEmail = String(req.body.EMAIL ?? '')
    ```

- PHP

    ```php
    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $sEmail = htmlspecialchars($_POST["EMAIL"]);
    ```

- Python

    ```python
    s_name = request.form.get("NAME", "")
    s_last_name = request.form.get("LAST_NAME", "")
    s_phone = request.form.get("PHONE", "")
    s_email = request.form.get("EMAIL", "")
    ```

{% endlist %}

Form an `$arFields` array with the new lead data.

{% list tabs %}

- JS

    ```javascript
    const arFields = {
        TITLE: 'From the site: ' + [sName, sLastName].join(' '),
        NAME: sName || 'Empty name',
        LAST_NAME: sLastName,
        PHONE: sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'HOME' }] : [],
        EMAIL: sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : [],
    }
    ```

- PHP

    ```php
    $arFields = [
        'TITLE' => 'From the site: ' . implode(' ', [$sName, $sLastName]),
        'NAME' => (!empty($sName)) ? $sName : 'Empty name',
        'LAST_NAME' => $sLastName,
        'PHONE' => (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'HOME')) : array(),
        'EMAIL' => (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array()
    ];
    ```

- Python

    ```python
    ar_fields = {
        "TITLE": "From the site: " + " ".join([s_name, s_last_name]),
        "NAME": s_name or "Empty name",
        "LAST_NAME": s_last_name,
        "PHONE": [{"VALUE": s_phone, "VALUE_TYPE": "HOME"}] if s_phone else [],
        "EMAIL": [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else [],
    }
    ```

{% endlist %}

Form the lead heading as `From the site: First Name Last Name`.

The system stores phone numbers and email addresses as arrays of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) objects, so we form the `PHONE` and `EMAIL` arrays using the `$sPhone` and `$sEmail` values.

- Write `$sPhone` and `$sEmail` to the `VALUE` fields.

- Pass [types](../../../api-reference/crm/data-types.md#crm_multifield), such as `HOME`, to the `VALUE_TYPE` fields.

If the `$sPhone` and `$sEmail` variables do not contain values, specify empty arrays.

### Searching for Lead Duplicates

To find repeat leads by phone number and email address, call the [crm.duplicate.findbycomm](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md) method twice. You must pass the following data to it:

- `entity_type` — the object type. Pass `LEAD` — lead.

- `type` — the communication type. Specify `PHONE` during the first call and `EMAIL` during the second.

- `values` — an array of values to search for. Pass the phone number value `$sPhone` retrieved from the form.

Searching by phone, `"type" => "PHONE"`.

{% list tabs %}

- JS

    ```javascript
    if (sPhone) {
        const resultDuplicate = await $b24.actions.v2.call.make({
            method: 'crm.duplicate.findbycomm',
            params: { entity_type: 'LEAD', type: 'PHONE', values: [sPhone] },
            requestId: 'dup-phone'
        })
        const found = resultDuplicate.getData()?.result?.LEAD
        if (found) arLeadDuplicate = arLeadDuplicate.concat(found)
    }
    ```

- PHP

    ```php
    if (!empty($sPhone)) {
        $result = $sb->getCRMScope()->duplicate()->findByPhone(
            [$sPhone],
            \Bitrix24\SDK\Services\CRM\Duplicates\Service\EntityType::Lead
        )->getCoreResponse()->getResponseData()->getResult();
        if (!empty($result['LEAD'])) {
            $arLeadDuplicate = array_merge($arLeadDuplicate, $result['LEAD']);
        }
    }
    ```

- Python

    ```python
    if s_phone:
        result_duplicate = client.crm.duplicate.findbycomm(
            type="PHONE", values=[s_phone], entity_type="LEAD",
        ).result
        if result_duplicate.get("LEAD"):
            ar_lead_duplicate += result_duplicate["LEAD"]
    ```

{% endlist %}

Searching for duplicates by email, `"type" => "EMAIL"`.

{% list tabs %}

- JS

    ```javascript
    if (sEmail) {
        const resultDuplicate = await $b24.actions.v2.call.make({
            method: 'crm.duplicate.findbycomm',
            params: { entity_type: 'LEAD', type: 'EMAIL', values: [sEmail] },
            requestId: 'dup-email'
        })
        const found = resultDuplicate.getData()?.result?.LEAD
        if (found) arLeadDuplicate = arLeadDuplicate.concat(found)
    }
    ```

- PHP

    ```php
    if (!empty($sEmail)) {
        $result = $sb->getCRMScope()->duplicate()->findByEmail(
            [$sEmail],
            \Bitrix24\SDK\Services\CRM\Duplicates\Service\EntityType::Lead
        )->getCoreResponse()->getResponseData()->getResult();
        if (!empty($result['LEAD'])) {
            $arLeadDuplicate = array_merge($arLeadDuplicate, $result['LEAD']);
        }
    }
    ```

- Python

    ```python
    if s_email:
        result_duplicate = client.crm.duplicate.findbycomm(
            type="EMAIL", values=[s_email], entity_type="LEAD",
        ).result
        if result_duplicate.get("LEAD"):
            ar_lead_duplicate += result_duplicate["LEAD"]
    ```

{% endlist %}

Combine the identifiers of the found duplicates into the `$arLeadDuplicate` array.

### Processing Duplicates

If duplicates are found, call the [crm.lead.list](../../../api-reference/crm/leads/crm-lead-list.md) method.

1. Apply a filter by identifier and status `CONVERTED`.

2. Select the fields: `ID`, `COMPANY_ID,`, and `CONTACT_ID`.

3. Retain the result in the `$arDuplicateLead` array.

4. Fill the `COMPANY_ID` and `CONTACT_ID` fields in the `$arFields` array with values from `$arDuplicateLead`.

{% list tabs %}

- JS

    ```javascript
    if (arLeadDuplicate.length) {
        const duplicateLead = await $b24.actions.v2.callList.make({
            method: 'crm.lead.list',
            params: {
                filter: { '=ID': arLeadDuplicate, STATUS_ID: 'CONVERTED' },
                select: ['ID', 'COMPANY_ID', 'CONTACT_ID']
            },
            requestId: 'dup-lead-list'
        })
        const arDuplicateLead = duplicateLead.getData()?.result ?? []
        const company = arDuplicateLead.map(r => r.COMPANY_ID).find(v => v > 0)
        const contact = arDuplicateLead.map(r => r.CONTACT_ID).find(v => v > 0)
        if (company) arFields.COMPANY_ID = company
        if (contact) arFields.CONTACT_ID = contact
    }
    ```

- PHP

    ```php
    if (!empty($arLeadDuplicate)) {
        $arDuplicateLead = [];
        foreach ($sb->getCRMScope()->lead()->batch->list(
            order: [],
            filter: ['=ID' => $arLeadDuplicate, 'STATUS_ID' => 'CONVERTED'],
            select: ['ID', 'COMPANY_ID', 'CONTACT_ID']
        ) as $lead) {
            $arDuplicateLead[] = $lead;
        }

        foreach ($arDuplicateLead as $lead) {
            if ($lead->COMPANY_ID > 0 && empty($arFields['COMPANY_ID'])) {
                $arFields['COMPANY_ID'] = $lead->COMPANY_ID;
            }
            if ($lead->CONTACT_ID > 0 && empty($arFields['CONTACT_ID'])) {
                $arFields['CONTACT_ID'] = $lead->CONTACT_ID;
            }
        }
    }
    ```

- Python

    ```python
    if ar_lead_duplicate:
        ar_duplicate_lead = client.crm.lead.list(
            filter={"=ID": ar_lead_duplicate, "STATUS_ID": "CONVERTED"},
            select=["ID", "COMPANY_ID", "CONTACT_ID"],
        ).as_list().result

        company = next((r["COMPANY_ID"] for r in ar_duplicate_lead if int(r["COMPANY_ID"] or 0) > 0), None)
        contact = next((r["CONTACT_ID"] for r in ar_duplicate_lead if int(r["CONTACT_ID"] or 0) > 0), None)
        if company:
            ar_fields["COMPANY_ID"] = company
        if contact:
            ar_fields["CONTACT_ID"] = contact
    ```

{% endlist %}

### Adding a New Lead

To add a lead, use the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method. Pass the `$arFields` array to it.

{% note warning "" %}

Check which mandatory fields are configured for leads in your Bitrix24. All mandatory fields must be passed to the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method.

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    await $b24.actions.v2.call.make({
        method: 'crm.lead.add',
        params: { fields: arFields },
        requestId: 'repeat-lead-add'
    })
    ```

- PHP

    ```php
    $sb->getCRMScope()->lead()->add($arFields);
    ```

- Python

    ```python
    client.crm.lead.add(fields=ar_fields)
    ```

{% endlist %}

If the lead is created successfully, the method will return its identifier. If you receive error `error`, review the possible error descriptions in the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method documentation.

```json
{
    "result": 3289,
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

    // Handler accepts form data via the /form route
    app.post('/form', async (req, res) => {
        const sName = String(req.body.NAME ?? '')
        const sLastName = String(req.body.LAST_NAME ?? '')
        const sPhone = String(req.body.PHONE ?? '')
        const sEmail = String(req.body.EMAIL ?? '')

        const arFields = {
            TITLE: sName || 'Empty name',
            LAST_NAME: sLastName,
            PHONE: sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'HOME' }] : [],
            EMAIL: sEmail ? [{ VALUE: sEmail, VALUE_TYPE: 'HOME' }] : [],
        }

        let arLeadDuplicate = []
        if (sPhone) { // search for duplicates by phone number
            const r = await $b24.actions.v2.call.make({
                method: 'crm.duplicate.findbycomm',
                params: { entity_type: 'LEAD', type: 'PHONE', values: [sPhone] },
                requestId: 'dup-phone'
            })
            const found = r.getData()?.result?.LEAD
            if (found) arLeadDuplicate = arLeadDuplicate.concat(found)
        }

        if (sEmail) { // search for duplicates by email
            const r = await $b24.actions.v2.call.make({
                method: 'crm.duplicate.findbycomm',
                params: { entity_type: 'LEAD', type: 'EMAIL', values: [sEmail] },
                requestId: 'dup-email'
            })
            const found = r.getData()?.result?.LEAD
            if (found) arLeadDuplicate = arLeadDuplicate.concat(found)
        }

        if (arLeadDuplicate.length) { // get lead duplicate with fields of associated contact and company
            const duplicateLead = await $b24.actions.v2.callList.make({
                method: 'crm.lead.list',
                params: {
                    filter: { '=ID': arLeadDuplicate, STATUS_ID: 'CONVERTED' },
                    select: ['ID', 'COMPANY_ID', 'CONTACT_ID']
                },
                requestId: 'dup-lead-list'
            })
            const arDuplicateLead = duplicateLead.getData()?.result ?? []
            const company = arDuplicateLead.map(r => r.COMPANY_ID).find(v => v > 0)
            const contact = arDuplicateLead.map(r => r.CONTACT_ID).find(v => v > 0)
            if (company) arFields.COMPANY_ID = company
            if (contact) arFields.CONTACT_ID = contact
        }

        const result = await $b24.actions.v2.call.make({ // create repeat lead
            method: 'crm.lead.add',
            params: { fields: arFields },
            requestId: 'repeat-lead-add'
        })

        if (result.isSuccess && result.getData()?.result) {
            res.json({ message: 'Lead add' })
        } else {
            res.json({ message: 'Lead not added: ' + result.getErrorMessages().join('; ') })
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
    use Bitrix24\SDK\Services\CRM\Duplicates\Service\EntityType;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Monolog\Logger;
    use Monolog\Handler\StreamHandler;

    $log = new Logger('b24');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $sb = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->initFromWebhook('https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/');

    $sName = htmlspecialchars($_POST["NAME"]);
    $sLastName = htmlspecialchars($_POST["LAST_NAME"]);
    $sPhone = htmlspecialchars($_POST["PHONE"]);
    $sEmail = htmlspecialchars($_POST["EMAIL"]);

    $arFields = [
        'TITLE' => (!empty($sName)) ? $sName : 'Empty name',
        'LAST_NAME' => $sLastName,
        'PHONE' => (!empty($sPhone)) ? array(array('VALUE' => $sPhone, 'VALUE_TYPE' => 'HOME')) : array(),
        'EMAIL' => (!empty($sEmail)) ? array(array('VALUE' => $sEmail, 'VALUE_TYPE' => 'HOME')) : array()
    ];

    $arLeadDuplicate = [];
    if (!empty($sPhone)) { // search for duplicates by phone number
        $r = $sb->getCRMScope()->duplicate()->findByPhone([$sPhone], EntityType::Lead)
            ->getCoreResponse()->getResponseData()->getResult();
        if (!empty($r['LEAD'])) {
            $arLeadDuplicate = array_merge($arLeadDuplicate, $r['LEAD']);
        }
    }

    if (!empty($sEmail)) { // search for duplicates by email
        $r = $sb->getCRMScope()->duplicate()->findByEmail([$sEmail], EntityType::Lead)
            ->getCoreResponse()->getResponseData()->getResult();
        if (!empty($r['LEAD'])) {
            $arLeadDuplicate = array_merge($arLeadDuplicate, $r['LEAD']);
        }
    }

    if (!empty($arLeadDuplicate)) { // get lead duplicate with fields of associated contact and company
        foreach ($sb->getCRMScope()->lead()->batch->list(
            order: [],
            filter: ['=ID' => $arLeadDuplicate, 'STATUS_ID' => 'CONVERTED'],
            select: ['ID', 'COMPANY_ID', 'CONTACT_ID']
        ) as $lead) {
            if ($lead->COMPANY_ID > 0 && empty($arFields['COMPANY_ID'])) {
                $arFields['COMPANY_ID'] = $lead->COMPANY_ID;
            }
            if ($lead->CONTACT_ID > 0 && empty($arFields['CONTACT_ID'])) {
                $arFields['CONTACT_ID'] = $lead->CONTACT_ID;
            }
        }
    }

    try {
        $sb->getCRMScope()->lead()->add($arFields); // create repeat lead
        echo json_encode(['message' => 'Lead add']);
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'Lead not added: ' . $e->getMessage()]);
    }
    ```

- Python

    ```python
    # pip install b24pysdk
    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # user_id/token only, without https://
    ))

    @app.route("/form", methods=["POST"])
    def handle_form():
        s_name = request.form.get("NAME", "")
        s_last_name = request.form.get("LAST_NAME", "")
        s_phone = request.form.get("PHONE", "")
        s_email = request.form.get("EMAIL", "")

        ar_fields = {
            "TITLE": s_name or "Empty name",
            "LAST_NAME": s_last_name,
            "PHONE": [{"VALUE": s_phone, "VALUE_TYPE": "HOME"}] if s_phone else [],
            "EMAIL": [{"VALUE": s_email, "VALUE_TYPE": "HOME"}] if s_email else [],
        }

        ar_lead_duplicate = []
        if s_phone:  # search for duplicates by phone number
            r = client.crm.duplicate.findbycomm(type="PHONE", values=[s_phone], entity_type="LEAD").result
            if r.get("LEAD"):
                ar_lead_duplicate += r["LEAD"]

        if s_email:  # search for duplicates by email
            r = client.crm.duplicate.findbycomm(type="EMAIL", values=[s_email], entity_type="LEAD").result
            if r.get("LEAD"):
                ar_lead_duplicate += r["LEAD"]

        if ar_lead_duplicate:  # get lead duplicate with fields of associated contact and company
            ar_duplicate_lead = client.crm.lead.list(
                filter={"=ID": ar_lead_duplicate, "STATUS_ID": "CONVERTED"},
                select=["ID", "COMPANY_ID", "CONTACT_ID"],
            ).as_list().result
            company = next((r["COMPANY_ID"] for r in ar_duplicate_lead if int(r["COMPANY_ID"] or 0) > 0), None)
            contact = next((r["CONTACT_ID"] for r in ar_duplicate_lead if int(r["CONTACT_ID"] or 0) > 0), None)
            if company:
                ar_fields["COMPANY_ID"] = company
            if contact:
                ar_fields["CONTACT_ID"] = contact

        try:
            client.crm.lead.add(fields=ar_fields)  # create repeat lead
            return jsonify({"message": "Lead add"})
        except Exception as e:
            return jsonify({"message": f"Lead not added: {e}"})
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/crm/duplicates/crm-duplicate-find-by-comm.md)
- [{#T}](../../../api-reference/crm/leads/crm-lead-list.md)
- [{#T}](../../../api-reference/crm/leads/crm-lead-add.md)
