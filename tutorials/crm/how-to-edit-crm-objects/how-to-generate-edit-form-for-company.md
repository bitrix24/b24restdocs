# How to Create Your Own Company Edit Card

> Scope: [`crm, user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: users with "read" permissions for CRM object items, "add" permissions for CRM object items, "change" permissions for CRM object items, and access to CRM settings

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

This example shows how to create a separate web page for working with companies from the CRM. Such a page is useful if you need to create and edit companies within your own interface, such as in an application. The standard CRM card remains unchanged.

The set of form fields is not manually defined in the code. The page retrieves the company field descriptions from Bitrix24 and builds an HTML form based on them. As a result, the form accounts for system and user fields and their configurations.

If you open the page without a company identifier, the form will be empty, and after saving, a new company will appear in Bitrix24. If you pass the identifier of an existing company, the form will load its data and save the changes to that company.

For each language, the example consists of two files:

- The generator retrieves the field descriptions and the data of an existing company, then builds an HTML form.
- The handler receives the filled form values and saves them to Bitrix24.

## How the Scenario Works

1. The [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method returns the company field descriptions in `result.fields`.
2. For editing, the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method returns the company in `result.item`.
3. The generator maps each field type to an HTML item form.
4. The handler retrieves the field descriptions again and accepts only the fields available for modification.
5. The [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method creates a company, while [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) updates it.
6. Both methods return the saved company in `result.item`.

The form retrieves additional data to display the contact name, employee name, and currency name instead of service codes. To do this, it uses the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md), [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md), [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md), and [user.get](../../../api-reference/user/user-get.md) methods.

The form takes the following from the responses and field descriptions:

- From [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — `STATUS_ID` and `NAME`
- From [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md) — `CURRENCY` and `FULL_NAME`
- From [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) for contacts — `id`, `name`, and `lastName`
- From [user.get](../../../api-reference/user/user-get.md) — `NAME` and `LAST_NAME`
- For the `enumeration` type — values from `items` in the field description

## 1. Prepare the Environment

Create an [incoming webhook](../../../local-integrations/local-webhooks.md#incoming-webhook) with `crm` and `user_brief` permissions. For the first run, you can create a webhook on behalf of an administrator. This is a recommendation rather than a REST requirement: the webhook user needs "read" permission for CRM object items, "add" permission for CRM object items, "write" permission for CRM object items, and access to CRM settings.

A webhook executes requests with the permissions of the user who created it. Do not publish files containing the webhook value in public repositories.

Select one language, create a separate folder for the example, and open a terminal in it. Save the code from steps 2 and 3 into two corresponding files, then execute the launch command. Do not close the terminal window while working with the form.

Before installing dependencies, check the PHP version using command `php -v` and the list of connected extensions via command `php -m`. B24PhpSDK version 3 requires PHP 8.4 or 8.5. B24PhpSDK and its dependencies require the `bcmath`, `curl`, `intl`, and `json` extensions.

- JavaScript
    - files — `form.mjs` and `save-form.mjs`
    - dependencies — `npm install express @bitrix24/b24jssdk`
    - launch in Bash — `B24_HOOK='https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/' node form.mjs`
    - launch in PowerShell — `$env:B24_HOOK='https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'; node form.mjs`
    - address — `http://localhost:3000/`
- PHP
    - files — `index.php` and `auto_form.php`
    - dependencies — `composer require bitrix24/b24phpsdk:^3.0`
    - launch — `php -S localhost:8000`
    - address — `http://localhost:8000/index.php`
- Python
    - files — `app.py` and `save_form.py`
    - dependencies — `pip install b24pysdk flask`
    - launch — `python app.py`
    - address — `http://localhost:5000/`

In PHP, the form sends data to the `auto_form.php` handler via a relative path.

In the examples, replace the following values:

- `your-domain.bitrix24.com` — your Bitrix24 domain
- `USER_ID/TOKEN` — the user identifier and secret code from the webhook URL

For JavaScript, pass the full URL in the `B24_HOOK` environment variable during launch. For PHP, replace the URL in the `initFromWebhook` calls in both files. For Python, replace the `domain` and `webhook_token` values in both files.

## 2. Create a Company Form

The [crm.item.*](../../../api-reference/crm/universal/index.md) methods retrieve the CRM object type via the `entityTypeId` parameter. For a company, pass the value `4`.

To ensure methods return custom fields with original names such as `UF_*` and accept these same names during saving, pass `useOriginalUfNames = Y`. In the Python SDK, this parameter corresponds to `use_original_uf_names = True`.

Retrieve the field descriptions using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method. For each field, use the following keys:

- `type` — the type of the field
- `isRequired` — required status
- `isReadOnly` — editability
- `isMultiple` — multiplicity

The abbreviated response from [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) shows how fields of different types are described:

```json
{
    "result": {
        "fields": {
            "title": {
                "type": "string",
                "isRequired": true,
                "isReadOnly": false,
                "isMultiple": false
            },
            "createdTime": {
                "type": "datetime",
                "isRequired": false,
                "isReadOnly": true,
                "isMultiple": false
            },
            "opened": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false
            }
        }
    }
}
```

The response is abbreviated to three fields to demonstrate the description format. The set of fields varies across different CRM object types.

When editing, pass the company identifier in the `id` parameter of the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method. In the response, system field names will be in camelCase, for example `title` and `contactIds`.

### How the Form Works with Multi-fields

Phones, emails, sites, and messengers are stored in a single field `fm`. Each item `result.item.fm` contains:

- `id` — the identifier of the existing value
- `typeId` — the multi-field type, for example `PHONE` or `EMAIL`
- `valueType` — the value type, for example `WORK` or `MOBILE`
- `value` — the phone, address, or other value

The generator outputs a separate row for each existing value and retains its `id` in a hidden field. At the end of the form, it adds three empty rows for new values.

For types requiring special handling, the example creates the following items.

| Field Type                | Form Item                                                                                                                             |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `crm_status`              | A drop-down list with values from [crm.status.list](../../../api-reference/crm/status/crm-status-list.md)                             |
| `enumeration`             | A drop-down list with values from `items`                                                                                             |
| `crm_multifield`          | Strings for the type, view, and value of a phone, email, site, or messenger                                                           |
| `crm_lead`                | A text field for the identifier and an e-Signature with the lead name                                                                 |
| `crm_contact`             | A numeric field for the contact identifier and an e-Signature with its name                                                           |
| `user`                    | A numeric field for the identifier and an e-Signature with the employee name from [user.get](../../../api-reference/user/user-get.md) |
| `date`                    | A date picker field                                                                                                                   |
| `boolean`, `char`         | A checkbox                                                                                                                            |
| `money`                   | A numeric field and a list of currencies                                                                                              |
| `file`, `resourcebooking` | An unsupported type message instead of an input field                                                                                 |

### Full Form Generator Code

{% include [Note on examples](../../../_includes/examples.md) %}

Save the code to a generator file: JavaScript — `form.mjs`, PHP — `index.php`, Python — `app.py`.

{% list tabs %}

- JS

    ```javascript
    // npm install express @bitrix24/b24jssdk
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'
    import { saveForm } from './save-form.mjs'

    const ENTITY_TYPE_ID = 4
    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    const app = express()
    app.use(express.urlencoded({ extended: true }))
    const SKIPPED_FIELDS = new Set(['contacts'])
    async function call(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })
        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }
        return response.getData().result
    }

    function escapeHtml(value) {
        return String(value ?? '')
            .replaceAll('&', '&amp;')
            .replaceAll('<', '&lt;')
            .replaceAll('>', '&gt;')
            .replaceAll('"', '&quot;')
            .replaceAll("'", '&#039;')
    }

    function input(p) {
        const current = p.MULTIPLE
            ? (Array.isArray(p.VALUE) ? p.VALUE : [p.VALUE ?? ''])
            : [p.VALUE ?? '']
        const values = p.MULTIPLE ? [...current, ''] : current

        return values.map((value, index) => {
            let html = '<input class="form-control"'
            if (p.NAME) html += ` name="${escapeHtml(p.NAME)}${p.MULTIPLE ? '[]' : ''}"`
            if (p.TYPE) html += ` type="${escapeHtml(p.TYPE)}"`
            if (p.STEP) html += ` step="${escapeHtml(p.STEP)}"`
            if (p.REQUIRED && index === 0) html += ' required'
            if (p.DISABLE) html += ' disabled'
            if (p.CHECKED) html += ' checked'
            html += ` value="${escapeHtml(value)}">`
            return html
        }).join('')
    }

    function select(p, list) {
        if (!list || !Object.keys(list).length) return ''
        let html = '<select class="form-control"'
        if (p.NAME) html += ` name="${escapeHtml(p.NAME)}${p.MULTIPLE ? '[]' : ''}"`
        if (p.REQUIRED) html += ' required'
        if (p.DISABLE) html += ' disabled'
        if (p.MULTIPLE) html += ' multiple'
        html += '>'
        if (!p.REQUIRED && !p.MULTIPLE) html += '<option value="">-- Not selected --</option>'

        const value = Array.isArray(p.VALUE) ? p.VALUE.map(String) : [String(p.VALUE ?? '')]
        for (const [key, title] of Object.entries(list)) {
            const selected = value.includes(String(key)) ? ' selected' : ''
            html += `<option value="${escapeHtml(key)}"${selected}>${escapeHtml(title)}</option>`
        }
        return html + '</select>'
    }

    function multifields(values = []) {
        const rows = [
            ...(Array.isArray(values) ? values : []),
            ...Array.from({ length: 3 }, () => ({ id: '', typeId: 'PHONE', valueType: 'WORK', value: '' })),
        ]

        return rows.map((row, index) => {
            const types = ['PHONE', 'EMAIL', 'WEB', 'IM']
                .map((type) => `<option value="${type}"${row.typeId === type ? ' selected' : ''}>${type}</option>`)
                .join('')
            return `<div class="border rounded p-2 mb-2">
                <input type="hidden" name="fm[${index}][id]" value="${escapeHtml(row.id)}">
                <select class="form-control mb-1" name="fm[${index}][typeId]">${types}</select>
                <input class="form-control mb-1" name="fm[${index}][valueType]" value="${escapeHtml(row.valueType || 'WORK')}" placeholder="WORK">
                <input class="form-control mb-1" name="fm[${index}][value]" value="${escapeHtml(row.value)}" placeholder="Value">
                ${row.id ? `<label><input type="checkbox" name="fm[${index}][delete]" value="Y"> Delete</label>` : ''}
            </div>`
        }).join('')
    }

    app.get('/', async (req, res) => {
        const id = parseInt(String(req.query.ID ?? '0'), 10) || 0

        try {
            const arResult = {}
            const fieldResult = await call('crm.item.fields', {
                entityTypeId: ENTITY_TYPE_ID,
                useOriginalUfNames: 'Y',
            }, 'company-fields')
            arResult.FIELDS = fieldResult.fields
            arResult.FIELD_VALUES_CURRENCY = await call('crm.currency.list', {}, 'currencies')

            if (id > 0) {
                const itemResult = await call('crm.item.get', {
                    entityTypeId: ENTITY_TYPE_ID,
                    id,
                    useOriginalUfNames: 'Y',
                }, 'company')
                arResult.ITEM = itemResult.item
            }

            let standard = ''
            let custom = ''
            for (const [key, field] of Object.entries(arResult.FIELDS)) {
                if (SKIPPED_FIELDS.has(key)) continue
                let value = arResult.ITEM?.[key] ?? ''
                let listKey = arResult['FIELD_VALUES_' + key] ?? []
                let list = {}
                let ret = ''
                const params = { NAME: `form[${key}]`, REQUIRED: field.isRequired, DISABLE: field.isReadOnly, MULTIPLE: field.isMultiple, VALUE: value }
                switch (field.type) {
                case 'crm_status':
                    if (!listKey.length) {
                        listKey = await call(
                            'crm.status.list',
                            { filter: { ENTITY_ID: field.statusType } },
                            `status-${key}`,
                        )
                    }
                    list = Object.fromEntries(listKey.map((s) => [s.STATUS_ID, s.NAME]))
                    ret = select(params, list)
                    break
                case 'crm_currency':
                    list = Object.fromEntries(arResult.FIELD_VALUES_CURRENCY.map((c) => [c.CURRENCY, c.FULL_NAME]))
                    ret = select(params, list)
                    break
                case 'enumeration':
                    if (field.items) {
                        list = Object.fromEntries(field.items.map((it) => [it.ID ?? it.id, it.VALUE ?? it.value]))
                    }
                    ret = select(params, list)
                    break
                case 'crm_multifield':
                    ret = multifields(value)
                    break
                case 'crm_lead': {
                    ret = input({ ...params, TYPE: 'text' })
                    if (value) {
                        const leadResult = await call('crm.item.get', {
                            entityTypeId: 1,
                            id: Number(value),
                        }, `lead-${key}`)
                        ret += ` (${escapeHtml(leadResult.item.title)})`
                    }
                    break
                }
                case 'crm_contact': {
                    ret = input({ ...params, TYPE: 'number' })
                    const ids = (Array.isArray(value) ? value : [value])
                        .map(Number)
                        .filter((contactId) => contactId > 0)
                    if (ids.length) {
                        const contactResult = await call('crm.item.list', {
                            entityTypeId: 3,
                            filter: { '@id': ids },
                            select: ['id', 'name', 'lastName'],
                        }, `contacts-${key}`)
                        const contacts = contactResult.items
                        if (contacts.length) {
                            const names = contacts.map((contact) => [contact.name, contact.lastName].filter(Boolean).join(' '))
                            ret += ` (${escapeHtml(names.join(', '))})`
                        }
                    }
                    break
                }
                case 'file':
                    ret = 'File type is not supported in the example'
                    break
                case 'date':
                    if (value) value = String(value).slice(0, 10)
                    ret = input({ ...params, VALUE: value, TYPE: 'date' })
                    break
                case 'datetime':
                    if (value) value = String(value).slice(0, 19)
                    ret = input({ ...params, VALUE: value, TYPE: 'datetime-local' })
                    break
                case 'char':
                    ret = input({ ...params, REQUIRED: false, VALUE: 'Y', CHECKED: value === 'Y', TYPE: 'checkbox' })
                    break
                case 'boolean':
                    ret = input({ ...params, REQUIRED: false, VALUE: 'Y', CHECKED: value === 'Y', TYPE: 'checkbox' })
                    break
                case 'double':
                    ret = input({ ...params, TYPE: 'number', STEP: 'any' })
                    break
                case 'integer':
                    ret = input({ ...params, TYPE: 'number' })
                    break
                case 'user': {
                    ret = input({ ...params, TYPE: 'number' })
                    if (value) {
                        const users = await call('user.get', { filter: { ID: value } }, `user-${key}`)
                        if (users.length) {
                            const names = users.map((user) => [user.NAME, user.LAST_NAME].filter(Boolean).join(' '))
                            ret += ` (${escapeHtml(names.join(', '))})`
                        }
                    }
                    break
                }
                case 'money': {
                    const [money, currency] = String(value).split('|')
                    ret = input({ ...params, VALUE: money, TYPE: 'number', STEP: 'any' })
                    list = Object.fromEntries(arResult.FIELD_VALUES_CURRENCY.map((c) => [c.CURRENCY, c.FULL_NAME]))
                    ret += select({ NAME: `form[${key}_CURRENCY]`, REQUIRED: field.isRequired, DISABLE: field.isReadOnly, MULTIPLE: field.isMultiple, VALUE: currency }, list)
                    break
                }
                case 'resourcebooking':
                    ret = 'resourcebooking type is not supported in the example'
                    break
                default:
                    ret = input({ ...params, TYPE: 'text' })
                    break
            }

                const label = escapeHtml(field.formLabel || field.title || key)
                const block = `<div class="col-4 mt-3">${label}: </div><div class="col-6 mt-3">${ret}</div>`
                if (key.startsWith('UF_')) custom += block
                else standard += block
            }

            res.send(`
                <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
                <div class="container">
                    <form id="auto_form" method="post">
                        ${arResult.ITEM?.id ? `<input type="hidden" name="form[id]" value="${escapeHtml(arResult.ITEM.id)}">` : ''}
                        <h2>System fields</h2>
                        <div class="row">${standard}</div>
                        <h2>Custom fields</h2>
                        <div class="row">${custom}</div>
                        <div class="row"><div class="col-sm-10 mt-5">
                            <input type="submit" class="btn btn-primary" value="Save">
                        </div></div>
                    </form>
                </div>
                <script>
                    document.getElementById('auto_form').addEventListener('submit', async (event) => {
                        event.preventDefault()
                        const body = new URLSearchParams(new FormData(event.currentTarget))
                        const response = await fetch('/form', { method: 'POST', body })
                        const json = await response.json()
                        alert(json.message || json.error)
                    })
                <\/script>
            `)
        } catch (error) {
            res.status(500).send(escapeHtml(error.message))
        }
    })

    app.post('/form', saveForm)
    app.listen(3000)
    ```

- Python

    ```python
    # pip install b24pysdk flask
    from html import escape
    from flask import Flask, request
    from b24pysdk import BitrixWebhook, Client
    from save_form import save_form

    app = Flask(__name__)
    ENTITY_TYPE_ID = 4

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # only user_id/token, without https://
    ))

    def input_field(params):
        """Builds an input or a set of inputs for a multiple field."""
        if params.get("MULTIPLE"):
            current = params.get("VALUE") if isinstance(params.get("VALUE"), list) else [params.get("VALUE") or ""]
            values = [*current, ""]
        else:
            values = [params.get("VALUE") or ""]

        html = ""
        for index, value in enumerate(values):
            html += '<input class="form-control"'
            if params.get("NAME"):
                html += f' name="{escape(params["NAME"])}{"[]" if params.get("MULTIPLE") else ""}"'
            if params.get("TYPE"):
                html += f' type="{escape(params["TYPE"])}"'
            if params.get("STEP"):
                html += f' step="{escape(params["STEP"])}"'
            if params.get("REQUIRED") and index == 0:
                html += " required"
            if params.get("DISABLE"):
                html += " disabled"
            if params.get("CHECKED"):
                html += " checked"
            html += f' value="{escape(str(value))}">'
        return html

    def select_field(params, options):
        """Builds a <select> from a dictionary of \"value => label\"."""
        if not options:
            return ""
        html = '<select class="form-control"'
        if params.get("NAME"):
            html += f' name="{params["NAME"]}{"[]" if params.get("MULTIPLE") else ""}"'
        if params.get("REQUIRED"):
            html += " required"
        if params.get("DISABLE"):
            html += " disabled"
        if params.get("MULTIPLE"):
            html += " multiple"
        html += ">"
        if not params.get("REQUIRED") and not params.get("MULTIPLE"):
            html += '<option value="">-- Not selected --</option>'
        value = params.get("VALUE")
        value = [str(v) for v in value] if isinstance(value, list) else [str(value or "")]
        for key, title in options.items():
            selected = " selected" if str(key) in value else ""
            html += f'<option value="{escape(str(key))}"{selected}>{escape(str(title))}</option>'
        return html + "</select>"

    def multifields(values):
        """Builds fm strings and saves the IDs of existing values."""
        rows = list(values) if isinstance(values, list) else []
        rows.extend({"id": "", "typeId": "PHONE", "valueType": "WORK", "value": ""} for _ in range(3))
        html = ""
        for index, row in enumerate(rows):
            options = "".join(
                f'<option value="{field_type}"{" selected" if row.get("typeId") == field_type else ""}>{field_type}</option>'
                for field_type in ("PHONE", "EMAIL", "WEB", "IM")
            )
            item_id = row.get("id") or ""
            delete = (
                f'<label><input type="checkbox" name="fm[{index}][delete]" value="Y"> Delete</label>'
                if item_id else ""
            )
            html += f"""<div class="border rounded p-2 mb-2">
                <input type="hidden" name="fm[{index}][id]" value="{escape(str(item_id))}">
                <select class="form-control mb-1" name="fm[{index}][typeId]">{options}</select>
                <input class="form-control mb-1" name="fm[{index}][valueType]" value="{escape(row.get('valueType') or 'WORK')}" placeholder="WORK">
                <input class="form-control mb-1" name="fm[{index}][value]" value="{escape(row.get('value') or '')}" placeholder="Value">
                {delete}
            </div>"""
        return html

    PAGE = """
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        <div class="container">
            <form id="auto_form" method="post">
                %(hidden_id)s
                <h2>System fields</h2>
                <div class="row">%(standard)s</div>
                <h2>Custom fields</h2>
                <div class="row">%(custom)s</div>
                <div class="row"><div class="col-sm-10 mt-5">
                    <input type="submit" class="btn btn-primary" value="Save">
                </div></div>
            </form>
        </div>
        <script>
            document.getElementById('auto_form').addEventListener('submit', async (el) => {
                el.preventDefault();
                const body = new URLSearchParams(new FormData(el.currentTarget));
                const response = await fetch('/form', { method: 'POST', body });
                const json = await response.json();
                alert(json.message || json.error);
            });
        </script>
    """

    @app.route("/")
    def form_page():
        item_id = int(request.args.get("ID", 0) or 0)

        ar_result = {}
        field_response = client.crm.item.fields(
            entity_type_id=ENTITY_TYPE_ID,
            use_original_uf_names=True,
        ).response
        ar_result["FIELDS"] = field_response.result["fields"]
        ar_result["FIELD_VALUES_CURRENCY"] = client.crm.currency.list().response.result

        if item_id > 0:
            item_response = client.crm.item.get(
                entity_type_id=ENTITY_TYPE_ID,
                bitrix_id=item_id,
                use_original_uf_names=True,
            ).response
            ar_result["ITEM"] = item_response.result["item"]

        item = ar_result.get("ITEM", {})
        s_result = ""
        s_result_custom = ""
        for key, field in ar_result["FIELDS"].items():
            if key == "contacts":
                continue
            value = item.get(key) or ""
            params = {"NAME": f"form[{key}]", "REQUIRED": field.get("isRequired"),
                      "DISABLE": field.get("isReadOnly"), "MULTIPLE": field.get("isMultiple"), "VALUE": value}
            ret = ""
            field_type = field.get("type")
            if field_type == "crm_status":
                rows = ar_result.get("FIELD_VALUES_" + key) or client.crm.status.list(
                    filter={"ENTITY_ID": field["statusType"]}).response.result
                ret = select_field(params, {r["STATUS_ID"]: r["NAME"] for r in rows})
            elif field_type == "crm_currency":
                options = {c["CURRENCY"]: c["FULL_NAME"] for c in ar_result["FIELD_VALUES_CURRENCY"]}
                ret = select_field(params, options)
            elif field_type == "enumeration":
                options = {
                    it.get("ID", it.get("id")): it.get("VALUE", it.get("value"))
                    for it in field.get("items", [])
                }
                ret = select_field(params, options)
            elif field_type == "crm_multifield":
                ret = multifields(value)
            elif field_type == "crm_lead":
                ret = input_field({**params, "TYPE": "text"})
                if value:
                    lead_response = client.crm.item.get(entity_type_id=1, bitrix_id=int(value)).response
                    ret += f" ({escape(lead_response.result['item']['title'])})"
            elif field_type == "crm_contact":
                ret = input_field({**params, "TYPE": "number"})
                ids = [int(contact_id) for contact_id in (value if isinstance(value, list) else [value]) if contact_id]
                if ids:
                    contact_response = client.crm.item.list(
                        entity_type_id=3,
                        filter={"@id": ids},
                        select=["id", "name", "lastName"],
                    ).response
                    contacts = contact_response.result["items"]
                    if contacts:
                        names = [" ".join(filter(None, [contact.get("name"), contact.get("lastName")])) for contact in contacts]
                        ret += f" ({escape(', '.join(names))})"
            elif field_type == "file":
                ret = "File type is not supported in the example"
            elif field_type == "date":
                if value:
                    value = str(value)[:10]
                ret = input_field({**params, "VALUE": value, "TYPE": "date"})
            elif field_type == "datetime":
                if value:
                    value = str(value)[:19]
                ret = input_field({**params, "VALUE": value, "TYPE": "datetime-local"})
            elif field_type == "char":
                ret = input_field({**params, "REQUIRED": False, "VALUE": "Y", "CHECKED": value == "Y", "TYPE": "checkbox"})
            elif field_type == "boolean":
                ret = input_field({**params, "REQUIRED": False, "VALUE": "Y", "CHECKED": value == "Y", "TYPE": "checkbox"})
            elif field_type == "double":
                ret = input_field({**params, "TYPE": "number", "STEP": "any"})
            elif field_type == "integer":
                ret = input_field({**params, "TYPE": "number"})
            elif field_type == "user":
                ret = input_field({**params, "TYPE": "number"})
                if value:
                    users = client.user.get(filter={"ID": value}).response.result
                    if users:
                        names = [" ".join(filter(None, [user.get("NAME"), user.get("LAST_NAME")])) for user in users]
                        ret += f" ({escape(', '.join(names))})"
            elif field_type == "money":
                money, _, currency = str(value).partition("|")
                ret = input_field({**params, "VALUE": money, "TYPE": "number", "STEP": "any"})
                options = {c["CURRENCY"]: c["FULL_NAME"] for c in ar_result["FIELD_VALUES_CURRENCY"]}
                ret += select_field({"NAME": f"form[{key}_CURRENCY]", "REQUIRED": field.get("isRequired"),
                                     "DISABLE": field.get("isReadOnly"), "MULTIPLE": field.get("isMultiple"),
                                     "VALUE": currency}, options)
            elif field_type == "resourcebooking":
                ret = "resourcebooking type is not supported in the example"
            else:
                ret = input_field({**params, "TYPE": "text"})

            label = escape(str(field.get("formLabel") or field.get("title") or key))
            block = f'<div class="col-4 mt-3">{label}: </div><div class="col-6 mt-3">{ret}</div>'
            if key.startswith("UF_"):
                s_result_custom += block
            else:
                s_result += block

        # Add a hidden id field only for the edit form
        hidden_id = ""
        if item.get("id"):
            hidden_id = f'<input type="hidden" name="form[id]" value="{escape(str(item["id"]))}">'
        return PAGE % {"hidden_id": hidden_id, "standard": s_result, "custom": s_result_custom}

    app.add_url_rule("/form", view_func=save_form, methods=["POST"])

    if __name__ == "__main__":
        app.run(port=5000)
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

        const ENTITY_TYPE_ID = 4;
        $ID = (int)($_REQUEST['ID'] ?? 0);

        class CPrintForm
        {
            public static function escape($value): string
            {
                return htmlspecialchars((string)$value, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8');
            }

            public static function input($arParams)
            {
                $sResult = '';
                $current = !empty($arParams['MULTIPLE'])
                    ? (is_array($arParams['VALUE']) ? $arParams['VALUE'] : [$arParams['VALUE'] ?? ''])
                    : [$arParams['VALUE'] ?? ''];
                if (!empty($arParams['MULTIPLE']))
                {
                    $current[] = '';
                }

                foreach ($current as $index => $value)
                {
                    $sResult .= '<input class="form-control"';
                    if (!empty($arParams['NAME']))
                    {
                        $sResult .= ' name="' . self::escape($arParams['NAME']) . (!empty($arParams['MULTIPLE']) ? '[]' : '') . '"';
                    }
                    if (!empty($arParams['TYPE']))
                    {
                        $sResult .= ' type="' . self::escape($arParams['TYPE']) . '"';
                    }
                    if (!empty($arParams['STEP']))
                    {
                        $sResult .= ' step="' . self::escape($arParams['STEP']) . '"';
                    }
                    if (!empty($arParams['REQUIRED']) && $index === 0)
                    {
                        $sResult .= ' required';
                    }
                    if (!empty($arParams['DISABLE']))
                    {
                        $sResult .= ' disabled';
                    }
                    if (!empty($arParams['CHECKED']))
                    {
                        $sResult .= ' checked';
                    }
                    $sResult .= ' value="' . self::escape($value) . '">';
                }

                return $sResult;
            }

            public static function select($arParams, $arList)
            {
                if (empty($arList) || !is_array($arList))
                {
                    return '';
                }

                $sResult = '<select class="form-control"';
                $sResult .= ' name="' . self::escape($arParams['NAME']) . (!empty($arParams['MULTIPLE']) ? '[]' : '') . '"';
                $sResult .= !empty($arParams['REQUIRED']) ? ' required' : '';
                $sResult .= !empty($arParams['DISABLE']) ? ' disabled' : '';
                $sResult .= !empty($arParams['MULTIPLE']) ? ' multiple' : '';
                $sResult .= '>';
                if (empty($arParams['REQUIRED']) && empty($arParams['MULTIPLE']))
                {
                    $sResult .= '<option value="">-- Not selected --</option>';
                }
                $values = array_map('strval', (array)($arParams['VALUE'] ?? []));
                foreach ($arList as $k => $v)
                {
                    $selected = in_array((string)$k, $values, true) ? ' selected' : '';
                    $sResult .= '<option value="' . self::escape($k) . '"' . $selected . '>' . self::escape($v) . '</option>';
                }
                $sResult .= '</select>';

                return $sResult;
            }

            public static function multifields($values): string
            {
                $rows = is_array($values) ? $values : [];
                for ($i = 0; $i < 3; $i++)
                {
                    $rows[] = ['id' => '', 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => ''];
                }

                $result = '';
                foreach ($rows as $index => $row)
                {
                    $options = '';
                    foreach (['PHONE', 'EMAIL', 'WEB', 'IM'] as $type)
                    {
                        $selected = ($row['typeId'] ?? '') === $type ? ' selected' : '';
                        $options .= '<option value="' . $type . '"' . $selected . '>' . $type . '</option>';
                    }
                    $id = $row['id'] ?? '';
                    $result .= '<div class="border rounded p-2 mb-2">';
                    $result .= '<input type="hidden" name="fm[' . $index . '][id]" value="' . self::escape($id) . '">';
                    $result .= '<select class="form-control mb-1" name="fm[' . $index . '][typeId]">' . $options . '</select>';
                    $result .= '<input class="form-control mb-1" name="fm[' . $index . '][valueType]" value="' . self::escape($row['valueType'] ?? 'WORK') . '" placeholder="WORK">';
                    $result .= '<input class="form-control mb-1" name="fm[' . $index . '][value]" value="' . self::escape($row['value'] ?? '') . '" placeholder="Value">';
                    if ($id)
                    {
                        $result .= '<label><input type="checkbox" name="fm[' . $index . '][delete]" value="Y"> Delete</label>';
                    }
                    $result .= '</div>';
                }

                return $result;
            }
        }

        function callCore($sb, string $method, array $params): array
        {
            return $sb->core
                ->call($method, $params)
                ->getResponseData()
                ->getResult();
        }

        $arResult = [];
        $fieldResult = callCore($sb, 'crm.item.fields', [
            'entityTypeId' => ENTITY_TYPE_ID,
            'useOriginalUfNames' => 'Y',
        ]);
        $arResult['FIELDS'] = $fieldResult['fields'];

        $arResult['FIELD_VALUES_CURRENCY'] = [];
        foreach ($crm->currency()->list([])->getCurrencies() as $currency)
        {
            // CURRENCY in B24PhpSDK — a Money\Currency object, the string code is retrieved via getCode()
            $arResult['FIELD_VALUES_CURRENCY'][$currency->CURRENCY->getCode()] = $currency->FULL_NAME;
        }

        if ($ID > 0)
        {
            $itemResult = callCore($sb, 'crm.item.get', [
                'entityTypeId' => ENTITY_TYPE_ID,
                'id' => $ID,
                'useOriginalUfNames' => 'Y',
            ]);
            $arResult['ITEM'] = $itemResult['item'];
        }

        $sResult = '';
        $sResultCustom = '';
    if (is_array($arResult['FIELDS'])):
        foreach ($arResult['FIELDS'] as $key => $arField)
        {
            if ($key === 'contacts')
            {
                continue;
            }
            $value = '';
            $return = '';
            if (!empty($arResult['ITEM'][$key]))
            {
                $value = $arResult['ITEM'][$key];
            }
            $arList = (isset($arResult['FIELD_VALUES_' . $key])) ? $arResult['FIELD_VALUES_' . $key] : [];
            switch ($arField['type'])
            {
                case 'crm_status':
                    if (empty($arList))
                    {
                        foreach ($crm->status()->list([], ['ENTITY_ID' => $arField['statusType']], [])->getStatuses() as $status)
                        {
                            $arList[$status->STATUS_ID] = $status->NAME;
                        }
                    }

                    $return = CPrintForm::select(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value
                        ],
                        $arList
                    );
                    break;
                case 'crm_currency':
                    $return = CPrintForm::select(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value
                        ],
                        $arResult['FIELD_VALUES_CURRENCY']
                    );
                    break;
                case 'enumeration':
                    foreach ($arField['items'] as $aItem)
                    {
                        $itemId = $aItem['ID'] ?? $aItem['id'];
                        $arList[$itemId] = $aItem['VALUE'] ?? $aItem['value'];
                    }

                    $return = CPrintForm::select(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value
                        ],
                        $arList
                    );
                    break;
                case 'crm_multifield':
                    $return = CPrintForm::multifields($value);
                    break;
                case 'crm_lead':
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'text',
                        ]
                    );

                    if (!empty($value))
                    {
                        $lead = $crm->item()->get(1, (int)$value)->item();
                        $return .= ' (' . CPrintForm::escape($lead->title) . ')';
                    }
                    break;
                case 'crm_contact':
                    $arContact = [];
                    $arContactIds = array_values(array_filter(array_map('intval', (array)$value)));
                    if (!empty($arContactIds))
                    {
                        foreach ($crm->item()->list(3, [], ['@id' => $arContactIds], ['id', 'name', 'lastName'], 0)->getItems() as $contact)
                        {
                            $arContact[] = trim(implode(' ', [$contact->name, $contact->lastName]));
                        }
                    }
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'number',
                        ]
                    );
                    if (!empty($arContact))
                    {
                        $return .= ' (' . CPrintForm::escape(implode(', ', $arContact)) . ')';
                    }
                    break;
                case 'file':
                    $return = 'File type is not supported in the example';
                    break;
                case 'date':
                    if (!empty($value))
                    {
                        $value = date('Y-m-d', strtotime($value));
                    }
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'date',
                        ]
                    );
                    break;
                case 'datetime':
                    if (!empty($value))
                    {
                        $value = date('Y-m-d\TH:i:s', strtotime($value));
                    }
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'datetime-local',
                        ]
                    );
                    break;
                case 'char':
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => false,
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => 'Y',
                            'CHECKED' => ($value == 'Y') ? true : false,
                            'TYPE' => 'checkbox',
                        ]
                    );
                    break;

                case 'boolean':
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => false,
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => 'Y',
                            'CHECKED' => ($value == 'Y') ? true : false,
                            'TYPE' => 'checkbox',
                        ]
                    );
                    break;
                case 'double':
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'number',
                            'STEP' => 'any',
                        ]
                    );
                    break;
                case 'user':
                    $arUserNames = [];
                    if (!empty($value))
                    {
                        foreach ($sb->getUserScope()->user()->get([], ['ID' => $value], true)->getUsers() as $user)
                        {
                            $arUserNames[] = implode(' ', [$user->NAME, $user->LAST_NAME]);
                        }
                    }
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'number'
                        ]
                    );
                    if (!empty($arUserNames))
                    {
                        $return .= ' (' . CPrintForm::escape(implode(', ', $arUserNames)) . ')';
                    }

                    break;
                case 'url':
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'text',
                        ]
                    );
                    break;
                case 'integer':
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'number',
                        ]
                    );
                    break;
                case 'money':
                    [$money, $currency] = array_pad(explode('|', (string)$value, 2), 2, '');
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $money,
                            'TYPE' => 'number',
                            'STEP' => 'any',
                        ]
                    );
                    $return .= CPrintForm::select(
                        [
                            'NAME' => 'form[' . $key . '_CURRENCY]',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $currency
                        ],
                        $arResult['FIELD_VALUES_CURRENCY']
                    );
                    break;
                case 'address':
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'text',
                        ]
                    );
                    break;
                case 'resourcebooking':
                    $return = 'resourcebooking type is not supported in the example';
                    break;
                default:
                    $return = CPrintForm::input(
                        [
                            'NAME' => 'form[' . $key . ']',
                            'REQUIRED' => $arField['isRequired'],
                            'DISABLE' => $arField['isReadOnly'],
                            'MULTIPLE' => $arField['isMultiple'],
                            'VALUE' => $value,
                            'TYPE' => 'text',
                        ]
                    );
                    break;
            }

            $label = CPrintForm::escape($arField['formLabel'] ?? $arField['title'] ?? $key);
            $block = '<div class="col-4 mt-3">' . $label . ': </div><div class="col-6 mt-3">' . $return . '</div>';
            if (strpos($key, 'UF_') === 0)
            {
                $sResultCustom .= $block;
            }
            else
            {
                $sResult .= $block;
            }
        }

    ?>
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        <script>
            document.addEventListener('DOMContentLoaded', () => {
                document.getElementById('auto_form').addEventListener('submit', async (event) => {
                    event.preventDefault();
                    const body = new URLSearchParams(new FormData(event.currentTarget));
                    const response = await fetch('auto_form.php', { method: 'POST', body });
                    const json = await response.json();
                    alert(json.message || json.error);
                });
            });
        </script>
        <div class="container">
            <form id="auto_form" action="" method="post">
                <?php if (!empty($arResult['ITEM']['id'])): ?>
                    <input type="hidden" name="form[id]" value="<?= CPrintForm::escape($arResult['ITEM']['id']) ?>">
                <?php endif; ?>
                <h2>System fields</h2>
                <div class="row">
                    <?= $sResult ?>
                </div>
                <h2>Custom fields</h2>
                <div class="row">
                    <?= $sResultCustom ?>
                </div>
                <div class="row">
                    <div class="col-sm-10 mt-5">
                        <input type="submit" class="btn btn-primary" value="Save">
                    </div>
                </div>
            </form>
        </div>
    <?php endif; ?>
    ```
{% endlist %}

## 3. Saving Form Data

The handler calls [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) again. This prevents the transmission of unknown or uneditable fields. The handler skips fields `file` and `resourcebooking`, so their current values are retained.

The browser does not send unchecked checkboxes. Therefore, for fields `boolean` and `char`, the handler passes `Y` when the checkbox is checked and `N` when it is unchecked.

If no other editable field is present in the submitted form, the handler clears it:

- for a single field, it passes an empty string
- for a multiple field, it passes an empty array

If a field is removed from the HTML form, its current value in the company will be overwritten upon saving.

### How the Handler Collects `fm`

Each `fm` row contains a hidden `id` and visible `typeId`, `valueType`, and `value`.

- for an existing non-empty value, its `id` becomes the array key — this allows the server to update the same record
- for a new non-empty value, the handler creates keys `n0`, `n1`, and so on — this allows the server to add a record
- if an existing value is cleared or marked with the **Delete** checkbox, the handler retains its `id`, but passes an empty `value` — this allows the server to delete the record
- the handler does not pass empty strings without `id`

For example, the `fm` object before updating may look like this:

```json
{
    "451": { "typeId": "PHONE", "valueType": "WORK", "value": "+49 495 111-22-33" },
    "452": { "typeId": "EMAIL", "valueType": "WORK", "value": "" },
    "n0": { "typeId": "EMAIL", "valueType": "WORK", "value": "info@example.com" }
}
```

Record `451` will be updated, `452` will be deleted, and `n0` will be added.

### How the Handler Selects the Save Method

If the form contains `id`, the handler calls [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md). In all other cases, it calls [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md).

### Complete Handler Code

Save the code to the handler file: JavaScript — `save-form.mjs`, PHP — `auto_form.php`, Python — `save_form.py`.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const ENTITY_TYPE_ID = 4
    const SKIPPED_FIELDS = new Set(['contacts'])
    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    // B24_HOOK = 'https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'

    async function call(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })
        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }
        return response.getData().result
    }

    function asArray(value) {
        if (Array.isArray(value)) return value
        return value === undefined || value === null ? [] : [value]
    }

    function buildMultifields(rows) {
        const result = {}
        let newIndex = 0
        for (const row of Object.values(rows ?? {})) {
            const id = parseInt(String(row.id ?? '0'), 10) || 0
            const value = String(row.value ?? '')
            const shouldDelete = row.delete === 'Y' || value === ''
            if (!id && shouldDelete) continue

            const key = id ? String(id) : `n${newIndex++}`
            result[key] = {
                typeId: String(row.typeId || 'PHONE'),
                valueType: String(row.valueType || 'WORK'),
                value: shouldDelete ? '' : value,
            }
        }
        return result
    }

    export async function saveForm(req, res) {
        try {
            const submitted = req.body.form ?? {}
            const fieldResult = await call('crm.item.fields', {
                entityTypeId: ENTITY_TYPE_ID,
                useOriginalUfNames: 'Y',
            }, 'company-fields-save')

            const fields = {}
            for (const [key, prop] of Object.entries(fieldResult.fields)) {
                if (SKIPPED_FIELDS.has(key) || prop.isReadOnly || ['file', 'resourcebooking'].includes(prop.type)) continue

                if (['boolean', 'char'].includes(prop.type)) {
                    fields[key] = key in submitted ? 'Y' : 'N'
                    continue
                }

                if (prop.type === 'crm_multifield') {
                    fields[key] = buildMultifields(req.body.fm)
                    continue
                }

                if (!(key in submitted)) {
                    fields[key] = prop.isMultiple ? [] : ''
                    continue
                }

                let value = submitted[key]
                if (prop.type === 'money') {
                    value = `${value ?? ''}|${submitted[`${key}_CURRENCY`] ?? ''}`
                } else if (prop.type === 'crm_contact') {
                    value = asArray(value).map(Number).filter((itemId) => itemId > 0)
                } else if (prop.isMultiple) {
                    value = asArray(value).filter((item) => item !== '')
                }
                fields[key] = value
            }

            const id = parseInt(String(submitted.id ?? '0'), 10) || 0
            const method = id > 0 ? 'crm.item.update' : 'crm.item.add'
            const params = {
                entityTypeId: ENTITY_TYPE_ID,
                fields,
                useOriginalUfNames: 'Y',
            }
            if (id > 0) params.id = id

            const result = await call(method, params, `company-${id > 0 ? 'update' : 'add'}`)
            res.json({ message: `Company saved, ID: ${result.item.id}` })
        } catch (error) {
            res.status(400).json({ error: error.message })
        }
    }
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import re
    from flask import request, jsonify
    from b24pysdk import BitrixWebhook, Client

    ENTITY_TYPE_ID = 4
    FORM_KEY = re.compile(r"^form\[([^]]+)](\[\])?$")
    FM_KEY = re.compile(r"^fm\[(\d+)]\[(id|typeId|valueType|value|delete)]$")

    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",  # only user_id/token, without https://
    ))

    def parse_form():
        result = {}
        for full_key in request.form:
            match = FORM_KEY.match(full_key)
            if not match:
                continue
            values = request.form.getlist(full_key)
            result[match.group(1)] = values if match.group(2) else values[-1]
        return result

    def parse_multifield_rows():
        rows = {}
        for full_key in request.form:
            match = FM_KEY.match(full_key)
            if match:
                rows.setdefault(int(match.group(1)), {})[match.group(2)] = request.form[full_key]
        return rows

    def build_multifields(rows):
        result = {}
        new_index = 0
        for row in rows.values():
            item_id = int(row.get("id") or 0)
            value = str(row.get("value") or "")
            should_delete = row.get("delete") == "Y" or value == ""
            if not item_id and should_delete:
                continue

            key = str(item_id) if item_id else f"n{new_index}"
            if not item_id:
                new_index += 1
            result[key] = {
                "typeId": str(row.get("typeId") or "PHONE"),
                "valueType": str(row.get("valueType") or "WORK"),
                "value": "" if should_delete else value,
            }
        return result

    def save_form():
        try:
            submitted = parse_form()
            field_response = client.crm.item.fields(
                entity_type_id=ENTITY_TYPE_ID,
                use_original_uf_names=True,
            ).response

            fields = {}
            for key, prop in field_response.result["fields"].items():
                if key == "contacts" or prop.get("isReadOnly") or prop.get("type") in ("file", "resourcebooking"):
                    continue
                if prop.get("type") in ("boolean", "char"):
                    fields[key] = "Y" if key in submitted else "N"
                    continue
                if prop.get("type") == "crm_multifield":
                    fields[key] = build_multifields(parse_multifield_rows())
                    continue
                if key not in submitted:
                    fields[key] = [] if prop.get("isMultiple") else ""
                    continue

                value = submitted[key]
                if prop.get("type") == "money":
                    value = f"{value or ''}|{submitted.get(f'{key}_CURRENCY', '')}"
                elif prop.get("type") == "crm_contact":
                    values = value if isinstance(value, list) else [value]
                    value = [int(item_id) for item_id in values if str(item_id).isdigit() and int(item_id) > 0]
                elif prop.get("isMultiple"):
                    value = [item for item in (value if isinstance(value, list) else [value]) if item != ""]
                fields[key] = value

            item_id = int(submitted.get("id") or 0)
            if item_id > 0:
                response = client.crm.item.update(
                    entity_type_id=ENTITY_TYPE_ID,
                    bitrix_id=item_id,
                    fields=fields,
                    use_original_uf_names=True,
                ).response
            else:
                response = client.crm.item.add(
                    entity_type_id=ENTITY_TYPE_ID,
                    fields=fields,
                    use_original_uf_names=True,
                ).response

            return jsonify(message=f"Company saved, ID: {response.result['item']['id']}")
        except Exception as error:
            return jsonify(error=str(error)), 400

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

        const ENTITY_TYPE_ID = 4;
        header('Content-Type: application/json; charset=utf-8');

            function callCore($sb, string $method, array $params): array
            {
                return $sb->core
                    ->call($method, $params)
                    ->getResponseData()
                    ->getResult();
            }

            function buildMultifields(array $rows): array
            {
                $result = [];
                $newIndex = 0;
                foreach ($rows as $row)
                {
                    $id = (int)($row['id'] ?? 0);
                    $value = (string)($row['value'] ?? '');
                    $shouldDelete = ($row['delete'] ?? '') === 'Y' || $value === '';
                    if ($id <= 0 && $shouldDelete)
                    {
                        continue;
                    }

                    $key = $id > 0 ? (string)$id : 'n' . $newIndex++;
                    $result[$key] = [
                        'typeId' => (string)($row['typeId'] ?? 'PHONE'),
                        'valueType' => (string)($row['valueType'] ?? 'WORK'),
                        'value' => $shouldDelete ? '' : $value,
                    ];
                }

                return $result;
            }

            try
            {
                $submitted = is_array($_POST['form'] ?? null) ? $_POST['form'] : [];
                $fieldResult = callCore($sb, 'crm.item.fields', [
                    'entityTypeId' => ENTITY_TYPE_ID,
                    'useOriginalUfNames' => 'Y',
                ]);

                $fields = [];
                foreach ($fieldResult['fields'] as $key => $prop)
                {
                    if ($key === 'contacts' || !empty($prop['isReadOnly']) || in_array($prop['type'], ['file', 'resourcebooking'], true))
                    {
                        continue;
                    }
                    if (in_array($prop['type'], ['boolean', 'char'], true))
                    {
                        $fields[$key] = array_key_exists($key, $submitted) ? 'Y' : 'N';
                        continue;
                    }
                    if ($prop['type'] === 'crm_multifield')
                    {
                        $fields[$key] = buildMultifields(is_array($_POST['fm'] ?? null) ? $_POST['fm'] : []);
                        continue;
                    }
                    if (!array_key_exists($key, $submitted))
                    {
                        $fields[$key] = !empty($prop['isMultiple']) ? [] : '';
                        continue;
                    }

                    $value = $submitted[$key];
                    if ($prop['type'] === 'money')
                    {
                        $value = (string)$value . '|' . (string)($submitted[$key . '_CURRENCY'] ?? '');
                    }
                    elseif ($prop['type'] === 'crm_contact')
                    {
                        $value = array_values(array_filter(array_map('intval', (array)$value)));
                    }
                    elseif (!empty($prop['isMultiple']))
                    {
                        $value = array_values(array_filter((array)$value, static fn($item) => $item !== ''));
                    }
                    $fields[$key] = $value;
                }

                $id = (int)($submitted['id'] ?? 0);
                $method = $id > 0 ? 'crm.item.update' : 'crm.item.add';
                $params = [
                    'entityTypeId' => ENTITY_TYPE_ID,
                    'fields' => $fields,
                    'useOriginalUfNames' => 'Y',
                ];
                if ($id > 0)
                {
                    $params['id'] = $id;
                }

                $result = callCore($sb, $method, $params);
                echo json_encode(
                    ['message' => 'Company saved, ID: ' . $result['item']['id']],
                    JSON_UNESCAPED_UNICODE | JSON_THROW_ON_ERROR
                );
            }
            catch (Throwable $error)
            {
                http_response_code(400);
                echo json_encode(['error' => $error->getMessage()], JSON_UNESCAPED_UNICODE);
            }
    ```
{% endlist %}

After a successful call to [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) or [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md), the identifier of the saved company is located in `result.item.id`:

```json
{
    "result": {
        "item": {
            "id": 123
        }
    }
}
```

Before testing, create two test contacts. Open the card for each contact and take the number from the URL. For example, from the URLs `/crm/contact/details/456/` and `/crm/contact/details/789/`, obtain identifiers `456` and `789`. The contact list can also be obtained using the [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) method by passing `entityTypeId` with the value `3`.

## Verify the Result

1. Open the generator without the `ID` parameter and fill in the company name
2. In the first empty row, leave the `PHONE` type and `WORK` kind, and enter a phone number
3. In the second empty row, select the `EMAIL` type, leave it as `WORK`, and enter an address
4. In the contacts field, specify the two identifiers from the previous paragraph
5. Save the form and ensure that the handler returned the identifier of the created company
6. Open the generator with the `ID` parameter and substitute the company identifier returned by the handler in the previous step, for example `/?ID=123`
7. In the row with the existing phone number, change the number
8. In the row with the existing email, check the **Delete** box, and in the first empty row, select the `EMAIL` type, leave it as `WORK`, and enter a new address
9. Save the form and verify the changes in the company card

Separately, verify the links to contacts: the composition must match the form, and the first identifier from `contactIds` will become the primary contact.

## Errors and Diagnostics

If the form does not open or the company is not saved, identify the error symptom and follow the recommendation.

| Symptom                                                                                  | What to Check and Fix                                                                                                                                        |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| CRM methods return an access error                                                       | Check the `crm` permission for the webhook, and ensure the webhook user has permissions to read, add, and edit CRM items and access to CRM settings          |
| An error occurs when displaying the responsible person field, but other fields load      | Check the `user_brief` permission required to call [user.get](../../../api-reference/user/user-get.md)                                                       |
| [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns `NOT_FOUND` | Specify the identifier of an existing company accessible to the webhook user, or remove the `ID` parameter to create a new company                           |
| The first request to Bitrix24 results in an authorization error                          | Check the domain, `USER_ID/TOKEN`, and the webhook status: it must not be deleted or revoked                                                                 |
| Upon saving, the handler returns an error regarding an empty field                       | Fill in all fields for which [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) returned `isRequired: true`, and save the form again |
| The server does not start: the port is occupied by another process                       | Free the port or change it in the code or launch command, then restart the server                                                                            |
| An import error or missing class error occurs on startup                                 | Install the dependencies using the command for your chosen language from the preparation section and restart the server                                      |
| The browser shows `localhost refused to connect`                                         | Look at the first error in the terminal, resolve it, and restart the server. Verify that you are opening the correct port                                    |
| PHP returns `Call to a member function error() on array`                                 | Replace the deprecated `callCore` function with the version from the generator or handler code and retry the request                                         |
| Composer reports an incompatible PHP version                                             | Use PHP 8.4 or 8.5. Check the version using the `php -v` command, then reinstall the dependencies                                                            |
| PHP reports a missing extension                                                          | Enable the `bcmath`, `curl`, `intl`, and `json` extensions, restart PHP, and check the list with the command `php -m`                                        |

## Key Considerations

{% note warning "" %}

The form only manages the composition of linked contacts via the `contactIds` field and does not allow changing their roles. Upon saving, the server re-establishes `SORT` in order of identifiers, assigns `IS_PRIMARY=Y` to the first contact, and retains the `ROLE_ID` of the existing link.

Do not use this example if you need to retain extended link properties without changes.

The field description also includes a service field `contacts`. The example skips it and only works with `contactIds` to avoid passing two versions of the same link.

{% endnote %}

The example does not support multiple fields of type `money`: for these, the value will be saved in an incorrect format.

The example is intended for local or test execution. The handler performs requests via a webhook with the permissions of the user who created it and does not verify who submitted the form. Do not publish the page in public access without your own authentication.

## Continue Learning

For a different CRM object, the `entityTypeId` and the set of fields change, but the rest of the logic remains the same:

- [{#T}](./how-to-generate-edit-form-for-deal.md)
- [{#T}](./how-to-generate-edit-form-for-lead.md)
- [{#T}](./how-to-make-contact-edit-card.md)
