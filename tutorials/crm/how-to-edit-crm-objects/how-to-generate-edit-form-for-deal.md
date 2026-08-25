# How to Create a Custom Deal Edit Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md), [`user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the scenario: a user with permissions to read, add, and edit deals, read related companies and contacts, and access CRM settings. Scope `user_brief` is required to call [user.get](../../../api-reference/user/user-get.md)
>
> The [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) and [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) methods are available to any user. The list of pipelines is filtered by read permissions

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In this example, we will create a web form for adding and editing a deal. The form does not contain a predefined list of fields: the generator retrieves the field descriptions using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method and selects the appropriate HTML control for each field type. Therefore, custom fields created in Bitrix24 appear in the form without any code changes.

If you open the page without the `ID` parameter, the handler will create a deal. If you pass an identifier, for example `?ID=342`, the generator will retrieve the deal using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method, populate the form, and the handler will update this deal.

The example consists of two files:

- The generator retrieves field descriptions, deal data, pipelines, stages, and lookup values, then outputs an HTML form
- The handler receives the form data and calls the add or update method

In universal methods, the deal type identifier `entityTypeId` is equal to `2`. This value is provided in the [CRM Object Type](../../../api-reference/crm/data-types.md#object_type) table.

## How the Scenario Works

1. [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) returns the field descriptions in `result.fields`
2. If the URL contains `ID`, [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns the deal values in `result.item`
3. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) returns the pipelines available to the user in `result.categories`
4. The generator determines the pipeline: for an existing deal, it takes `categoryId` from `result.item`, for a new one — the pipeline with `isDefault = Y`
5. For each pipeline, the generator retrieves the stages using the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method. For the default pipeline, it passes `ENTITY_ID = DEAL_STAGE`; for the other pipelines, it passes `ENTITY_ID = DEAL_STAGE_{categoryId}`
6. The generator replaces internal codes with readable names using additional methods:
   - [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — fields `STATUS_ID` and `NAME` for deal types, stages, sources, and other CRM lookup lists
   - [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md) — fields `CURRENCY` and `FULL_NAME` for currencies
   - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — the `title` field of the linked company
   - [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — fields `id`, `name`, `lastName` of linked contacts
   - [user.get](../../../api-reference/user/user-get.md) — fields `ID`, `NAME`, `LAST_NAME` of users
7. The handler retrieves the field descriptions again using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method and converts the form values to Bitrix24 REST API types
8. For a new record, [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) returns the created deal in `result.item`. For an existing record, [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) returns the updated deal in the same result key

### How Pipeline and Stage Are Linked

The `stageId` field description from [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) contains `statusType = DEAL_STAGE`. This value refers to the default pipeline and does not change based on the currently opened deal. Therefore, you cannot always pass `stageId.statusType` to [crm.status.list](../../../api-reference/crm/status/crm-status-list.md): for a deal from a different pipeline, the method will return stages from the wrong pipeline.

The generator builds `ENTITY_ID` based on `categoryId`. For `categoryId = 0`, it uses `DEAL_STAGE`; for a positive identifier, it uses `DEAL_STAGE_{categoryId}`. When the user selects a different pipeline, the browser script replaces the stage field options and requires selecting a stage from the new list.

Upon saving, the server checks whether `stageId` belongs to the selected pipeline. If a stage from a different pipeline is passed during a transfer, the built-in processing selects the first stage in the new pipeline with the same semantics — in progress, successful, or failed. The form does not rely on this replacement: after changing the pipeline, the user explicitly selects a stage.

## 1. Prepare the Environment

Create an [Incoming webhook](../../../local-integrations/local-webhooks.md#incoming-webhook) with `crm` and `user_brief` permissions.

{% note warning "Keep the webhook secret" %}

The webhook executes requests with the permissions of the user who created it. Do not add the webhook URL to public repositories, client-side JavaScript, or error messages.

{% endnote %}

Select one language, create a separate folder for the example, and open a terminal in it. First, save the code from steps 2 and 3, then execute the launch command. Do not close the terminal window while working with the form.

Create two files:

#|
|| Language | Form Generator | Handler ||
|| JavaScript | `form.mjs` | `save-form.mjs` ||
|| PHP | `index.php` | `auto_form.php` ||
|| Python | `app.py` | `save_form.py` ||
|#

For PHP, install PHP 8.4 or 8.5 and Composer first. The `php` and `composer` commands must be available on the command line.

Install dependencies.

{% list tabs %}

- JS

    ```bash
    npm init -y
    npm install @bitrix24/b24jssdk express
    ```

- Python

    ```bash
    pip install b24pysdk flask
    ```


- PHP

    ```bash
    composer require bitrix24/b24phpsdk:"^3.0"
    ```
{% endlist %}

Before installing dependencies, check the versions of Node.js, PHP, and Python using the commands `node --version`, `php -v`, and `python --version`, and the list of PHP extensions with the command `php -m`. `@bitrix24/b24jssdk` supports Node.js 18, 20, 22, and newer. B24PhpSDK version 3 also requires the `bcmath`, `curl`, `intl`, and `json` extensions. `b24pysdk` requires Python 3.9 or newer. After installation, run `composer check-platform-reqs`.

Specify the webhook URL:

- In JavaScript, set the `B24_HOOK` environment variable
- In PHP, replace the full URL in `initFromWebhook`
- In Python, replace `your-domain.bitrix24.com` and `USER_ID/TOKEN`

Launch the example.

{% list tabs %}

- JS

    Bash:

    ```bash
    export B24_HOOK='https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'
    node form.mjs
    ```

    PowerShell:

    ```powershell
    $env:B24_HOOK='https://your-domain.bitrix24.com/rest/USER_ID/TOKEN/'
    node form.mjs
    ```

- Python

    Bash and PowerShell:

    ```bash
    python app.py
    ```


- PHP

    Bash and PowerShell:

    ```bash
    php -S localhost:8000
    ```
{% endlist %}

The form pages will be available at the following addresses:

#|
|| Language | New deal | Deal with identifier `342` ||
|| JavaScript | `http://localhost:3000/` | `http://localhost:3000/?ID=342` ||
|| PHP | `http://localhost:8000/index.php` | `http://localhost:8000/index.php?ID=342` ||
|| Python | `http://localhost:5000/` | `http://localhost:5000/?ID=342` ||
|#

## 2. Create a Deal Form

The generator passes two parameters to [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md):

- `entityTypeId: 2` — the identifier of the "deal" object type from the [CRM Object Type](../../../api-reference/crm/data-types.md#object_type) table
- `useOriginalUfNames: Y` — return the original names of custom fields `UF_*`

In the Python SDK, the `useOriginalUfNames` parameter is named `use_original_uf_names` and accepts a boolean value `True`.

The abbreviated response of [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) for a deal looks like this:

```json
{
    "result": {
        "fields": {
            "title": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false
            },
            "typeId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false,
                "statusType": "DEAL_TYPE"
            },
            "categoryId": {
                "type": "crm_category",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false
            },
            "stageId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false,
                "statusType": "DEAL_STAGE"
            },
            "companyId": {
                "type": "crm_company",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false
            },
            "contactIds": {
                "type": "crm_contact",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": true
            },
            "opportunity": {
                "type": "double",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false
            },
            "isManualOpportunity": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false
            }
        }
    }
}
```

The next step requires the keys `type`, `isRequired`, `isReadOnly`, `isMultiple`, `title`, or `formLabel`. For fields of type `crm_status`, a `statusType` is also required, except for the field `stageId`: its directory generator is determined by `categoryId`.

The [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) method retrieves the current list of deal pipelines by `entityTypeId = 2`. In the response, the generator uses `id`, `name`, and `isDefault`:

```json
{
    "result": {
        "categories": [
            {
                "id": 9,
                "name": "Funnel with original name",
                "entityTypeId": 2,
                "isDefault": "N"
            },
            {
                "id": 0,
                "name": "General",
                "entityTypeId": 2,
                "isDefault": "Y"
            }
        ]
    }
}
```

If `ID=342` is passed, [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) will return data in `result.item`. The generator retrieves values from this object using the same camelCase keys that arrived in the field description:

```json
{
    "result": {
        "item": {
            "id": 342,
            "title": "New deal (specifically for REST method examples)",
            "typeId": "SERVICE",
            "categoryId": 9,
            "stageId": "C9:UC_KN8KFI",
            "companyId": 5,
            "contactIds": [4, 5],
            "opportunity": 999.99,
            "currencyId": "EUR",
            "isManualOpportunity": "Y"
        }
    }
}
```

The `categoryId` field is present in the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) response and is available for modification. The [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) and [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) methods accept it in `fields`. When changing the pipeline, simultaneously select the appropriate `stageId` from the updated list.

The `typeId` field uses the `DEAL_TYPE` directory. The generator takes `statusType` from the field description and retrieves options using the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method.

The deal amount is stored in `opportunity`, and its calculation mode is in `isManualOpportunity`. If a deal has line items and `isManualOpportunity = N`, after saving, Bitrix24 will recalculate the amount based on the line items and delivery. To retain the value from the form, enable the Calculate amount manually checkbox.

Links to customers are stored in `companyId` and `contactIds`. The example does not output the obsolete single field `contactId` and the service field `contacts` to avoid sending multiple representations of the same link. Universal methods retrieve and modify the `contactIds` array without calling a separate deal links method.

A deal does not have a `fm` multi-field, so the form does not display phones, email addresses, sites, or messengers. This data belongs to the linked contacts and companies.

The generator maps field types to form controls.

#|
|| Field type | Form control ||
|| `crm_status`, `crm_currency`, `crm_category`, `enumeration` | `<select>` with directory values ||
|| `crm_company` | numeric field and current company name ||
|| `crm_contact` | one or more numeric fields and current contact names ||
|| `user` | numeric field and username ||
|| `date`, `datetime` | `date`, `datetime-local` ||
|| `boolean`, `char` | checkbox ||
|| `integer`, `double` | numeric field ||
|| `money` | amount and list of currencies ||
|| `file`, `resourcebooking`, `crm_product_row` | unsupported type message ||
|| Other types | text field ||
|#

{% include [Note on examples](../../../_includes/examples.md) %}

### Full Form Generator Code

Save the code to the generator file: JavaScript — `form.mjs`, PHP — `index.php`, Python — `app.py`.

{% list tabs %}

- JS

    ```javascript
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'
    import { saveForm } from './save-form.mjs'

    const ENTITY_TYPE_ID = 2
    const SKIPPED_FIELDS = new Set(['contactId', 'contacts'])
    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)
    const app = express()

    app.use(express.urlencoded({ extended: true }))

    async function call(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })
        if (!response.isSuccess) throw new Error(response.getErrorMessages().join('; '))
        return response.getData().result
    }

    function escapeHtml(value) {
        return String(value ?? '').replace(/[&<>"']/g, (char) => ({
            '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#039;',
        })[char])
    }

    function input(params) {
        const values = params.MULTIPLE
            ? [...(Array.isArray(params.VALUE) ? params.VALUE : [params.VALUE ?? '']), '']
            : [params.VALUE ?? '']
        return values.map((value, index) => {
            let html = '<input class="form-control"'
            html += ` name="${escapeHtml(params.NAME)}${params.MULTIPLE ? '[]' : ''}"`
            html += ` type="${escapeHtml(params.TYPE || 'text')}"`
            if (params.STEP) html += ` step="${escapeHtml(params.STEP)}"`
            if (params.REQUIRED && index === 0) html += ' required'
            if (params.DISABLE) html += ' disabled'
            if (params.CHECKED) html += ' checked'
            return html + ` value="${escapeHtml(value)}">`
        }).join('')
    }

    function select(params, options) {
        let html = `<select class="form-control" name="${escapeHtml(params.NAME)}${params.MULTIPLE ? '[]' : ''}"`
        if (params.ID) html += ` id="${escapeHtml(params.ID)}"`
        if (params.REQUIRED) html += ' required'
        if (params.DISABLE) html += ' disabled'
        if (params.MULTIPLE) html += ' multiple'
        html += '>'
        if (!params.REQUIRED && !params.MULTIPLE) html += '<option value="">-- Not selected --</option>'
        const selectedValues = (Array.isArray(params.VALUE) ? params.VALUE : [params.VALUE ?? '']).map(String)
        for (const [key, title] of Object.entries(options)) {
            const selected = selectedValues.includes(String(key)) ? ' selected' : ''
            html += `<option value="${escapeHtml(key)}"${selected}>${escapeHtml(title)}</option>`
        }
        return html + '</select>'
    }

    app.get('/', async (req, res) => {
        const id = parseInt(String(req.query.ID ?? '0'), 10) || 0
        try {
            const fieldResult = await call('crm.item.fields', {
                entityTypeId: ENTITY_TYPE_ID,
                useOriginalUfNames: 'Y',
            }, 'deal-fields')
            const fields = fieldResult.fields
            const currencies = await call('crm.currency.list', {}, 'currencies')
            const item = id > 0
                ? (await call('crm.item.get', {
                    entityTypeId: ENTITY_TYPE_ID,
                    id,
                    useOriginalUfNames: 'Y',
                }, 'deal-get')).item
                : {}
            const categories = (await call('crm.category.list', { entityTypeId: ENTITY_TYPE_ID }, 'deal-categories')).categories
            const defaultCategory = categories.find((category) => category.isDefault === 'Y')
            const categoryId = Number(item.categoryId ?? defaultCategory?.id)
            if (!Number.isInteger(categoryId)) throw new Error('Default funnel not found')
            const categoryOptions = Object.fromEntries(categories.map((category) => [category.id, category.name]))
            const stagesByCategory = {}
            for (const category of categories) {
                const currentCategoryId = Number(category.id)
                const statusType = currentCategoryId === 0 ? 'DEAL_STAGE' : `DEAL_STAGE_${currentCategoryId}`
                const rows = await call('crm.status.list', { filter: { ENTITY_ID: statusType } }, `stages-${currentCategoryId}`)
                stagesByCategory[String(currentCategoryId)] = Object.fromEntries(rows.map((row) => [row.STATUS_ID, row.NAME]))
            }

            let standard = ''
            let custom = ''
            for (const [key, field] of Object.entries(fields)) {
                if (SKIPPED_FIELDS.has(key)) continue
                let value = key === 'categoryId' ? categoryId : (item[key] ?? '')
                let control = ''
                const params = {
                    NAME: `form[${key}]`, VALUE: value,
                    REQUIRED: field.isRequired, DISABLE: field.isReadOnly, MULTIPLE: field.isMultiple,
                    ID: key === 'categoryId' ? 'category-id' : (key === 'stageId' ? 'stage-id' : ''),
                }

                if (key === 'stageId') {
                    control = select({ ...params, REQUIRED: true }, stagesByCategory[String(categoryId)] || {})
                } else if (field.type === 'crm_status') {
                    const rows = await call('crm.status.list', { filter: { ENTITY_ID: field.statusType } }, `status-${key}`)
                    control = select(params, Object.fromEntries(rows.map((row) => [row.STATUS_ID, row.NAME])))
                } else if (field.type === 'crm_category') {
                    control = select(params, categoryOptions)
                } else if (field.type === 'crm_currency') {
                    control = select(params, Object.fromEntries(currencies.map((row) => [row.CURRENCY, row.FULL_NAME])))
                } else if (field.type === 'enumeration') {
                    const options = Object.fromEntries((field.items || []).map((row) => [row.ID ?? row.id, row.VALUE ?? row.value]))
                    control = select(params, options)
                } else if (field.type === 'crm_company') {
                    control = input({ ...params, TYPE: 'number' })
                    if (value) {
                        const company = (await call('crm.item.get', {
                            entityTypeId: 4, id: Number(value),
                        }, `company-${key}`)).item
                        control += ` (${escapeHtml(company.title)})`
                    }
                } else if (field.type === 'crm_contact') {
                    control = input({ ...params, TYPE: 'number' })
                    const ids = (Array.isArray(value) ? value : [value]).map(Number).filter((itemId) => itemId > 0)
                    if (ids.length) {
                        const contacts = (await call('crm.item.list', {
                            entityTypeId: 3,
                            filter: { '@id': ids },
                            select: ['id', 'name', 'lastName'],
                        }, `contacts-${key}`)).items
                        const names = contacts.map((contact) => [contact.name, contact.lastName].filter(Boolean).join(' '))
                        control += ` (${escapeHtml(names.join(', '))})`
                    }
                } else if (field.type === 'user') {
                    control = input({ ...params, TYPE: 'number' })
                    if (value) {
                        const users = await call('user.get', { filter: { ID: value } }, `user-${key}`)
                        const names = users.map((user) => [user.NAME, user.LAST_NAME].filter(Boolean).join(' '))
                        control += ` (${escapeHtml(names.join(', '))})`
                    }
                } else if (['file', 'resourcebooking', 'crm_product_row'].includes(field.type)) {
                    control = `Type ${escapeHtml(field.type)} not supported in example`
                } else if (field.type === 'date') {
                    control = input({ ...params, VALUE: value ? String(value).slice(0, 10) : '', TYPE: 'date' })
                } else if (field.type === 'datetime') {
                    control = input({ ...params, VALUE: value ? String(value).slice(0, 19) : '', TYPE: 'datetime-local' })
                } else if (['boolean', 'char'].includes(field.type)) {
                    control = input({ ...params, REQUIRED: false, VALUE: 'Y', CHECKED: value === 'Y', TYPE: 'checkbox' })
                } else if (['integer', 'double'].includes(field.type)) {
                    control = input({ ...params, TYPE: 'number', STEP: field.type === 'double' ? 'any' : '' })
                } else if (field.type === 'money') {
                    const [amount, currency] = String(value).split('|')
                    control = input({ ...params, VALUE: amount, TYPE: 'number', STEP: 'any' })
                    control += select({ ...params, NAME: `form[${key}_CURRENCY]`, VALUE: currency },
                        Object.fromEntries(currencies.map((row) => [row.CURRENCY, row.FULL_NAME])))
                } else {
                    control = input({ ...params, TYPE: 'text' })
                }

                const label = escapeHtml(field.formLabel || field.title || key)
                const block = `<div class="col-4 mt-3">${label}: </div><div class="col-6 mt-3">${control}</div>`
                if (key.startsWith('UF_')) custom += block
                else standard += block
            }

            res.send(`
                <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
                <div class="container"><form id="auto_form" method="post">
                    ${item.id ? `<input type="hidden" name="form[id]" value="${escapeHtml(item.id)}">` : ''}
                    <h2>System fields</h2><div class="row">${standard}</div>
                    <h2>Custom fields</h2><div class="row">${custom}</div>
                    <div class="row"><div class="col-sm-10 mt-5"><input type="submit" class="btn btn-primary" value="Save"></div></div>
                </form></div>
                <script>
                    const stagesByCategory = ${JSON.stringify(stagesByCategory).replace(/</g, '\u003c')}
                    const categoryField = document.getElementById('category-id')
                    const stageField = document.getElementById('stage-id')
                    categoryField?.addEventListener('change', () => {
                        stageField.innerHTML = ''
                        const emptyOption = document.createElement('option')
                        emptyOption.value = ''; emptyOption.textContent = '-- Select stage --'; emptyOption.disabled = true; emptyOption.selected = true
                        stageField.appendChild(emptyOption)
                        for (const [stageId, stageName] of Object.entries(stagesByCategory[categoryField.value] || {})) {
                            const option = document.createElement('option'); option.value = stageId; option.textContent = stageName; stageField.appendChild(option)
                        }
                    })
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
    import json
    from html import escape

    from flask import Flask, request
    from b24pysdk import BitrixWebhook, Client

    from save_form import save_form

    app = Flask(__name__)
    ENTITY_TYPE_ID = 2
    SKIPPED_FIELDS = {"contactId", "contacts"}
    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",
    ))

    def value_or_empty(value):
        return "" if value is None else value

    def input_field(params):
        value = params.get("VALUE")
        values = (
            [*(value if isinstance(value, list) else [value_or_empty(value)]), ""]
            if params.get("MULTIPLE") else [value_or_empty(value)]
        )
        html = ""
        for index, value in enumerate(values):
            html += f'<input class="form-control" name="{escape(params["NAME"])}{"[]" if params.get("MULTIPLE") else ""}"'
            html += f' type="{escape(params.get("TYPE", "text"))}"'
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
        html = f'<select class="form-control" name="{escape(params["NAME"])}{"[]" if params.get("MULTIPLE") else ""}"'
        if params.get("ID"):
            html += f' id="{escape(params["ID"])}"'
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
        selected_values = value if isinstance(value, list) else [value_or_empty(value)]
        selected_values = [str(value) for value in selected_values]
        for key, title in options.items():
            selected = " selected" if str(key) in selected_values else ""
            html += f'<option value="{escape(str(key))}"{selected}>{escape(str(title))}</option>'
        return html + "</select>"

    PAGE = """
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        <div class="container"><form id="auto_form" method="post">
            %(hidden_id)s
            <h2>System fields</h2><div class="row">%(standard)s</div>
            <h2>Custom fields</h2><div class="row">%(custom)s</div>
            <div class="row"><div class="col-sm-10 mt-5"><input type="submit" class="btn btn-primary" value="Save"></div></div>
        </form></div>
        <script>
            const stagesByCategory = %(stages)s;
            const categoryField = document.getElementById('category-id');
            const stageField = document.getElementById('stage-id');
            categoryField?.addEventListener('change', () => {
                stageField.innerHTML = '';
                const emptyOption = document.createElement('option'); emptyOption.value = ''; emptyOption.textContent = '-- Select stage --'; emptyOption.disabled = true; emptyOption.selected = true; stageField.appendChild(emptyOption);
                for (const [stageId, stageName] of Object.entries(stagesByCategory[categoryField.value] || {})) {
                    const option = document.createElement('option'); option.value = stageId; option.textContent = stageName; stageField.appendChild(option);
                }
            });
            document.getElementById('auto_form').addEventListener('submit', async (event) => {
                event.preventDefault();
                const body = new URLSearchParams(new FormData(event.currentTarget));
                const response = await fetch('/form', { method: 'POST', body });
                const json = await response.json();
                alert(json.message || json.error);
            });
        </script>
    """

    @app.route("/")
    def form_page():
        raw_id = request.args.get("ID", "0")
        try:
            item_id = int(raw_id)
        except ValueError:
            return "ID parameter must be an integer", 400

        fields = client.crm.item.fields(
            entity_type_id=ENTITY_TYPE_ID,
            use_original_uf_names=True,
        ).response.result["fields"]
        currencies = client.crm.currency.list().response.result
        item = {}
        if item_id > 0:
            item = client.crm.item.get(entity_type_id=ENTITY_TYPE_ID, bitrix_id=item_id, use_original_uf_names=True).response.result["item"]
        categories = client.crm.category.list(entity_type_id=ENTITY_TYPE_ID).response.result["categories"]
        default_category = next((row for row in categories if row["isDefault"] == "Y"), None)
        category_id = int(item["categoryId"]) if "categoryId" in item else (int(default_category["id"]) if default_category else None)
        if category_id is None:
            raise RuntimeError("Default funnel not found")
        category_options = {row["id"]: row["name"] for row in categories}
        stages_by_category = {}
        for category in categories:
            current_category_id = int(category["id"])
            status_type = "DEAL_STAGE" if current_category_id == 0 else f"DEAL_STAGE_{current_category_id}"
            rows = client.crm.status.list(filter={"ENTITY_ID": status_type}).response.result
            stages_by_category[str(current_category_id)] = {row["STATUS_ID"]: row["NAME"] for row in rows}

        standard = ""
        custom = ""
        for key, field in fields.items():
            if key in SKIPPED_FIELDS:
                continue
            value = category_id if key == "categoryId" else value_or_empty(item.get(key))
            params = {
                "NAME": f"form[{key}]", "VALUE": value,
                "REQUIRED": field.get("isRequired"), "DISABLE": field.get("isReadOnly"), "MULTIPLE": field.get("isMultiple"),
                "ID": "category-id" if key == "categoryId" else ("stage-id" if key == "stageId" else ""),
            }
            field_type = field.get("type")
            control = ""

            if key == "stageId":
                control = select_field({**params, "REQUIRED": True}, stages_by_category.get(str(category_id), {}))
            elif field_type == "crm_status":
                rows = client.crm.status.list(filter={"ENTITY_ID": field["statusType"]}).response.result
                control = select_field(params, {row["STATUS_ID"]: row["NAME"] for row in rows})
            elif field_type == "crm_category":
                control = select_field(params, category_options)
            elif field_type == "crm_currency":
                control = select_field(params, {row["CURRENCY"]: row["FULL_NAME"] for row in currencies})
            elif field_type == "enumeration":
                options = {
                    row.get("ID", row.get("id")): row.get("VALUE", row.get("value"))
                    for row in field.get("items", [])
                }
                control = select_field(params, options)
            elif field_type == "crm_company":
                control = input_field({**params, "TYPE": "number"})
                if value:
                    company = client.crm.item.get(entity_type_id=4, bitrix_id=int(value)).response.result["item"]
                    control += f" ({escape(company['title'])})"
            elif field_type == "crm_contact":
                control = input_field({**params, "TYPE": "number"})
                ids = [int(contact_id) for contact_id in (value if isinstance(value, list) else [value]) if contact_id]
                if ids:
                    contacts = client.crm.item.list(
                        entity_type_id=3,
                        filter={"@id": ids},
                        select=["id", "name", "lastName"],
                    ).response.result["items"]
                    names = [" ".join(filter(None, [row.get("name"), row.get("lastName")])) for row in contacts]
                    control += f" ({escape(', '.join(names))})"
            elif field_type == "user":
                control = input_field({**params, "TYPE": "number"})
                if value:
                    users = client.user.get(filter={"ID": value}).response.result
                    names = [" ".join(filter(None, [row.get("NAME"), row.get("LAST_NAME")])) for row in users]
                    control += f" ({escape(', '.join(names))})"
            elif field_type in ("file", "resourcebooking", "crm_product_row"):
                control = f"Type {escape(field_type)} not supported in example"
            elif field_type == "date":
                control = input_field({**params, "VALUE": str(value)[:10] if value else "", "TYPE": "date"})
            elif field_type == "datetime":
                control = input_field({**params, "VALUE": str(value)[:19] if value else "", "TYPE": "datetime-local"})
            elif field_type in ("boolean", "char"):
                control = input_field({**params, "REQUIRED": False, "VALUE": "Y", "CHECKED": value == "Y", "TYPE": "checkbox"})
            elif field_type in ("integer", "double"):
                control = input_field({**params, "TYPE": "number", "STEP": "any" if field_type == "double" else ""})
            elif field_type == "money":
                amount, _, currency = str(value).partition("|")
                control = input_field({**params, "VALUE": amount, "TYPE": "number", "STEP": "any"})
                control += select_field(
                    {**params, "NAME": f"form[{key}_CURRENCY]", "VALUE": currency},
                    {row["CURRENCY"]: row["FULL_NAME"] for row in currencies},
                )
            else:
                control = input_field({**params, "TYPE": "text"})

            label = escape(str(field.get("formLabel") or field.get("title") or key))
            block = f'<div class="col-4 mt-3">{label}: </div><div class="col-6 mt-3">{control}</div>'
            if key.startswith("UF_"):
                custom += block
            else:
                standard += block

        hidden_id = f'<input type="hidden" name="form[id]" value="{escape(str(item["id"]))}">' if item.get("id") else ""
        return PAGE % {
            "hidden_id": hidden_id, "standard": standard, "custom": custom,
            "stages": json.dumps(stages_by_category, ensure_ascii=False).replace("<", "\\u003c"),
        }

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

        const ENTITY_TYPE_ID = 2;
        $ID = (int)($_REQUEST['ID'] ?? 0);

        function callCore($sb, string $method, array $params): array
        {
            return $sb->core->call($method, $params)->getResponseData()->getResult();
        }

        function esc($value): string
        {
            return htmlspecialchars((string)$value, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8');
        }

        function inputField(array $params): string
        {
            $values = !empty($params['MULTIPLE'])
                ? array_merge((array)($params['VALUE'] ?? []), [''])
                : [$params['VALUE'] ?? ''];
            $html = '';
            foreach ($values as $index => $value)
            {
                $html .= '<input class="form-control" name="' . esc($params['NAME']) . (!empty($params['MULTIPLE']) ? '[]' : '') . '"';
                $html .= ' type="' . esc($params['TYPE'] ?? 'text') . '"';
                $html .= !empty($params['STEP']) ? ' step="' . esc($params['STEP']) . '"' : '';
                $html .= !empty($params['REQUIRED']) && $index === 0 ? ' required' : '';
                $html .= !empty($params['DISABLE']) ? ' disabled' : '';
                $html .= !empty($params['CHECKED']) ? ' checked' : '';
                $html .= ' value="' . esc($value) . '">';
            }
            return $html;
        }

        function selectField(array $params, array $options): string
        {
            $html = '<select class="form-control" name="' . esc($params['NAME']) . (!empty($params['MULTIPLE']) ? '[]' : '') . '"';
            $html .= !empty($params['ID']) ? ' id="' . esc($params['ID']) . '"' : '';
            $html .= !empty($params['REQUIRED']) ? ' required' : '';
            $html .= !empty($params['DISABLE']) ? ' disabled' : '';
            $html .= !empty($params['MULTIPLE']) ? ' multiple' : '';
            $html .= '>';
            if (empty($params['REQUIRED']) && empty($params['MULTIPLE']))
            {
                $html .= '<option value="">-- Not selected --</option>';
            }
            $selectedValues = array_map('strval', (array)($params['VALUE'] ?? []));
            foreach ($options as $key => $title)
            {
                $selected = in_array((string)$key, $selectedValues, true) ? ' selected' : '';
                $html .= '<option value="' . esc($key) . '"' . $selected . '>' . esc($title) . '</option>';
            }
            return $html . '</select>';
        }

        $fieldResult = callCore($sb, 'crm.item.fields', [
            'entityTypeId' => ENTITY_TYPE_ID,
            'useOriginalUfNames' => 'Y',
        ]);
        $fields = $fieldResult['fields'];
        $currencies = [];
        foreach ($crm->currency()->list([])->getCurrencies() as $currency)
        {
            $currencies[$currency->CURRENCY->getCode()] = $currency->FULL_NAME;
        }
        $item = [];
        if ($ID > 0)
        {
            $item = callCore($sb, 'crm.item.get', ['entityTypeId' => ENTITY_TYPE_ID, 'id' => $ID, 'useOriginalUfNames' => 'Y'])['item'];
        }
        $categories = callCore($sb, 'crm.category.list', ['entityTypeId' => ENTITY_TYPE_ID])['categories'];
        $defaultCategoryId = null;
        $categoryOptions = [];
        foreach ($categories as $category)
        {
            $categoryOptions[$category['id']] = $category['name'];
            if ($category['isDefault'] === 'Y') $defaultCategoryId = (int)$category['id'];
        }
        $categoryId = array_key_exists('categoryId', $item) ? (int)$item['categoryId'] : $defaultCategoryId;
        if ($categoryId === null) throw new RuntimeException('Default funnel not found');
        $stagesByCategory = [];
        foreach ($categories as $category)
        {
            $currentCategoryId = (int)$category['id'];
            $statusType = $currentCategoryId === 0 ? 'DEAL_STAGE' : 'DEAL_STAGE_' . $currentCategoryId;
            $stagesByCategory[$currentCategoryId] = [];
            foreach (callCore($sb, 'crm.status.list', ['filter' => ['ENTITY_ID' => $statusType]]) as $status)
            {
                $stagesByCategory[$currentCategoryId][$status['STATUS_ID']] = $status['NAME'];
            }
        }
        $stagesJson = json_encode($stagesByCategory, JSON_UNESCAPED_UNICODE | JSON_HEX_TAG | JSON_HEX_AMP | JSON_HEX_APOS | JSON_HEX_QUOT);

        $standard = '';
        $custom = '';
        foreach ($fields as $key => $field)
        {
            if (in_array($key, ['contactId', 'contacts'], true)) continue;
            $value = $key === 'categoryId' ? $categoryId : ($item[$key] ?? '');
            $params = [
                'NAME' => 'form[' . $key . ']', 'VALUE' => $value,
                'REQUIRED' => $field['isRequired'], 'DISABLE' => $field['isReadOnly'], 'MULTIPLE' => $field['isMultiple'],
                'ID' => $key === 'categoryId' ? 'category-id' : ($key === 'stageId' ? 'stage-id' : ''),
            ];
            $control = '';

            if ($key === 'stageId')
            {
                $control = selectField(['REQUIRED' => true] + $params, $stagesByCategory[$categoryId] ?? []);
            }
            elseif ($field['type'] === 'crm_status')
            {
                $options = [];
                foreach ($crm->status()->list([], ['ENTITY_ID' => $field['statusType']], [])->getStatuses() as $status) $options[$status->STATUS_ID] = $status->NAME;
                $control = selectField($params, $options);
            }
            elseif ($field['type'] === 'crm_category')
            {
                $control = selectField($params, $categoryOptions);
            }
            elseif ($field['type'] === 'crm_currency')
            {
                $control = selectField($params, $currencies);
            }
            elseif ($field['type'] === 'enumeration')
            {
                $options = [];
                foreach ($field['items'] ?? [] as $row)
                {
                    $options[$row['ID'] ?? $row['id']] = $row['VALUE'] ?? $row['value'];
                }
                $control = selectField($params, $options);
            }
            elseif ($field['type'] === 'crm_company')
            {
                $control = inputField(['TYPE' => 'number'] + $params);
                if ($value)
                {
                    $company = $crm->item()->get(4, (int)$value)->item();
                    $control .= ' (' . esc($company->title) . ')';
                }
            }
            elseif ($field['type'] === 'crm_contact')
            {
                $control = inputField(['TYPE' => 'number'] + $params);
                $ids = array_values(array_filter(array_map('intval', (array)$value)));
                if ($ids)
                {
                    $names = [];
                    foreach ($crm->item()->list(3, [], ['@id' => $ids], ['id', 'name', 'lastName'], 0)->getItems() as $contact)
                    {
                        $names[] = trim($contact->name . ' ' . $contact->lastName);
                    }
                    $control .= ' (' . esc(implode(', ', $names)) . ')';
                }
            }
            elseif ($field['type'] === 'user')
            {
                $control = inputField(['TYPE' => 'number'] + $params);
                if ($value)
                {
                    $names = [];
                    foreach ($sb->getUserScope()->user()->get([], ['ID' => $value], true)->getUsers() as $user)
                    {
                        $names[] = trim($user->NAME . ' ' . $user->LAST_NAME);
                    }
                    $control .= ' (' . esc(implode(', ', $names)) . ')';
                }
            }
            elseif (in_array($field['type'], ['file', 'resourcebooking', 'crm_product_row'], true))
            {
                $control = 'Type ' . esc($field['type']) . ' not supported in example';
            }
            elseif ($field['type'] === 'date')
            {
                $formatted = $value ? substr((string)$value, 0, 10) : '';
                $control = inputField(['TYPE' => 'date', 'VALUE' => $formatted] + $params);
            }
            elseif ($field['type'] === 'datetime')
            {
                $formatted = $value
                    ? (new DateTimeImmutable((string)$value))->format('Y-m-d\TH:i:s')
                    : '';
                $control = inputField(['TYPE' => 'datetime-local', 'VALUE' => $formatted] + $params);
            }
            elseif (in_array($field['type'], ['boolean', 'char'], true))
            {
                $control = inputField(['TYPE' => 'checkbox', 'VALUE' => 'Y', 'CHECKED' => $value === 'Y', 'REQUIRED' => false] + $params);
            }
            elseif (in_array($field['type'], ['integer', 'double'], true))
            {
                $control = inputField(['TYPE' => 'number', 'STEP' => $field['type'] === 'double' ? 'any' : ''] + $params);
            }
            elseif ($field['type'] === 'money')
            {
                [$amount, $currency] = array_pad(explode('|', (string)$value, 2), 2, '');
                $control = inputField(['TYPE' => 'number', 'STEP' => 'any', 'VALUE' => $amount] + $params);
                $control .= selectField(['NAME' => 'form[' . $key . '_CURRENCY]', 'VALUE' => $currency] + $params, $currencies);
            }
            else
            {
                $control = inputField($params + ['TYPE' => 'text']);
            }

            $label = esc($field['formLabel'] ?? $field['title'] ?? $key);
            $block = '<div class="col-4 mt-3">' . $label . ': </div><div class="col-6 mt-3">' . $control . '</div>';
            if (str_starts_with($key, 'UF_')) $custom .= $block;
            else $standard .= $block;
        }
    ?>
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        <div class="container">
            <form id="auto_form" method="post">
                <?php if (!empty($item['id'])): ?>
                    <input type="hidden" name="form[id]" value="<?= esc($item['id']) ?>">
                <?php endif; ?>
                <h2>System fields</h2><div class="row"><?= $standard ?></div>
                <h2>Custom fields</h2><div class="row"><?= $custom ?></div>
                <div class="row"><div class="col-sm-10 mt-5"><input type="submit" class="btn btn-primary" value="Save"></div></div>
            </form>
        </div>
        <script>
            const stagesByCategory = <?= $stagesJson ?>;
            const categoryField = document.getElementById('category-id');
            const stageField = document.getElementById('stage-id');
            categoryField?.addEventListener('change', () => {
                stageField.innerHTML = '';
                const emptyOption = document.createElement('option'); emptyOption.value = ''; emptyOption.textContent = '-- Select stage --'; emptyOption.disabled = true; emptyOption.selected = true; stageField.appendChild(emptyOption);
                for (const [stageId, stageName] of Object.entries(stagesByCategory[categoryField.value] || {})) {
                    const option = document.createElement('option'); option.value = stageId; option.textContent = stageName; stageField.appendChild(option);
                }
            });
            document.getElementById('auto_form').addEventListener('submit', async (event) => {
                event.preventDefault();
                const body = new URLSearchParams(new FormData(event.currentTarget));
                const response = await fetch('auto_form.php', { method: 'POST', body });
                const json = await response.json();
                alert(json.message || json.error);
            });
        </script>
    ```
{% endlist %}

## 3. Save the Form Data

The browser sends the form to the handler in `application/x-www-form-urlencoded` format. The handler calls [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) again to avoid trusting types from the client request.

For each writable field, the handler performs a conversion:

- `boolean` and `char` are passed as `Y` or `N`
- `crm_company` is passed as a single numeric identifier
- `crm_contact`, `user`, and other multiple fields are passed as an array
- `money` combines the amount and currency into an `amount|currency` string
- fields of types `file`, `resourcebooking`, and `crm_product_row` are skipped
- fields with `isReadOnly = true` are skipped
- a missing single field is passed as an empty string, and a missing multiple field is passed as an empty array

If the form contains `id`, the handler calls [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md). Without `id`, it calls [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md). In both cases, the identifier of the saved deal is located in `result.item.id`:

```json
{
    "result": {
        "item": {
            "id": 342,
            "title": "New deal (specifically for REST method examples)",
            "categoryId": 9,
            "stageId": "C9:UC_KN8KFI",
            "typeId": "SERVICE",
            "companyId": 5,
            "contactIds": [4, 5],
            "opportunity": 999.99,
            "isManualOpportunity": "Y",
            "currencyId": "EUR"
        }
    }
}
```

The next action requires `result.item.id`: add it to the `ID` parameter of the form address to open the created deal for editing.

### Full Handler Code

Save the handler to a file: JavaScript — `save-form.mjs`, PHP — `auto_form.php`, Python — `save_form.py`.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const ENTITY_TYPE_ID = 2
    const SKIPPED_FIELDS = new Set(['contactId', 'contacts'])
    const $b24 = B24Hook.fromWebhookUrl(process.env.B24_HOOK)

    async function call(method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })
        if (!response.isSuccess) throw new Error(response.getErrorMessages().join('; '))
        return response.getData().result
    }

    function asArray(value) {
        if (Array.isArray(value)) return value
        return value === undefined || value === null ? [] : [value]
    }

    export async function saveForm(req, res) {
        try {
            const submitted = req.body.form ?? {}
            const fieldResult = await call('crm.item.fields', {
                entityTypeId: ENTITY_TYPE_ID,
                useOriginalUfNames: 'Y',
            }, 'deal-fields-save')
            const fields = {}

            for (const [key, prop] of Object.entries(fieldResult.fields)) {
                if (SKIPPED_FIELDS.has(key) || prop.isReadOnly || ['file', 'resourcebooking', 'crm_product_row'].includes(prop.type)) continue
                if (['boolean', 'char'].includes(prop.type)) {
                    fields[key] = key in submitted ? 'Y' : 'N'
                    continue
                }
                if (!(key in submitted)) {
                    fields[key] = prop.isMultiple ? [] : ''
                    continue
                }

                let value = submitted[key]
                if (prop.type === 'money') {
                    value = `${value ?? ''}|${submitted[`${key}_CURRENCY`] ?? ''}`
                } else if (prop.type === 'crm_company') {
                    value = Number(value) || 0
                } else if (prop.type === 'crm_contact') {
                    value = asArray(value).map(Number).filter((itemId) => itemId > 0)
                } else if (prop.isMultiple) {
                    value = asArray(value).filter((item) => item !== '')
                }
                fields[key] = value
            }

            const id = parseInt(String(submitted.id ?? '0'), 10) || 0
            const method = id > 0 ? 'crm.item.update' : 'crm.item.add'
            const params = { entityTypeId: ENTITY_TYPE_ID, fields, useOriginalUfNames: 'Y' }
            if (id > 0) params.id = id
            const result = await call(method, params, `deal-${id > 0 ? 'update' : 'add'}`)
            res.json({ message: `Deal saved, ID: ${result.item.id}` })
        } catch (error) {
            res.status(400).json({ error: error.message })
        }
    }
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import re

    from flask import jsonify, request
    from b24pysdk import BitrixWebhook, Client

    ENTITY_TYPE_ID = 2
    SKIPPED_FIELDS = {"contactId", "contacts"}
    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",
    ))

    def values_for(name):
        return request.form.getlist(name) or request.form.getlist(f"{name}[]")

    def save_form():
        try:
            submitted = {}
            for full_key in request.form:
                match = re.match(r"^form\[([^]]+)](\[\])?$", full_key)
                if match:
                    submitted[match.group(1)] = values_for(full_key) if match.group(2) else request.form[full_key]

            field_result = client.crm.item.fields(
                entity_type_id=ENTITY_TYPE_ID,
                use_original_uf_names=True,
            ).response.result
            fields = {}
            for key, prop in field_result["fields"].items():
                if key in SKIPPED_FIELDS or prop.get("isReadOnly") or prop.get("type") in ("file", "resourcebooking", "crm_product_row"):
                    continue
                field_type = prop.get("type")
                if field_type in ("boolean", "char"):
                    fields[key] = "Y" if key in submitted else "N"
                    continue
                if key not in submitted:
                    fields[key] = [] if prop.get("isMultiple") else ""
                    continue

                value = submitted[key]
                if field_type == "money":
                    value = f"{value}|{submitted.get(f'{key}_CURRENCY', '')}"
                elif field_type == "crm_company":
                    value = int(value or 0)
                elif field_type == "crm_contact":
                    value = [int(item_id) for item_id in (value if isinstance(value, list) else [value]) if item_id]
                elif prop.get("isMultiple"):
                    value = [item for item in (value if isinstance(value, list) else [value]) if item != ""]
                fields[key] = value

            item_id = int(submitted.get("id") or 0)
            if item_id > 0:
                result = client.crm.item.update(
                    entity_type_id=ENTITY_TYPE_ID,
                    bitrix_id=item_id,
                    fields=fields,
                    use_original_uf_names=True,
                ).response.result
            else:
                result = client.crm.item.add(
                    entity_type_id=ENTITY_TYPE_ID,
                    fields=fields,
                    use_original_uf_names=True,
                ).response.result
            return jsonify(message=f"Deal saved, ID: {result['item']['id']}")
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
        const ENTITY_TYPE_ID = 2;
        header('Content-Type: application/json; charset=utf-8');

        function callCore($sb, string $method, array $params): array
        {
            return $sb->core->call($method, $params)->getResponseData()->getResult();
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
                if (in_array($key, ['contactId', 'contacts'], true)
                    || !empty($prop['isReadOnly'])
                    || in_array($prop['type'], ['file', 'resourcebooking', 'crm_product_row'], true))
                {
                    continue;
                }
                if (in_array($prop['type'], ['boolean', 'char'], true))
                {
                    $fields[$key] = array_key_exists($key, $submitted) ? 'Y' : 'N';
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
                elseif ($prop['type'] === 'crm_company')
                {
                    $value = (int)$value;
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
            if ($id > 0) $params['id'] = $id;
            $result = callCore($sb, $method, $params);
            echo json_encode(['message' => 'Deal saved, ID: ' . $result['item']['id']], JSON_UNESCAPED_UNICODE);
        }
        catch (Throwable $error)
        {
            http_response_code(400);
            echo json_encode(['error' => $error->getMessage()], JSON_UNESCAPED_UNICODE);
        }
    ```
{% endlist %}

## Verify the Result

1. Open the page without `ID`
2. Ensure that the default pipeline is selected in the pipeline field and the stage field contains only its stages
3. Fill in the title, deal type, and other required fields, then click **Save**
4. Add the identifier from the message to the `ID` parameter and open the form again
5. Compare the title, `categoryId`, `stageId`, `companyId`, `contactIds`, and custom fields with the entered values
6. Change the pipeline and ensure that the list of stages has updated. Select a stage from the new pipeline and save the deal
7. If the deal has no line items or if manual amount calculation is enabled, compare `opportunity` with the form value. If there are line items and manual amount calculation is disabled, check the recalculated amount separately for line items and delivery

For additional verification, call [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) with `entityTypeId = 2`, the deal identifier and `useOriginalUfNames = Y`.

## Errors and Diagnostics

#|
|| Symptom | Cause | What to check and how to proceed ||
|| Requests end with an authorization error | Webhook address is incorrect or the webhook has been deleted | Check the domain and `USER_ID/TOKEN`, if necessary create a new incoming webhook and retry opening the form ||
|| `The request requires higher privileges than provided by the webhook token.` | The webhook lacks the `user_brief` scope required for [user.get](../../../api-reference/user/user-get.md) | Add the `user_brief` scope to the webhook permissions and reload the form page ||
|| `ACCESS_DENIED` or `Access denied.` when opening or saving the form | The webhook user lacks permissions for the deal or access to CRM settings for [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md) | Grant read, add, and edit permissions for deals and access to CRM settings, then retry opening or saving the form ||
|| Deal does not open via `ID` | Identifier does not exist or the deal is unavailable to the user | Verify the identifier using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method as the same user, then open the form with an available `ID` ||
|| `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Field value does not match its type | Find the field in the error text, compare the value with `type`, `isMultiple` and the options from [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md), then correct the field and retry saving ||
|| After clearing all values of a multiple custom field of type "list", saving proceeds without error, but the values remain | The handler sends an empty array, but [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns the previous values | A separate verified approach is needed to clear the field; after doing so, call [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) again ||
|| The list of stages does not match the deal pipeline | The page is open with old data or stages were obtained via `stageId.statusType` instead of `categoryId` | Reload the page, check the deal's `categoryId` and the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) request with `DEAL_STAGE` or `DEAL_STAGE_{categoryId}`, then select the pipeline and stage again ||
|| The form is not submitted after changing the pipeline | A stage from the updated list has not been selected | Select a stage for the new pipeline and try saving again ||
|| The entered amount changed after saving | The deal has line items and manual amount calculation is turned off | Enable `isManualOpportunity` for a manual value or check the sum of line items and shipping, then reopen the deal ||
|| An unsupported type message is shown instead of the field | The example does not implement `file`, `resourcebooking`, or `crm_product_row` | Handle the field with separate code or leave it unchanged, then save the remaining fields ||
|| PHP reports an incompatible platform | The PHP version or extension is not compatible with B24PhpSDK | Run `php -v`, `php -m`, and `composer check-platform-reqs`, fix the environment, and restart the server ||
|#

## Key Considerations

- When selecting deal stages, do not use `stageId.statusType` without verification: the generator builds `DEAL_STAGE` or `DEAL_STAGE_{categoryId}` based on the current pipeline
- The `categoryId` field can be passed to [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) and [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md). When changing the pipeline, pass the stage from its respective list
- `typeId` refers to the `DEAL_TYPE` directory; values are retrieved via [crm.status.list](../../../api-reference/crm/status/crm-status-list.md)
- If a deal has product rows and `isManualOpportunity = N`, the entered value `opportunity` is replaced by the calculation of product rows and delivery after saving
- The form does not manage product rows. Retrieve them using the [crm.item.productrow.list](../../../api-reference/crm/universal/product-rows/crm-item-productrow-list.md) method and modify them using the [crm.item.productrow.set](../../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md) method with `ownerType = D` and the deal identifier
- Universal methods work with deal links via `companyId` and `contactIds`. The form does not send fields `contactId` and `contacts`
- The deal does not have a `fm` multi-field: edit phone numbers and email addresses via associated contacts or companies
- The `file` type in the example is not implemented. To upload, pass an array containing the filename and the content in Base64. [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns an object for files with keys `id`, `url`, and `urlMachine`. The `urlMachine` value may contain a valid webhook token in the path `/rest/{user_id}/{token}/`. Do not write file URLs from the response into logs, reports, or error messages, and do not pass them to third parties without removing the token
- The handler passes empty values for writable fields that are not present in the form. If a field needs to be retained without changes, add it to the skip fields list
- If all values are removed from a multiple user field of the "list" type, the handler will send an empty array. The [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) request will complete without an error, but [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) will return the previous values. A separate approach is required to clear such a field
- [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) returns paginated data. If a field has more than 50 associated items, add processing for subsequent pages
- [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md), [crm.status.list](../../../api-reference/crm/status/crm-status-list.md), and [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md) return no more than 50 records per single call. The example retrieves only the first page. If there are more than 50 pipelines in Bitrix24, request subsequent pages in your production application via `start`; use the same processing for directories and currencies when exceeding the limit
- In your production application, add CSRF verification, form access authorization, and secure error logging
