# How to Embed Your UI in Robot Parameters

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In *Bitrix24*, you can configure an automation rule or a workflow action using an application interface. This is implemented via the standard [widget embedding mechanism](../../api-reference/widgets/index.md). In the Self-hosted version, the Business processes module is available starting from version [20.0.600](../../settings/cloud-and-on-premise/on-premise/versions.md).

In this example, the application adds an automation rule that has 2 parameters of type `string`.

{% note info "" %}

`bizproc.robot.*` methods work only within the [application](../../settings/app-installation/index.md) context. Via an incoming webhook, the method will return error `ACCESS_DENIED` with the description "Application context required". For SDK initialization within an application context, see the [How to Add an Action to Create an Invoice](./activity.md#initializing-the-sdk-in-an-application-context) scenario.

{% endnote %}

## Registering an Automation Rule

To make parameters configurable via an application, pass `USE_PLACEMENT=Y` and the `PLACEMENT_HANDLER` handler when adding the automation rule.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    // The application page is open in a Bitrix24 iframe
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.robot.add',
        params: {
            CODE: 'robot',
            HANDLER: 'https://your-domain.example/handler.php',
            AUTH_USER_ID: 1,
            NAME: 'Example of an embedded robot',
            USE_PLACEMENT: 'Y',
            PLACEMENT_HANDLER: 'https://your-domain.example/handler.php',
            PROPERTIES: {
                string: { Name: 'Parameter 1', Type: 'string' },
                stringm: { Name: 'Parameter 2', Type: 'string', Multiple: 'Y', Default: ['value 1', 'value 2'] },
            },
        },
        requestId: 'bizproc-robot-add',
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }
    ```

- PHP

    ```php
    <?php
    // $b24 is built on the application token (see the scenario "How to add an action to create an invoice")

    // The typed getBizProcScope()->robot()->add(...) accepts localized
    // arrays. For a simple example, we call the method directly via the core.
    $b24->core->call('bizproc.robot.add', [
        'CODE' => 'robot',
        'HANDLER' => 'https://your-domain.example/handler.php',
        'AUTH_USER_ID' => 1,
        'NAME' => 'Example of an embedded robot',
        'USE_PLACEMENT' => 'Y',
        'PLACEMENT_HANDLER' => 'https://your-domain.example/handler.php',
        'PROPERTIES' => [
            'string' => ['Name' => 'Parameter 1', 'Type' => 'string'],
            'stringm' => ['Name' => 'Parameter 2', 'Type' => 'string', 'Multiple' => 'Y', 'Default' => ['value 1', 'value 2']],
        ],
    ]);
    ```

- Python

    ```python
    # client is built on the application token (see the scenario "How to add an action to create an invoice")
    result = client.bizproc.robot.add(
        code="robot",
        handler="https://your-domain.example/handler.php",
        name="Example of an embedded robot",
        auth_user_id=1,
        use_placement=True,
        placement_handler="https://your-domain.example/handler.php",
        properties={
            "string": {"Name": "Parameter 1", "Type": "string"},
            "stringm": {"Name": "Parameter 2", "Type": "string", "Multiple": "Y", "Default": ["value 1", "value 2"]},
        },
    ).response
    ```

{% endlist %}

## Embedding Handler Data

Bitrix24 passes the following data to the handler in `PLACEMENT_OPTIONS`:

- `code` — your automation rule code provided during registration
- `activity_name` — the action identifier in the workflow template
- `properties` — a list of properties and their descriptions
- `current_values` — current property values
- `document_type` — the document type being configured
- `document_fields` — a list of document fields
- `template` — a list of available template fields (parameters, variables, constants, global variables and constants, `return_activities`). In the Self-hosted version, this is available starting from version [24.200.0](../../settings/cloud-and-on-premise/on-premise/versions.md)

The property structure is unified into a single format:

```js
{
    Id: 'string',        // property identifier (code)
    Type: 'string',      // property type identifier
    Name: 'string',      // name
    Description: 'string',
    Multiple: false,     // whether it is a multiple property or not
    Required: false,     // whether it is a required property or not
    Options: '',         // depends on the property type
    Settings: [],        // depends on the property type
    Default: ''           // default value
}
```

## Saving Automation Rule Parameters

To save parameter values in an automation rule, use the `setPropertyValue` command within the embedding handler. In b24jssdk it is called via `$b24.placement.call`:

```js
import { initializeB24Frame } from '@bitrix24/b24jssdk'

const $b24 = await initializeB24Frame()

