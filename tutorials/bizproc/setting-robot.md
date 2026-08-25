# How to Embed Your UI in Automation Rule Parameters

> Scope: [`bizproc`, `placement`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: administrator

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

In Bitrix24, you can configure an Automation rule or a workflow action through an application interface. The user opens the Automation rule settings, Bitrix24 shows the application page in a slider, and the application passes the selected values back to the Automation rule form. This is implemented through the standard [widget embedding mechanism](../../api-reference/widgets/index.md).

In the Self-hosted version, Automation rule configuration through embedding is available starting with version [20.0.600](../../settings/cloud-and-on-premise/on-premise/versions.md) of the `Workflows` module.

In this example, the application adds an Automation rule with two `string` parameters.

The scenario consists of four steps.

1. Register the Automation rule using [bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md) with `USE_PLACEMENT` and `PLACEMENT_HANDLER`
2. Get data from `PLACEMENT_OPTIONS` in the handler
3. Save parameter values with the [BX24.placement.call](../../api-reference/widgets/ui-interaction/bx24-placement-call.md) command `setPropertyValue`
4. Check the Automation rule list with [bizproc.robot.list](../../api-reference/bizproc/bizproc-robot/bizproc-robot-list.md) or delete the test Automation rule with [bizproc.robot.delete](../../api-reference/bizproc/bizproc-robot/bizproc-robot-delete.md)

{% note info "" %}

`bizproc.robot.*` methods work only within an [application](../../settings/app-installation/index.md) context. An incoming webhook will return the `ACCESS_DENIED` error with the description `Application context required`.

{% endnote %}

## Prepare the Application

Before registering the Automation rule, prepare:

- an installed application with the `bizproc` permission
- a public HTTPS Automation rule handler `HANDLER`
- a public HTTPS settings page handler `PLACEMENT_HANDLER`
- the `AUTH_USER_ID` of the user whose token Bitrix24 will pass to the application when the Automation rule runs
- SDK dependencies for the selected stack: `npm install @bitrix24/b24jssdk`, `composer require bitrix24/b24phpsdk:"^3.0"`, or `pip install b24pysdk`

`HANDLER` and `PLACEMENT_HANDLER` can point to the same URL if the application separates Automation rule execution requests from settings page requests itself.

The scenario has three code areas:

- application page in an iframe: registers the Automation rule through `bizproc.robot.add` and can check the Automation rule list
- server Automation rule handler `HANDLER`: receives data when automation runs the Automation rule
- settings handler `PLACEMENT_HANDLER`: returns the Automation rule settings page and saves values through `setPropertyValue`

In the examples, replace:

- `https://your-domain.example/handler.php` with your application handler URL
- `AUTH_USER_ID` with the ID of the user on whose behalf the Automation rule will execute requests
- `robot` with an Automation rule code that is unique within the application

## Initialize the SDK in an Application Context

`bizproc.robot.*` methods require an application context. For the application page in an iframe, use `initializeB24Frame()`. For server-side PHP and Python handlers, create a client from the `auth` object that Bitrix24 passes in the application request.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()
    ```

- Python

    ```python
    # pip install b24pysdk
    from flask import request
    from b24pysdk import BitrixApp, BitrixToken, Client

    APP = BitrixApp(client_id="local.xxxxxxxx.xxxxxxxx", client_secret="yyyyyyyy")

    def make_client(auth: dict) -> tuple[Client, BitrixToken]:
        token = BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth.get("refresh_token", ""),
            bitrix_app=APP,
        )
        return Client(token), token

    auth = request.json["auth"]  # auth dictionary from the handler request body
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
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => 'local.xxxxxxxx.xxxxxxxx',
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => 'yyyyyyyy',
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
            HANDLER: 'https://your-domain.example/handler.php',
            AUTH_USER_ID: 1,
            NAME: 'Embedded Automation rule example',
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

    console.log(response.getData().result)
    ```

- Python

    ```python
    # client is built on the application token
    result = client.bizproc.robot.add(
        code="robot",
        handler="https://your-domain.example/handler.php",
        name="Embedded Automation rule example",
        auth_user_id=1,
        use_placement=True,
        placement_handler="https://your-domain.example/handler.php",
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
        'HANDLER' => 'https://your-domain.example/handler.php',
        'AUTH_USER_ID' => 1,
        'NAME' => 'Embedded Automation rule example',
        'USE_PLACEMENT' => 'Y',
        'PLACEMENT_HANDLER' => 'https://your-domain.example/handler.php',
        'PROPERTIES' => [
            'string' => ['Name' => 'Parameter 1', 'Type' => 'string'],
            'stringm' => ['Name' => 'Parameter 2', 'Type' => 'string', 'Multiple' => 'Y', 'Default' => ['value 1', 'value 2']],
        ],
    ]);

    var_dump($response->getResponseData()->getResult());
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
- `properties` - a list of properties and their descriptions
- `current_values` - current property values
- `document_type` - the document type being configured
- `document_fields` - a list of document fields
- `template` - a list of available template fields (parameters, variables, constants, global variables and constants, `return_activities`). In the Self-hosted version, this is available starting from version [24.200.0](../../settings/cloud-and-on-premise/on-premise/versions.md)

The property structure is unified into a single format:

