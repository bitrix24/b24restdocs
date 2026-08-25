# How to Create a Custom Contact Edit Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md), [`user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the scenario: a user with permissions to read, add, and edit contacts, read associated companies and leads, and access CRM settings. The `user_brief` scope is required to call [user.get](../../../api-reference/user/user-get.md)

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In this example, we will create a web form for adding and editing a contact. The generator retrieves field descriptions using [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) and selects an HTML form control based on the type of each writable field. As a result, custom fields appear in the form without code changes.

If you open the page without the `ID` parameter, the handler creates a contact. If you pass an identifier, such as `?ID=123`, the generator loads the contact using [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md), populates the form, and the handler updates that contact.

The example consists of two files:

- the generator retrieves field descriptions, contact data, pipelines, and lookup values, then renders the HTML form
- the handler validates the form data against the current field descriptions and saves the contact

A contact does not have a separate card title field. The `name` and `lastName` fields serve this purpose. The API does not return a separate full name field, so verify these two fields after saving.

## How the Scenario Works

1. [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) returns the contact field descriptions in `result.fields`
2. If the URL contains `ID`, [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns the contact values in `result.item`, including the `fm` multifield
3. [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) returns contact pipelines in `result.categories`
4. The generator replaces internal codes with readable names using additional methods
   - [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — the `STATUS_ID` and `NAME` fields for the salutation, contact type, source, and other CRM lookups
   - [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md) — the `CURRENCY` and `FULL_NAME` fields for currencies in custom fields of the `money` type
   - [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — the `id` and `title` fields of associated companies
   - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — the `title` field of the associated lead
   - [user.get](../../../api-reference/user/user-get.md) — the users' `ID`, `NAME`, and `LAST_NAME` fields
5. The handler retrieves the field descriptions again using [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) and converts the form values to the API types
6. For a new contact, [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) returns the created contact in `result.item`. For an existing contact, [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) returns the updated contact under the same key

## 1. Prepare the Environment

Create an [Incoming Webhook](../../../local-integrations/local-webhooks.md#incoming-webhook) with the `crm` and `user_brief` permissions.

{% note warning "Keep the Webhook Secret" %}

The webhook sends requests with the permissions of the user who created it. Do not add the webhook URL to a public repository, client-side JavaScript, or error messages.

{% endnote %}

Choose one language, create a separate folder for the example, and open a terminal in it. First save the code from steps 2 and 3, then run the startup command.

#|
|| Language | Form Generator | Handler ||
|| JavaScript | `form.mjs` | `save-form.mjs` ||
|| PHP | `index.php` | `auto_form.php` ||
|| Python | `app.py` | `save_form.py` ||
|#

Install the dependencies.

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

Before installing the dependencies, check the Node.js, PHP, and Python versions using `node --version`, `php -v`, and `python --version`, and list the PHP extensions using `php -m`. `@bitrix24/b24jssdk` supports Node.js 18, 20, 22, and later. B24PhpSDK version 3 requires PHP 8.4 or 8.5 and the `bcmath`, `curl`, `intl`, and `json` extensions. `b24pysdk` requires Python 3.9 or later. After installing the PHP dependencies, run `composer check-platform-reqs`.

Specify the webhook URL:

- in JavaScript, set the `B24_HOOK` environment variable
- in PHP, replace the full URL in `initFromWebhook`
- in Python, replace `your-domain.bitrix24.com` and `USER_ID/TOKEN`

In PHP and Python, the URL is specified twice: in the generator file and in the handler file. If you replace it in only one file, the form will open, but saving will not work.

Run the example. It is designed to run locally: the form page performs actions on behalf of the webhook user, so follow the requirements in the Key Considerations section before publishing it.

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
    php -d max_execution_time=0 -S localhost:8000
    ```
{% endlist %}

Two of these commands may look unusual. The generator builds the form using several consecutive requests, and the built-in PHP server can terminate the page after its 30-second limit. The `max_execution_time` parameter removes this limit for local runs. For the same reason, the Python example increases the client timeout: by default, `b24pysdk` waits about three seconds for a response, which can cause the request to fail on a busy Bitrix24 account.

The form pages will be available at these URLs:

#|
|| Language | New Contact | Contact with ID `123` ||
|| JavaScript | `http://localhost:3000/` | `http://localhost:3000/?ID=123` ||
|| PHP | `http://localhost:8000/index.php` | `http://localhost:8000/index.php?ID=123` ||
|| Python | `http://localhost:5000/` | `http://localhost:5000/?ID=123` ||
|#

## 2. Create the Form

The generator passes two parameters to [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md):

- `entityTypeId: 3` — the contact object type identifier from the [CRM Object Type](../../../api-reference/crm/data-types.md#object_type) table
- `useOriginalUfNames: Y` — return the original names of custom fields `UF_*`

In the Python SDK, the `useOriginalUfNames` parameter is named `use_original_uf_names` and accepts the Boolean value `True`.

The verified [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) response is shortened to the fields relevant to a contact:

```json
{
  "result": {
    "fields": {
      "name": {
        "type": "string",
        "isRequired": false,
        "isReadOnly": false,
        "isMultiple": false
      },
      "categoryId": {
        "type": "crm_category",
        "isRequired": false,
        "isReadOnly": false,
        "isMultiple": false
      },
      "companyIds": {
        "type": "crm_company",
        "isRequired": false,
        "isReadOnly": false,
        "isMultiple": true
      },
      "fm": {
        "type": "crm_multifield",
        "isRequired": false,
        "isReadOnly": false,
        "isMultiple": true
      }
    }
  }
}
```

Other response properties are omitted.

The verified [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) response for the contact being edited is shortened to the fields used by the form:

```json
{
  "result": {
    "item": {
      "id": 123,
      "name": "Klaus",
      "lastName": "Weber",
      "categoryId": 0,
      "companyIds": [27, 31],
      "leadId": 17,
      "fm": [
        {
          "id": 451,
          "typeId": "PHONE",
          "valueType": "WORK",
          "value": "+49 30 12345678"
        }
      ]
    }
  }
}
```

The `fm` field returns the numeric `id` of an existing value, while `companyIds` returns an array of company identifiers.

The contact description contains standard fields, links, lookups, and automatically calculated values.

#|
|| Field | Type or Attribute | How the Form Uses It ||
|| `honorific` | `crm_status`, the `HONORIFIC` lookup | Displays a list of salutations ||
|| `name`, `lastName` | Strings | Displays the first and last name fields ||
|| `fullName` | The `Hidden`, `ReadOnly`, and `AutoGenerated` attributes | Is not returned in either `result.fields` from [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) or `result.item` from [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md); the form neither displays nor sends it ||
|| `photo` | `file`, image | Displays an unsupported type message and does not send the field ||
|| `typeId` | `crm_status`, the `CONTACT_TYPE` lookup | Displays a list of contact types ||
|| `sourceId` | `crm_status`, the `SOURCE` lookup | Displays a list of sources ||
|| `comments` | BBCode-formatted text | Displays a multiline field ||
|| `opened` | Required Boolean field | Always displays a checkbox and sends `Y` or `N` ||
|| `assignedById` | User; the value cannot be cleared | Displays a numeric field and the responsible person's name ||
|| `companyIds` | Multiple links to companies | Displays identifiers in link order and company names ||
|| `leadId` | Link to a lead | Displays the identifier and attempts to retrieve the lead name ||
|| `categoryId` | Contact pipeline | Displays a list from [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md) ||
|| `fm` | Multifield for phone numbers, email addresses, websites, and messengers | Displays rows containing `id`, `typeId`, `valueType`, and `value` ||
|#

The `birthdaySort` field is also hidden by the `Hidden` attribute and is absent from `result.fields` in the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) response. The `hasPhone`, `hasEmail`, `hasImol`, `id`, `createdBy`, `updatedBy`, `createdTime`, `updatedTime`, and `lastCommunicationTime` fields are read-only. The generator skips them based on `isReadOnly`, and the handler does not send them. The `webformId` field is returned in the description with `isImmutable = true`: it can be set only when adding a contact, while an update returns the previous value without an error. The generator and handler skip these fields as well.

Contacts have pipelines but no stages or Sales Funnels. The pipeline list includes the default pipeline with `id = 0` and custom pipelines. For a new contact, the example selects `categoryId = 0`.

The `NotDisplayed` attribute describes how `categoryId` appears in the standard card; it does not prevent the field from being used through the universal REST API. The `categoryId` field is returned by [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) and accepted by [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) and [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md), so the form displays it.

Comments in `comments` are stored as BBCode. Plain text and BBCode are retained as entered, while Bitrix24 removes HTML tags and keeps only their content: `<strong>Important contact</strong>` is saved as `Important contact`. To apply emphasis, pass `[b]Important contact[/b]`.

You can create contact custom fields using [crm.contact.userfield.add](../../../api-reference/crm/contacts/userfield/crm-contact-userfield-add.md) and retrieve them using [crm.contact.userfield.list](../../../api-reference/crm/contacts/userfield/crm-contact-userfield-list.md). For the `money` type, the generator also retrieves currencies using [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md).

### How the Form Handles Multifields

The `fm` field combines phone numbers, email addresses, websites, and messengers. The example supports the `PHONE`, `EMAIL`, `WEB`, and `IM` values for `typeId`. The `value` format depends on the type: a phone number, email address, URL, or messenger identifier.

The generator displays each existing value and stores its numeric `id` in a hidden field. After the existing values, it adds three empty rows for new data.

The generator maps field types to form controls.

#|
|| Field Type | Form Control ||
|| `crm_status`, `crm_currency`, `crm_category`, `enumeration` | `<select>` with lookup values ||
|| `crm_company` | One or more numeric fields and the current company names ||
|| `crm_lead` | A numeric field and the current lead name, if available ||
|| `user` | A numeric field and the user name ||
|| `crm_multifield` | Rows containing `typeId`, `valueType`, and `value`, with `id` retained ||
|| `date`, `datetime` | `date` and `datetime-local` without changing the time zone ||
|| `boolean`, `char` | Checkbox ||
|| `integer`, `double` | Numeric field ||
|| `money` | Amount and currency list ||
|| `file`, `resourcebooking` | Unsupported type message ||
|| Other Types | Text field ||
|#

If the webhook user cannot read the associated lead, the form still opens. The generator keeps the `leadId` field and adds a diagnostic message instead of the lead name.

{% include [Example Note](../../../_includes/examples.md) %}

### Full Form Generator Code

Save the code to the generator file: JavaScript — `form.mjs`, PHP — `index.php`, Python — `app.py`.

{% list tabs %}

- JS

    ```javascript
    import express from 'express'
    import { B24Hook } from '@bitrix24/b24jssdk'
    import { saveForm } from './save-form.mjs'

    const ENTITY_TYPE_ID = 3
    const SKIPPED_FIELDS = new Set([
        'companyId', 'companies', 'hasPhone', 'hasEmail', 'hasImol',
        'birthdaySort', 'fullName', 'id', 'createdBy', 'updatedBy',
        'createdTime', 'updatedTime', 'lastCommunicationTime',
    ])
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
        const current = params.MULTIPLE
            ? (Array.isArray(params.VALUE) ? params.VALUE : [params.VALUE ?? ''])
            : [params.VALUE ?? '']
        const values = params.MULTIPLE ? [...current, ''] : current
        return values.map((value, index) => {
            let html = '<input class="form-control"'
            html += ` name="${escapeHtml(params.NAME)}${params.MULTIPLE ? '[]' : ''}"`
            html += ` type="${escapeHtml(params.TYPE || 'text')}"`
            if (params.STEP) html += ` step="${escapeHtml(params.STEP)}"`
            if (params.REQUIRED && index === 0) html += ' required'
            if (params.CHECKED) html += ' checked'
            return html + ` value="${escapeHtml(value)}">`
        }).join('')
    }

    function textarea(params) {
        return `<textarea class="form-control" name="${escapeHtml(params.NAME)}"${params.REQUIRED ? ' required' : ''}>${escapeHtml(params.VALUE)}</textarea>`
    }

    function select(params, options) {
        let html = `<select class="form-control" name="${escapeHtml(params.NAME)}${params.MULTIPLE ? '[]' : ''}"`
        if (params.REQUIRED) html += ' required'
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

    function multifields(values) {
        const rows = [...(Array.isArray(values) ? values : [])]
        rows.push(...Array.from({ length: 3 }, () => ({
            id: '', typeId: 'PHONE', valueType: 'WORK', value: '',
        })))
        return rows.map((row, index) => {
            const typeOptions = ['PHONE', 'EMAIL', 'WEB', 'IM']
                .map((type) => `<option value="${type}"${row.typeId === type ? ' selected' : ''}>${type}</option>`)
                .join('')
            const itemId = row.id ?? ''
            return `<div class="border rounded p-2 mb-2">
                <input type="hidden" name="fm[${index}][id]" value="${escapeHtml(itemId)}">
                <select class="form-control mb-1" name="fm[${index}][typeId]">${typeOptions}</select>
                <input class="form-control mb-1" name="fm[${index}][valueType]" value="${escapeHtml(row.valueType ?? 'WORK')}" placeholder="WORK">
                <input class="form-control mb-1" name="fm[${index}][value]" value="${escapeHtml(row.value ?? '')}" placeholder="Value">
                ${itemId ? `<label><input type="checkbox" name="fm[${index}][delete]" value="Y"> Delete</label>` : ''}
            </div>`
        }).join('')
    }

    app.get('/', async (req, res) => {
        const rawId = String(req.query.ID ?? '').trim()
        const id = /^\d+$/.test(rawId) ? Number(rawId) : 0
        try {
            const fields = (await call('crm.item.fields', {
                entityTypeId: ENTITY_TYPE_ID,
                useOriginalUfNames: 'Y',
            }, 'contact-fields')).fields
            const currencies = await call('crm.currency.list', {}, 'currencies')
            const categories = (await call('crm.category.list', {
                entityTypeId: ENTITY_TYPE_ID,
            }, 'contact-categories')).categories
            const item = id > 0
                ? (await call('crm.item.get', {
                    entityTypeId: ENTITY_TYPE_ID,
                    id,
                    useOriginalUfNames: 'Y',
                }, 'contact-get')).item
                : {}
            const categoryOptions = Object.fromEntries(
                categories.map((category) => [category.id, category.name]),
            )
            const diagnostics = []
            let standard = ''
            let custom = ''

            for (const [key, field] of Object.entries(fields)) {
                if (field.isReadOnly || field.isImmutable || SKIPPED_FIELDS.has(key)) continue
                const value = key === 'categoryId' ? (item[key] ?? 0) : (item[key] ?? '')
                const params = {
                    NAME: `form[${key}]`, VALUE: value,
                    REQUIRED: field.isRequired, MULTIPLE: field.isMultiple,
                }
                let control = ''

                if (field.type === 'crm_status') {
                    const rows = await call('crm.status.list', {
                        filter: { ENTITY_ID: field.statusType },
                    }, `status-${key}`)
                    control = select(params, Object.fromEntries(
                        rows.map((row) => [row.STATUS_ID, row.NAME]),
                    ))
                } else if (field.type === 'crm_category') {
                    control = select(params, categoryOptions)
                } else if (field.type === 'crm_currency') {
                    control = select(params, Object.fromEntries(
                        currencies.map((row) => [row.CURRENCY, row.FULL_NAME]),
                    ))
                } else if (field.type === 'enumeration') {
                    const options = Object.fromEntries((field.items || []).map(
                        (row) => [row.ID ?? row.id, row.VALUE ?? row.value],
                    ))
                    control = select(params, options)
                } else if (field.type === 'crm_multifield') {
                    control = multifields(value)
                } else if (field.type === 'crm_company') {
                    control = input({ ...params, TYPE: 'number' })
                    const ids = (Array.isArray(value) ? value : [value])
                        .map(Number).filter((companyId) => companyId > 0)
                    if (ids.length) {
                        const companies = (await call('crm.item.list', {
                            entityTypeId: 4,
                            filter: { '@id': ids },
                            select: ['id', 'title'],
                        }, `companies-${key}`)).items
                        const titleById = Object.fromEntries(
                            companies.map((company) => [String(company.id), company.title]),
                        )
                        const titles = ids.map((companyId) => titleById[String(companyId)] || `ID ${companyId}`)
                        control += ` (${escapeHtml(titles.join(', '))})`
                    }
                } else if (field.type === 'crm_lead') {
                    control = input({ ...params, TYPE: 'number' })
                    if (Number(value) > 0) {
                        try {
                            const lead = (await call('crm.item.get', {
                                entityTypeId: 1,
                                id: Number(value),
                            }, `lead-${key}`)).item
                            control += ` (${escapeHtml(lead.title)})`
                        } catch (error) {
                            diagnostics.push(`Lead name for ID ${value} is unavailable: ${error.message}`)
                        }
                    }
                } else if (field.type === 'user') {
                    control = input({ ...params, TYPE: 'number' })
                    const ids = (Array.isArray(value) ? value : [value])
                        .map(Number).filter((userId) => userId > 0)
                    if (ids.length) {
                        const users = await call('user.get', {
                            filter: { ID: ids },
                        }, `users-${key}`)
                        const names = users.map((user) => [user.NAME, user.LAST_NAME]
                            .filter((part) => part !== null && part !== undefined && part !== '')
                            .join(' '))
                        control += ` (${escapeHtml(names.join(', '))})`
                    }
                } else if (key === 'comments') {
                    control = textarea(params)
                } else if (['file', 'resourcebooking'].includes(field.type)) {
                    control = `The ${escapeHtml(field.type)} type is not supported in this example`
                } else if (field.type === 'date') {
                    control = input({
                        ...params, VALUE: value ? String(value).slice(0, 10) : '', TYPE: 'date',
                    })
                } else if (field.type === 'datetime') {
                    control = input({
                        ...params, VALUE: value ? String(value).slice(0, 19) : '', TYPE: 'datetime-local',
                    })
                } else if (['boolean', 'char'].includes(field.type)) {
                    control = input({
                        ...params, REQUIRED: false, VALUE: 'Y',
                        CHECKED: value === 'Y', TYPE: 'checkbox',
                    })
                } else if (['integer', 'double'].includes(field.type)) {
                    control = input({
                        ...params, TYPE: 'number', STEP: field.type === 'double' ? 'any' : '',
                    })
                } else if (field.type === 'money') {
                    const [amount, currency] = String(value).split('|')
                    control = input({ ...params, VALUE: amount, TYPE: 'number', STEP: 'any' })
                    control += select({
                        ...params, NAME: `form[${key}_CURRENCY]`, VALUE: currency,
                    }, Object.fromEntries(
                        currencies.map((row) => [row.CURRENCY, row.FULL_NAME]),
                    ))
                } else {
                    control = input({ ...params, TYPE: 'text' })
                }

                const label = escapeHtml(field.formLabel || field.title || key)
                const block = `<div class="col-4 mt-3">${label}: </div><div class="col-6 mt-3">${control}</div>`
                if (key.startsWith('UF_')) custom += block
                else standard += block
            }

            const diagnosticHtml = diagnostics.length
                ? `<div class="alert alert-warning">${diagnostics.map(escapeHtml).join('<br>')}</div>`
                : ''
            res.send(`
                <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
                <div class="container"><form id="auto_form" method="post">
                    ${item.id ? `<input type="hidden" name="form[id]" value="${escapeHtml(item.id)}">` : ''}
                    ${diagnosticHtml}
                    <h2>System Fields</h2><div class="row">${standard}</div>
                    <h2>Custom Fields</h2><div class="row">${custom}</div>
                    <div class="row"><div class="col-sm-10 mt-5"><input type="submit" class="btn btn-primary" value="Save"></div></div>
                </form></div>
                <script>
                    document.getElementById('auto_form').addEventListener('submit', async (event) => {
                        event.preventDefault()
                        const body = new URLSearchParams(new FormData(event.currentTarget))
                        try {
                            const response = await fetch('/form', { method: 'POST', body })
                            const text = await response.text()
                            try {
                                const json = JSON.parse(text)
                                alert(json.message || json.error || ('Response code ' + response.status))
                            } catch (parseError) {
                                alert('The server response is not JSON, code ' + response.status + ': ' + text.slice(0, 200))
                            }
                        } catch (networkError) {
                            alert('Could not submit the form: ' + networkError.message)
                        }
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
    from html import escape

    from flask import Flask, request
    from b24pysdk import BitrixWebhook, Client

    from save_form import save_form

    app = Flask(__name__)
    ENTITY_TYPE_ID = 3
    SKIPPED_FIELDS = {
        "companyId", "companies", "hasPhone", "hasEmail", "hasImol",
        "birthdaySort", "fullName", "id", "createdBy", "updatedBy",
        "createdTime", "updatedTime", "lastCommunicationTime",
    }
    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",
    ), timeout=60)


    def value_or_empty(value):
        return "" if value is None else value


    def input_field(params):
        value = params.get("VALUE")
        current = value if isinstance(value, list) else [value_or_empty(value)]
        values = [*current, ""] if params.get("MULTIPLE") else current
        html = ""
        for index, item in enumerate(values):
            html += f'<input class="form-control" name="{escape(params["NAME"])}{"[]" if params.get("MULTIPLE") else ""}"'
            html += f' type="{escape(params.get("TYPE", "text"))}"'
            if params.get("STEP"):
                html += f' step="{escape(params["STEP"])}"'
            if params.get("REQUIRED") and index == 0:
                html += " required"
            if params.get("CHECKED"):
                html += " checked"
            html += f' value="{escape(str(value_or_empty(item)))}">'
        return html


    def textarea_field(params):
        required = " required" if params.get("REQUIRED") else ""
        return (
            f'<textarea class="form-control" name="{escape(params["NAME"])}"{required}>'
            f'{escape(str(value_or_empty(params.get("VALUE"))))}</textarea>'
        )


    def select_field(params, options):
        html = f'<select class="form-control" name="{escape(params["NAME"])}{"[]" if params.get("MULTIPLE") else ""}"'
        if params.get("REQUIRED"):
            html += " required"
        if params.get("MULTIPLE"):
            html += " multiple"
        html += ">"
        if not params.get("REQUIRED") and not params.get("MULTIPLE"):
            html += '<option value="">-- Not selected --</option>'
        value = params.get("VALUE")
        selected_values = value if isinstance(value, list) else [value_or_empty(value)]
        selected_values = [str(item) for item in selected_values]
        for key, title in options.items():
            selected = " selected" if str(key) in selected_values else ""
            html += f'<option value="{escape(str(key))}"{selected}>{escape(str(title))}</option>'
        return html + "</select>"


    def multifields(values):
        rows = list(values) if isinstance(values, list) else []
        rows.extend(
            {"id": "", "typeId": "PHONE", "valueType": "WORK", "value": ""}
            for _ in range(3)
        )
        html = ""
        for index, row in enumerate(rows):
            options = "".join(
                f'<option value="{field_type}"{" selected" if row.get("typeId") == field_type else ""}>{field_type}</option>'
                for field_type in ("PHONE", "EMAIL", "WEB", "IM")
            )
            item_id = value_or_empty(row.get("id"))
            delete = (
                f'<label><input type="checkbox" name="fm[{index}][delete]" value="Y"> Delete</label>'
                if item_id != "" else ""
            )
            value_type = value_or_empty(row.get("valueType"))
            if value_type == "":
                value_type = "WORK"
            html += f"""<div class="border rounded p-2 mb-2">
                <input type="hidden" name="fm[{index}][id]" value="{escape(str(item_id))}">
                <select class="form-control mb-1" name="fm[{index}][typeId]">{options}</select>
                <input class="form-control mb-1" name="fm[{index}][valueType]" value="{escape(str(value_type))}" placeholder="WORK">
                <input class="form-control mb-1" name="fm[{index}][value]" value="{escape(str(value_or_empty(row.get('value'))))}" placeholder="Value">
                {delete}
            </div>"""
        return html


    PAGE = """
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        <div class="container"><form id="auto_form" method="post">
            %(hidden_id)s
            %(diagnostics)s
            <h2>System Fields</h2><div class="row">%(standard)s</div>
            <h2>Custom Fields</h2><div class="row">%(custom)s</div>
            <div class="row"><div class="col-sm-10 mt-5"><input type="submit" class="btn btn-primary" value="Save"></div></div>
        </form></div>
        <script>
            document.getElementById('auto_form').addEventListener('submit', async (event) => {
                event.preventDefault();
                const body = new URLSearchParams(new FormData(event.currentTarget));
                try {
                    const response = await fetch('/form', { method: 'POST', body });
                    const text = await response.text();
                    try {
                        const json = JSON.parse(text);
                        alert(json.message || json.error || ('Response code ' + response.status));
                    } catch (parseError) {
                        alert('The server response is not JSON, code ' + response.status + ': ' + text.slice(0, 200));
                    }
                } catch (networkError) {
                    alert('Could not submit the form: ' + networkError.message);
                }
            });
        </script>
    """


    @app.route("/")
    def form_page():
        raw_id = request.args.get("ID", "0")
        try:
            item_id = int(raw_id)
        except (TypeError, ValueError):
            item_id = 0

        fields = client.crm.item.fields(
            entity_type_id=ENTITY_TYPE_ID,
            use_original_uf_names=True,
        ).response.result["fields"]
        currencies = client.crm.currency.list().response.result
        categories = client.crm.category.list(
            entity_type_id=ENTITY_TYPE_ID,
        ).response.result["categories"]
        item = {}
        if item_id > 0:
            item = client.crm.item.get(
                entity_type_id=ENTITY_TYPE_ID,
                bitrix_id=item_id,
                use_original_uf_names=True,
            ).response.result["item"]
        currency_options = {
            row["CURRENCY"]: row["FULL_NAME"] for row in currencies
        }
        category_options = {row["id"]: row["name"] for row in categories}
        diagnostics = []
        standard = ""
        custom = ""

        for key, field in fields.items():
            if field.get("isReadOnly") or field.get("isImmutable") or key in SKIPPED_FIELDS:
                continue
            value = item.get(key, 0) if key == "categoryId" else value_or_empty(item.get(key))
            params = {
                "NAME": f"form[{key}]", "VALUE": value,
                "REQUIRED": field.get("isRequired"), "MULTIPLE": field.get("isMultiple"),
            }
            field_type = field.get("type")
            control = ""

            if field_type == "crm_status":
                rows = client.crm.status.list(
                    filter={"ENTITY_ID": field["statusType"]},
                ).response.result
                control = select_field(params, {
                    row["STATUS_ID"]: row["NAME"] for row in rows
                })
            elif field_type == "crm_category":
                control = select_field(params, category_options)
            elif field_type == "crm_currency":
                control = select_field(params, currency_options)
            elif field_type == "enumeration":
                control = select_field(params, {
                    row.get("ID", row.get("id")): row.get("VALUE", row.get("value"))
                    for row in field.get("items", [])
                })
            elif field_type == "crm_multifield":
                control = multifields(value)
            elif field_type == "crm_company":
                control = input_field({**params, "TYPE": "number"})
                ids = [
                    int(company_id) for company_id in (value if isinstance(value, list) else [value])
                    if str(company_id).isdigit() and int(company_id) > 0
                ]
                if ids:
                    companies = client.crm.item.list(
                        entity_type_id=4,
                        filter={"@id": ids},
                        select=["id", "title"],
                    ).response.result["items"]
                    title_by_id = {str(company["id"]): company["title"] for company in companies}
                    titles = [title_by_id.get(str(company_id), f"ID {company_id}") for company_id in ids]
                    control += f" ({escape(', '.join(titles))})"
            elif field_type == "crm_lead":
                control = input_field({**params, "TYPE": "number"})
                if str(value).isdigit() and int(value) > 0:
                    try:
                        lead = client.crm.item.get(
                            entity_type_id=1,
                            bitrix_id=int(value),
                        ).response.result["item"]
                        control += f" ({escape(str(lead['title']))})"
                    except Exception as error:
                        diagnostics.append(f"Lead name for ID {value} is unavailable: {error}")
            elif field_type == "user":
                control = input_field({**params, "TYPE": "number"})
                ids = [
                    int(user_id) for user_id in (value if isinstance(value, list) else [value])
                    if str(user_id).isdigit() and int(user_id) > 0
                ]
                if ids:
                    users = client.user.get(filter={"ID": ids}).response.result
                    names = [
                        " ".join(
                            str(part) for part in (user.get("NAME"), user.get("LAST_NAME"))
                            if part not in (None, "")
                        )
                        for user in users
                    ]
                    control += f" ({escape(', '.join(names))})"
            elif key == "comments":
                control = textarea_field(params)
            elif field_type in ("file", "resourcebooking"):
                control = f"The {escape(str(field_type))} type is not supported in this example"
            elif field_type == "date":
                control = input_field({
                    **params, "VALUE": str(value)[:10] if value != "" else "", "TYPE": "date",
                })
            elif field_type == "datetime":
                control = input_field({
                    **params, "VALUE": str(value)[:19] if value != "" else "", "TYPE": "datetime-local",
                })
            elif field_type in ("boolean", "char"):
                control = input_field({
                    **params, "REQUIRED": False, "VALUE": "Y",
                    "CHECKED": value == "Y", "TYPE": "checkbox",
                })
            elif field_type in ("integer", "double"):
                control = input_field({
                    **params, "TYPE": "number", "STEP": "any" if field_type == "double" else "",
                })
            elif field_type == "money":
                amount, _, currency = str(value).partition("|")
                control = input_field({
                    **params, "VALUE": amount, "TYPE": "number", "STEP": "any",
                })
                control += select_field({
                    **params, "NAME": f"form[{key}_CURRENCY]", "VALUE": currency,
                }, currency_options)
            else:
                control = input_field({**params, "TYPE": "text"})

            label = escape(str(field.get("formLabel") or field.get("title") or key))
            block = f'<div class="col-4 mt-3">{label}: </div><div class="col-6 mt-3">{control}</div>'
            if key.startswith("UF_"):
                custom += block
            else:
                standard += block

        hidden_id = (
            f'<input type="hidden" name="form[id]" value="{escape(str(item["id"]))}">'
            if "id" in item else ""
        )
        diagnostic_html = (
            '<div class="alert alert-warning">'
            + "<br>".join(escape(str(message)) for message in diagnostics)
            + "</div>"
            if diagnostics else ""
        )
        return PAGE % {
            "hidden_id": hidden_id, "diagnostics": diagnostic_html,
            "standard": standard, "custom": custom,
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

        const ENTITY_TYPE_ID = 3;
        const SKIPPED_FIELDS = [
            'companyId', 'companies', 'hasPhone', 'hasEmail', 'hasImol',
            'birthdaySort', 'fullName', 'id', 'createdBy', 'updatedBy',
            'createdTime', 'updatedTime', 'lastCommunicationTime',
        ];
        $rawId = trim((string)($_GET['ID'] ?? ''));
        $ID = preg_match('/^\d+$/D', $rawId) === 1 ? (int)$rawId : 0;

        function callCore($sb, string $method, array $params): array
        {
            return $sb->core->call($method, $params)->getResponseData()->getResult();
        }

        function esc($value): string
        {
            return htmlspecialchars((string)($value ?? ''), ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8');
        }

        function inputField(array $params): string
        {
            $current = !empty($params['MULTIPLE'])
                ? (is_array($params['VALUE']) ? $params['VALUE'] : [$params['VALUE'] ?? ''])
                : [$params['VALUE'] ?? ''];
            $values = !empty($params['MULTIPLE']) ? array_merge($current, ['']) : $current;
            $html = '';
            foreach ($values as $index => $value)
            {
                $html .= '<input class="form-control" name="' . esc($params['NAME'])
                    . (!empty($params['MULTIPLE']) ? '[]' : '') . '"';
                $html .= ' type="' . esc($params['TYPE'] ?? 'text') . '"';
                $html .= !empty($params['STEP']) ? ' step="' . esc($params['STEP']) . '"' : '';
                $html .= !empty($params['REQUIRED']) && $index === 0 ? ' required' : '';
                $html .= !empty($params['CHECKED']) ? ' checked' : '';
                $html .= ' value="' . esc($value) . '">';
            }
            return $html;
        }

        function textareaField(array $params): string
        {
            return '<textarea class="form-control" name="' . esc($params['NAME']) . '"'
                . (!empty($params['REQUIRED']) ? ' required' : '') . '>'
                . esc($params['VALUE'] ?? '') . '</textarea>';
        }

        function selectField(array $params, array $options): string
        {
            $html = '<select class="form-control" name="' . esc($params['NAME'])
                . (!empty($params['MULTIPLE']) ? '[]' : '') . '"';
            $html .= !empty($params['REQUIRED']) ? ' required' : '';
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
                $html .= '<option value="' . esc($key) . '"' . $selected . '>'
                    . esc($title) . '</option>';
            }
            return $html . '</select>';
        }

        function multifields($values): string
        {
            $rows = is_array($values) ? $values : [];
            for ($i = 0; $i < 3; $i++)
            {
                $rows[] = ['id' => '', 'typeId' => 'PHONE', 'valueType' => 'WORK', 'value' => ''];
            }
            $html = '';
            foreach ($rows as $index => $row)
            {
                $options = '';
                foreach (['PHONE', 'EMAIL', 'WEB', 'IM'] as $type)
                {
                    $selected = ($row['typeId'] ?? '') === $type ? ' selected' : '';
                    $options .= '<option value="' . $type . '"' . $selected . '>' . $type . '</option>';
                }
                $itemId = $row['id'] ?? '';
                $html .= '<div class="border rounded p-2 mb-2">';
                $html .= '<input type="hidden" name="fm[' . $index . '][id]" value="' . esc($itemId) . '">';
                $html .= '<select class="form-control mb-1" name="fm[' . $index . '][typeId]">' . $options . '</select>';
                $html .= '<input class="form-control mb-1" name="fm[' . $index . '][valueType]" value="'
                    . esc($row['valueType'] ?? 'WORK') . '" placeholder="WORK">';
                $html .= '<input class="form-control mb-1" name="fm[' . $index . '][value]" value="'
                    . esc($row['value'] ?? '') . '" placeholder="Value">';
                if ($itemId !== '' && $itemId !== null)
                {
                    $html .= '<label><input type="checkbox" name="fm[' . $index
                        . '][delete]" value="Y"> Delete</label>';
                }
                $html .= '</div>';
            }
            return $html;
        }

        $fields = callCore($sb, 'crm.item.fields', [
            'entityTypeId' => ENTITY_TYPE_ID,
            'useOriginalUfNames' => 'Y',
        ])['fields'];
        $currencies = callCore($sb, 'crm.currency.list', []);
        $categories = callCore($sb, 'crm.category.list', [
            'entityTypeId' => ENTITY_TYPE_ID,
        ])['categories'];
        $item = $ID > 0 ? callCore($sb, 'crm.item.get', [
            'entityTypeId' => ENTITY_TYPE_ID,
            'id' => $ID,
            'useOriginalUfNames' => 'Y',
        ])['item'] : [];
        $currencyOptions = [];
        foreach ($currencies as $currency)
        {
            $currencyOptions[$currency['CURRENCY']] = $currency['FULL_NAME'];
        }
        $categoryOptions = [];
        foreach ($categories as $category)
        {
            $categoryOptions[$category['id']] = $category['name'];
        }

        $standard = '';
        $custom = '';
        $diagnostics = [];
        foreach ($fields as $key => $field)
        {
            if (!empty($field['isReadOnly']) || !empty($field['isImmutable'])
                || in_array($key, SKIPPED_FIELDS, true)) continue;
            $value = $key === 'categoryId' ? ($item[$key] ?? 0) : ($item[$key] ?? '');
            $params = [
                'NAME' => 'form[' . $key . ']', 'VALUE' => $value,
                'REQUIRED' => $field['isRequired'], 'MULTIPLE' => $field['isMultiple'],
            ];
            $control = '';

            if ($field['type'] === 'crm_status')
            {
                $options = [];
                foreach (callCore($sb, 'crm.status.list', [
                    'filter' => ['ENTITY_ID' => $field['statusType']],
                ]) as $row)
                {
                    $options[$row['STATUS_ID']] = $row['NAME'];
                }
                $control = selectField($params, $options);
            }
            elseif ($field['type'] === 'crm_category')
            {
                $control = selectField($params, $categoryOptions);
            }
            elseif ($field['type'] === 'crm_currency')
            {
                $control = selectField($params, $currencyOptions);
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
            elseif ($field['type'] === 'crm_multifield')
            {
                $control = multifields($value);
            }
            elseif ($field['type'] === 'crm_company')
            {
                $control = inputField(['TYPE' => 'number'] + $params);
                $ids = array_values(array_filter(array_map('intval', (array)$value)));
                if ($ids)
                {
                    $companyResult = callCore($sb, 'crm.item.list', [
                        'entityTypeId' => 4,
                        'filter' => ['@id' => $ids],
                        'select' => ['id', 'title'],
                    ]);
                    $titleById = [];
                    foreach ($companyResult['items'] as $company)
                    {
                        $titleById[(string)$company['id']] = $company['title'];
                    }
                    $titles = [];
                    foreach ($ids as $companyId)
                    {
                        $titles[] = $titleById[(string)$companyId] ?? 'ID ' . $companyId;
                    }
                    $control .= ' (' . esc(implode(', ', $titles)) . ')';
                }
            }
            elseif ($field['type'] === 'crm_lead')
            {
                $control = inputField(['TYPE' => 'number'] + $params);
                if ((int)$value > 0)
                {
                    try
                    {
                        $lead = callCore($sb, 'crm.item.get', [
                            'entityTypeId' => 1,
                            'id' => (int)$value,
                        ])['item'];
                        $control .= ' (' . esc($lead['title']) . ')';
                    }
                    catch (Throwable $error)
                    {
                        $diagnostics[] = 'Lead name for ID ' . $value
                            . ' is unavailable: ' . $error->getMessage();
                    }
                }
            }
            elseif ($field['type'] === 'user')
            {
                $control = inputField(['TYPE' => 'number'] + $params);
                $ids = array_values(array_filter(array_map('intval', (array)$value)));
                if ($ids)
                {
                    $users = callCore($sb, 'user.get', ['filter' => ['ID' => $ids]]);
                    $names = [];
                    foreach ($users as $user)
                    {
                        $names[] = trim(($user['NAME'] ?? '') . ' ' . ($user['LAST_NAME'] ?? ''));
                    }
                    $control .= ' (' . esc(implode(', ', $names)) . ')';
                }
            }
            elseif ($key === 'comments')
            {
                $control = textareaField($params);
            }
            elseif (in_array($field['type'], ['file', 'resourcebooking'], true))
            {
                $control = 'The ' . esc($field['type']) . ' type is not supported in this example';
            }
            elseif ($field['type'] === 'date')
            {
                $formatted = $value !== '' ? substr((string)$value, 0, 10) : '';
                $control = inputField(['TYPE' => 'date', 'VALUE' => $formatted] + $params);
            }
            elseif ($field['type'] === 'datetime')
            {
                $formatted = $value !== ''
                    ? substr(str_replace(' ', 'T', (string)$value), 0, 19)
                    : '';
                $control = inputField(['TYPE' => 'datetime-local', 'VALUE' => $formatted] + $params);
            }
            elseif (in_array($field['type'], ['boolean', 'char'], true))
            {
                $control = inputField([
                    'TYPE' => 'checkbox', 'VALUE' => 'Y',
                    'CHECKED' => $value === 'Y', 'REQUIRED' => false,
                ] + $params);
            }
            elseif (in_array($field['type'], ['integer', 'double'], true))
            {
                $control = inputField([
                    'TYPE' => 'number',
                    'STEP' => $field['type'] === 'double' ? 'any' : '',
                ] + $params);
            }
            elseif ($field['type'] === 'money')
            {
                [$amount, $currency] = array_pad(explode('|', (string)$value, 2), 2, '');
                $control = inputField(['TYPE' => 'number', 'STEP' => 'any', 'VALUE' => $amount] + $params);
                $control .= selectField([
                    'NAME' => 'form[' . $key . '_CURRENCY]', 'VALUE' => $currency,
                ] + $params, $currencyOptions);
            }
            else
            {
                $control = inputField(['TYPE' => 'text'] + $params);
            }

            $label = esc($field['formLabel'] ?? $field['title'] ?? $key);
            $block = '<div class="col-4 mt-3">' . $label
                . ': </div><div class="col-6 mt-3">' . $control . '</div>';
            if (str_starts_with($key, 'UF_')) $custom .= $block;
            else $standard .= $block;
        }

        $diagnosticHtml = '';
        if ($diagnostics)
        {
            $diagnosticHtml = '<div class="alert alert-warning">'
                . implode('<br>', array_map('esc', $diagnostics)) . '</div>';
        }
    ?>
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
        <div class="container">
            <form id="auto_form" method="post">
                <?php if (array_key_exists('id', $item)): ?>
                    <input type="hidden" name="form[id]" value="<?= esc($item['id']) ?>">
                <?php endif; ?>
                <?= $diagnosticHtml ?>
                <h2>System Fields</h2><div class="row"><?= $standard ?></div>
                <h2>Custom Fields</h2><div class="row"><?= $custom ?></div>
                <div class="row"><div class="col-sm-10 mt-5"><input type="submit" class="btn btn-primary" value="Save"></div></div>
            </form>
        </div>
        <script>
            document.getElementById('auto_form').addEventListener('submit', async (event) => {
                event.preventDefault();
                const body = new URLSearchParams(new FormData(event.currentTarget));
                try {
                    const response = await fetch('auto_form.php', { method: 'POST', body });
                    const text = await response.text();
                    try {
                        const json = JSON.parse(text);
                        alert(json.message || json.error || ('Response code ' + response.status));
                    } catch (parseError) {
                        alert('The server response is not JSON, code ' + response.status + ': ' + text.slice(0, 200));
                    }
                } catch (networkError) {
                    alert('Could not submit the form: ' + networkError.message);
                }
            });
        </script>
    ```
{% endlist %}

## 3. Save the Form Data

The browser submits the form to the handler in `application/x-www-form-urlencoded` format. The handler calls [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) again so that it does not trust types from the client request.

The handler skips fields with `isReadOnly = true` and `isImmutable = true`, internal automatically calculated fields, and the `file` and `resourcebooking` types. Therefore, the `photo` field retains its previous value.

For the remaining fields, the handler performs these conversions:

- passes `boolean` and `char`, including the required `opened` field, as `Y` or `N`
- converts `crm_company`, `crm_lead`, and `user` to numeric identifiers while accounting for multiple values
- combines the amount and currency for `money` into the `amount|currency` string
- passes a missing single-value field as an empty string and a multiple field as an empty array

If `assignedById` is absent from the submitted form, the handler passes an empty string. When updating an existing contact, Bitrix24 restores the previous responsible person without an error.

### How the Handler Builds `fm`

Each `fm` row contains a hidden `id` and visible `typeId`, `valueType`, and `value` fields.

- for an existing nonempty value, its numeric `id` becomes the key, so the server updates the same entry
- for a new nonempty value, the handler creates the key `n0`, `n1`, and so on, so the server adds an entry
- if an existing value is cleared or the **Delete** checkbox is selected, the handler retains its `id` but passes an empty `value`, so the server deletes the entry
- the handler does not send empty rows without an `id`

All `fm` changes are sent in a single request. For example, the object can look like this:

```json
{
    "451": { "typeId": "PHONE", "valueType": "WORK", "value": "+49 30 12345678" },
    "452": { "typeId": "EMAIL", "valueType": "WORK", "value": "" },
    "n0": { "typeId": "EMAIL", "valueType": "WORK", "value": "contact@example.com" }
}
```

Entry `451` is updated, `452` is deleted, and `n0` is added.

If the form contains `id`, the handler calls [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md). Without `id`, it calls [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md). The saved contact identifier is located in `result.item.id`:

```json
{
    "result": {
        "item": {
            "id": 123
        }
    }
}
```

The next action requires `result.item.id`: add it to the `ID` parameter in the form URL to open the created contact for editing.

### Full Handler Code

Save the handler to a file: JavaScript — `save-form.mjs`, PHP — `auto_form.php`, Python — `save_form.py`.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const ENTITY_TYPE_ID = 3
    const SKIPPED_FIELDS = new Set([
        'companyId', 'companies', 'hasPhone', 'hasEmail', 'hasImol',
        'birthdaySort', 'fullName', 'id', 'createdBy', 'updatedBy',
        'createdTime', 'updatedTime', 'lastCommunicationTime',
    ])
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

    function relationValue(value, multiple) {
        const ids = asArray(value).map(Number).filter((itemId) => itemId > 0)
        return multiple ? ids : (ids[0] ?? 0)
    }

    export async function saveForm(req, res) {
        try {
            const submitted = req.body.form ?? {}
            const fieldResult = await call('crm.item.fields', {
                entityTypeId: ENTITY_TYPE_ID,
                useOriginalUfNames: 'Y',
            }, 'contact-fields-save')
            const fields = {}

            for (const [key, prop] of Object.entries(fieldResult.fields)) {
                if (SKIPPED_FIELDS.has(key) || prop.isReadOnly || prop.isImmutable
                    || ['file', 'resourcebooking'].includes(prop.type)) continue
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
                    const amount = String(value ?? '').trim()
                    const currency = String(submitted[`${key}_CURRENCY`] ?? '').trim()
                    value = amount === '' ? '' : (currency === '' ? amount : `${amount}|${currency}`)
                } else if (['crm_company', 'crm_lead', 'user'].includes(prop.type)) {
                    value = relationValue(value, prop.isMultiple)
                } else if (prop.isMultiple) {
                    value = asArray(value).filter((item) => item !== '')
                }
                fields[key] = value
            }

            const rawId = String(submitted.id ?? '').trim()
            const id = /^\d+$/.test(rawId) ? Number(rawId) : 0
            const method = id > 0 ? 'crm.item.update' : 'crm.item.add'
            const params = {
                entityTypeId: ENTITY_TYPE_ID,
                fields,
                useOriginalUfNames: 'Y',
            }
            if (id > 0) params.id = id
            const result = await call(method, params, `contact-${id > 0 ? 'update' : 'add'}`)
            res.json({ message: `Contact saved, ID: ${result.item.id}` })
        } catch (error) {
            res.status(400).json({ error: error.message })
        }
    }
    ```

- Python

    ```python
    import re

    from flask import jsonify, request
    from b24pysdk import BitrixWebhook, Client

    ENTITY_TYPE_ID = 3
    SKIPPED_FIELDS = {
        "companyId", "companies", "hasPhone", "hasEmail", "hasImol",
        "birthdaySort", "fullName", "id", "createdBy", "updatedBy",
        "createdTime", "updatedTime", "lastCommunicationTime",
    }
    FORM_KEY = re.compile(r"^form\[([^]]+)](\[\])?$")
    FM_KEY = re.compile(r"^fm\[(\d+)]\[(id|typeId|valueType|value|delete)]$")
    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",
    ), timeout=60)


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
            try:
                item_id = int(row.get("id") or 0)
            except (TypeError, ValueError):
                item_id = 0
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


    def relation_value(value, multiple):
        values = value if isinstance(value, list) else [value]
        ids = [int(item_id) for item_id in values if str(item_id).isdigit() and int(item_id) > 0]
        return ids if multiple else (ids[0] if ids else 0)


    def save_form():
        try:
            submitted = parse_form()
            field_result = client.crm.item.fields(
                entity_type_id=ENTITY_TYPE_ID,
                use_original_uf_names=True,
            ).response.result
            fields = {}

            for key, prop in field_result["fields"].items():
                if (key in SKIPPED_FIELDS or prop.get("isReadOnly")
                        or prop.get("isImmutable")
                        or prop.get("type") in ("file", "resourcebooking")):
                    continue
                field_type = prop.get("type")
                if field_type in ("boolean", "char"):
                    fields[key] = "Y" if key in submitted else "N"
                    continue
                if field_type == "crm_multifield":
                    fields[key] = build_multifields(parse_multifield_rows())
                    continue
                if key not in submitted:
                    fields[key] = [] if prop.get("isMultiple") else ""
                    continue

                value = submitted[key]
                if field_type == "money":
                    amount = str(value or "").strip()
                    currency = str(submitted.get(f"{key}_CURRENCY", "") or "").strip()
                    value = "" if amount == "" else (amount if currency == "" else f"{amount}|{currency}")
                elif field_type in ("crm_company", "crm_lead", "user"):
                    value = relation_value(value, bool(prop.get("isMultiple")))
                elif prop.get("isMultiple"):
                    values = value if isinstance(value, list) else [value]
                    value = [item for item in values if item != ""]
                fields[key] = value

            try:
                item_id = int(submitted.get("id") or 0)
            except (TypeError, ValueError):
                item_id = 0
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

            return jsonify(message=f"Contact saved, ID: {response.result['item']['id']}")
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

        const ENTITY_TYPE_ID = 3;
        const SKIPPED_FIELDS = [
            'companyId', 'companies', 'hasPhone', 'hasEmail', 'hasImol',
            'birthdaySort', 'fullName', 'id', 'createdBy', 'updatedBy',
            'createdTime', 'updatedTime', 'lastCommunicationTime',
        ];
        header('Content-Type: application/json; charset=utf-8');

        function callCore($sb, string $method, array $params): array
        {
            return $sb->core->call($method, $params)->getResponseData()->getResult();
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
                if ($id <= 0 && $shouldDelete) continue;
                $key = $id > 0 ? (string)$id : 'n' . $newIndex++;
                $result[$key] = [
                    'typeId' => (string)($row['typeId'] ?? 'PHONE'),
                    'valueType' => (string)($row['valueType'] ?? 'WORK'),
                    'value' => $shouldDelete ? '' : $value,
                ];
            }
            return $result;
        }

        function relationValue($value, bool $multiple)
        {
            $ids = array_values(array_filter(array_map('intval', (array)$value)));
            return $multiple ? $ids : ($ids[0] ?? 0);
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
                if (in_array($key, SKIPPED_FIELDS, true)
                    || !empty($prop['isReadOnly'])
                    || !empty($prop['isImmutable'])
                    || in_array($prop['type'], ['file', 'resourcebooking'], true))
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
                    $fields[$key] = buildMultifields(
                        is_array($_POST['fm'] ?? null) ? $_POST['fm'] : []
                    );
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
                    $amount = trim((string)$value);
                    $currency = trim((string)($submitted[$key . '_CURRENCY'] ?? ''));
                    $value = $amount === '' ? '' : ($currency === '' ? $amount : $amount . '|' . $currency);
                }
                elseif (in_array($prop['type'], ['crm_company', 'crm_lead', 'user'], true))
                {
                    $value = relationValue($value, !empty($prop['isMultiple']));
                }
                elseif (!empty($prop['isMultiple']))
                {
                    $value = array_values(array_filter(
                        (array)$value,
                        static fn($item) => $item !== ''
                    ));
                }
                $fields[$key] = $value;
            }

            $rawId = trim((string)($submitted['id'] ?? ''));
            $id = preg_match('/^\d+$/D', $rawId) === 1 ? (int)$rawId : 0;
            $method = $id > 0 ? 'crm.item.update' : 'crm.item.add';
            $params = [
                'entityTypeId' => ENTITY_TYPE_ID,
                'fields' => $fields,
                'useOriginalUfNames' => 'Y',
            ];
            if ($id > 0) $params['id'] = $id;
            $result = callCore($sb, $method, $params);
            echo json_encode(
                ['message' => 'Contact saved, ID: ' . $result['item']['id']],
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

## Verify the Result

1. Open the page without `ID`
2. Enter a first or last name, then select a salutation, contact type, source, and pipeline
3. Enter a comment in BBCode, such as `[b]Contact for a follow-up call[/b]`
4. In the first empty `fm` row, keep `PHONE` and `WORK`, then enter a phone number. In the second row, select `EMAIL`, keep `WORK`, and enter an email address
5. Add two company identifiers in the required order and, if necessary, a lead identifier. Retrieve suitable identifiers using [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) with `entityTypeId = 4` for companies and `entityTypeId = 1` for leads, or copy them from the card URL in Bitrix24
6. Click **Save** and copy the contact identifier from the message
7. Open the form with this identifier and verify the first name, last name, pipeline, companies, lead, comment, and all `fm` rows
8. Change the phone number, select **Delete** for the old email address, add a new email address, and save the form again
9. In the browser developer tools, remove the element with `name="form[assignedById]"`, save the form, and verify that the previous responsible person remains assigned without an error
10. Call [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) with `entityTypeId = 3`, the retrieved `id`, and `useOriginalUfNames = Y`. Compare `result.item` with the form and verify `name`, `lastName`, `companyIds`, and `fm`

Also verify the name rule: clear both `name` and `lastName` for an existing contact. [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) should return the `CRM_FIELD_ERROR_REQUIRED` error. When creating a contact with empty `name` and `lastName`, Bitrix24 assigns a default name, but the example recommends filling in at least one field.

## Errors and Diagnostics

#|
|| Symptom | What to Check and Fix ||
|| The first request to Bitrix24 fails with an authorization error | Check the full webhook URL, user identifier, and secret token. In Python, make sure `domain` does not contain `https://` and `webhook_token` contains only `USER_ID/TOKEN`. In PHP and Python, the URL is specified in both the generator and the handler. Reload the form page after correcting it ||
|| `ACCESS_DENIED` | Grant the webhook user permissions to read, add, and edit contacts, read associated companies and leads, and access CRM settings. Reload the form page after correcting the permissions ||
|| [user.get](../../../api-reference/user/user-get.md) returns an access error | Add the [`user_brief`](../../../api-reference/scopes/permissions.md) scope to the webhook, then reload the form page ||
|| The form opens empty or [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns an error | The example converts a nonnumeric, zero, or negative `ID` to `0` and opens the new contact form. Verify that a contact with this identifier exists and is accessible to the webhook user, then open the form with the correct `ID` ||
|| `CRM_FIELD_ERROR_REQUIRED` with a message about the First name and Last name fields | You cannot clear both the first and last name of an existing contact. Fill in at least one field and submit the form again ||
|| `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Check the field named in `error_description`: a salutation, contact type, or source code, or a company, lead, or user identifier. Correct the value and submit the form again ||
|| Previous values remain after clearing all values from a multiple custom list field | The handler sends an empty array, and [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) succeeds, but [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns the previous values. The example does not implement a way to clear this field ||
|| A message appears instead of the associated lead name | The webhook user does not have permission to read this lead. Leave `leadId` unchanged or grant permission to read the lead, then reload the form page ||
|| PHP does not start after installing the dependencies | Run `composer check-platform-reqs`, enable the `bcmath`, `curl`, `intl`, and `json` extensions, then restart the PHP server ||
|#

## Key Considerations

- The `companyId` field is deprecated, but `companyIds` is available in the universal methods and is not deprecated. When saving, identifier order is preserved through `SORT`, the first company becomes the primary company, existing link roles are retained, and new links receive the default role
- The form manages only the `companyIds` list and does not directly edit `ROLE_ID`, the primary company flag, or `SORT`. Retrieve advanced link properties using [crm.contact.company.items.get](../../../api-reference/crm/contacts/company/crm-contact-company-items-get.md), replace them using [crm.contact.company.items.set](../../../api-reference/crm/contacts/company/crm-contact-company-items-set.md), add them using [crm.contact.company.add](../../../api-reference/crm/contacts/company/crm-contact-company-add.md), and delete them using [crm.contact.company.delete](../../../api-reference/crm/contacts/company/crm-contact-company-delete.md)
- The system `photo` field has its own rules. The universal [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method does not accept a photo: both the `["photo.jpg", "Base64"]` array and the `{"fileData": ["photo.jpg", "Base64"]}` object return an error. Set a photo using [crm.contact.update](../../../api-reference/crm/contacts/crm-contact-update.md) by passing the `{"fileData": ["photo.jpg", "Base64"]}` object in the `PHOTO` field. [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) then returns the photo as a number, such as `36933`, which is the file identifier
- A custom file field works differently: upload the file as an [array containing the file name and Base64 content](../../../api-reference/files/how-to-upload-files.md#array) using [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) with `useOriginalUfNames = Y`. The `fileData` variant succeeds without an error, but the field remains empty. In the response, this field is returned as an object with the `id`, `url`, and `urlMachine` keys. Both URLs point to an internal file download and may contain authorization data. Do not record them in logs, reports, or error messages, and do not share them with third parties
- A custom field of the `money` type accepts either an amount alone or an amount with a currency. If you send `1234.56`, Bitrix24 adds the default currency and saves `1234.56|EUR`. If the amount is missing, the field is cleared: `""`, `|EUR`, and `EUR` are saved as empty values. Therefore, the handler sends an empty string when the field is blank. The currency code is not validated, so `1234.56|XXX` is saved as entered
- A multiple custom field of the `money` type requires separate handling for multiple amount-currency pairs
- To retain the value of a field that is absent from the form, add it to the skipped fields list
- [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) returns no more than 50 items per page. If a contact has more than 50 associated companies or values in a multiple `user` field, add pagination
- [crm.category.list](../../../api-reference/crm/universal/category/crm-category-list.md), [crm.status.list](../../../api-reference/crm/status/crm-status-list.md), and [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md) return no more than 50 records per call. The example retrieves only the first page, so request subsequent pages using `start` in a production application
- Add your own authentication, CSRF protection, and secure error logging in a production application

## Continue Learning

- [{#T}](./how-to-generate-edit-form-for-lead.md)
- [{#T}](./how-to-generate-edit-form-for-company.md)
- [{#T}](./how-to-generate-edit-form-for-deal.md)
