# Add a Company with Requisites via Web Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed permissions is required — "Add|Import" for companies
>
> - [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) — any user
> - [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) — a user with permission to read contacts and companies
> - [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) — a user with the "Add|Import" permission for companies
> - [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) and [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) — a user with permission to add the company that owns the requisite

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

You can place a form on your website to collect client data and requisites. When a client fills out the form, the data is sent to a handler. The handler script creates objects in the CRM via the REST API.

As a result of the scenario, three linked objects appear in the CRM: a company, its requisite, and the requisite address.

The setup consists of two stages.

1. Prepare the fields and place the web form on the page. The set of form fields is taken from the [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) and [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) methods

2. Create a handler file that sequentially calls the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md), [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md), and [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) methods

The order of the calls is set by the links between the objects: the requisite is created for an existing company, and the address for an existing requisite.

## Before You Start

- At least one requisite template is configured in Bitrix24. If there are no templates, the [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) method returns an empty list and there is nothing to build the form from

- The webhook is created on behalf of a user who has the "Add|Import" permission for companies

- You have a server that serves the page with the form and accepts the form data using the `POST` method. In the examples, this is Express for JS, a PHP script, Flask for Python, and `net/http` for Go

- The webhook URL is stored in the environment, not in the page code. The page with the form is public, and the secret must not end up in it

## 1. Create the Web Form

To generate the fields, we use two methods:

- [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) — retrieves a list of address fields. Save the result in the `$arAddressFields` array

- [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) — retrieves a list of requisite templates based on the `ID` and `NAME` fields. Save the result in the `$arPresets` array

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```javascript
    const arAddressFields = (await $b24.actions.v2.call.make({
        method: 'crm.address.fields', params: {}, requestId: 'address-fields'
    })).getData().result
    const arPresets = (await $b24.actions.v2.call.make({
        method: 'crm.requisite.preset.list', params: { select: ['ID', 'NAME'] }, requestId: 'preset-list'
    })).getData().result
    ```

- Python

    ```python
    ar_address_fields = client.crm.address.fields().result
    ar_presets = client.crm.requisite.preset.list(select=["ID", "NAME"]).result
    ```

- PHP

    ```php
    $arAddressFields = $sb->getCRMScope()->address()->fields()->getFieldsDescription();
    $arPresets = $sb->getCRMScope()->requisitePreset()->list(
        order: [], filter: [], select: ["ID", "NAME"]
    )->getRequisitePresets();
    ```

- Go

    ```go
    res, err := core.Call(ctx, "crm.address.fields", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.address.fields: %w", err)
    }

    // The response is not a list but an object "field name -> description", hence a map.
    var addressFields map[string]struct {
    	Type       string `json:"type"`
    	Title      string `json:"title"`
    	IsReadOnly bool   `json:"isReadOnly"`
    }
    if err := json.Unmarshal(res.Result, &addressFields); err != nil {
    	return fmt.Errorf("parsing address fields: %w", err)
    }

    res, err = core.Call(ctx, "crm.requisite.preset.list", b24.Params{
    	"select": []string{"ID", "NAME"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.requisite.preset.list: %w", err)
    }

    // Here the identifier arrives as a STRING ("1"), whereas crm.enum.* returns
    // numbers. b24.ID parses both notations.
    var presets []struct {
    	ID   b24.ID `json:"ID"`
    	Name string `json:"NAME"`
    }
    if err := json.Unmarshal(res.Result, &presets); err != nil {
    	return fmt.Errorf("parsing requisite templates: %w", err)
    }
    if len(presets) == 0 {
    	return fmt.Errorf("there are no requisite templates in Bitrix24")
    }
    ```

{% endlist %}

The [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) method returns an array of objects, not identifier-name pairs. For the drop-down list, iterate over this array and take `ID` and `NAME` from each object.

```json
{
    "result": [
        { "ID": "1", "NAME": "Organization" },
        { "ID": "3", "NAME": "Sole Proprietorship" },
        { "ID": "5", "NAME": "Individual" }
    ]
}
```

The [crm.address.fields](../../../api-reference/crm/requisites/addresses/crm-address-fields.md) method returns an object where the key is the field code and the value is its description with the `isRequired` mandatory flag and the `title` name.

```json
{
    "result": {
        "TYPE_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "TYPE_ID"
        },
        "ADDRESS_1": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Street, house, building, structure"
        },
        "CITY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "City"
        }
    }
}
```

Remove unnecessary address fields from the `$arAddressFields` array so they are not displayed in the form. Three of them — `TYPE_ID`, `ENTITY_TYPE_ID`, and `ENTITY_ID` — are mandatory system fields that the client does not fill in; the handler substitutes them itself.

{% list tabs %}

- JS

    ```javascript
    for (const f of ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID']) {
        delete arAddressFields[f]
    }
    ```

- Python

    ```python
    for f in ("TYPE_ID", "ENTITY_TYPE_ID", "ENTITY_ID", "COUNTRY_CODE", "ANCHOR_TYPE_ID", "ANCHOR_ID"):
        ar_address_fields.pop(f, None)
    ```

- PHP

    ```php
    foreach (['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID'] as $field) {
        unset($arAddressFields[$field]);
    }
    ```

- Go

    ```go
    // The form takes only the string fields that are available for writing: TYPE_ID,
    // ENTITY_ID, and ENTITY_TYPE_ID also arrive in this response, but the handler
    // substitutes them itself. Map keys in Go are unordered — sort them, otherwise
    // the form fields will jump around from run to run.
    var addressNames []string
    for name, f := range addressFields {
    	if f.Type == "string" && !f.IsReadOnly {
    		addressNames = append(addressNames, name)
    	}
    }
    sort.Strings(addressNames)
    ```

{% endlist %}

Create an HTML form with the following fields:

- `REQ_TYPE` — a drop-down list with requisite templates from the `$arPresets` array. Mandatory field

- `TITLE` — company name. Mandatory field

- `INN` — company Taxpayer ID

- `PHONE` — phone number

- `ADDRESS` — address fields are created dynamically from `$arAddressFields`. If the field is mandatory, add the attribute `required`

The form collects the data and sends it to the handler using the `POST` method. The form markup is shown below — the requisite drop-down list and the address fields are populated from the retrieved data.

{% list tabs %}

