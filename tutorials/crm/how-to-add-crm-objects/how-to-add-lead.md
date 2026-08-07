# Add a Lead via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, both permissions are required — to add leads and to read leads
>
> - [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) — a user with permission to add leads
> - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — a user with permission to read leads

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can place a form on your website to collect data from potential clients. When a client fills out the form, a handler on your server creates a lead in the CRM and returns its identifier. As a result, you get two files — a page with the form and a handler built on the stack of your choice — plus a separate script to verify the result.

The scenario consists of two steps.

1. Place the form on an HTML page. The form sends the data to the handler

2. Create the handler. It validates the data and creates a lead using the universal [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method

After that, we verify the result with the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method using the identifier from the second step response: this call only confirms the creation and does not change any data in the CRM.

## Prepare the Data

To run this example, you need:

- an [incoming webhook](../../../local-integrations/local-webhooks.md) with the `crm` scope. The handler runs on the server: the page with the form does not use the webhook

- permissions for the user on whose behalf the webhook is created: to add leads — for step 2, and to read leads — for the verification step

- classic CRM mode. In simple mode, there are no leads in the CRM: when a lead is created with the first name filled in, the system automatically converts it into a deal, and the `STATUS_ID` field takes the value `CONVERTED`. The [crm.settings.mode.get](../../../api-reference/crm/crm-settings-mode-get.md) method returns the current mode, and the scenario for both modes is covered in the tutorial [Add a CRM Activity to a New Lead or Deal Depending on the CRM Mode](./how-to-add-objects-with-crm-mode.md)

- the page with the form and the handler on the same domain and port. In the example, `handlerUrl` is a relative path, so the request goes to the address the page was opened from. If you open `form.html` from a different address or from the file system, the handler will not receive the data

- environment variables with the webhook data. Set them in the process environment before startup, do not write them into the code:

   - `B24_HOOK` — the full webhook URL, for JS and PHP

   - `B24_DOMAIN` and `B24_WEBHOOK_TOKEN` — the domain and `USER_ID/TOKEN` without `https://`, for Python

   For example, for a local run:

   - Node.js — `B24_HOOK='https://your-domain.bitrix24.com/rest/1/TOKEN/' node handler.mjs`

   - PHP — `B24_HOOK='https://your-domain.bitrix24.com/rest/1/TOKEN/' php -S localhost:3000 -t public`

   - Python — `B24_DOMAIN='your-domain.bitrix24.com' B24_WEBHOOK_TOKEN='1/TOKEN' python handler.py`

If mandatory fields are configured for leads in your Bitrix24, they must also be passed in the `fields` of the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method — otherwise the method returns an error. The [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method with `entityTypeId` set to `1` returns the list of fields with the `isRequired` flag.

For server-side JS examples with `B24Hook`, Node.js 18, 20, 22 or newer is required. For new projects, take 22 or newer: community support for Node.js 18 and 20 has ended. [B24JsSDK](../../../sdk/b24jssdk/index.md) is an ES module: save the code in an `.mjs` file or add `"type": "module"` to `package.json`.

For examples with [b24pysdk](../../../sdk/b24pysdk/index.md), Python 3.9 or newer is required.

For examples with `bitrix24/b24phpsdk:"^3.3"`, PHP 8.4 or newer is required with the `curl`, `intl`, and `json` extensions, and `mbstring` for the value length check in the example. For the SDK requirements and the recommended file layout, see the [B24PhpSDK](../../../sdk/b24phpsdk/index.md) page.

## 1. Creating the Web Form

In Bitrix24, a contact and a company can be created automatically from a lead. To make the form suitable for different cases, we will make it universal. For a contact, a first name and last name are required, and for a company, a name is required. We will create a web form on a website page with five fields:

- `NAME` — first name, a required form field

- `LAST_NAME` — last name

- `COMPANY_TITLE` — company name

- `EMAIL` — email

- `PHONE` — phone

Save the page to the `form.html` file. Where it must be located depends on the handler:

- Node.js — in the `public` subfolder of the directory that contains the handler file. Run `node` from this directory: `express.static` looks for the `public` folder relative to the working directory of the process. The page opens at `http://localhost:3000/form.html`

- Flask — in the `static` folder. The page opens at `http://localhost:3000/static/form.html`

- PHP — in the public directory of the web server together with `form.php`. Keep the `vendor` directory above the public one so that it is not accessible from the browser. With the local run `php -S localhost:3000 -t public`, the page opens at `http://localhost:3000/form.html`

The addresses start working after the startup from step 2: in Node.js and Flask, the page is served by the same application that receives the form data, and in PHP — by the web server that hosts `form.php`.

The form sends the data to the handler with the `POST` method in the `application/x-www-form-urlencoded` format. The same code works with all three handler options: only the address in the `handlerUrl` variable changes.

```html
<form id="form_to_crm">
    <input type="text" name="NAME" placeholder="First Name" required>
    <input type="text" name="LAST_NAME" placeholder="Last Name">
    <input type="text" name="COMPANY_TITLE" placeholder="Company Name">
    <input type="text" name="EMAIL" placeholder="Email">
    <input type="text" name="PHONE" placeholder="Phone">
    <input type="submit" value="Submit">
</form>

<script>
    // Handler address: '/form' — for Node.js and Flask, 'form.php' — for PHP
    const handlerUrl = '/form';

    document.getElementById('form_to_crm').addEventListener('submit', async (event) => {
        event.preventDefault(); // Canceling the standard form submission
        // Collecting the form fields into the request body
        const body = new URLSearchParams(new FormData(event.currentTarget));

        const response = await fetch(handlerUrl, { method: 'POST', body });
        // The handler responds with JSON. If something else arrives, we show a generic message
        const data = await response.json().catch(() => ({ message: 'The server returned an unexpected response' }));

        // Showing the result: the identifier will be needed for verification via REST
        alert(data.id ? data.message + '. ID: ' + data.id : data.message);
    });
</script>
```

The `required` attribute checks the mandatory field only in the browser. A request can be sent bypassing the form, so the handler validates the data again.

## 2. Creating a Form Handler

The handler receives the form data, validates it, and adds a lead using the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method. In the `entityTypeId` parameter, we pass `1` — the "Lead" object type; the values for the other types are listed in the [CRM object types reference](../../../api-reference/crm/data-types.md#object_type). In the `fields` object, we pass the fields:

- `title` — lead title. We compose it from the first name, last name, and company name

- `name` — first name

- `lastName` — last name

- `companyTitle` — company name

- `fm` — an array of multifields, in which we pass the phone and email

The universal `crm.item.*` methods use field names in camelCase. They differ from the names in the methods of individual objects: `title` instead of `TITLE`, `lastName` instead of `LAST_NAME`, and the phone and email are passed in a single `fm` array instead of separate `PHONE` and `EMAIL` fields.

We retrieve the field values from the form. Each action of the handler is described below, and the full code is provided at the end of the section.

{% include [Note on examples](../../../_includes/examples.md) %}

### Receiving the Request and Connecting the SDK

The handler receives a POST request at the address specified in the `handlerUrl` variable on the page with the form. We work with Bitrix24 through an incoming webhook.

{% list tabs %}

- JS

    ```javascript
    // npm install express @bitrix24/b24jssdk
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const app = express()
    // The form sends the data in the application/x-www-form-urlencoded format
    app.use(express.urlencoded({ extended: true }))
    // Serving the page with the form from the public folder
    app.use(express.static('public'))

    // Pattern for validating the email address
    const emailPattern = /^[^@\s]+@[^@\s]+\.[^@\s]+$/
    // Value length limit: the form is public
    const maxLength = 100

    // The handler receives the form data via the /form route
    app.post('/form', async (req, res) => {
        // The handler body — in the following steps
    })

    // Run: node handler.mjs
    app.listen(3000)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.3"
    // form.php and form.html are located in the public directory, vendor — above it
    // Local run: php -S localhost:3000 -t public
    require_once __DIR__ . '/../vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;

    header('Content-Type: application/json; charset=utf-8');

    // Value length limit: the form is public
    const MAX_LENGTH = 100;

    // The handler accepts POST requests only
    if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
        http_response_code(405);
        echo json_encode(['message' => 'Method not supported']);
        exit;
    }

    $sb = ServiceBuilderFactory::createServiceBuilderFromWebhook(getenv('B24_HOOK'));
    ```

- Python

    ```python
    # pip install flask b24pysdk
    import os
    import re

    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # We place the form.html page in the static folder
    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],  # your-domain.bitrix24.com
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],  # user_id/token only, without https://
    ))

    # Pattern for validating the email address
    EMAIL_PATTERN = re.compile(r"[^@\s]+@[^@\s]+\.[^@\s]+")
    # Value length limit: the form is public
    MAX_LENGTH = 100


    @app.route("/form", methods=["POST"])
    def handle_form():
        ...  # The handler body — in the following steps


    # Run: python handler.py
    if __name__ == "__main__":
        app.run(port=3000)
    ```

- Go

    ```go
    // The webhook path is a secret: it comes from the environment rather than from the code, and it
    // never reaches the public page with the form. The client is built ONCE
    // per portal and is reused by all requests — it holds the HTTP client and
    // the authorization state, while http.Server calls the handler from many goroutines.
    core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    mux := http.NewServeMux()
    // The page with the form is served from here as well: then the form request goes to the same
    // address it was opened from, and no static file setup is required.
    mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    	w.Header().Set("Content-Type", "text/html; charset=utf-8")
    	fmt.Fprint(w, formPage)
    })
    mux.HandleFunc("/form", func(w http.ResponseWriter, r *http.Request) {
    	if r.Method != http.MethodPost {
    		reply(w, http.StatusMethodNotAllowed, "POST is required", 0)
    		return
    	}
    	handleForm(w, r, core)
    })

    log.Println("form and handler: http://localhost:3000/")
    if err := http.ListenAndServe(":3000", mux); err != nil {
    	log.Fatal(err)
    }
    ```

{% endlist %}

### Validating the Form Data

The data comes from an anonymous visitor, so the handler validates it before calling the method:

- accesses the fields with a default value: the required key may be missing from the request

- trims the surrounding spaces and does not create a lead if the first name is empty

- checks the email format so that a knowingly invalid address is not saved in the CRM

- rejects values that are too long: anything can arrive in a public form

In REST, we pass the values as is. Do not apply `htmlspecialchars` or other HTML escaping functions to them: those are needed when displaying data on a page, and because of them `Weber & Son` would end up in the CRM as `Weber &amp; Son`.

{% list tabs %}

- JS

    ```javascript
    // Getting the data from the form
    const sName = String(req.body.NAME ?? '').trim()
    const sLastName = String(req.body.LAST_NAME ?? '').trim()
    const sCompanyTitle = String(req.body.COMPANY_TITLE ?? '').trim()
    const sPhone = String(req.body.PHONE ?? '').trim()
    const sEmail = String(req.body.EMAIL ?? '').trim()

    // Validating the data before calling the method
    if (!sName) {
        res.status(400).json({ message: 'Enter the first name' })
        return
    }

    if (sEmail && !emailPattern.test(sEmail)) {
        res.status(400).json({ message: 'Check the email address' })
        return
    }

    if ([sName, sLastName, sCompanyTitle, sPhone, sEmail].some(value => value.length > maxLength)) {
        res.status(400).json({ message: 'One of the fields is too long' })
        return
    }
    ```

- PHP

    ```php
    // Getting the data from the form
    $sName = trim((string)($_POST['NAME'] ?? ''));
    $sLastName = trim((string)($_POST['LAST_NAME'] ?? ''));
    $sCompanyTitle = trim((string)($_POST['COMPANY_TITLE'] ?? ''));
    $sPhone = trim((string)($_POST['PHONE'] ?? ''));
    $sEmail = trim((string)($_POST['EMAIL'] ?? ''));

    // Validating the data before calling the method
    if ($sName === '') {
        http_response_code(400);
        echo json_encode(['message' => 'Enter the first name']);
        exit;
    }

    if ($sEmail !== '' && !preg_match('/^[^@\s]+@[^@\s]+\.[^@\s]+$/u', $sEmail)) {
        http_response_code(400);
        echo json_encode(['message' => 'Check the email address']);
        exit;
    }

    foreach ([$sName, $sLastName, $sCompanyTitle, $sPhone, $sEmail] as $value) {
        if (mb_strlen($value) > MAX_LENGTH) {
            http_response_code(400);
            echo json_encode(['message' => 'One of the fields is too long']);
            exit;
        }
    }
    ```

- Python

    ```python
    # Getting the data from the form
    s_name = request.form.get("NAME", "").strip()
    s_last_name = request.form.get("LAST_NAME", "").strip()
    s_company_title = request.form.get("COMPANY_TITLE", "").strip()
    s_phone = request.form.get("PHONE", "").strip()
    s_email = request.form.get("EMAIL", "").strip()

    # Validating the data before calling the method
    if not s_name:
        return jsonify({"message": "Enter the first name"}), 400

    if s_email and not EMAIL_PATTERN.fullmatch(s_email):
        return jsonify({"message": "Check the email address"}), 400

    if any(len(value) > MAX_LENGTH for value in (s_name, s_last_name, s_company_title, s_phone, s_email)):
        return jsonify({"message": "One of the fields is too long"}), 400
    ```

- Go

    ```go
    // The data comes from an anonymous visitor, so before the method is called
    // the handler validates them.
    if err := r.ParseForm(); err != nil {
    	reply(w, http.StatusBadRequest, "Failed to parse the form", 0)
    	return
    }
    // r.PostFormValue returns an empty string when the key is missing entirely, so
    // there is nothing to fail on for a missing field.
    name := strings.TrimSpace(r.PostFormValue("NAME"))
    lastName := strings.TrimSpace(r.PostFormValue("LAST_NAME"))
    companyTitle := strings.TrimSpace(r.PostFormValue("COMPANY_TITLE"))
    phone := strings.TrimSpace(r.PostFormValue("PHONE"))
    email := strings.TrimSpace(r.PostFormValue("EMAIL"))

    if name == "" {
    	reply(w, http.StatusBadRequest, "Enter the first name", 0)
    	return
    }
    if email != "" && !emailPattern.MatchString(email) {
    	reply(w, http.StatusBadRequest, "Check the email address", 0)
    	return
    }
    for _, value := range []string{name, lastName, companyTitle, phone, email} {
    	// The length is counted in RUNES rather than bytes: in UTF-8 a non-Latin letter
    	// takes two bytes, and len() would reject a name twice as short.
    	if len([]rune(value)) > maxLength {
    		reply(w, http.StatusBadRequest, "One of the fields is too long", 0)
    		return
    	}
    }
    // In REST, values are sent as is: html.EscapeString and the like
    // is needed when rendering to a page, while in CRM it turns "Weber & Son" into
    // "Weber &amp; Son".
    ```

{% endlist %}

### Collecting the Phone and Email into Multifields

The method accepts the phone and email in the `fm` field — an array of [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) objects. Each object has three keys:

- `typeId` — the multifield type: `PHONE` for a phone, `EMAIL` for an email

- `valueType` — the value type, for example `WORK` — work, `HOME` — home

- `value` — the value from the form

If the visitor did not fill in a field, we do not add the object to the array. If no value is filled in, we pass an empty array.

In the [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) reference, the multifield keys are listed in uppercase — `TYPE_ID`, `VALUE_TYPE`, `VALUE`. That is the format of the methods of individual objects, for example `crm.lead.add`. The universal `crm.item.*` methods accept and return the same keys in camelCase.

{% list tabs %}

- JS

    ```javascript
    // Collecting the phone and email into multifields
    const arFm = []

    if (sPhone) {
        arFm.push({ typeId: 'PHONE', valueType: 'WORK', value: sPhone })
    }

    if (sEmail) {
        arFm.push({ typeId: 'EMAIL', valueType: 'HOME', value: sEmail })
    }
    ```

- PHP

    ```php
    // Collecting the phone and email into multifields
    $arFm = [];

    if ($sPhone !== '') {
        $arFm[] = ['typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => $sPhone];
    }

    if ($sEmail !== '') {
        $arFm[] = ['typeId' => 'EMAIL', 'valueType' => 'HOME', 'value' => $sEmail];
    }
    ```

- Python

    ```python
    # Collecting the phone and email into multifields
    ar_fm = []

    if s_phone:
        ar_fm.append({"typeId": "PHONE", "valueType": "WORK", "value": s_phone})

    if s_email:
        ar_fm.append({"typeId": "EMAIL", "valueType": "HOME", "value": s_email})
    ```

- Go

    ```go
    // The phone and the email go into the fm field — an array of crm_multifield objects.
    // The universal crm.item.* methods accept keys in camelCase, whereas
    // crm.lead.add would expect the same keys in UPPERCASE.
    fm := make([]b24.Params, 0, 2)
    if phone != "" {
    	fm = append(fm, b24.Params{"typeId": "PHONE", "valueType": "WORK", "value": phone})
    }
    if email != "" {
    	fm = append(fm, b24.Params{"typeId": "EMAIL", "valueType": "HOME", "value": email})
    }
    ```

{% endlist %}

### Composing the Lead Title

We compose the title from the first name and last name. If the visitor specified a company name, we add it after a dash — this way the manager sees in the lead list who submitted the request.

{% list tabs %}

- JS

    ```javascript
    // Creating the lead title from the first and last name
    let sTitle = 'From the website: ' + `${sName} ${sLastName}`.trim()

    // If there is a company name — add it via a dash after the first and last name

    if (sCompanyTitle) {
        sTitle += ' — ' + sCompanyTitle
    }
    ```

- PHP

    ```php
    // Creating the lead title from the first and last name
    $sTitle = 'From the website: ' . trim($sName . ' ' . $sLastName);

    // If there is a company name — add it via a dash after the first and last name

    if ($sCompanyTitle !== '') {
        $sTitle .= ' — ' . $sCompanyTitle;
    }
    ```

- Python

    ```python
    # Creating the lead title from the first and last name
    s_title = "From the website: " + f"{s_name} {s_last_name}".strip()

    # If there is a company name — add it via a dash after the first and last name

    if s_company_title:
        s_title += " — " + s_company_title
    ```

- Go

    ```go
    // The title is assembled from the first and last name, and the company name is appended after
    // a dash — this way the manager sees in the lead list who the request came from.
    title := "From the website: " + strings.TrimSpace(name+" "+lastName)
    if companyTitle != "" {
    	title += " — " + companyTitle
    }
    ```

{% endlist %}

### Creating the Lead

We pass the prepared values in the `fields` of the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method. The handler returns the identifier of the created lead to the page in the `id` field. We write the error text to the server log and return a generic message to the visitor: this way technical details do not end up on a public page.

{% list tabs %}

- JS

    ```javascript
    // Sending the data to Bitrix24
    try {
        const response = await $b24.actions.v2.call.make({
            method: 'crm.item.add',
            params: {
                entityTypeId: 1, // CRM object type — lead
                fields: {
                    title: sTitle, // Lead title
                    name: sName, // First name
                    lastName: sLastName, // Last name
                    companyTitle: sCompanyTitle, // Company name
                    fm: arFm, // Phone and email
                }
            },
            requestId: 'lead-add'
        })

        // Checking the result and displaying a message
        if (!response.isSuccess) {
            // We write the error details to the log and do not show them to the visitor
            console.error(response.getErrorMessages().join('; '))
            res.status(502).json({ message: 'Could not create the lead, try again later' })
            return
        }

        const leadId = response.getData().result.item.id // Identifier of the created lead
        console.info('Lead created with ID ' + leadId)
        res.json({ message: 'Lead created', id: leadId })
    } catch (error) {
        // Network errors and SDK failures arrive as an exception
        console.error(error)
        res.status(502).json({ message: 'Could not create the lead, try again later' })
    }
    ```

- PHP

    ```php
    // Sending the data to Bitrix24
    try {
        $result = $sb->getCRMScope()->item()->add(1, [ // 1 — the "Lead" CRM object type
            'title' => $sTitle, // Lead title
            'name' => $sName, // First name
            'lastName' => $sLastName, // Last name
            'companyTitle' => $sCompanyTitle, // Company name
            'fm' => $arFm, // Phone and email
        ]);

        $leadId = $result->item()->id; // Identifier of the created lead
        error_log('Lead created with ID ' . $leadId);
        echo json_encode(['message' => 'Lead created', 'id' => $leadId]);
    } catch (\Throwable $e) {
        // We write the error details to the log and do not show them to the visitor
        error_log($e->getMessage());
        http_response_code(502);
        echo json_encode(['message' => 'Could not create the lead, try again later']);
    }
    ```

- Python

    ```python
    # Sending the data to Bitrix24
    try:
        bitrix_response = client.crm.item.add(
            entity_type_id=1,  # CRM object type — lead
            fields={
                "title": s_title,  # Lead title
                "name": s_name,  # First name
                "lastName": s_last_name,  # Last name
                "companyTitle": s_company_title,  # Company name
                "fm": ar_fm,  # Phone and email
            },
        ).response
        lead_id = bitrix_response.result["item"]["id"]  # Identifier of the created lead
        app.logger.info("Lead created with ID %s", lead_id)
        return jsonify({"message": "Lead created", "id": lead_id})
    except (BitrixAPIError, BitrixSDKException) as error:
        # We write the error details to the log and do not show them to the visitor
        app.logger.error(error)
        return jsonify({"message": "Could not create the lead, try again later"}), 502
    ```

- Go

    ```go
    res, err := core.Call(r.Context(), "crm.item.add", b24.Params{
    	"entityTypeId": entityTypeLead,
    	"fields": b24.Params{
    		"title":        title,
    		"name":         name,
    		"lastName":     lastName,
    		"companyTitle": companyTitle,
    		"fm":           fm,
    	},
    }) // no WithIdempotent: a retry would create a second lead
    if err != nil {
    	// The details go to the server log, and the visitor gets a generic
    	// message: technical details have no place on a public page.
    	// The webhook address will not appear in the error text — the SDK strips it itself.
    	log.Println("crm.item.add:", err)
    	reply(w, http.StatusBadGateway, "Could not create the lead, try again later", 0)
    	return
    }

    // The method wraps the response in an object with the item key.
    raw, ok := b24.Unwrap(res.Result, "item", "id")
    if !ok {
    	log.Println("no item.id in the response:", string(res.Result))
    	reply(w, http.StatusBadGateway, "Could not create the lead, try again later", 0)
    	return
    }
    var leadID b24.ID
    if err := json.Unmarshal(raw, &leadID); err != nil {
    	log.Println("parse lead ID:", err)
    	reply(w, http.StatusBadGateway, "Could not create the lead, try again later", 0)
    	return
    }

    log.Printf("lead %d created", leadID)
    reply(w, http.StatusOK, "Lead created", leadID)
    ```

{% endlist %}

The method returns the data of the created lead in the `result.item` object.

Abbreviated response:

```json
{
    "result": {
        "item": {
            "id": 3465,
            "title": "From the website: Klaus Weber — Müller GmbH",
            "name": "Klaus",
            "lastName": "Weber",
            "companyTitle": "Müller GmbH",
            "entityTypeId": 1
        }
    }
}
```

The handler returns `{ "message": "Lead created", "id": 3465 }` to the page. The identifier will be needed to open the lead in the interface or to request its data via REST.

### Full Handler Code Example

{% list tabs %}

- JS

    ```javascript
    // npm install express @bitrix24/b24jssdk
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    const app = express()
    // The form sends the data in the application/x-www-form-urlencoded format
    app.use(express.urlencoded({ extended: true }))
    // Serving the page with the form from the public folder
    app.use(express.static('public'))

    // Pattern for validating the email address
    const emailPattern = /^[^@\s]+@[^@\s]+\.[^@\s]+$/
    // Value length limit: the form is public
    const maxLength = 100

    // The handler receives the form data via the /form route
    app.post('/form', async (req, res) => {
        // Getting the data from the form
        const sName = String(req.body.NAME ?? '').trim()
        const sLastName = String(req.body.LAST_NAME ?? '').trim()
        const sCompanyTitle = String(req.body.COMPANY_TITLE ?? '').trim()
        const sPhone = String(req.body.PHONE ?? '').trim()
        const sEmail = String(req.body.EMAIL ?? '').trim()

        // Validating the data before calling the method
        if (!sName) {
            res.status(400).json({ message: 'Enter the first name' })
            return
        }

        if (sEmail && !emailPattern.test(sEmail)) {
            res.status(400).json({ message: 'Check the email address' })
            return
        }

        if ([sName, sLastName, sCompanyTitle, sPhone, sEmail].some(value => value.length > maxLength)) {
            res.status(400).json({ message: 'One of the fields is too long' })
            return
        }

        // Collecting the phone and email into multifields
        const arFm = []

        if (sPhone) {
            arFm.push({ typeId: 'PHONE', valueType: 'WORK', value: sPhone })
        }

        if (sEmail) {
            arFm.push({ typeId: 'EMAIL', valueType: 'HOME', value: sEmail })
        }

        // Creating the lead title from the first and last name
        let sTitle = 'From the website: ' + `${sName} ${sLastName}`.trim()
        // If there is a company name — add it via a dash after the first and last name
        if (sCompanyTitle) {
            sTitle += ' — ' + sCompanyTitle
        }

        // Sending the data to Bitrix24
        try {
            const response = await $b24.actions.v2.call.make({
                method: 'crm.item.add',
                params: {
                    entityTypeId: 1, // CRM object type — lead
                    fields: {
                        title: sTitle, // Lead title
                        name: sName, // First name
                        lastName: sLastName, // Last name
                        companyTitle: sCompanyTitle, // Company name
                        fm: arFm, // Phone and email
                    }
                },
                requestId: 'lead-add'
            })

            // Checking the result and displaying a message
            if (!response.isSuccess) {
                // We write the error details to the log and do not show them to the visitor
                console.error(response.getErrorMessages().join('; '))
                res.status(502).json({ message: 'Could not create the lead, try again later' })
                return
            }

            const leadId = response.getData().result.item.id // Identifier of the created lead
            console.info('Lead created with ID ' + leadId)
            res.json({ message: 'Lead created', id: leadId })
        } catch (error) {
            // Network errors and SDK failures arrive as an exception
            console.error(error)
            res.status(502).json({ message: 'Could not create the lead, try again later' })
        }
    })

    // Run: node handler.mjs
    app.listen(3000)
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.3"
    // form.php and form.html are located in the public directory, vendor — above it
    // Local run: php -S localhost:3000 -t public
    require_once __DIR__ . '/../vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;

    header('Content-Type: application/json; charset=utf-8');

    // Value length limit: the form is public
    const MAX_LENGTH = 100;

    // The handler accepts POST requests only
    if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
        http_response_code(405);
        echo json_encode(['message' => 'Method not supported']);
        exit;
    }

    $sb = ServiceBuilderFactory::createServiceBuilderFromWebhook(getenv('B24_HOOK'));

    // Getting the data from the form
    $sName = trim((string)($_POST['NAME'] ?? ''));
    $sLastName = trim((string)($_POST['LAST_NAME'] ?? ''));
    $sCompanyTitle = trim((string)($_POST['COMPANY_TITLE'] ?? ''));
    $sPhone = trim((string)($_POST['PHONE'] ?? ''));
    $sEmail = trim((string)($_POST['EMAIL'] ?? ''));

    // Validating the data before calling the method
    if ($sName === '') {
        http_response_code(400);
        echo json_encode(['message' => 'Enter the first name']);
        exit;
    }

    if ($sEmail !== '' && !preg_match('/^[^@\s]+@[^@\s]+\.[^@\s]+$/u', $sEmail)) {
        http_response_code(400);
        echo json_encode(['message' => 'Check the email address']);
        exit;
    }

    foreach ([$sName, $sLastName, $sCompanyTitle, $sPhone, $sEmail] as $value) {
        if (mb_strlen($value) > MAX_LENGTH) {
            http_response_code(400);
            echo json_encode(['message' => 'One of the fields is too long']);
            exit;
        }
    }

    // Collecting the phone and email into multifields
    $arFm = [];

    if ($sPhone !== '') {
        $arFm[] = ['typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => $sPhone];
    }

    if ($sEmail !== '') {
        $arFm[] = ['typeId' => 'EMAIL', 'valueType' => 'HOME', 'value' => $sEmail];
    }

    // Creating the lead title from the first and last name
    $sTitle = 'From the website: ' . trim($sName . ' ' . $sLastName);
    // If there is a company name — add it via a dash after the first and last name
    if ($sCompanyTitle !== '') {
        $sTitle .= ' — ' . $sCompanyTitle;
    }

    // Sending the data to Bitrix24
    try {
        $result = $sb->getCRMScope()->item()->add(1, [ // 1 — the "Lead" CRM object type
            'title' => $sTitle, // Lead title
            'name' => $sName, // First name
            'lastName' => $sLastName, // Last name
            'companyTitle' => $sCompanyTitle, // Company name
            'fm' => $arFm, // Phone and email
        ]);

        $leadId = $result->item()->id; // Identifier of the created lead
        error_log('Lead created with ID ' . $leadId);
        echo json_encode(['message' => 'Lead created', 'id' => $leadId]);
    } catch (\Throwable $e) {
        // We write the error details to the log and do not show them to the visitor
        error_log($e->getMessage());
        http_response_code(502);
        echo json_encode(['message' => 'Could not create the lead, try again later']);
    }
    ```

- Python

    ```python
    # pip install flask b24pysdk
    import os
    import re

    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    # We place the form.html page in the static folder
    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],  # your-domain.bitrix24.com
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],  # user_id/token only, without https://
    ))

    # Pattern for validating the email address
    EMAIL_PATTERN = re.compile(r"[^@\s]+@[^@\s]+\.[^@\s]+")
    # Value length limit: the form is public
    MAX_LENGTH = 100


    @app.route("/form", methods=["POST"])
    def handle_form():
        # Getting the data from the form
        s_name = request.form.get("NAME", "").strip()
        s_last_name = request.form.get("LAST_NAME", "").strip()
        s_company_title = request.form.get("COMPANY_TITLE", "").strip()
        s_phone = request.form.get("PHONE", "").strip()
        s_email = request.form.get("EMAIL", "").strip()

        # Validating the data before calling the method
        if not s_name:
            return jsonify({"message": "Enter the first name"}), 400

        if s_email and not EMAIL_PATTERN.fullmatch(s_email):
            return jsonify({"message": "Check the email address"}), 400

        if any(len(value) > MAX_LENGTH for value in (s_name, s_last_name, s_company_title, s_phone, s_email)):
            return jsonify({"message": "One of the fields is too long"}), 400

        # Collecting the phone and email into multifields
        ar_fm = []

        if s_phone:
            ar_fm.append({"typeId": "PHONE", "valueType": "WORK", "value": s_phone})

        if s_email:
            ar_fm.append({"typeId": "EMAIL", "valueType": "HOME", "value": s_email})

        # Creating the lead title from the first and last name
        s_title = "From the website: " + f"{s_name} {s_last_name}".strip()
        # If there is a company name — add it via a dash after the first and last name
        if s_company_title:
            s_title += " — " + s_company_title

        # Sending the data to Bitrix24
        try:
            bitrix_response = client.crm.item.add(
                entity_type_id=1,  # CRM object type — lead
                fields={
                    "title": s_title,  # Lead title
                    "name": s_name,  # First name
                    "lastName": s_last_name,  # Last name
                    "companyTitle": s_company_title,  # Company name
                    "fm": ar_fm,  # Phone and email
                },
            ).response
            lead_id = bitrix_response.result["item"]["id"]  # Identifier of the created lead
            app.logger.info("Lead created with ID %s", lead_id)
            return jsonify({"message": "Lead created", "id": lead_id})
        except (BitrixAPIError, BitrixSDKException) as error:
            # We write the error details to the log and do not show them to the visitor
            app.logger.error(error)
            return jsonify({"message": "Could not create the lead, try again later"}), 502


    # Run: python handler.py
    if __name__ == "__main__":
        app.run(port=3000)
    ```

- Go

    ```go
    // Setup in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/' && go run .
    //
    // A separate form.html file is not needed: the page with the form is served by the same program,
    // open http://localhost:3000/. To check the result, run the same program with
    // an argument: go run . check 3465
    package main

    import (
    	"context"
    	"encoding/json"
    	"fmt"
    	"log"
    	"net/http"
    	"os"
    	"regexp"
    	"strconv"
    	"strings"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // entityTypeLead is the ID of the "lead" object type for the universal methods
    // crm.item.*.
    const entityTypeLead = 1

    // A length limit on the values: the form is public.
    const maxLength = 100

    // emailPattern validates an email address.
    var emailPattern = regexp.MustCompile(`^[^@\s]+@[^@\s]+\.[^@\s]+$`)

    func main() {
    	// To check the result: go run . check 3465
    	if len(os.Args) > 2 && os.Args[1] == "check" {
    		id, err := strconv.ParseInt(os.Args[2], 10, 64)
    		if err != nil {
    			log.Fatal("a numeric lead ID is required")
    		}
    		if err := check(context.Background(), b24.ID(id)); err != nil {
    			log.Fatal(err)
    		}
    		return
    	}
    	// The webhook path is a secret: it comes from the environment rather than from the code, and it
    	// never reaches the public page with the form. The client is built ONCE
    	// per portal and is reused by all requests — it holds the HTTP client and
    	// the authorization state, while http.Server calls the handler from many goroutines.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	mux := http.NewServeMux()
    	// The page with the form is served from here as well: then the form request goes to the same
    	// address it was opened from, and no static file setup is required.
    	mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    		w.Header().Set("Content-Type", "text/html; charset=utf-8")
    		fmt.Fprint(w, formPage)
    	})
    	mux.HandleFunc("/form", func(w http.ResponseWriter, r *http.Request) {
    		if r.Method != http.MethodPost {
    			reply(w, http.StatusMethodNotAllowed, "POST is required", 0)
    			return
    		}
    		handleForm(w, r, core)
    	})

    	log.Println("form and handler: http://localhost:3000/")
    	if err := http.ListenAndServe(":3000", mux); err != nil {
    		log.Fatal(err)
    	}
    }

    func handleForm(w http.ResponseWriter, r *http.Request, core *b24.Core) {
    	// The data comes from an anonymous visitor, so before the method is called
    	// the handler validates them.
    	if err := r.ParseForm(); err != nil {
    		reply(w, http.StatusBadRequest, "Failed to parse the form", 0)
    		return
    	}
    	// r.PostFormValue returns an empty string when the key is missing entirely, so
    	// there is nothing to fail on for a missing field.
    	name := strings.TrimSpace(r.PostFormValue("NAME"))
    	lastName := strings.TrimSpace(r.PostFormValue("LAST_NAME"))
    	companyTitle := strings.TrimSpace(r.PostFormValue("COMPANY_TITLE"))
    	phone := strings.TrimSpace(r.PostFormValue("PHONE"))
    	email := strings.TrimSpace(r.PostFormValue("EMAIL"))

    	if name == "" {
    		reply(w, http.StatusBadRequest, "Enter the first name", 0)
    		return
    	}
    	if email != "" && !emailPattern.MatchString(email) {
    		reply(w, http.StatusBadRequest, "Check the email address", 0)
    		return
    	}
    	for _, value := range []string{name, lastName, companyTitle, phone, email} {
    		// The length is counted in RUNES rather than bytes: in UTF-8 a non-Latin letter
    		// takes two bytes, and len() would reject a name twice as short.
    		if len([]rune(value)) > maxLength {
    			reply(w, http.StatusBadRequest, "One of the fields is too long", 0)
    			return
    		}
    	}
    	// In REST, values are sent as is: html.EscapeString and the like
    	// is needed when rendering to a page, while in CRM it turns "Weber & Son" into
    	// "Weber &amp; Son".
    	// The phone and the email go into the fm field — an array of crm_multifield objects.
    	// The universal crm.item.* methods accept keys in camelCase, whereas
    	// crm.lead.add would expect the same keys in UPPERCASE.
    	fm := make([]b24.Params, 0, 2)
    	if phone != "" {
    		fm = append(fm, b24.Params{"typeId": "PHONE", "valueType": "WORK", "value": phone})
    	}
    	if email != "" {
    		fm = append(fm, b24.Params{"typeId": "EMAIL", "valueType": "HOME", "value": email})
    	}
    	// The title is assembled from the first and last name, and the company name is appended after
    	// a dash — this way the manager sees in the lead list who the request came from.
    	title := "From the website: " + strings.TrimSpace(name+" "+lastName)
    	if companyTitle != "" {
    		title += " — " + companyTitle
    	}
    	res, err := core.Call(r.Context(), "crm.item.add", b24.Params{
    		"entityTypeId": entityTypeLead,
    		"fields": b24.Params{
    			"title":        title,
    			"name":         name,
    			"lastName":     lastName,
    			"companyTitle": companyTitle,
    			"fm":           fm,
    		},
    	}) // no WithIdempotent: a retry would create a second lead
    	if err != nil {
    		// The details go to the server log, and the visitor gets a generic
    		// message: technical details have no place on a public page.
    		// The webhook address will not appear in the error text — the SDK strips it itself.
    		log.Println("crm.item.add:", err)
    		reply(w, http.StatusBadGateway, "Could not create the lead, try again later", 0)
    		return
    	}

    	// The method wraps the response in an object with the item key.
    	raw, ok := b24.Unwrap(res.Result, "item", "id")
    	if !ok {
    		log.Println("no item.id in the response:", string(res.Result))
    		reply(w, http.StatusBadGateway, "Could not create the lead, try again later", 0)
    		return
    	}
    	var leadID b24.ID
    	if err := json.Unmarshal(raw, &leadID); err != nil {
    		log.Println("parse lead ID:", err)
    		reply(w, http.StatusBadGateway, "Could not create the lead, try again later", 0)
    		return
    	}

    	log.Printf("lead %d created", leadID)
    	reply(w, http.StatusOK, "Lead created", leadID)
    }

    // check displays the lead data by the ID from the handler response. The call
    // only reads — it does not change any data in CRM.
    func check(ctx context.Context, leadID b24.ID) error {
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	res, err := core.Call(ctx, "crm.item.get", b24.Params{
    		"entityTypeId": entityTypeLead,
    		"id":           leadID,
    	}, b24.WithIdempotent()) // a read: an ambiguous network failure can be retried
    	if err != nil {
    		return fmt.Errorf("crm.item.get: %w", err)
    	}

    	var out struct {
    		Item struct {
    			ID           b24.ID `json:"id"`
    			Title        string `json:"title"`
    			Name         string `json:"name"`
    			LastName     string `json:"lastName"`
    			CompanyTitle string `json:"companyTitle"`
    			FM           []struct {
    				TypeID string `json:"typeId"`
    				Value  string `json:"value"`
    			} `json:"fm"`
    		} `json:"item"`
    	}
    	if err := json.Unmarshal(res.Result, &out); err != nil {
    		return fmt.Errorf("parse lead: %w", err)
    	}

    	fmt.Printf("lead %d: %s\n", out.Item.ID, out.Item.Title)
    	for _, f := range out.Item.FM {
    		fmt.Printf("  %s: %s\n", f.TypeID, f.Value)
    	}
    	return nil
    }

    // reply answers the page with the same JSON as the handlers in other languages:
    // {"message": "...", "id": 3465}.
    func reply(w http.ResponseWriter, status int, message string, id b24.ID) {
    	w.Header().Set("Content-Type", "application/json; charset=utf-8")
    	w.WriteHeader(status)
    	body := map[string]any{"message": message}
    	if id != 0 {
    		body["id"] = id
    	}
    	_ = json.NewEncoder(w).Encode(body)
    }

    // formPage is the same form as in step 1, only served by the program rather than
    // stored as a separate file.
    const formPage = `<!doctype html>
    <meta charset="utf-8">
    <title>Request</title>
    <form method="post" action="/form">
      <p><label>First name*<br><input name="NAME" required maxlength="100"></label></p>
      <p><label>Last name<br><input name="LAST_NAME" maxlength="100"></label></p>
      <p><label>Company<br><input name="COMPANY_TITLE" maxlength="100"></label></p>
      <p><label>Phone<br><input name="PHONE" type="tel" maxlength="100"></label></p>
      <p><label>Email<br><input name="EMAIL" type="email" maxlength="100"></label></p>
      <p><button type="submit">Submit</button></p>
    </form>`
    ```

{% endlist %}

## Verify the Result

1. Submit the form. A message like "Lead created. ID: 3465" appears in the browser — the identifier will be needed in step 3

2. Open the CRM section and go to Leads. The new lead has the title "From the website: First Name Last Name", and if the visitor filled in the company name — "From the website: First Name Last Name — Company Name"

3. Request the lead data using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method. Pass `entityTypeId` set to `1` and the `id` from the handler response

Run the verification with a separate script: it does not depend on the handler and connects to Bitrix24 through the same webhook.

{% list tabs %}

- JS

    ```javascript
    // Save to the check.mjs file in the project directory next to node_modules
    // Run: B24_HOOK='https://your-domain.bitrix24.com/rest/1/TOKEN/' node check.mjs
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    const leadId = 3465 // Identifier from the handler response

    const response = await $b24.actions.v2.call.make({
        method: 'crm.item.get',
        params: { entityTypeId: 1, id: leadId },
        requestId: 'lead-get'
    })

    console.info(response.getData().result.item)
    ```

- PHP

    ```php
    <?php
    // Save to the check.php file in the project root next to the vendor directory
    // Run: B24_HOOK='https://your-domain.bitrix24.com/rest/1/TOKEN/' php check.php
    require_once __DIR__ . '/vendor/autoload.php';

    use Bitrix24\SDK\Services\ServiceBuilderFactory;

    $sb = ServiceBuilderFactory::createServiceBuilderFromWebhook(getenv('B24_HOOK'));
    $leadId = 3465; // Identifier from the handler response

    $result = $sb->getCRMScope()->item()->get(1, $leadId);

    print_r($result->item());
    ```

- Python

    ```python
    # Save to the check.py file in the project directory
    # Run: B24_DOMAIN='your-domain.bitrix24.com' B24_WEBHOOK_TOKEN='1/TOKEN' python check.py
    import os

    from b24pysdk import BitrixWebhook, Client

    client = Client(BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],
        webhook_token=os.environ["B24_WEBHOOK_TOKEN"],
    ))
    lead_id = 3465  # Identifier from the handler response

    bitrix_response = client.crm.item.get(entity_type_id=1, bitrix_id=lead_id).response

    print(bitrix_response.result["item"])
    ```

- Go

    ```go
    core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    res, err := core.Call(ctx, "crm.item.get", b24.Params{
    	"entityTypeId": entityTypeLead,
    	"id":           leadID,
    }, b24.WithIdempotent()) // a read: an ambiguous network failure can be retried
    if err != nil {
    	return fmt.Errorf("crm.item.get: %w", err)
    }

    var out struct {
    	Item struct {
    		ID           b24.ID `json:"id"`
    		Title        string `json:"title"`
    		Name         string `json:"name"`
    		LastName     string `json:"lastName"`
    		CompanyTitle string `json:"companyTitle"`
    		FM           []struct {
    			TypeID string `json:"typeId"`
    			Value  string `json:"value"`
    		} `json:"fm"`
    	} `json:"item"`
    }
    if err := json.Unmarshal(res.Result, &out); err != nil {
    	return fmt.Errorf("parse lead: %w", err)
    }

    fmt.Printf("lead %d: %s\n", out.Item.ID, out.Item.Title)
    for _, f := range out.Item.FM {
    	fmt.Printf("  %s: %s\n", f.TypeID, f.Value)
    }
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": {
        "item": {
            "id": 3465,
            "title": "From the website: Klaus Weber — Müller GmbH",
            "name": "Klaus",
            "lastName": "Weber",
            "companyTitle": "Müller GmbH",
            "hasPhone": "Y",
            "hasEmail": "Y",
            "fm": [
                {
                    "id": 11658,
                    "valueType": "WORK",
                    "value": "+49000000000",
                    "typeId": "PHONE"
                },
                {
                    "id": 11659,
                    "valueType": "HOME",
                    "value": "klaus@example.com",
                    "typeId": "EMAIL"
                }
            ]
        }
    }
}
```

The `name`, `lastName`, and `companyTitle` values match the form fields. The phone and email are returned in the `fm` array with the same `typeId` and `valueType` types that the handler passed. The `hasPhone` and `hasEmail` flags show that the lead has a phone and email filled in — they confirm that the multifields were saved.

## Errors and Diagnostics

If the method returns an error, check the request data.

#|
|| **Code** | **Reason and action** ||
|| `ACCESS_DENIED` | The user on whose behalf the webhook is created lacks the required permission: to add leads — for step 2, and to read leads — for the verification step. Check the permissions in the CRM settings ||
|| `NOT_FOUND` | At step 2 — an invalid `entityTypeId` was passed, a lead requires the value `1`. At the verification step — there is no object with such an `id`: substitute the identifier from the handler response ||
|| `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Invalid field value. Check that the field names are written in camelCase and that the multifields are passed in `fm` ||
|| `100` | A non-iterable value was passed to a multiple field. Make sure that `fm` is an array, even if it is empty ||
|| `NO_AUTH_FOUND` | Invalid webhook code. Check the value of the environment variable with the webhook URL ||
|| `insufficient_scope` | The webhook does not have the `crm` scope. Create the webhook again and select the required permission ||
|| `QUERY_LIMIT_EXCEEDED` | The request rate limit is exceeded. Repeat the call later ||
|#

The method checks the mandatory fields itself and returns an error if a field is empty. Look for the text of such an error in the handler log: only a generic message is sent to the visitor. The method silently ignores something else — an unknown field name: a typo in `fields` does not raise an error, and the value is not saved. That is why, after the first run, you should verify the saved data with the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method. The full list of method errors is provided on the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) page.

The handler makes a single call, so after fixing an error, submit the form again in full — no incomplete records remain in the CRM. The messages "Enter the first name", "Check the email address", and "One of the fields is too long" are returned by the handler itself with the status `400`, before contacting Bitrix24.

The method does not return errors that occur between the form and the handler — they are visible in the browser.

#|
|| **Symptom** | **Reason and action** ||
|| The handler exits immediately at startup or responds with an error to the very first request | The environment variables with the webhook data are not set: `B24Hook.fromWebhookUrl` and `os.environ` terminate with an error, while in PHP `getenv` returns `false` and the SDK throws the error. `fromWebhookUrl` also validates the URL: the protocol must be HTTPS, and the user identifier must be a number ||
|| The page is opened from the file system at an address like `file://` | There is nothing to resolve the relative path from `handlerUrl` against, and the request is not sent. Open the page at the application address from step 1 ||
|| A CORS error in the browser console | An absolute address of a different domain or port is substituted in `handlerUrl`. The browser will not let the page read the response, but the request is executed and the object is created in the CRM — check for duplicates. With the relative path from the example, this error does not occur ||
|| A 404 error in the browser console | An invalid address in the `handlerUrl` variable. For Node.js and Flask, it is `/form`, and for PHP — `form.php` ||
|| A `405` response with the message "Method not supported" | The `form.php` file is opened directly with a GET request. The handler accepts POST only — send the data with the form ||
|| The message "The server returned an unexpected response" | The handler responded with something other than JSON: it crashed before `header` or returned a web server error page. Check the log: `console.error` — in the Node.js output, `error_log` — in the PHP log, `app.logger.error` — in the Flask output ||
|#

## Key Considerations

- Each form submission creates a new lead. If a client contacts you again, duplicates appear. How to find them and link them to existing records is described in the tutorial [Add Duplicate Lead](./how-to-add-repeat-lead.md)

- A webhook grants access to the entire CRM. Call REST from the server only and do not pass the webhook URL to the browser

- The form is filled out by anonymous visitors. The handler from the example limits the value length, but protection against automated submissions must be added separately, for example a captcha

- When switching to a different CRM object type, it is not only `entityTypeId` that changes: each type has its own set of fields. Refer to the description of the `fields` parameter on the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) page

- The built-in servers from the examples — `app.listen`, `app.run`, `php -S` — are suitable for local verification of the scenario. Host the public page on a web server over HTTPS: the form collects the client's personal data

- The scenario uses the universal [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method. Development of the [crm.lead.add](../../../api-reference/crm/leads/crm-lead-add.md) method has stopped: it continues to work, but use `crm.item.add` in new integrations

## Continue Learning

- [{#T}](../../../api-reference/crm/universal/crm-item-add.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-get.md)
- [{#T}](./how-to-add-repeat-lead.md)
- [{#T}](./how-to-add-lead-with-files.md)
- [{#T}](./how-to-add-contact.md)
- [{#T}](./how-to-add-company.md)
