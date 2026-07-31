# How to Create a Custom Lead Edit Form

> Scope: [`crm`](../../../api-reference/scopes/permissions.md), [`user_brief`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the scenario: a user with permissions to read, add, and edit leads, read associated companies and contacts, and access CRM settings. Scope `user_brief` is required to call [user.get](../../../api-reference/user/user-get.md)

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In this example, we will create a web form for adding and editing a lead. The form does not contain a predefined list of fields: the generator retrieves the field descriptions using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method and selects the appropriate HTML control for each field type. Therefore, custom fields created in Bitrix24 appear in the form without any code changes.

If you open the page without the `ID` parameter, the form will be empty, and the handler will create a lead. If you pass an identifier, for example `?ID=123`, the generator will retrieve the lead using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method, populate the form, and the handler will update this lead.

The example consists of two files:

- The generator retrieves field descriptions, lead data, and dictionary values, then outputs an HTML form.
- The handler receives the form data and calls the add or update method.

## How the Scenario Works

1. The generator retrieves field descriptions using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method.
2. If the URL contains `ID`, the generator retrieves the lead values using the [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) method.
3. The generator replaces internal codes with readable names using additional methods:
   - [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) — fields `STATUS_ID` and `NAME` for stages, sources, and other CRM lookup lists.
   - [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md) — fields `CURRENCY` and `FULL_NAME` for currencies.
   - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — the `title` field of the associated company.
   - [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) — fields `id`, `name`, and `lastName` of associated contacts.
   - [user.get](../../../api-reference/user/user-get.md) — fields `ID`, `NAME`, and `LAST_NAME` of users.
4. The handler retrieves the field descriptions again using the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) method and converts the form values to Bitrix24 REST API types.
5. The handler adds a new lead using the [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) method or updates an existing one using the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method.

## 1. Prepare the Environment