- JS

    ```javascript
    // assemble the form string from the received data and insert it into the server response
    const options = arPresets.map(p => `<option value="${p.ID}">${p.NAME}</option>`).join('')
    const addressInputs = Object.entries(arAddressFields).map(([key, field]) =>
        `<input type="text" name="ADDRESS[${key}]" placeholder="${field.title}" ${field.isRequired ? 'required' : ''}>`
    ).join('')

    const formHtml = `
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select a requisite type</option>
                ${options}
            </select>
            <input type="text" name="TITLE" placeholder="Org name" required>
            <input type="text" name="INN" placeholder="INN">
            <input type="text" name="PHONE" placeholder="Phone">
            ${addressInputs}
            <input type="submit" value="Submit">
        </form>`
    ```

- Python

    ```python
    # assemble the form string from the received data and insert it into the server response
    from markupsafe import escape

    options = "".join(
        f'<option value="{escape(preset["ID"])}">{escape(preset["NAME"])}</option>'
        for preset in ar_presets
    )
    address_inputs = "".join(
        f'<input type="text" name="ADDRESS[{escape(key)}]" '
        f'placeholder="{escape(field["title"])}" '
        f'{"required" if field["isRequired"] else ""}>'
        for key, field in ar_address_fields.items()
    )

    form_html = f"""
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select a requisite type</option>
                {options}
            </select>
            <input type="text" name="TITLE" placeholder="Org name" required>
            <input type="text" name="INN" placeholder="INN">
            <input type="text" name="PHONE" placeholder="Phone">
            {address_inputs}
            <input type="submit" value="Submit">
        </form>"""
    ```

- PHP

    ```html
    <form id="form_to_crm">
        <select name="REQ_TYPE" required>
            <option value="" disabled selected>Select a requisite type</option>
            <?php foreach($arPresets as $preset):?>
                <option value="<?=$preset->ID?>"><?=$preset->NAME?></option>
            <?php endforeach;?>
        </select>
        <input type="text" name="TITLE" placeholder="Org name" required>
        <input type="text" name="INN" placeholder="INN">
        <input type="text" name="PHONE" placeholder="Phone">
        <?php if(is_array($arAddressFields)):?>
            <?php foreach($arAddressFields as $key=>$arField):?>
                <input type="text" name="ADDRESS[<?=$key?>]" placeholder="<?=$arField['title']?>" <?=($arField['isRequired'])?'required':'';?>>
            <?php endforeach;?>
        <?php endif;?>
        <input type="submit" value="Submit">
    </form>
    ```

- Go

    ```go
    var form strings.Builder
    form.WriteString(`<!doctype html>
    <meta charset="utf-8">
    <title>Request</title>
    <form method="post" action="/form">
    <p><label>Requisite type*<br><select name="REQ_TYPE" required>`)
    for _, p := range presets {
    	fmt.Fprintf(&form, `<option value="%d">%s</option>`, p.ID, html.EscapeString(p.Name))
    }
    form.WriteString(`</select></label></p>
    <p><label>Org name*<br><input name="TITLE" required></label></p>
    <p><label>INN<br><input name="INN"></label></p>
    <p><label>Phone<br><input name="PHONE" type="tel"></label></p>`)
    // The address fields are created dynamically: their set is defined by Bitrix24,
    // not by the code. Names such as ADDRESS[CITY] — the handler parses them back.
    for _, name := range addressNames {
    	fmt.Fprintf(&form, "<p><label>%s<br><input name=\"ADDRESS[%s]\"></label></p>\n",
    		html.EscapeString(addressFields[name].Title), name)
    }
    form.WriteString(`<p><button type="submit">Submit</button></p>
    </form>`)
    page := form.String()
    ```

{% endlist %}

### Full Code Example of the Form Page

{% list tabs %}

- JS

    ```javascript
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const app = express()

    // Form page: retrieve data from Bitrix24 and render HTML
    app.get('/', async (req, res) => {
        const arAddressFields = (await $b24.actions.v2.call.make({
            method: 'crm.address.fields', params: {}, requestId: 'address-fields'
        })).getData().result
        const arPresets = (await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.list', params: { select: ['ID', 'NAME'] }, requestId: 'preset-list'
        })).getData().result

        if (!arPresets.length) {
            res.send('<p>No requisite types available.</p>')
            return
        }

        // Remove system and unused address fields
        for (const f of ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID']) {
            delete arAddressFields[f]
        }

        // Assemble the requisite drop-down list and the address fields
        const options = arPresets.map(p => `<option value="${p.ID}">${p.NAME}</option>`).join('')
        const addressInputs = Object.entries(arAddressFields).map(([key, field]) =>
            `<input type="text" name="ADDRESS[${key}]" placeholder="${field.title}" ${field.isRequired ? 'required' : ''}>`
        ).join('')

        res.send(`
            <form id="form_to_crm">
                <select name="REQ_TYPE" required>
                    <option value="" disabled selected>Select a requisite type</option>
                    ${options}
                </select>
                <input type="text" name="TITLE" placeholder="Org name" required>
                <input type="text" name="INN" placeholder="INN">
                <input type="text" name="PHONE" placeholder="Phone">
                ${addressInputs}
                <input type="submit" value="Submit">
            </form>
            <script>
                document.getElementById('form_to_crm').addEventListener('submit', async (el) => {
                    el.preventDefault()
                    const formData = Object.fromEntries(new FormData(el.currentTarget).entries())
                    const response = await fetch('/form', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(formData),
                    })
                    alert((await response.json()).message)
                })
            <\/script>
        `)
    })

    app.listen(3000)
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import os

    from flask import Flask
    from markupsafe import escape
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],  # your-domain.bitrix24.com
        webhook_token=os.environ["B24_TOKEN"],  # user_id/token only, without https://
    ))

    # Page template: %(options)s and %(address_inputs)s are substituted from Python
    PAGE = """
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select a requisite type</option>
                %(options)s
            </select>
            <input type="text" name="TITLE" placeholder="Org name" required>
            <input type="text" name="INN" placeholder="INN">
            <input type="text" name="PHONE" placeholder="Phone">
            %(address_inputs)s
            <input type="submit" value="Submit">
        </form>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
        <script>
        $(document).ready(function() {
            $('#form_to_crm').on('submit', function(el) {
                el.preventDefault();
                $.ajax({
                    method: 'POST', dataType: 'json', url: '/form',
                    data: $(this).serialize(),
                    success: function(data) { alert(data.message); }
                });
            });
        });
        </script>
    """

    EMPTY_PAGE = "<p>No requisite types available.</p>"


    @app.route("/")
    def form_page():
        # Retrieve the list of address fields and requisite templates
        ar_address_fields = client.crm.address.fields().result
        ar_presets = client.crm.requisite.preset.list(select=["ID", "NAME"]).result

        if not ar_presets:
            return EMPTY_PAGE

        # Remove system and unused address fields
        for f in ("TYPE_ID", "ENTITY_TYPE_ID", "ENTITY_ID", "COUNTRY_CODE", "ANCHOR_TYPE_ID", "ANCHOR_ID"):
            ar_address_fields.pop(f, None)

        # Assemble the requisite drop-down list and the address fields
        options = "".join(
            f'<option value="{escape(preset["ID"])}">{escape(preset["NAME"])}</option>'
            for preset in ar_presets
        )
        address_inputs = "".join(
            f'<input type="text" name="ADDRESS[{escape(key)}]" '
            f'placeholder="{escape(field["title"])}" '
            f'{"required" if field["isRequired"] else ""}>'
            for key, field in ar_address_fields.items()
        )

        return PAGE % {"options": options, "address_inputs": address_inputs}
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

    // Retrieve the list of address fields and requisite templates
    $arAddressFields = $sb->getCRMScope()->address()->fields()->getFieldsDescription();
    $arPresets = $sb->getCRMScope()->requisitePreset()->list(
        order: [], filter: [], select: ["ID", "NAME"]
    )->getRequisitePresets();

    if(!empty($arPresets)):
        // Remove system and unused address fields
        $excludeFields = ['TYPE_ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'COUNTRY_CODE', 'ANCHOR_TYPE_ID', 'ANCHOR_ID'];
        foreach($excludeFields as $field) {
            unset($arAddressFields[$field]);
        }
    ?>
        <form id="form_to_crm">
            <select name="REQ_TYPE" required>
                <option value="" disabled selected>Select a requisite type</option>
                <?php foreach($arPresets as $preset): ?>
                    <option value="<?=$preset->ID?>"><?=$preset->NAME?></option>
                <?php endforeach; ?>
            </select>
            <input type="text" name="TITLE" placeholder="Org name" required>
            <input type="text" name="INN" placeholder="INN">
            <input type="text" name="PHONE" placeholder="Phone">
            <?php foreach($arAddressFields as $key => $arField): ?>
                <input type="text" name="ADDRESS[<?=$key?>]"
                       placeholder="<?=$arField['title']?>"
                       <?=$arField['isRequired'] ? 'required' : ''?>>
            <?php endforeach; ?>
            <input type="submit" value="Submit">
        </form>
    <?php else: ?>
        <p>No requisite types available.</p>
    <?php endif; ?>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
    $(document).ready(function() {
        $('#form_to_crm').on('submit', function(el) {
            el.preventDefault();
            $.ajax({
                method: 'POST',
                dataType: 'json',
                url: 'form.php', // handler file from step 2
                data: $(this).serialize(),
                success: function(data) {
                    alert(data.message);
                }
            });
        });
    });
    </script>
    ```