```js
{
    Id: 'string',        // property identifier (code)
    Type: 'string',      // property type identifier
    Name: 'string',      // name
    Description: 'string',
    Multiple: false,     // whether it is a multiple property
    Required: false,     // whether it is a required property
    Options: '',         // depends on the property type
    Settings: [],        // depends on the property type
    Default: ''          // default value
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

The command accepts an object where the key is the property ID from `PROPERTIES`, and the value is the new property value. After that, the user saves the Automation rule as usual.

The next time the settings are opened, Bitrix24 will pass the saved values in `current_values`.

## 4. Check or Delete the Automation Rule

To retrieve a list of installed Automation rules and delete an Automation rule:

{% list tabs %}

- JS

    ```js
    // Application Automation rule list
    const listResponse = await $b24.actions.v2.call.make({
        method: 'bizproc.robot.list',
        requestId: 'bizproc-robot-list',
    })
    const codes = listResponse.getData().result
    console.log(codes)

    // Delete Automation rule by code
    await $b24.actions.v2.call.make({
        method: 'bizproc.robot.delete',
        params: { CODE: 'robot' },
        requestId: 'bizproc-robot-delete',
    })
    ```

- Python

    ```python
    # Application Automation rule list
    codes = client.bizproc.robot.list().response.result
    print(codes)

    # Delete Automation rule by code
    client.bizproc.robot.delete(code="robot").response
    ```


- PHP

    ```php
    // Application Automation rule list
    $codes = $b24->getBizProcScope()->robot()->list()->getRobots();

    // Delete Automation rule by code
    $b24->getBizProcScope()->robot()->delete('robot');
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

## Full Embedding Handler Code

The handler renders a form from the `properties` list and saves values with the `setPropertyValue` command. The form can be built in the browser through b24jssdk in frame mode.

{% list tabs %}

- JS

    ```js
    // Embedding handler page (application iframe)
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()
    const options = $b24.placement.options
    const form = document.createElement('form')

    for (const [id, property] of Object.entries(options.properties || {})) {
        const multiple = property.Multiple === true || property.Multiple === 'Y' || property.MULTIPLE === 'Y'
        const values = [].concat(options.current_values?.[id] ?? '')

        const label = document.createElement('label')
        label.textContent = property.Name || property.NAME
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

- Python

    ```python
    # Flask: the server returns the handler's HTML, PLACEMENT_OPTIONS arrives as a JSON string
    from flask import request
    import json, html

    options = json.loads(request.form.get("PLACEMENT_OPTIONS", "{}") or "{}")

    rows = []
    for prop_id, prop in (options.get("properties") or {}).items():
        multiple = prop.get("Multiple") is True or prop.get("Multiple") == "Y" or prop.get("MULTIPLE") == "Y"
        values = options.get("current_values", {}).get(prop_id, "")
        values = values if isinstance(values, list) else [values]
        name = f"{prop_id}[]" if multiple else prop_id
        inputs = "".join(
            f'<input name="{name}" value="{html.escape(str(v))}" '
            f'onchange="setPropertyValue(\'{prop_id}\', this.name, {int(multiple)})">'
            for v in values
        )
        rows.append(f'<label>{html.escape(prop.get("Name") or prop.get("NAME"))}:</label>{inputs}')

    # Keep JS in a regular string without the f-prefix: curly braces remain as they are
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
                $multiple = ($property['Multiple'] ?? false) === true || ($property['Multiple'] ?? '') === 'Y' || ($property['MULTIPLE'] ?? '') === 'Y';
                $values = (array)($options['current_values'][$id] ?? '');
                $name = $multiple ? $id . '[]' : $id; ?>
                <label><?=htmlspecialchars($property['Name'] ?? $property['NAME'])?>:</label>
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
{% endlist %}

## Check the Result

1. Open CRM automation settings or a workflow template.
2. Add the application Automation rule named `Embedded Automation rule example`.
3. Open the Automation rule settings and check that Bitrix24 opens `PLACEMENT_HANDLER` in a slider.
4. Change the parameter values and save the Automation rule.

Use REST to check that the `robot` code is present in the `bizproc.robot.list` response.

## Error Diagnostics

If a method returns an error, check the request data.

- `ACCESS_DENIED` with the description `Application context required`: the method was called outside the application context
- `ACCESS_DENIED` with the description `Access denied!`: the method was called by a non-administrator
- `ERROR_ACTIVITY_VALIDATION_FAILURE`: a required parameter is missing or `CODE`, `PROPERTIES`, `DOCUMENT_TYPE`, or `FILTER` is filled incorrectly
- `ERROR_UNSUPPORTED_PROTOCOL`: the handler URL uses an unsupported protocol
- `ERROR_WRONG_HANDLER_URL`: the handler URL failed validation
- `ERROR_ACTIVITY_ALREADY_INSTALLED`: an Automation rule with this code has already been registered by this application

After correcting the registration parameters, repeat the scenario from step 1. If the error occurred while saving values through `setPropertyValue`, repeat the scenario from step 3.

## Important Notes

- The `bizproc.robot.add`, `bizproc.robot.list`, and `bizproc.robot.delete` methods are not marked as deprecated in the documentation and are registered in the source code as current methods
- `PLACEMENT_HANDLER` must be available over HTTPS and be located on the installed application's domain
- Values passed through `setPropertyValue` are saved in the settings form. To apply the changes to the automation or workflow template, the user must save the Automation rule
- Running the example again with the same `CODE` will return an error if the Automation rule is already registered

## Continue Learning

- [Application Automation rules: method overview](../../api-reference/bizproc/bizproc-robot/index.md)
- [Register a new Automation rule bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md)
- [Update Automation rule fields bizproc.robot.update](../../api-reference/bizproc/bizproc-robot/bizproc-robot-update.md)
- [Call a registered UI command BX24.placement.call](../../api-reference/widgets/ui-interaction/bx24-placement-call.md)