Create an [incoming webhook](../../../local-integrations/local-webhooks.md#incoming-webhook) with `crm` and `user_brief` permissions. The webhook user requires permissions to read, add, and edit leads, read companies and contacts, and access CRM settings.

{% note warning "Keep the webhook secret" %}

The webhook executes requests with the permissions of the user who created it. Do not add the webhook URL to public repositories, client-side JavaScript, or error messages.

{% endnote %}

Choose one language, create a separate folder for the example, and open a terminal in it. Save the code from steps 2 and 3 into two corresponding files before executing the launch command.

#|
|| Language | Form Generator | Handler ||
|| JavaScript | `form.mjs` | `save-form.mjs` ||
|| PHP | `index.php` | `auto_form.php` ||
|| Python | `app.py` | `save_form.py` ||
|#

Install dependencies.

{% list tabs %}

- JS

    ```bash
    npm init -y
    npm install @bitrix24/b24jssdk express
    ```

- PHP

    ```bash
    composer require bitrix24/b24phpsdk:"^3.0"
    ```

- Python

    ```bash
    pip install b24pysdk flask
    ```

{% endlist %}

Before installing dependencies, check the PHP version using the `php -v` command and the list of enabled extensions using the `php -m` command. B24PhpSDK version 3 requires PHP 8.4 or 8.5. B24PhpSDK and its dependencies require the `bcmath`, `curl`, `intl`, and `json` extensions. After installation, run `composer check-platform-reqs`.

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

- PHP

    Bash and PowerShell:

    ```bash
    php -S localhost:8000
    ```

- Python

    Bash and PowerShell:

    ```bash
    python app.py
    ```

{% endlist %}

The form pages will be available at the following addresses:

#|
|| Language | New lead | Existing lead with identifier `123` ||
|| JavaScript | `http://localhost:3000/` | `http://localhost:3000/?ID=123` ||
|| PHP | `http://localhost:8000/index.php` | `http://localhost:8000/index.php?ID=123` ||
|| Python | `http://localhost:5000/` | `http://localhost:5000/?ID=123` ||
|#

## 2. Create a Lead Form

The generator passes two parameters to [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md):

- `entityTypeId: 1` — the "lead" object type. Values for other objects are provided in the [CRM Object Type](../../../api-reference/crm/data-types.md#object_type) table.
- `useOriginalUfNames: Y` — return the original names of custom fields `UF_*`.

In the Python SDK, the `useOriginalUfNames` parameter is named `use_original_uf_names` and accepts a boolean value `True`.

The abbreviated response of [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) for a lead looks like this:

```json
{
    "result": {
        "fields": {
            "title": {
                "type": "string",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false,
                "title": "Lead Name"
            },
            "stageId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false,
                "statusType": "STATUS"
            },
            "sourceId": {
                "type": "crm_status",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false,
                "statusType": "SOURCE"
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
            "currencyId": {
                "type": "crm_currency",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false
            },
            "isManualOpportunity": {
                "type": "boolean",
                "isRequired": false,
                "isReadOnly": false,
                "isMultiple": false
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

The next step requires the keys `type`, `isRequired`, `isReadOnly`, `isMultiple`, `title`, or `formLabel`. For fields of type `crm_status`, a `statusType` is also required: for `stageId` it is `STATUS`, for `sourceId` — `SOURCE`. The generator passes this value to the `ENTITY_ID` filter of the [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) method.

If `ID=123` is passed, [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) will return data in `result.item`. The generator retrieves values from this object using the same camelCase keys that arrived in the field description:

```json
{
    "result": {
        "item": {
            "id": 123,
            "title": "Website inquiry",
            "stageId": "NEW",
            "sourceId": "WEB",
            "companyId": 27,
            "contactIds": [31, 44],
            "opportunity": 15000,
            "currencyId": "EUR",
            "isManualOpportunity": "Y",
            "fm": [
                {
                    "id": 451,
                    "typeId": "PHONE",
                    "valueType": "WORK",
                    "value": "+49 495 111-22-33"
                }
            ]
        }
    }
}
```

The `companyId` field contains a single company identifier, while `contactIds` is an array of contact identifiers. The `contactIds` field itself is structured the same way as for a company. The difference for a lead is an additional single link `companyId`: a lead can simultaneously have one company and several contacts. The example does not output the deprecated single field `contactId` and the service field `contacts` to avoid sending multiple representations of the same link.

The `fm` multi-field is applicable to leads. The following types are available in the form: `PHONE`, `EMAIL`, `WEB`, `IM`.

The lead amount is stored in `opportunity` of type `double`, and the currency is in `currencyId` of type `crm_currency`. The amount calculation mode is stored separately in the logical field `isManualOpportunity`, so the form displays a separate checkbox for it.

The generator maps field types to form controls.

#|
|| Field type | Form control ||
|| `crm_status`, `crm_currency`, `enumeration` | `<select>` with directory values ||
|| `crm_company` | numeric field and current company name ||
|| `crm_contact` | one or more numeric fields and current contact names ||
|| `user` | numeric field and user name ||
|| `crm_multifield` | rows `typeId`, `valueType`, `value` while preserving `id` ||
|| `date`, `datetime` | `date`, `datetime-local` ||
|| `boolean`, `char` | checkbox ||
|| `integer`, `double` | numeric field ||
|| `money` | amount and list of currencies ||
|| `file`, `resourcebooking` | unsupported type message ||
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

    const ENTITY_TYPE_ID = 1
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

    function multifields(values) {
        const rows = [...(Array.isArray(values) ? values : [])]
        rows.push(...Array.from({ length: 3 }, () => ({ id: '', typeId: 'PHONE', valueType: 'WORK', value: '' })))
        return rows.map((row, index) => {
            const typeOptions = ['PHONE', 'EMAIL', 'WEB', 'IM']
                .map((type) => `<option value="${type}"${row.typeId === type ? ' selected' : ''}>${type}</option>`)
                .join('')
            return `<div class="border rounded p-2 mb-2">
                <input type="hidden" name="fm[${index}][id]" value="${escapeHtml(row.id)}">
                <select class="form-control mb-1" name="fm[${index}][typeId]">${typeOptions}</select>
                <input class="form-control mb-1" name="fm[${index}][valueType]" value="${escapeHtml(row.valueType || 'WORK')}" placeholder="WORK">
                <input class="form-control mb-1" name="fm[${index}][value]" value="${escapeHtml(row.value)}" placeholder="Value">
                ${row.id ? `<label><input type="checkbox" name="fm[${index}][delete]" value="Y"> Delete</label>` : ''}
            </div>`
        }).join('')
    }

    app.get('/', async (req, res) => {
        const id = parseInt(String(req.query.ID ?? '0'), 10) || 0
        try {
            const fieldResult = await call('crm.item.fields', {
                entityTypeId: ENTITY_TYPE_ID,
                useOriginalUfNames: 'Y',
            }, 'lead-fields')
            const fields = fieldResult.fields
            const currencies = await call('crm.currency.list', {}, 'currencies')
            const item = id > 0
                ? (await call('crm.item.get', {
                    entityTypeId: ENTITY_TYPE_ID,
                    id,
                    useOriginalUfNames: 'Y',
                }, 'lead-get')).item
                : {}

            let standard = ''
            let custom = ''
            for (const [key, field] of Object.entries(fields)) {
                if (SKIPPED_FIELDS.has(key)) continue
                let value = item[key] ?? ''
                let control = ''
                const params = {
                    NAME: `form[${key}]`, VALUE: value,
                    REQUIRED: field.isRequired, DISABLE: field.isReadOnly, MULTIPLE: field.isMultiple,
                }

                if (field.type === 'crm_status') {
                    const rows = await call('crm.status.list', {
                        filter: { ENTITY_ID: field.statusType },
                    }, `status-${key}`)
                    control = select(params, Object.fromEntries(rows.map((row) => [row.STATUS_ID, row.NAME])))
                } else if (field.type === 'crm_currency') {
                    control = select(params, Object.fromEntries(currencies.map((row) => [row.CURRENCY, row.FULL_NAME])))
                } else if (field.type === 'enumeration') {
                    const options = Object.fromEntries((field.items || []).map((row) => [row.ID ?? row.id, row.VALUE ?? row.value]))
                    control = select(params, options)
                } else if (field.type === 'crm_multifield') {
                    control = multifields(value)
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
                } else if (['file', 'resourcebooking'].includes(field.type)) {
                    control = `Type ${escapeHtml(field.type)} not supported in this example`
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

        const ENTITY_TYPE_ID = 1;
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
                $id = $row['id'] ?? '';
                $html .= '<div class="border rounded p-2 mb-2">';
                $html .= '<input type="hidden" name="fm[' . $index . '][id]" value="' . esc($id) . '">';
                $html .= '<select class="form-control mb-1" name="fm[' . $index . '][typeId]">' . $options . '</select>';
                $html .= '<input class="form-control mb-1" name="fm[' . $index . '][valueType]" value="' . esc($row['valueType'] ?? 'WORK') . '" placeholder="WORK">';
                $html .= '<input class="form-control mb-1" name="fm[' . $index . '][value]" value="' . esc($row['value'] ?? '') . '" placeholder="Value">';
                if ($id)
                {
                    $html .= '<label><input type="checkbox" name="fm[' . $index . '][delete]" value="Y"> Delete</label>';
                }
                $html .= '</div>';
            }
            return $html;
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
            $item = callCore($sb, 'crm.item.get', [
                'entityTypeId' => ENTITY_TYPE_ID,
                'id' => $ID,
                'useOriginalUfNames' => 'Y',
            ])['item'];
        }

        $standard = '';
        $custom = '';
        foreach ($fields as $key => $field)
        {
            if (in_array($key, ['contactId', 'contacts'], true)) continue;
            $value = $item[$key] ?? '';
            $params = [
                'NAME' => 'form[' . $key . ']', 'VALUE' => $value,
                'REQUIRED' => $field['isRequired'], 'DISABLE' => $field['isReadOnly'], 'MULTIPLE' => $field['isMultiple'],
            ];
            $control = '';

            if ($field['type'] === 'crm_status')
            {
                $options = [];
                foreach ($crm->status()->list([], ['ENTITY_ID' => $field['statusType']], [])->getStatuses() as $status)
                {
                    $options[$status->STATUS_ID] = $status->NAME;
                }
                $control = selectField($params, $options);
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
            elseif ($field['type'] === 'crm_multifield')
            {
                $control = multifields($value);
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
            elseif (in_array($field['type'], ['file', 'resourcebooking'], true))
            {
                $control = 'Type ' . esc($field['type']) . ' not supported in this example';
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
            document.getElementById('auto_form').addEventListener('submit', async (event) => {
                event.preventDefault();
                const body = new URLSearchParams(new FormData(event.currentTarget));
                const response = await fetch('auto_form.php', { method: 'POST', body });
                const json = await response.json();
                alert(json.message || json.error);
            });
        </script>
    ```

- Python

    ```python
    # pip install b24pysdk flask
    from html import escape

    from flask import Flask, request
    from b24pysdk import BitrixWebhook, Client

    from save_form import save_form

    app = Flask(__name__)
    ENTITY_TYPE_ID = 1
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

    def multifields(values):
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
        <div class="container"><form id="auto_form" method="post">
            %(hidden_id)s
            <h2>System fields</h2><div class="row">%(standard)s</div>
            <h2>Custom fields</h2><div class="row">%(custom)s</div>
            <div class="row"><div class="col-sm-10 mt-5"><input type="submit" class="btn btn-primary" value="Save"></div></div>
        </form></div>
        <script>
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
            return "The ID parameter must be an integer", 400

        fields = client.crm.item.fields(
            entity_type_id=ENTITY_TYPE_ID,
            use_original_uf_names=True,
        ).response.result["fields"]
        currencies = client.crm.currency.list().response.result
        item = {}
        if item_id > 0:
            item = client.crm.item.get(
                entity_type_id=ENTITY_TYPE_ID,
                bitrix_id=item_id,
                use_original_uf_names=True,
            ).response.result["item"]

        standard = ""
        custom = ""
        for key, field in fields.items():
            if key in SKIPPED_FIELDS:
                continue
            value = value_or_empty(item.get(key))
            params = {
                "NAME": f"form[{key}]", "VALUE": value,
                "REQUIRED": field.get("isRequired"), "DISABLE": field.get("isReadOnly"), "MULTIPLE": field.get("isMultiple"),
            }
            field_type = field.get("type")
            control = ""

            if field_type == "crm_status":
                rows = client.crm.status.list(filter={"ENTITY_ID": field["statusType"]}).response.result
                control = select_field(params, {row["STATUS_ID"]: row["NAME"] for row in rows})
            elif field_type == "crm_currency":
                control = select_field(params, {row["CURRENCY"]: row["FULL_NAME"] for row in currencies})
            elif field_type == "enumeration":
                options = {
                    row.get("ID", row.get("id")): row.get("VALUE", row.get("value"))
                    for row in field.get("items", [])
                }
                control = select_field(params, options)
            elif field_type == "crm_multifield":
                control = multifields(value)
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
            elif field_type in ("file", "resourcebooking"):
                control = f"Type {escape(field_type)} is not supported in this example"
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
        return PAGE % {"hidden_id": hidden_id, "standard": standard, "custom": custom}

    app.add_url_rule("/form", view_func=save_form, methods=["POST"])

    if __name__ == "__main__":
        app.run(port=5000)
    ```

{% endlist %}

## 3. Save the Form Data

The handler calls [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) again. It passes only known fields, skips read-only fields, and does not send unsupported `file` and `resourcebooking` types.

The browser does not send unchecked checkboxes. Therefore, the handler passes `Y` for a checked `boolean` field or `char` and `N` for an unchecked one. For multiple fields, the handler collects an array; for the `money` field, it joins the amount and currency via `|`; for `companyId`, it passes a single numeric identifier; and for `contactIds`, it passes an array of numeric identifiers.

If a field available for editing is not present in the submitted form, the handler passes an empty value: an empty string for a single field, and an empty array for a multiple field, including a custom field of the "list" type. Therefore, do not remove a field from the HTML form without making corresponding changes to the handler.

Each `fm` row contains `id`, `typeId`, `valueType`, `value`:

- an existing non-empty row retains the numeric `id` and updates the record
- a new non-empty row receives a key such as `n0` or `n1` and adds the record
- an existing cleared row retains `id`, but passes an empty `value` and deletes the record
- a new empty row is not passed

For example, the `fm` object before updating may look like this:

```json
{
    "451": { "typeId": "PHONE", "valueType": "WORK", "value": "+49 495 111-22-33" },
    "452": { "typeId": "EMAIL", "valueType": "WORK", "value": "" },
    "n0": { "typeId": "EMAIL", "valueType": "WORK", "value": "lead@example.com" }
}
```

The `451` record will be updated, `452` will be deleted, and `n0` will be added.

If the form contains `id`, the handler calls [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md). Without `id`, it calls [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md). In both cases, the identifier of the saved lead is located in `result.item.id`:

```json
{
    "result": {
        "item": {
            "id": 123,
            "title": "Website inquiry",
            "stageId": "NEW",
            "sourceId": "WEB",
            "companyId": 27,
            "contactIds": [31, 44],
            "opportunity": 15000,
            "currencyId": "EUR",
            "isManualOpportunity": "Y"
        }
    }
}
```

The next action requires `result.item.id`: add it to the `ID` parameter of the form address to open the created lead for editing.

### Full Handler Code

Save the handler to a file: JavaScript — `save-form.mjs`, PHP — `auto_form.php`, Python — `save_form.py`.

{% list tabs %}

- JS

    ```javascript
    import { B24Hook } from '@bitrix24/b24jssdk'

    const ENTITY_TYPE_ID = 1
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
            }, 'lead-fields-save')
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
            const result = await call(method, params, `lead-${id > 0 ? 'update' : 'add'}`)
            res.json({ message: `Lead saved, ID: ${result.item.id}` })
        } catch (error) {
            res.status(400).json({ error: error.message })
        }
    }
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
        const ENTITY_TYPE_ID = 1;
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
            echo json_encode(['message' => 'Lead saved, ID: ' . $result['item']['id']], JSON_UNESCAPED_UNICODE);
        }
        catch (Throwable $error)
        {
            http_response_code(400);
            echo json_encode(['error' => $error->getMessage()], JSON_UNESCAPED_UNICODE);
        }
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import re

    from flask import jsonify, request
    from b24pysdk import BitrixWebhook, Client

    ENTITY_TYPE_ID = 1
    SKIPPED_FIELDS = {"contactId", "contacts"}
    client = Client(BitrixWebhook(
        domain="your-domain.bitrix24.com",
        webhook_token="USER_ID/TOKEN",
    ))

    def values_for(name):
        return request.form.getlist(name) or request.form.getlist(f"{name}[]")

    def build_multifields():
        rows = {}
        pattern = re.compile(r"^fm\[(\d+)]\[(id|typeId|valueType|value|delete)]$")
        for full_key in request.form:
            match = pattern.match(full_key)
            if match:
                rows.setdefault(match.group(1), {})[match.group(2)] = request.form[full_key]

        result = {}
        new_index = 0
        for row in rows.values():
            item_id = int(row.get("id") or 0)
            value = row.get("value", "")
            should_delete = row.get("delete") == "Y" or value == ""
            if not item_id and should_delete:
                continue
            key = str(item_id) if item_id else f"n{new_index}"
            if not item_id:
                new_index += 1
            result[key] = {
                "typeId": row.get("typeId") or "PHONE",
                "valueType": row.get("valueType") or "WORK",
                "value": "" if should_delete else value,
            }
        return result

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
                if key in SKIPPED_FIELDS or prop.get("isReadOnly") or prop.get("type") in ("file", "resourcebooking"):
                    continue
                field_type = prop.get("type")
                if field_type in ("boolean", "char"):
                    fields[key] = "Y" if key in submitted else "N"
                    continue
                if field_type == "crm_multifield":
                    fields[key] = build_multifields()
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
            return jsonify(message=f"Lead saved, ID: {result['item']['id']}")
        except Exception as error:
            return jsonify(error=str(error)), 400
    ```

{% endlist %}

## Verify the Result

1. Open the form page without the `ID` parameter
2. Fill in the lead name, stage, source, amount, and currency. Check the Calculate amount manually checkbox. If necessary, specify a company identifier, several contact identifiers, a phone number, and an email address
3. Click **Save**. Copy the identifier of the created lead from the message, for example `123`
4. Open the form with this identifier: JavaScript — `http://localhost:3000/?ID=123`, PHP — `http://localhost:8000/index.php?ID=123`, Python — `http://localhost:5000/?ID=123`
5. Verify that the form contains the saved values, including the company, all contacts, and the `fm` entries
6. Change the name, amount, or source, then click **Save** again
7. Open the lead card in Bitrix24 and check the modified fields
8. Call [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) with `entityTypeId = 1` and the obtained `id`. Make sure that `result.item` contains the saved `title`, `stageId`, `sourceId`, `companyId`, `contactIds`, `opportunity`, `currencyId`, `isManualOpportunity`, and `fm`

## Errors and Diagnostics

#|
|| Symptom | What to check and fix ||
|| The first request to Bitrix24 ends with an authorization error | Check the full webhook address, user identifier, and secret token. Ensure that in Python `domain` does not contain `https://`, and `webhook_token` contains only `USER_ID/TOKEN`. After fixing, reload the form page ||
|| `ACCESS_DENIED` | Grant the webhook user permissions to read, add, and edit leads, read related companies and contacts, and access CRM settings. After fixing, reload the form page ||
|| [user.get](../../../api-reference/user/user-get.md) returns an access error | Add scope [`user_brief`](../../../api-reference/scopes/permissions.md) to the webhook, then reload the form page ||
|| [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns `NOT_FOUND` | Check that a lead with this identifier exists and is accessible to the webhook user. After fixing, open the form with the correct `ID` ||
|| `CRM_FIELD_ERROR_VALUE_NOT_VALID` | Check the value of the field named in `error_description`: stage or source code, currency, company, contact, or user identifier. Fix the value and resubmit the form ||
|| Multiple field error | Pass `contactIds` and other multiple fields as an array. For existing `fm` values, preserve the numeric `id`, for new ones use keys `n0`, `n1`. Fix the data and resubmit the form ||
|| After removing all values from a multiple "list" type custom field, the previous values remained | The handler sends an empty array, the [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) request succeeds, but [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns the previous values. The clearing method in the example is not implemented ||
|| The lead is not saved after submitting a required field | Check `isRequired` in the [crm.item.fields](../../../api-reference/crm/universal/crm-item-fields.md) response, fill in the field, and resubmit the form ||
|| The empty lead name was not saved | The `title` field cannot be cleared. When updating, the previous name remains; when adding, Bitrix24 generates a default name. To change the name, enter a non-empty value and resubmit the form ||
|| PHP does not start after installing dependencies | Perform `composer check-platform-reqs`, enable extensions `bcmath`, `curl`, `intl`, `json`, and restart the PHP server ||
|| Port is already in use | Stop the process on the port or start the server on a different port and change the verification address ||
|#

## Key Considerations

- The example outputs all available fields, so a large form may be inconvenient for a real application
- File uploading requires a separate implementation

  To upload a file via [crm.item.add](../../../api-reference/crm/universal/crm-item-add.md) or [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md), pass an [array containing the filename and Base64 content](../../../api-reference/files/how-to-upload-files.md#array) to the field, for example `["document.pdf", "Base64"]`. For a multiple field, pass an array of such pairs. [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) returns the already uploaded file as an object with keys `id` and `url`: use for reading the file `url`
- Types unknown to the generator are output as text fields; add format handling for them before using them in a production application
- A multiple custom field of type `money` requires a separate implementation for multiple "amount — currency" pairs
- If all values are cleared from a multiple custom field of type "list", the handler will send an empty array. The [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) request will succeed, but [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) will return the previous values
- [crm.item.list](../../../api-reference/crm/universal/crm-item-list.md) returns paginated data. The example loads only the first page. If a lead has more than 50 associated contacts, some contact labels will be missing. For a production application, split `contactIds` into groups of up to 50 identifiers and merge the results. The same limit applies to multiple fields of type `user`. The example also loads only the first page from [crm.status.list](../../../api-reference/crm/status/crm-status-list.md) and [crm.currency.list](../../../api-reference/crm/currency/crm-currency-list.md) — no more than 50 records. If a lookup list contains more values, some options will be missing from the form
- The example does not validate the format of phone numbers, email addresses, dates, and other values in the browser beyond the capabilities of standard HTML fields

{% note warning "Do not publish the example without your own authentication" %}

The form page allows performing actions on behalf of the webhook user. Before publishing it on the internet, add your own authentication, user permission checks, CSRF protection, and secure error logging.

{% endnote %}