- Go

    ```go
    // The full code of the page and the handler is in the example below, in step 2:
    // the same program assembles and serves the page, there is no separate file for
    // the form.
    mux := http.NewServeMux()
    mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    	w.Header().Set("Content-Type", "text/html; charset=utf-8")
    	fmt.Fprint(w, page)
    })
    mux.HandleFunc("/form", func(w http.ResponseWriter, r *http.Request) {
    	if r.Method != http.MethodPost {
    		reply(w, http.StatusMethodNotAllowed, "POST required", 0)
    		return
    	}
    	handleForm(w, r, core)
    })

    log.Println("form and handler: http://localhost:3000/")
    return http.ListenAndServe(":3000", mux)
    ```

{% endlist %}

## 2. Create a Form Handler

Create a file that accepts the form data and retains it in the CRM. In the PHP examples this is `form.php`; in the others it is the `/form` route handler.

### Retrieve Data

Retrieve and process the data from the form.

{% list tabs %}

- JS

    ```javascript
    const iRequisitePresetID = parseInt(req.body.REQ_TYPE, 10)
    const sTitle = String(req.body.TITLE ?? '')
    const sINN = String(req.body.INN ?? '')
    const sPhone = String(req.body.PHONE ?? '')
    const arAddress = {}
    for (const [key, val] of Object.entries(req.body.ADDRESS ?? {})) {
        arAddress[key] = String(val)
    }
    ```

- Python

    ```python
    i_requisite_preset_id = int(request.form.get("REQ_TYPE", 0))
    s_title = request.form.get("TITLE", "")
    s_inn = request.form.get("INN", "")
    s_phone = request.form.get("PHONE", "")
    ar_address = {k[len("ADDRESS["):-1]: v for k, v in request.form.to_dict().items()
                  if k.startswith("ADDRESS[")}
    ```

- PHP

    ```php
    $iRequisitePresetID = intval($_POST["REQ_TYPE"] ?? 0);
    $sTitle = htmlspecialchars($_POST["TITLE"] ?? '');
    $sINN = htmlspecialchars($_POST["INN"] ?? '');
    $sPhone = htmlspecialchars($_POST["PHONE"] ?? '');
    $arAddress = [];
    foreach (($_POST["ADDRESS"] ?? []) as $key => $val) {
        $arAddress[$key] = htmlspecialchars($val);
    }
    ```

- Go

    ```go
    // The requisite type is converted to a number, the rest is cleared of HTML tags.
    // The tags are CUT OUT rather than escaped: escaping is needed when rendering to
    // the page, and because of it the CRM would receive "Weber &amp; Son" instead of
    // "Weber & Son".
    presetID, _ := strconv.Atoi(r.PostFormValue("REQ_TYPE"))
    title := stripTags(r.PostFormValue("TITLE"))
    inn := stripTags(r.PostFormValue("INN"))
    phone := stripTags(r.PostFormValue("PHONE"))

    if presetID == 0 || title == "" {
    	reply(w, http.StatusBadRequest, "Fill in the requisite type and the name", 0)
    	return
    }

    // The address fields arrived with names such as ADDRESS[CITY] — parse them back.
    address := b24.Params{}
    for key, values := range r.PostForm {
    	if inner, ok := addressKey(key); ok && len(values) > 0 && values[0] != "" {
    		address[inner] = stripTags(values[0])
    	}
    }
    ```

{% endlist %}

- `$iRequisitePresetID` — convert the requisite template identifier `REQ_TYPE` to an integer

- `$sTitle`, `$sINN`, `$sPhone` — safely process the data from `TITLE`, `INN`, `PHONE` to prevent XSS attacks

- `$arAddress` — save the data from the array containing address fields `ADDRESS`

### Prepare Data

Add two mandatory system fields to the `$arAddress` array.

- `TYPE_ID` — address type. We will specify `1` — actual address. You can retrieve the list of address types using the [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md) method