// you can pass several properties: property ID → value
await $b24.placement.call('setPropertyValue', {
    string: 'test string',
    stringm: ['test2', 'test3'],
})
```

The user then saves the automation rule as usual.

## Managing Automation Rules

To retrieve a list of installed automation rules and delete an automation rule:

{% list tabs %}

- JS

    ```js
    // Application robot list
    const listResponse = await $b24.actions.v2.call.make({
        method: 'bizproc.robot.list',
        requestId: 'bizproc-robot-list',
    })
    const codes = listResponse.getData().result

    // Delete robot by code
    await $b24.actions.v2.call.make({
        method: 'bizproc.robot.delete',
        params: { CODE: 'robot' },
        requestId: 'bizproc-robot-delete',
    })
    ```

- PHP

    ```php
    // Application robot list
    $codes = $b24->getBizProcScope()->robot()->list()->getRobots();

    // Delete robot by code
    $b24->getBizProcScope()->robot()->delete('robot');
    ```

- Python

    ```python
    # Application robot list
    codes = client.bizproc.robot.list().response.result

    # Delete robot by code
    client.bizproc.robot.delete(code="robot").response
    ```

{% endlist %}

## Full Embedding Handler Code

The handler renders a form based on the `properties` list and saves values using the `setPropertyValue` command. It is most convenient to build the form on the browser side using b24jssdk in frame mode.

{% list tabs %}

- JS

    ```js
    // Embedding handler page (application iframe)
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()
    const options = $b24.placement.options
    const form = document.createElement('form')

    for (const [id, property] of Object.entries(options.properties || {})) {
        const multiple = property.MULTIPLE === 'Y'
        const values = [].concat(options.current_values?.[id] ?? '')

        const label = document.createElement('label')
        label.textContent = property.NAME
        form.appendChild(label)

        values.forEach((value) => {
            const input = document.createElement('input')
            input.value = value
            input.addEventListener('change', () => {
                const all = Array.from(form.querySelectorAll(`[data-id="${id}"]`)).map((i) => i.value)
                $b24.placement.call('setPropertyValue', { [id]: multiple ? all : all[0] })
            })
            input.dataset.id = id
            form.appendChild(input)
        })
    }

    document.body.appendChild(form)
    ```

- PHP

    ```php
    <?php
    // The server returns the handler's HTML page. PLACEMENT_OPTIONS arrives as a JSON string.
    $options = json_decode($_POST['PLACEMENT_OPTIONS'] ?? '{}', true) ?: [];
    ?>
    <!DOCTYPE html>
    <html>
        <body>
            <form name="props">
            <?php foreach (($options['properties'] ?? []) as $id => $property):
                $multiple = ($property['MULTIPLE'] ?? '') === 'Y';
                $values = (array)($options['current_values'][$id] ?? '');
                $name = $multiple ? $id . '[]' : $id; ?>
                <label><?=htmlspecialchars($property['NAME'])?>:</label>
                <?php foreach ($values as $v): ?>
                    <input name="<?=$name?>" value="<?=htmlspecialchars((string)$v)?>"
                           onchange="setPropertyValue('<?=$id?>', this.name, <?=(int)$multiple?>)">
                <?php endforeach; ?>
            <?php endforeach; ?>
            </form>
            <script type="module">
                // b24jssdk is connected via an ESM build or compiled by a bundler
                import { initializeB24Frame } from 'https://esm.sh/@bitrix24/b24jssdk'
                const $b24 = await initializeB24Frame()
                window.setPropertyValue = (id, inputName, multiple) => {
                    const data = new FormData(document.forms.props)
                    const value = multiple ? data.getAll(inputName) : data.get(inputName)
                    $b24.placement.call('setPropertyValue', { [id]: value })
                }
            </script>
        </body>
    </html>
    ```

- Python

    ```python
    # Flask: the server returns the handler's HTML, PLACEMENT_OPTIONS arrives as a JSON string
    from flask import request
    import json, html

    options = json.loads(request.form.get("PLACEMENT_OPTIONS", "{}") or "{}")

    rows = []
    for prop_id, prop in (options.get("properties") or {}).items():
        multiple = prop.get("MULTIPLE") == "Y"
        values = options.get("current_values", {}).get(prop_id, "")
        values = values if isinstance(values, list) else [values]
        name = f"{prop_id}[]" if multiple else prop_id
        inputs = "".join(
            f'<input name="{name}" value="{html.escape(str(v))}" '
            f'onchange="setPropertyValue(\'{prop_id}\', this.name, {int(multiple)})">'
            for v in values
        )
        rows.append(f'<label>{html.escape(prop["NAME"])}:</label>{inputs}')

    # Keep JS in a regular string without the f-prefix — curly braces remain as they are
    script = """<script type="module">
        import { initializeB24Frame } from 'https://esm.sh/@bitrix24/b24jssdk'
        const $b24 = await initializeB24Frame()
        window.setPropertyValue = (id, inputName, multiple) => {
            const data = new FormData(document.forms.props)
            const value = multiple ? data.getAll(inputName) : data.get(inputName)
            $b24.placement.call('setPropertyValue', { [id]: value })
        }
    </script>"""

    form_html = f'<form name="props">{"".join(rows)}</form>'
    page = f"<!DOCTYPE html><html><body>\n{form_html}\n" + script + "</body></html>"
    ```

{% endlist %}
