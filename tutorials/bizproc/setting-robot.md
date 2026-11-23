# How to Embed Your UI in Automation Rule Parameters

> Scope: [`bizproc`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: administrator

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

In Bitrix24, you can configure an Automation rule or a workflow action through an application interface. The user opens the Automation rule settings, Bitrix24 shows the application page there, and the application passes the selected values back to the Automation rule form. This is implemented through the standard [widget embedding mechanism](../../api-reference/widgets/index.md).

In the Self-hosted version, Automation rule configuration through embedding is available starting with version [20.0.600](../../settings/cloud-and-on-premise/on-premise/versions.md) of the `Workflows` module.

In this example, the application adds an Automation rule with two `string` parameters.

The scenario consists of four steps.

1. Register the Automation rule using [bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md) with `USE_PLACEMENT` and `PLACEMENT_HANDLER`
2. Get data from `PLACEMENT_OPTIONS` in the handler
3. Save parameter values with the [BX24.placement.call](../../api-reference/widgets/ui-interaction/bx24-placement-call.md) command `setPropertyValue`
4. Check the Automation rule list with [bizproc.robot.list](../../api-reference/bizproc/bizproc-robot/bizproc-robot-list.md)

{% note info "" %}

`bizproc.robot.*` methods work only within an [application](../../settings/app-installation/index.md) context. An inbound webhook will return the `ACCESS_DENIED` error with the description `Access denied! Application context required`.

{% endnote %}

## Prepare the Application

Before registering the Automation rule, prepare:

- an installed application with the `bizproc` permission
- a public HTTPS Automation rule handler `HANDLER`
- a public HTTPS settings page handler `PLACEMENT_HANDLER`
- the user ID `AUTH_USER_ID`, which you can find in the employee profile URL. This is the default user in the *Run as* field in the Automation rule settings: Bitrix24 passes the token of the user selected in this field to the handler, or the token of `AUTH_USER_ID` if the field is empty
- the `B24_CLIENT_ID` and `B24_CLIENT_SECRET` environment variables for the server examples
- storage for the `application_token` from the [ONAPPINSTALL](../../api-reference/common/events/on-app-install.md) event: save the value for each Bitrix24 account when the application is installed and implement a function that returns it by domain. The `HANDLER` handler calls this function
- SDK dependencies for the selected stack: `npm install @bitrix24/b24jssdk`, `composer require bitrix24/b24phpsdk:"^3.0"`, or `pip install b24pysdk`. The server handlers in the examples also require Express for Node.js or Flask for Python

`HANDLER` and `PLACEMENT_HANDLER` can point to the same URL if the application separates Automation rule execution requests from settings page requests itself. The examples use two different URLs.

The scenario has three code areas:

- application page in an iframe: registers the Automation rule through `bizproc.robot.add` and can check the Automation rule list
- server Automation rule handler `HANDLER`: receives data when automation runs the Automation rule. See the example in [Automation Rule Execution Handler](#handler)
- settings handler `PLACEMENT_HANDLER`: returns the Automation rule settings page and saves values through `setPropertyValue`

Before testing the scenario, deploy the handlers from [Full Embedding Handler Code](#settings-handler) and [Automation Rule Execution Handler](#handler) at the `PLACEMENT_HANDLER` and `HANDLER` URLs.

In the examples, replace:

- `https://your-domain.example/robot-handler.php` with your Automation rule execution handler URL
- `https://your-domain.example/robot-settings.php` with your Automation rule settings page URL
- `AUTH_USER_ID` with the ID of the user on whose behalf the Automation rule will execute requests by default
- `robot` with an Automation rule code that is unique within the application

## Initialize the SDK in an Application Context

For the application page in an iframe, use `initializeB24Frame()`. On the server, register the Automation rule, for example, in the [ONAPPINSTALL](../../api-reference/common/events/on-app-install.md) event handler: Bitrix24 passes the `auth` object as form fields such as `auth[domain]` and `auth[access_token]`. The token in the request to `HANDLER` belongs to the user on whose behalf the Automation rule runs and can be used for registration only if that user is an administrator.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()
    ```

- Python

    ```python
    # pip install b24pysdk flask
    import os
    from flask import request
    from b24pysdk import BitrixApp, BitrixToken, Client

    APP = BitrixApp(
        client_id=os.environ["B24_CLIENT_ID"],
        client_secret=os.environ["B24_CLIENT_SECRET"],
    )

    def make_client(auth: dict) -> tuple[Client, BitrixToken]:
        token = BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth.get("refresh_token", ""),
            bitrix_app=APP,
        )
        return Client(token), token

    # Inside a Flask handler: Bitrix24 passes auth as form fields auth[domain], auth[access_token], ...
    auth = {k[5:-1]: v for k, v in request.form.items() if k.startswith("auth[")}
    client, token = make_client(auth)
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Credentials\ApplicationProfile;
    use Bitrix24\SDK\Core\Credentials\AuthToken;
    use Bitrix24\SDK\Core\Credentials\DefaultOAuthServerUrl;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Monolog\Handler\StreamHandler;
    use Monolog\Logger;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();
    $appProfile = ApplicationProfile::initFromArray([
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => getenv('B24_CLIENT_ID'),
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => getenv('B24_CLIENT_SECRET'),
        'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'bizproc',
    ]);

    $authToken = AuthToken::initFromEventRequest($request);
    $domain = (string)$request->request->all('auth')['domain'];

    $log = new Logger('bizproc');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->init($appProfile, $authToken, $domain, DefaultOAuthServerUrl::default());
    ```
{% endlist %}

## 1. Register the Automation Rule

To make parameters configurable through the application, pass `USE_PLACEMENT = 'Y'` and the handler URL in `PLACEMENT_HANDLER` when adding the Automation rule.

{% include [Note on examples](../../_includes/examples.md) %}

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
            HANDLER: 'https://your-domain.example/robot-handler.php',
            AUTH_USER_ID: 1,
            NAME: 'Embedded Automation rule example',
            USE_PLACEMENT: 'Y',
            PLACEMENT_HANDLER: 'https://your-domain.example/robot-settings.php',
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

    console.log(response.getData().result)
    ```

- Python

    ```python
    # client is built on the application token
    result = client.bizproc.robot.add(
        code="robot",
        handler="https://your-domain.example/robot-handler.php",
        name="Embedded Automation rule example",
        auth_user_id=1,
        use_placement=True,
        placement_handler="https://your-domain.example/robot-settings.php",
        properties={
            "string": {"Name": "Parameter 1", "Type": "string"},
            "stringm": {"Name": "Parameter 2", "Type": "string", "Multiple": "Y", "Default": ["value 1", "value 2"]},
        },
    ).response

    print(result.result)
    ```


- PHP

    ```php
    <?php
    // $b24 is built on the application token

    // The typed getBizProcScope()->robot()->add(...) method accepts localized
    // arrays. For a short example, call the method directly through the SDK core.
    $response = $b24->core->call('bizproc.robot.add', [
        'CODE' => 'robot',
        'HANDLER' => 'https://your-domain.example/robot-handler.php',
        'AUTH_USER_ID' => 1,
        'NAME' => 'Embedded Automation rule example',
        'USE_PLACEMENT' => 'Y',
        'PLACEMENT_HANDLER' => 'https://your-domain.example/robot-settings.php',
        'PROPERTIES' => [
            'string' => ['Name' => 'Parameter 1', 'Type' => 'string'],
            'stringm' => ['Name' => 'Parameter 2', 'Type' => 'string', 'Multiple' => 'Y', 'Default' => ['value 1', 'value 2']],
        ],
    ]);

    var_dump($response->getResponseData()->getResult()[0]); // The SDK returns a scalar result as an array
    ```
{% endlist %}

A successful call returns `true`.

```json
{
    "result": true
}
```

After registration, save the Automation rule code `robot`. Bitrix24 will pass it to the settings handler in the `code` field of the `PLACEMENT_OPTIONS` object.

## 2. Get Embedding Handler Data

Bitrix24 passes the following data to the handler in `PLACEMENT_OPTIONS`:

- `code` - your automation rule code provided during registration
- `activity_name` - the action identifier in the workflow template
- `properties` - descriptions of the Automation rule properties
- `current_values` - current property values
- `document_type` - the document type being configured
- `document_fields` - a list of document fields
- `template` - a list of available template fields (parameters, variables, constants, global variables and constants, `return_activities`). In the Self-hosted version, this is available starting from version [24.200.0](../../settings/cloud-and-on-premise/on-premise/versions.md)

In an iframe, b24jssdk provides this data as a ready-to-use object in `$b24.placement.options`. On the server, `PLACEMENT_OPTIONS` arrives in the POST request as a JSON string: parse it with `json_decode($_POST['PLACEMENT_OPTIONS'], true)` in PHP or `json.loads(request.form["PLACEMENT_OPTIONS"])` in Python.

Properties in `properties` arrive in the format saved by `bizproc.robot.add`: each key is a property code from `PROPERTIES`, and the description fields are uppercase. For the Automation rule in the example:

```json
{
    "string": {
        "NAME": "Parameter 1",
        "TYPE": "string"
    },
    "stringm": {
        "NAME": "Parameter 2",
        "TYPE": "string",
        "MULTIPLE": "Y",
        "DEFAULT": ["value 1", "value 2"]
    }
}
```

## 3. Save Automation Rule Parameters

To save parameter values in the Automation rule form, use the `setPropertyValue` command in the embedding handler. In b24jssdk, it is called through `$b24.placement.call`:

```js
import { initializeB24Frame } from '@bitrix24/b24jssdk'

const $b24 = await initializeB24Frame()

// You can pass several properties: property ID -> value
await $b24.placement.call('setPropertyValue', {
    string: 'test string',
    stringm: ['test2', 'test3'],
})
```

The command accepts an object where the key is the property ID from `PROPERTIES` and the value is the new property value. Bitrix24 skips keys that are not in `PROPERTIES` without an error. After that, the user saves the Automation rule as usual.

The next time the settings are opened, Bitrix24 will pass the saved values in `current_values`.

## 4. Check the Automation Rule List

Retrieve the list of application Automation rules:

{% list tabs %}

- JS

    ```js
    // Application Automation rule list
    const listResponse = await $b24.actions.v2.call.make({
        method: 'bizproc.robot.list',
        requestId: 'bizproc-robot-list',
    })
    if (!listResponse.isSuccess) {
        throw new Error(listResponse.getErrorMessages().join('; '))
    }

    const codes = listResponse.getData().result
    console.log(codes)
    ```

- Python

    ```python
    # Application Automation rule list
    codes = client.bizproc.robot.list().response.result
    print(codes)
    ```


- PHP

    ```php
    // Application Automation rule list
    $codes = $b24->getBizProcScope()->robot()->list()->getRobots();
    ```
{% endlist %}

The `bizproc.robot.list` method returns an array of application Automation rule codes.

```json
{
    "result": [
        "robot"
    ]
}
```

## Full Embedding Handler Code {#settings-handler}

Deploy the page at the `PLACEMENT_HANDLER` URL. Include the JS code on the page as a module: build it with a bundler with `@bitrix24/b24jssdk` installed, or replace the import with an ESM URL as in the Python and PHP examples. The handler renders a form from the `properties` list and saves values with the `setPropertyValue` command. A separate field is created for each current value, or one empty field for an empty property. A multiple property has an *Add value* button: it creates an empty field, and when any field changes, the application passes the entire list of values for that property.

{% list tabs %}

- JS

    ```js
    // Embedding handler page (application iframe)
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()
    const options = $b24.placement.options
    const form = document.createElement('form')
    form.name = 'props'

    for (const [id, property] of Object.entries(options.properties || {})) {
        const multiple = property.MULTIPLE === 'Y'
        const current = [].concat(options.current_values?.[id] ?? '')
        const values = current.length ? current : ['']

        const label = document.createElement('label')
        label.textContent = property.NAME
        form.appendChild(label)

        for (const value of values) {
            const input = document.createElement('input')
            input.value = value
            input.dataset.id = id
            input.dataset.multiple = multiple ? '1' : '0'
            form.appendChild(input)
        }

        if (multiple) {
            const button = document.createElement('button')
            button.type = 'button'
            button.textContent = 'Add value'
            button.dataset.add = id
            form.appendChild(button)
        }
    }

    document.body.appendChild(form)

    const send = (id) => {
        const inputs = [...form.querySelectorAll(`[data-id="${id}"]`)]
        const values = inputs.map((input) => input.value)
        $b24.placement.call('setPropertyValue', { [id]: inputs[0].dataset.multiple === '1' ? values : values[0] })
    }
    form.addEventListener('change', (event) => {
        if (event.target.dataset.id) send(event.target.dataset.id)
    })
    form.addEventListener('click', (event) => {
        const id = event.target.dataset.add
        if (!id) return
        const last = [...form.querySelectorAll(`[data-id="${id}"]`)].pop()
        const input = last.cloneNode()
        input.value = ''
        last.after(input)
    })
    ```

- Python

    ```python
    # Flask: the server returns the settings page HTML; PLACEMENT_OPTIONS arrives as a JSON string
    import html
    import json
    from flask import Flask, request

    app = Flask(__name__)

    # Keep JS in a regular string without the f-prefix: curly braces remain as they are
    SCRIPT = """<script type="module">
        import { initializeB24Frame } from 'https://esm.sh/@bitrix24/b24jssdk'
        const $b24 = await initializeB24Frame()
        const form = document.forms.props
        const send = (id) => {
            const inputs = [...form.querySelectorAll(`[data-id="${id}"]`)]
            const values = inputs.map((input) => input.value)
            $b24.placement.call('setPropertyValue', { [id]: inputs[0].dataset.multiple === '1' ? values : values[0] })
        }
        form.addEventListener('change', (event) => {
            if (event.target.dataset.id) send(event.target.dataset.id)
        })
        form.addEventListener('click', (event) => {
            const id = event.target.dataset.add
            if (!id) return
            const last = [...form.querySelectorAll(`[data-id="${id}"]`)].pop()
            const input = last.cloneNode()
            input.value = ''
            last.after(input)
        })
    </script>"""

    @app.post("/robot-settings.php")
    def robot_settings():
        options = json.loads(request.form.get("PLACEMENT_OPTIONS", "{}") or "{}")

        rows = []
        for prop_id, prop in (options.get("properties") or {}).items():
            multiple = prop.get("MULTIPLE") == "Y"
            values = options.get("current_values", {}).get(prop_id)
            values = values if isinstance(values, list) else [values if values is not None else ""]
            values = values or [""]
            attrs = f'data-id="{html.escape(prop_id)}" data-multiple="{int(multiple)}"'
            row = "".join(f'<input {attrs} value="{html.escape(str(v))}">' for v in values)
            if multiple:
                row += f'<button type="button" data-add="{html.escape(prop_id)}">Add value</button>'
            title = html.escape(prop.get("NAME") or prop_id)
            rows.append(f"<label>{title}:</label>{row}")

        form_html = f'<form name="props">{"".join(rows)}</form>'
        return f"<!DOCTYPE html><html><body>\n{form_html}\n{SCRIPT}</body></html>"
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
                $values = (array)($options['current_values'][$id] ?? '') ?: [''];
                $attrs = 'data-id="' . htmlspecialchars($id) . '" data-multiple="' . (int)$multiple . '"'; ?>
                <label><?=htmlspecialchars($property['NAME'] ?? $id)?>:</label>
                <?php foreach ($values as $v): ?>
                    <input <?=$attrs?> value="<?=htmlspecialchars((string)$v)?>">
                <?php endforeach; ?>
                <?php if ($multiple): ?>
                    <button type="button" data-add="<?=htmlspecialchars($id)?>">Add value</button>
                <?php endif; ?>
            <?php endforeach; ?>
            </form>
            <script type="module">
                // b24jssdk is connected via an ESM build or compiled by a bundler
                import { initializeB24Frame } from 'https://esm.sh/@bitrix24/b24jssdk'
                const $b24 = await initializeB24Frame()
                const form = document.forms.props
                const send = (id) => {
                    const inputs = [...form.querySelectorAll(`[data-id="${id}"]`)]
                    const values = inputs.map((input) => input.value)
                    $b24.placement.call('setPropertyValue', { [id]: inputs[0].dataset.multiple === '1' ? values : values[0] })
                }
                form.addEventListener('change', (event) => {
                    if (event.target.dataset.id) send(event.target.dataset.id)
                })
                form.addEventListener('click', (event) => {
                    const id = event.target.dataset.add
                    if (!id) return
                    const last = [...form.querySelectorAll(`[data-id="${id}"]`)].pop()
                    const input = last.cloneNode()
                    input.value = ''
                    last.after(input)
                })
            </script>
        </body>
    </html>
    ```

{% endlist %}

## Automation Rule Execution Handler {#handler}

When automation reaches the Automation rule, Bitrix24 sends a POST request to `HANDLER` in `application/x-www-form-urlencoded` format. The request contains the Automation rule code `code`, the document `document_id`, parameter values `properties`, and tokens `auth`. For the full list of fields, see [How Result Waiting Works](../../api-reference/bizproc/bizproc-robot/index.md#handler).

The Automation rule in the example is registered without `USE_SUBSCRIPTION`, so Bitrix24 does not wait for the handler result: automation continues after the request is queued. The handler only needs to read the parameters and respond with status code `200`.

The `HANDLER` URL is public. Before running the main logic, the handler checks `auth[application_token]` against the stored value and responds with `403` if they do not match. In the examples, the stored value is returned by `getSavedAppToken` in JS and PHP and `get_saved_app_token` in Python. Implement this function using your storage. Do not log tokens from `auth`.

{% list tabs %}

- JS

    ```js
    // npm install express
    import { timingSafeEqual } from 'node:crypto'
    import express from 'express'

    const app = express()
    app.use(express.urlencoded({ extended: true }))

    const sameToken = (a = '', b = '') => {
        const left = Buffer.from(String(a))
        const right = Buffer.from(String(b))
        return left.length > 0 && left.length === right.length && timingSafeEqual(left, right)
    }

    app.post('/robot-handler.php', async (req, res) => {
        const { code, document_id: documentId = [], properties = {}, auth = {} } = req.body

        // getSavedAppToken is your function that returns the application_token saved from ONAPPINSTALL
        if (!sameToken(auth.application_token, await getSavedAppToken(auth.domain))) {
            return res.sendStatus(403)
        }

        if (code === 'robot') {
            const single = properties.string ?? ''
            const multiple = [].concat(properties.stringm ?? [])

            // Automation rule logic goes here, for example, sending data to an external system
            console.log(`Automation rule started for ${documentId[2]}`, single, multiple)
        }

        res.sendStatus(200)
    })

    app.listen(3000)
    ```

- Python

    ```python
    # pip install flask
    import hmac
    from flask import Flask, request

    app = Flask(__name__)

    @app.post("/robot-handler.php")
    def robot_handler():
        form = request.form  # Fields such as properties[string], properties[stringm][0]

        # get_saved_app_token is your function that returns the application_token saved from ONAPPINSTALL
        saved = get_saved_app_token(form.get("auth[domain]", ""))
        if not saved or not hmac.compare_digest(form.get("auth[application_token]", ""), saved):
            return "", 403

        if form.get("code") == "robot":
            document_id = form.get("document_id[2]", "")  # For example, DEAL_17
            single = form.get("properties[string]", "")
            multiple = [v for k, v in form.items(multi=True) if k.startswith("properties[stringm]")]

            # Automation rule logic goes here, for example, sending data to an external system
            print(f"Automation rule started for {document_id}", single, multiple)

        return "", 200
    ```

- PHP

    ```php
    <?php
    // robot-handler.php: PHP parses form fields into arrays automatically
    // getSavedAppToken is your function that returns the application_token saved from ONAPPINSTALL
    $saved = getSavedAppToken($_POST['auth']['domain'] ?? '');
    if ($saved === '' || !hash_equals($saved, (string)($_POST['auth']['application_token'] ?? ''))) {
        http_response_code(403);
        exit;
    }

    if (($_POST['code'] ?? '') === 'robot') {
        $documentId = $_POST['document_id'][2] ?? ''; // For example, DEAL_17
        $single = $_POST['properties']['string'] ?? '';
        $multiple = (array)($_POST['properties']['stringm'] ?? []);

        // Automation rule logic goes here, for example, sending data to an external system
        error_log("Automation rule started for {$documentId}: {$single}; " . implode(', ', $multiple));
    }

    http_response_code(200);
    ```

{% endlist %}

If the Automation rule needs to return a result to automation, register it with `USE_SUBSCRIPTION = 'Y'` and `RETURN_PROPERTIES`, and call [bizproc.event.send](../../api-reference/bizproc/bizproc-robot/bizproc-event-send.md) in the handler with the `event_token` from the request. This type of handler is covered in [How to Create a Custom Workflow Action](./how-to-create-custom-activity.md).

## Check the Result

1. Open CRM automation settings or a workflow template
2. Add the application Automation rule named `Embedded Automation rule example`
3. Open the Automation rule settings and check that they display the page at `PLACEMENT_HANDLER`
4. Change parameter values and save the Automation rule. Open its settings again and make sure the fields contain the saved values
5. Start automation, for example, by moving a deal to a stage with the Automation rule, and check that `HANDLER` received a request with the entered values in `properties`

Use REST to check that the `robot` code is present in the `bizproc.robot.list` response.

To delete the test Automation rule after checking it, call [bizproc.robot.delete](../../api-reference/bizproc/bizproc-robot/bizproc-robot-delete.md) with `CODE: 'robot'`. The Automation rule will disappear from the automation settings.

## Error Diagnostics

If a method returns an error, check the request data.

- `ACCESS_DENIED` with the description `Access denied! Application context required`: the method was called outside the application context. Open the application and repeat the call in its context
- `ACCESS_DENIED` with the description `Access denied!`: the method was called by a non-administrator. Repeat the call as an administrator
- `ERROR_ACTIVITY_VALIDATION_FAILURE`: a required parameter is missing or `CODE`, `PROPERTIES`, `DOCUMENT_TYPE`, or `FILTER` is filled incorrectly
- `ERROR_UNSUPPORTED_PROTOCOL`: the handler URL uses an unsupported protocol
- `ERROR_WRONG_HANDLER_URL`: the handler URL failed validation
- `ERROR_ACTIVITY_ALREADY_INSTALLED`: an Automation rule with this code has already been registered by this application
- `HANDLER` responded with `403`: no stored `application_token` was found for this Bitrix24 account, or the storage function returned a different value. Check the storage and run automation again
- `HANDLER` did not receive a request: check that the URL is accessible from the internet over HTTPS and review the web server log. Then run automation again
- `ERROR_CORE` with the description `Unable to set placement handler: You should use unique placement handler for each CODE!`: this `PLACEMENT_HANDLER` is already used by another Automation rule or workflow action of the application. Set a different settings page URL for each `CODE`. If a different message follows `Unable to set placement handler:`, investigate that message
- the Automation rule settings open in the standard form instead of the application page: the Automation rule was registered without `USE_PLACEMENT = 'Y'`. Pass `USE_PLACEMENT` and `PLACEMENT_HANDLER` using [bizproc.robot.update](../../api-reference/bizproc/bizproc-robot/bizproc-robot-update.md)
- a value did not appear in the form after `setPropertyValue`: the key in the command does not match the property code from `PROPERTIES`

After correcting the registration parameters, repeat the scenario from step 1. If the error occurred while saving values through `setPropertyValue`, repeat the scenario from step 3.

## Important Notes

- Bitrix24 opens `PLACEMENT_HANDLER` in an iframe on its own HTTPS page, so the browser will not load a settings page served over HTTP

## Continue Learning

- [Application Automation rules: method overview](../../api-reference/bizproc/bizproc-robot/index.md)
- [Register a new Automation rule bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md)
- [Update Automation rule fields bizproc.robot.update](../../api-reference/bizproc/bizproc-robot/bizproc-robot-update.md)
- [Call a registered UI command BX24.placement.call](../../api-reference/widgets/ui-interaction/bx24-placement-call.md)