- `ENTITY_TYPE_ID` — [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We pass `8` — requisite. You can retrieve the full list of object types using the [crm.enum.ownertype](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md) method

The third mandatory field, `ENTITY_ID`, is substituted later: it is the requisite identifier, and the requisite does not exist yet.

{% list tabs %}

- JS

    ```javascript
    arAddress.TYPE_ID = 1
    arAddress.ENTITY_TYPE_ID = 8
    ```

- Python

    ```python
    ar_address["TYPE_ID"] = 1
    ar_address["ENTITY_TYPE_ID"] = 8
    ```

- PHP

    ```php
    $arAddress['TYPE_ID'] = 1;
    $arAddress['ENTITY_TYPE_ID'] = 8;
    ```

- Go

    ```go
    // The handler substitutes the address type and the owner type itself: they are not in the form.
    address["TYPE_ID"] = addressTypeActual
    address["ENTITY_TYPE_ID"] = typeRequisite
    ```

{% endlist %}

The system retains the phone number as a [crm_multifield](../../../api-reference/crm/data-types.md#crm_multifield) array of objects, so the `$sPhone` value must be converted to an array format:

- in the first item `VALUE`, we write `$sPhone`

- in the second item `VALUE_TYPE`, we pass, for example, `WORK`

If the `$sPhone` variable has no value, specify an empty array.

{% list tabs %}

- JS

    ```javascript
    const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []
    ```

- Python

    ```python
    ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []
    ```

- PHP

    ```php
    $arPhone = !empty($sPhone) ? [['VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK']] : [];
    ```

- Go

    ```go
    // The phone number is retained as a multifield — a list of objects, even when
    // there is a single number. A row WITHOUT an ID adds a value; MultifieldAdd
    // assembles it for you.
    phones := []map[string]any{}
    if phone != "" {
    	phones = append(phones, b24.MultifieldAdd(phone, "WORK"))
    }
    ```

{% endlist %}

### Add a Company

To add a company, use the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method. You must pass the following data to it:

- `TITLE` — company name. We pass `$sTitle`, which was retrieved from the form

- `COMPANY_TYPE` — company type. We will specify `CUSTOMER` — customer. You can retrieve the list of types using the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method with the `'filter'=>['ENTITY_ID'=>'COMPANY_TYPE']` filter

- `PHONE` — an array containing the phone number `$arPhone` retrieved from the form

{% note warning "" %}

Check which mandatory fields are configured for companies in your Bitrix24. All mandatory fields must be passed to the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) method.

{% endnote %}

{% list tabs %}

- JS

    ```javascript
    const companyResponse = await $b24.actions.v2.call.make({
        method: 'crm.company.add',
        params: { fields: { TITLE: sTitle, COMPANY_TYPE: 'CUSTOMER', PHONE: arPhone } },
        requestId: 'company-add'
    })
    const iCompanyID = companyResponse.getData()?.result
    ```

- Python

    ```python
    i_company_id = client.crm.company.add(fields={
        "TITLE": s_title,
        "COMPANY_TYPE": "CUSTOMER",
        "PHONE": ar_phone,
    }).result
    ```

- PHP

    ```php
    $iCompanyID = $sb->getCRMScope()->company()->add([
        'TITLE' => $sTitle,
        'COMPANY_TYPE' => 'CUSTOMER',
        'PHONE' => $arPhone,
    ])->getId();
    ```

- Go

    ```go
    res, err := core.Call(ctx, "crm.company.add", b24.Params{
    	"fields": b24.Params{
    		"TITLE":        title,
    		"COMPANY_TYPE": "CUSTOMER",
    		"PHONE":        phones,
    	},
    }) // without WithIdempotent: a retry would create a second company
    if err != nil {
    	// The details go to the server log, they are not shown to the visitor.
    	log.Println("crm.company.add:", err)
    	reply(w, http.StatusBadGateway, "Failed to create the company", 0)
    	return
    }

    // There is no wrapper: result is the identifier of the new company right away.
    var companyID b24.ID
    if err := json.Unmarshal(res.Result, &companyID); err != nil {
    	log.Println("parsing the company identifier:", err)
    	reply(w, http.StatusBadGateway, "Failed to create the company", 0)
    	return
    }
    ```

{% endlist %}

If the company is successfully created, the method returns its identifier in `$iCompanyID`. Retain the value: the requisite needs it.

```json
{
    "result": 5
}
```

### Add Requisites to the Company

To add requisites, use the [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) method. You must pass the following data to it:

- `ENTITY_TYPE_ID` — [CRM object type identifier](../../../api-reference/crm/data-types.md#object_type). We pass `4` — company

- `ENTITY_ID` — company identifier. We pass `$iCompanyID`, which was obtained during company creation

- `PRESET_ID` — requisite template identifier. We specify `$iRequisitePresetID`, which was retrieved from the form

- `NAME` — requisite name. We pass `$sTitle`, which was retrieved from the form

- `RQ_INN` — company Taxpayer ID. We pass `$sINN`, which was retrieved from the form

- `ACTIVE` — an activity flag; we will specify `Y`

{% list tabs %}

- JS

    ```javascript
    const requisiteResponse = await $b24.actions.v2.call.make({
        method: 'crm.requisite.add',
        params: {
            fields: {
                ENTITY_TYPE_ID: 4,
                ENTITY_ID: iCompanyID,
                PRESET_ID: iRequisitePresetID,
                ACTIVE: 'Y',
                NAME: sTitle,
                RQ_INN: sINN,
            }
        },
        requestId: 'requisite-add'
    })
    const iRequisiteID = requisiteResponse.getData()?.result
    ```

- Python

    ```python
    i_requisite_id = client.crm.requisite.add(fields={
        "ENTITY_TYPE_ID": 4,
        "ENTITY_ID": i_company_id,
        "PRESET_ID": i_requisite_preset_id,
        "ACTIVE": "Y",
        "NAME": s_title,
        "RQ_INN": s_inn,
    }).result
    ```

- PHP

    ```php
    $iRequisiteID = $sb->getCRMScope()->requisite()->add(
        entityId: $iCompanyID,
        entityTypeId: 4,
        requisitePresetId: $iRequisitePresetID,
        requisiteName: $sTitle,
        fields: ['ACTIVE' => 'Y', 'RQ_INN' => $sINN]
    )->getId();
    ```

- Go

    ```go
    res, err = core.Call(ctx, "crm.requisite.add", b24.Params{
    	"fields": b24.Params{
    		"ENTITY_TYPE_ID": typeCompany,
    		"ENTITY_ID":      companyID,
    		"PRESET_ID":      presetID,
    		"ACTIVE":         "Y",
    		"NAME":           title,
    		"RQ_INN":         inn,
    	},
    })
    if err != nil {
    	// The company has already been created, so this is no reason to answer
    	// "nothing worked": report that the requisites were not added and return the
    	// identifier.
    	log.Println("crm.requisite.add:", err)
    	reply(w, http.StatusOK, "The company was created, the requisites could not be added", companyID)
    	return
    }
    var requisiteID b24.ID
    if err := json.Unmarshal(res.Result, &requisiteID); err != nil {
    	log.Println("parsing the requisite identifier:", err)
    	reply(w, http.StatusOK, "The company was created, the requisites could not be added", companyID)
    	return
    }
    ```

{% endlist %}

If the requisites are successfully added, the method returns the record identifier in `$iRequisiteID`.

```json
{
    "result": 27
}
```

{% note warning "" %}

The method does not check whether a template with the passed `PRESET_ID` exists. With a nonexistent identifier, the requisite is still created but remains without the template fields. Take `PRESET_ID` from the [crm.requisite.preset.list](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md) response instead of substituting an arbitrary number.

{% endnote %}

### Add an Address to the Requisite

1. Add the `ENTITY_ID` field — the requisite identifier — to the `$arAddress` array. Pass the `$iRequisiteID` obtained during the creation of the requisite

   {% list tabs %}

   - JS

       ```javascript
       arAddress.ENTITY_ID = iRequisiteID
       ```

   - Python

       ```python
       ar_address["ENTITY_ID"] = i_requisite_id
       ```

   - PHP

       ```php
       $arAddress['ENTITY_ID'] = $iRequisiteID;
       ```

   - Go

       ```go
       address["ENTITY_ID"] = requisiteID
       ```

   {% endlist %}

2. Use the [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) method. You must pass the `$arAddress` array to it

   {% list tabs %}

   - JS

       ```javascript
       const bAddressAdded = (await $b24.actions.v2.call.make({
           method: 'crm.address.add', params: { fields: arAddress }, requestId: 'address-add'
       })).getData().result
       ```

   - Python

       ```python
       b_address_added = client.crm.address.add(fields=ar_address).result
       ```

   - PHP

       ```php
       $bAddressAdded = $sb->getCRMScope()->address()->add($arAddress)->isSuccess();
       ```

   - Go

       ```go
       // The address is linked to the REQUISITE, not to the company, so ENTITY_ID
       // is filled in only now — the requisite identifier did not exist earlier.
       if _, err := core.Call(ctx, "crm.address.add", b24.Params{"fields": address}); err != nil {
       	log.Println("crm.address.add:", err)
       	reply(w, http.StatusOK, "The company and the requisites were created, the address could not be added", companyID)
       	return
       }
       ```

   {% endlist %}

The method returns one of the following values in the `$bAddressAdded` variable:

- `true` — the address was added

- `false` — the address was not added

```json
{
    "result": true
}
```

### Full Handler Code Example

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    export async function handler(req, res) {
        const iRequisitePresetID = parseInt(req.body.REQ_TYPE, 10)
        const sTitle = String(req.body.TITLE ?? '')
        const sINN = String(req.body.INN ?? '')
        const sPhone = String(req.body.PHONE ?? '')

        const arAddress = {}
        for (const [key, val] of Object.entries(req.body.ADDRESS ?? {})) {
            arAddress[key] = String(val)
        }
        arAddress.TYPE_ID = 1 // 1 — actual address (crm.enum.addresstype)
        arAddress.ENTITY_TYPE_ID = 8 // 8 — requisite (crm.enum.ownertype)

        const arPhone = sPhone ? [{ VALUE: sPhone, VALUE_TYPE: 'WORK' }] : []

        try {
            const companyResponse = await $b24.actions.v2.call.make({
                method: 'crm.company.add',
                params: { fields: { TITLE: sTitle, COMPANY_TYPE: 'CUSTOMER', PHONE: arPhone } },
                requestId: 'company-add'
            })
            const iCompanyID = companyResponse.getData()?.result
            if (!iCompanyID) {
                res.json({ message: 'Error: ' + companyResponse.getErrorMessages().join('; ') })
                return
            }

            const requisiteResponse = await $b24.actions.v2.call.make({
                method: 'crm.requisite.add',
                params: {
                    fields: {
                        ENTITY_TYPE_ID: 4, // 4 — company (crm.enum.ownertype)
                        ENTITY_ID: iCompanyID,
                        PRESET_ID: iRequisitePresetID,
                        ACTIVE: 'Y',
                        NAME: sTitle,
                        RQ_INN: sINN,
                    }
                },
                requestId: 'requisite-add'
            })
            const iRequisiteID = requisiteResponse.getData()?.result

            if (iRequisiteID) {
                arAddress.ENTITY_ID = iRequisiteID
                await $b24.actions.v2.call.make({
                    method: 'crm.address.add', params: { fields: arAddress }, requestId: 'address-add'
                })
            }

            res.json({ message: 'The company has been added' })
        } catch (e) {
            res.json({ message: 'Error: ' + e.message })
        }
    }

    // Attach the handler to the server from step 1. Without express.json()
    // the request body is not parsed and req.body stays empty
    // app.use(express.json())
    // app.post('/form', handler)
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import os

    from flask import Flask, request, jsonify
    from b24pysdk import BitrixWebhook, Client

    app = Flask(__name__)

    client = Client(BitrixWebhook(
        domain=os.environ["B24_DOMAIN"],  # your-domain.bitrix24.com
        webhook_token=os.environ["B24_TOKEN"],  # user_id/token only, without https://
    ))


    @app.route("/form", methods=["POST"])
    def handle_form():
        # Retrieve and sanitize the form data
        i_requisite_preset_id = int(request.form.get("REQ_TYPE", 0))
        s_title = request.form.get("TITLE", "")
        s_inn = request.form.get("INN", "")
        s_phone = request.form.get("PHONE", "")

        # Prepare the address
        ar_address = {k[len("ADDRESS["):-1]: v for k, v in request.form.to_dict().items()
                      if k.startswith("ADDRESS[")}
        ar_address["TYPE_ID"] = 1  # 1 — actual address (crm.enum.addresstype)
        ar_address["ENTITY_TYPE_ID"] = 8  # 8 — requisite (crm.enum.ownertype)

        # Format the phone number into the crm_multifield format
        ar_phone = [{"VALUE": s_phone, "VALUE_TYPE": "WORK"}] if s_phone else []

        try:
            i_company_id = client.crm.company.add(fields={
                "TITLE": s_title,
                "COMPANY_TYPE": "CUSTOMER",  # client (crm.status.list ENTITY_ID=COMPANY_TYPE)
                "PHONE": ar_phone,
            }).result

            i_requisite_id = client.crm.requisite.add(fields={
                "ENTITY_TYPE_ID": 4,  # 4 — company (crm.enum.ownertype)
                "ENTITY_ID": i_company_id,
                "PRESET_ID": i_requisite_preset_id,
                "ACTIVE": "Y",
                "NAME": s_title,
                "RQ_INN": s_inn,
            }).result

            if i_requisite_id:
                ar_address["ENTITY_ID"] = i_requisite_id
                client.crm.address.add(fields=ar_address)

            return jsonify({"message": "The company has been added"})
        except Exception as e:
            return jsonify({"message": f"Error: {e}"})
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

    // Retrieve and sanitize the form data
    $iRequisitePresetID = intval($_POST["REQ_TYPE"] ?? 0);
    $sTitle = htmlspecialchars($_POST["TITLE"] ?? '');
    $sINN = htmlspecialchars($_POST["INN"] ?? '');
    $sPhone = htmlspecialchars($_POST["PHONE"] ?? '');

    // Prepare the address
    $arAddress = [];
    foreach(($_POST["ADDRESS"] ?? []) as $key => $val) {
        $arAddress[$key] = htmlspecialchars($val);
    }
    $arAddress['TYPE_ID'] = 1; // 1 — actual address (crm.enum.addresstype)
    $arAddress['ENTITY_TYPE_ID'] = 8; // 8 — requisite (crm.enum.ownertype)

    // Format the phone number into the crm_multifield format
    $arPhone = !empty($sPhone) ? [['VALUE' => $sPhone, 'VALUE_TYPE' => 'WORK']] : [];

    try {
        $iCompanyID = $crm->company()->add([
            'TITLE' => $sTitle,
            'COMPANY_TYPE' => 'CUSTOMER', // client (crm.status.list ENTITY_ID=COMPANY_TYPE)
            'PHONE' => $arPhone,
        ])->getId();

        $iRequisiteID = $crm->requisite()->add(
            entityId: $iCompanyID,
            entityTypeId: 4, // 4 — company (crm.enum.ownertype)
            requisitePresetId: $iRequisitePresetID,
            requisiteName: $sTitle,
            fields: ['ACTIVE' => 'Y', 'RQ_INN' => $sINN]
        )->getId();

        if (!empty($iRequisiteID)) {
            $arAddress['ENTITY_ID'] = $iRequisiteID;
            $crm->address()->add($arAddress);
        }

        echo json_encode(['message' => 'The company has been added']);
    } catch (\Throwable $e) {
        echo json_encode(['message' => 'Error: ' . $e->getMessage()]);
    }
    ```

- Go

    ```go
    // Preparation in an empty directory — go get will not work without go mod init:
    //
    //	go mod init example && go get github.com/bitrix24/b24gosdk
    //
    // Run:
    //
    //	export B24_WEBHOOK_URL='https://your-domain.bitrix24.com/rest/1/token/' && go run .
    //
    // A separate file with the form is not needed: the same program assembles and
    // serves the page — it takes the address fields and the list of requisite
    // templates from Bitrix24. Open http://localhost:3000/
    package main

    import (
    	"context"
    	"encoding/json"
    	"fmt"
    	"html"
    	"log"
    	"net/http"
    	"os"
    	"regexp"
    	"sort"
    	"strconv"
    	"strings"

    	b24 "github.com/bitrix24/b24gosdk"
    )

    // CRM object type identifiers from crm.enum.ownertype.
    const (
    	typeCompany   = 4
    	typeRequisite = 8
    )

    // addressTypeActual — actual address; the full list of types is returned by
    // crm.enum.addresstype.
    const addressTypeActual = 1

    func main() {
    	if err := run(context.Background()); err != nil {
    		log.Fatal(err)
    	}
    }

    func run(ctx context.Context) error {
    	// The webhook URL is a secret: it comes from the environment, not from the
    	// code, and never ends up on the public page with the form. The client is
    	// built ONCE per Bitrix24: http.Server calls the handler from many goroutines.
    	core := b24.NewClient(os.Getenv("B24_WEBHOOK_URL")).Core()

    	// --- assemble the form from the Bitrix24 settings
    	res, err := core.Call(ctx, "crm.address.fields", nil, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.address.fields: %w", err)
    	}

    	// The response is not a list but an object "field name -> description", hence a map.
    	var addressFields map[string]struct {
    		Type       string `json:"type"`
    		Title      string `json:"title"`
    		IsReadOnly bool   `json:"isReadOnly"`
    	}
    	if err := json.Unmarshal(res.Result, &addressFields); err != nil {
    		return fmt.Errorf("parsing address fields: %w", err)
    	}

    	// The form takes only the string fields that are available for writing: TYPE_ID,
    	// ENTITY_ID, and ENTITY_TYPE_ID also arrive in this response, but the handler
    	// substitutes them itself. Map keys in Go are unordered — sort them, otherwise
    	// the form fields will jump around from run to run.
    	var addressNames []string
    	for name, f := range addressFields {
    		if f.Type == "string" && !f.IsReadOnly {
    			addressNames = append(addressNames, name)
    		}
    	}
    	sort.Strings(addressNames)
    	res, err = core.Call(ctx, "crm.requisite.preset.list", b24.Params{
    		"select": []string{"ID", "NAME"},
    	}, b24.WithIdempotent())
    	if err != nil {
    		return fmt.Errorf("crm.requisite.preset.list: %w", err)
    	}

    	// Here the identifier arrives as a STRING ("1"), whereas crm.enum.* returns
    	// numbers. b24.ID parses both notations.
    	var presets []struct {
    		ID   b24.ID `json:"ID"`
    		Name string `json:"NAME"`
    	}
    	if err := json.Unmarshal(res.Result, &presets); err != nil {
    		return fmt.Errorf("parsing requisite templates: %w", err)
    	}
    	if len(presets) == 0 {
    		return fmt.Errorf("there are no requisite templates in Bitrix24")
    	}
    	// --- the page with the form
    	var form strings.Builder
    	form.WriteString(`<!doctype html>
    <meta charset="utf-8">
    <title>Request</title>
    <form method="post" action="/form">
    <p><label>Requisite type*<br><select name="REQ_TYPE" required>`)
    	for _, p := range presets {
    		fmt.Fprintf(&form, `<option value="%d">%s</option>`, p.ID, html.EscapeString(p.Name))
    	}
    	form.WriteString(`</select></label></p>
    <p><label>Org name*<br><input name="TITLE" required></label></p>
    <p><label>INN<br><input name="INN"></label></p>
    <p><label>Phone<br><input name="PHONE" type="tel"></label></p>`)
    	// The address fields are created dynamically: their set is defined by Bitrix24,
    	// not by the code. Names such as ADDRESS[CITY] — the handler parses them back.
    	for _, name := range addressNames {
    		fmt.Fprintf(&form, "<p><label>%s<br><input name=\"ADDRESS[%s]\"></label></p>\n",
    			html.EscapeString(addressFields[name].Title), name)
    	}
    	form.WriteString(`<p><button type="submit">Submit</button></p>
    </form>`)
    	page := form.String()
    	mux := http.NewServeMux()
    	mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    		w.Header().Set("Content-Type", "text/html; charset=utf-8")
    		fmt.Fprint(w, page)
    	})
    	mux.HandleFunc("/form", func(w http.ResponseWriter, r *http.Request) {
    		if r.Method != http.MethodPost {
    			reply(w, http.StatusMethodNotAllowed, "POST required", 0)
    			return
    		}
    		handleForm(w, r, core)
    	})

    	log.Println("form and handler: http://localhost:3000/")
    	return http.ListenAndServe(":3000", mux)
    }

    func handleForm(w http.ResponseWriter, r *http.Request, core *b24.Core) {
    	ctx := r.Context()
    	if err := r.ParseForm(); err != nil {
    		reply(w, http.StatusBadRequest, "Failed to parse the form", 0)
    		return
    	}
    	// The requisite type is converted to a number, the rest is cleared of HTML tags.
    	// The tags are CUT OUT rather than escaped: escaping is needed when rendering to
    	// the page, and because of it the CRM would receive "Weber &amp; Son" instead of
    	// "Weber & Son".
    	presetID, _ := strconv.Atoi(r.PostFormValue("REQ_TYPE"))
    	title := stripTags(r.PostFormValue("TITLE"))
    	inn := stripTags(r.PostFormValue("INN"))
    	phone := stripTags(r.PostFormValue("PHONE"))

    	if presetID == 0 || title == "" {
    		reply(w, http.StatusBadRequest, "Fill in the requisite type and the name", 0)
    		return
    	}
    	// The address fields arrived with names such as ADDRESS[CITY] — parse them back.
    	address := b24.Params{}
    	for key, values := range r.PostForm {
    		if inner, ok := addressKey(key); ok && len(values) > 0 && values[0] != "" {
    			address[inner] = stripTags(values[0])
    		}
    	}
    	// The handler substitutes the address type and the owner type itself: they are not in the form.
    	address["TYPE_ID"] = addressTypeActual
    	address["ENTITY_TYPE_ID"] = typeRequisite
    	// The phone number is retained as a multifield — a list of objects, even when
    	// there is a single number. A row WITHOUT an ID adds a value; MultifieldAdd
    	// assembles it for you.
    	phones := []map[string]any{}
    	if phone != "" {
    		phones = append(phones, b24.MultifieldAdd(phone, "WORK"))
    	}
    	res, err := core.Call(ctx, "crm.company.add", b24.Params{
    		"fields": b24.Params{
    			"TITLE":        title,
    			"COMPANY_TYPE": "CUSTOMER",
    			"PHONE":        phones,
    		},
    	}) // without WithIdempotent: a retry would create a second company
    	if err != nil {
    		// The details go to the server log, they are not shown to the visitor.
    		log.Println("crm.company.add:", err)
    		reply(w, http.StatusBadGateway, "Failed to create the company", 0)
    		return
    	}

    	// There is no wrapper: result is the identifier of the new company right away.
    	var companyID b24.ID
    	if err := json.Unmarshal(res.Result, &companyID); err != nil {
    		log.Println("parsing the company identifier:", err)
    		reply(w, http.StatusBadGateway, "Failed to create the company", 0)
    		return
    	}
    	res, err = core.Call(ctx, "crm.requisite.add", b24.Params{
    		"fields": b24.Params{
    			"ENTITY_TYPE_ID": typeCompany,
    			"ENTITY_ID":      companyID,
    			"PRESET_ID":      presetID,
    			"ACTIVE":         "Y",
    			"NAME":           title,
    			"RQ_INN":         inn,
    		},
    	})
    	if err != nil {
    		// The company has already been created, so this is no reason to answer
    		// "nothing worked": report that the requisites were not added and return the
    		// identifier.
    		log.Println("crm.requisite.add:", err)
    		reply(w, http.StatusOK, "The company was created, the requisites could not be added", companyID)
    		return
    	}
    	var requisiteID b24.ID
    	if err := json.Unmarshal(res.Result, &requisiteID); err != nil {
    		log.Println("parsing the requisite identifier:", err)
    		reply(w, http.StatusOK, "The company was created, the requisites could not be added", companyID)
    		return
    	}
    	// The address is linked to the REQUISITE, not to the company, so ENTITY_ID
    	// is filled in only now — the requisite identifier did not exist earlier.
    	if requisiteID != 0 {
    		address["ENTITY_ID"] = requisiteID
    		if _, err := core.Call(ctx, "crm.address.add", b24.Params{"fields": address}); err != nil {
    			log.Println("crm.address.add:", err)
    			reply(w, http.StatusOK, "The company and the requisites were created, the address could not be added", companyID)
    			return
    		}
    	}
    	log.Printf("created company %d, requisite %d", companyID, requisiteID)
    	reply(w, http.StatusOK, "The company with requisites has been created", companyID)
    }

    // tagPattern cuts HTML tags out of a form value.
    var tagPattern = regexp.MustCompile(`<[^>]*>`)

    func stripTags(s string) string {
    	return strings.TrimSpace(tagPattern.ReplaceAllString(s, ""))
    }

    // addressKey extracts CITY from the ADDRESS[CITY] field name.
    func addressKey(key string) (string, bool) {
    	if strings.HasPrefix(key, "ADDRESS[") && strings.HasSuffix(key, "]") {
    		return key[len("ADDRESS[") : len(key)-1], true
    	}
    	return "", false
    }

    // reply answers the page with the same JSON as the handlers in the other languages.
    func reply(w http.ResponseWriter, status int, message string, id b24.ID) {
    	w.Header().Set("Content-Type", "application/json; charset=utf-8")
    	w.WriteHeader(status)
    	body := map[string]any{"message": message}
    	if id != 0 {
    		body["id"] = id
    	}
    	_ = json.NewEncoder(w).Encode(body)
    }
    ```

{% endlist %}

## Verify the Result

Open the created company in Bitrix24. On the "Requisites" tab, the requisite with the Taxpayer ID and the address from the form is displayed.

Through REST, the result is verified with two methods:

- [crm.requisite.list](../../../api-reference/crm/requisites/universal/crm-requisite-list.md) with a filter by `ENTITY_TYPE_ID`: `4` and `ENTITY_ID` — the identifier of the created company

- [crm.address.list](../../../api-reference/crm/requisites/addresses/crm-address-list.md) with a filter by `ENTITY_TYPE_ID`: `8` and `ENTITY_ID` — the identifier of the created requisite

{% list tabs %}

- JS

    ```javascript
    const requisites = (await $b24.actions.v2.call.make({
        method: 'crm.requisite.list',
        params: {
            filter: { ENTITY_TYPE_ID: 4, ENTITY_ID: iCompanyID },
            select: ['ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'NAME']
        },
        requestId: 'requisite-list'
    })).getData().result

    const addresses = (await $b24.actions.v2.call.make({
        method: 'crm.address.list',
        params: { filter: { ENTITY_TYPE_ID: 8, ENTITY_ID: iRequisiteID } },
        requestId: 'address-list'
    })).getData().result

    console.dir({ requisites, addresses })
    ```

- Python

    ```python
    requisites = client.crm.requisite.list(
        filter={"ENTITY_TYPE_ID": 4, "ENTITY_ID": i_company_id},
        select=["ID", "ENTITY_TYPE_ID", "ENTITY_ID", "NAME"],
    ).result

    addresses = client.crm.address.list(
        filter={"ENTITY_TYPE_ID": 8, "ENTITY_ID": i_requisite_id},
    ).result

    print(requisites)
    print(addresses)
    ```

- PHP

    ```php
    $requisites = $sb->getCRMScope()->requisite()->list(
        [],
        ['ENTITY_TYPE_ID' => 4, 'ENTITY_ID' => $iCompanyID],
        ['ID', 'ENTITY_TYPE_ID', 'ENTITY_ID', 'NAME']
    )->getRequisites();

    $addresses = $sb->getCRMScope()->address()->list(
        [],
        ['ENTITY_TYPE_ID' => 8, 'ENTITY_ID' => $iRequisiteID],
        []
    )->getAddresses();

    print_r($requisites);
    print_r($addresses);
    ```

- Go

    ```go
    res, err := core.Call(ctx, "crm.requisite.list", b24.Params{
    	"filter": b24.Params{"ENTITY_TYPE_ID": 4, "ENTITY_ID": companyID},
    	"select": []string{"ID", "ENTITY_TYPE_ID", "ENTITY_ID", "NAME"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.requisite.list: %w", err)
    }
    log.Println("company requisites:", string(res.Result))

    res, err = core.Call(ctx, "crm.address.list", b24.Params{
    	"filter": b24.Params{"ENTITY_TYPE_ID": 8, "ENTITY_ID": requisiteID},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.address.list: %w", err)
    }
    log.Println("requisite addresses:", string(res.Result))
    ```

{% endlist %}

The scenario is complete if [crm.requisite.list](../../../api-reference/crm/requisites/universal/crm-requisite-list.md) returned the requisite with the `ID` from the "Add Requisites to the Company" step, and [crm.address.list](../../../api-reference/crm/requisites/addresses/crm-address-list.md) returned the address with the same `ENTITY_ID`.

```json
{
    "result": [
        {
            "ENTITY_TYPE_ID": "4",
            "ENTITY_ID": "5",
            "ID": "27",
            "NAME": "Müller GmbH"
        }
    ],
    "total": 1
}
```

```json
{
    "result": [
        {
            "TYPE_ID": "1",
            "ENTITY_TYPE_ID": "8",
            "ENTITY_ID": "27",
            "ADDRESS_1": "Tiergartenstraße 17",
            "CITY": "Berlin",
            "POSTAL_CODE": "10785"
        }
    ],
    "total": 1
}
```

## Errors and Diagnostics

If the method returns an error, check the request data. The requisite and address methods return errors with an empty code, so rely on the text in `error_description`.

#|
|| **Error text** | **Reason and action** ||
|| `Entity not found.` | An `ENTITY_ID` of a nonexistent company was passed to [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md). Take the identifier from the [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) response ||
|| `ENTITY_TYPE_ID is not defined or invalid.` | The owner type was not passed or is incorrect. A company requisite requires `4`, a requisite address requires `8` ||
|| `ENTITY_ID is not defined or invalid.` | The owner identifier was not passed. In [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md), this is the requisite identifier, not the company identifier ||
|| `TYPE_ID is not defined or invalid.` | The address type was not passed to [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md). The list of values is returned by the [crm.enum.addresstype](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md) method ||
|| `TypeAddress exists.` | The requisite already has an address of this type. One requisite retains one address of each type — modify the existing one with the [crm.address.update](../../../api-reference/crm/requisites/addresses/crm-address-update.md) method or pass a different `TYPE_ID` ||
|| `Access denied.` | The user does not have permission to add or import companies. Check which user the webhook was created on behalf of ||
|#

The scenario creates three objects in a row, and an error at any step leaves the previous objects in the CRM. Repeat the step that failed rather than the whole handler:

- An error in [crm.company.add](../../../api-reference/crm/companies/crm-company-add.md) — nothing has been created in the CRM, you can repeat the entire handler

- An error in [crm.requisite.add](../../../api-reference/crm/requisites/universal/crm-requisite-add.md) — the company has already been created. Running the handler again creates a duplicate of it, so pass the existing `ENTITY_ID`

- An error in [crm.address.add](../../../api-reference/crm/requisites/addresses/crm-address-add.md) — the company and the requisite have already been created. Add the address with a separate call using the `ENTITY_ID` of the existing requisite

## Key Considerations

- An address has no identifier of its own: it is recognized by the `ENTITY_TYPE_ID` and `ENTITY_ID` pair plus `TYPE_ID`

- The set of requisite fields depends on the template. The individual template has no `RQ_INN` field: the value is retained and returned by [crm.requisite.get](../../../api-reference/crm/requisites/universal/crm-requisite-get.md), but it is not displayed on the requisite card. The set of template fields is returned by the [crm.requisite.preset.field.list](../../../api-reference/crm/requisites/presets/fields/crm-requisite-preset-field-list.md) method

- Submitting the form again with the same data creates a new company and a new requisite. Duplicates are not filtered out

## Continue Learning

- [{#T}](../../../api-reference/crm/companies/crm-company-add.md)
- [{#T}](../../../api-reference/crm/requisites/universal/crm-requisite-add.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-add.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-fields.md)
- [{#T}](../../../api-reference/crm/requisites/presets/crm-requisite-preset-list.md)
- [{#T}](../../../api-reference/crm/requisites/universal/crm-requisite-list.md)
- [{#T}](../../../api-reference/crm/requisites/addresses/crm-address-list.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-address-type.md)
- [{#T}](../../../api-reference/crm/auxiliary/enum/crm-enum-owner-type.md)
- [{#T}](how-to-add-contact-with-requisite.md)
