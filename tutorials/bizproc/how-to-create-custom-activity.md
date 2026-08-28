# How to Create Your Own Workflow Action

> Scope: [`bizproc`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, you need a Bitrix24 administrator
>
> - [bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md) - administrator
> - [bizproc.event.send](../../api-reference/bizproc/bizproc-robot/bizproc-event-send.md) - any user with a valid `EVENT_TOKEN`

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

An application workflow action adds a custom step to the workflow designer. When a workflow reaches this step, Bitrix24 calls the application handler, passes input parameters to it, and can wait for a result. The result is returned using the [bizproc.event.send](../../api-reference/bizproc/bizproc-robot/bizproc-event-send.md) method and becomes available in the next workflow actions.

This scenario shows how to create an `MD5 generator` action. It receives a string from the document, calculates an MD5 hash, and returns it to the workflow as an additional `outputString` result.

The scenario consists of three steps.

1. Register the action using [bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md)
2. Add the action to a workflow template and start the workflow
3. Return the result from the handler using [bizproc.event.send](../../api-reference/bizproc/bizproc-robot/bizproc-event-send.md)

## Before You Start

The `bizproc.activity.add` method works only in the context of an [application](../../settings/app-installation/index.md). An incoming webhook is not suitable: the method returns the `ACCESS_DENIED` error with the `Application context required` description.

Before you start, prepare:

- an installed application with the `bizproc` scope
- a public HTTPS handler address, for example `https://your-domain.example/bp-md5-handler`
- the administrator identifier for the `AUTH_USER_ID` parameter
- a workflow template where you will add the action after registration
- the `B24_CLIENT_ID` and `B24_CLIENT_SECRET` environment variables with the application ID and secret

`AUTH_USER_ID` specifies the user whose authorization Bitrix24 passes to the handler. The handler receives the `auth` object with `domain`, `access_token`, and `refresh_token`. These values are required to call `bizproc.event.send`.

Do not store the application secret in source code. Pass it through environment variables or the application's server-side secret storage.

{% include [Examples Note](../../_includes/examples.md) %}

## Initialize the SDK in the Handler

The handler receives authorization in the request from Bitrix24. Use `auth` to create an SDK client for calling the `bizproc.event.send` method.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk express
    import express from 'express'
    import crypto from 'node:crypto'
    import { B24OAuth } from '@bitrix24/b24jssdk'

    const APP = {
        clientId: process.env.B24_CLIENT_ID,
        clientSecret: process.env.B24_CLIENT_SECRET,
    }
    const app = express()
    app.use(express.json())
    app.use(express.urlencoded({ extended: true }))

    function makeClient(auth) {
        const $b24 = new B24OAuth({
            domain: auth.domain,
            accessToken: auth.access_token,
            refreshToken: auth.refresh_token,
            memberId: auth.member_id,
        }, APP)
        $b24.offClientSideWarning()
        return $b24
    }
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

- Python

    ```python
    # pip install b24pysdk flask
    import hashlib
    import os
    from flask import Flask, request
    from b24pysdk import BitrixApp, BitrixToken, Client

    APP = BitrixApp(
        client_id=os.environ["B24_CLIENT_ID"],
        client_secret=os.environ["B24_CLIENT_SECRET"],
    )
    app = Flask(__name__)

    def get_payload() -> dict:
        json_payload = request.get_json(silent=True)
        if json_payload:
            return json_payload

        form = request.form
        return {
            "event_token": form["event_token"],
            "auth": {
                "domain": form["auth[domain]"],
                "access_token": form["auth[access_token]"],
                "refresh_token": form.get("auth[refresh_token]", ""),
            },
            "properties": {
                "inputString": form.get("properties[inputString]", ""),
            },
        }

    def make_client(auth: dict) -> tuple[Client, BitrixToken]:
        token = BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth.get("refresh_token", ""),
            bitrix_app=APP,
        )
        return Client(token), token
    ```

{% endlist %}

## 1. Register the Action

The [bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md) method adds a new action for use in workflows. Pass a unique application-level action code in `CODE`. In `HANDLER`, specify the public URL where Bitrix24 will send data when the action runs.

Run the registration examples in an installed application with the `bizproc` scope. In the JS example, the code runs in the application iframe. In PHP and Python, the `$b24` and `client` variables must be created using the installed application's token.

To let the handler return a result to the workflow, set:

- `USE_SUBSCRIPTION: 'Y'` - the action will wait for the application response
- `RETURN_PROPERTIES` - the list of results that the handler can return in `RETURN_VALUES` of the `bizproc.event.send` method

In the example, the action receives the `inputString` parameter and returns the `outputString` result.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.activity.add',
        params: {
            CODE: 'md5_action',
            HANDLER: 'https://your-domain.example/bp-md5-handler',
            AUTH_USER_ID: 1,
            USE_SUBSCRIPTION: 'Y',
            NAME: {
                ru: 'MD5 generator',
                en: 'MD5 generator',
            },
            DESCRIPTION: {
                ru: 'The action returns the MD5 hash of the input parameter',
                en: 'The action returns the MD5 hash of the input parameter',
            },
            PROPERTIES: {
                inputString: {
                    Name: {
                        ru: 'Input string',
                        en: 'Input string',
                    },
                    Description: {
                        ru: 'Enter the string to hash',
                        en: 'Enter the string to hash',
                    },
                    Type: 'string',
                    Required: 'Y',
                    Multiple: 'N',
                    Default: '{=Document:NAME}',
                },
            },
            RETURN_PROPERTIES: {
                outputString: {
                    Name: {
                        ru: 'MD5',
                        en: 'MD5',
                    },
                    Type: 'string',
                    Multiple: 'N',
                    Default: null,
                },
            },
        },
        requestId: 'bizproc-activity-add',
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    console.info(response.getData().result) // true
    ```

- PHP

    ```php
    <?php
    // $b24 is built using the installed application's token.
    // The typed activity()->add method accepts extended DTOs,
    // so this short example uses a direct SDK core call.
    $response = $b24->core->call('bizproc.activity.add', [
        'CODE' => 'md5_action',
        'HANDLER' => 'https://your-domain.example/bp-md5-handler',
        'AUTH_USER_ID' => 1,
        'USE_SUBSCRIPTION' => 'Y',
        'NAME' => [
            'ru' => 'MD5 generator',
            'en' => 'MD5 generator',
        ],
        'DESCRIPTION' => [
            'ru' => 'The action returns the MD5 hash of the input parameter',
            'en' => 'The action returns the MD5 hash of the input parameter',
        ],
        'PROPERTIES' => [
            'inputString' => [
                'Name' => [
                    'ru' => 'Input string',
                    'en' => 'Input string',
                ],
                'Description' => [
                    'ru' => 'Enter the string to hash',
                    'en' => 'Enter the string to hash',
                ],
                'Type' => 'string',
                'Required' => 'Y',
                'Multiple' => 'N',
                'Default' => '{=Document:NAME}',
            ],
        ],
        'RETURN_PROPERTIES' => [
            'outputString' => [
                'Name' => [
                    'ru' => 'MD5',
                    'en' => 'MD5',
                ],
                'Type' => 'string',
                'Multiple' => 'N',
                'Default' => null,
            ],
        ],
    ]);

    print_r($response->getResponseData()->getResult()); // true
    ```

- Python

    ```python
    # client is built using the installed application's token.
    result = client.bizproc.activity.add(
        code="md5_action",
        handler="https://your-domain.example/bp-md5-handler",
        auth_user_id=1,
        use_subscription=True,
        name={
            "ru": "MD5 generator",
            "en": "MD5 generator",
        },
        description={
            "ru": "The action returns the MD5 hash of the input parameter",
            "en": "The action returns the MD5 hash of the input parameter",
        },
        properties={
            "inputString": {
                "Name": {
                    "ru": "Input string",
                    "en": "Input string",
                },
                "Description": {
                    "ru": "Enter the string to hash",
                    "en": "Enter the string to hash",
                },
                "Type": "string",
                "Required": "Y",
                "Multiple": "N",
                "Default": "{=Document:NAME}",
            },
        },
        return_properties={
            "outputString": {
                "Name": {
                    "ru": "MD5",
                    "en": "MD5",
                },
                "Type": "string",
                "Multiple": "N",
                "Default": None,
            },
        },
    ).response.result

    print(result)  # True
    ```

{% endlist %}

Example of a successful response:

```json
{
    "result": true
}
```

The method returns `true` if the action has been registered. The action identifier is not returned: use the code from the `CODE` parameter for further work. After registration, the action appears in the workflow designer.

## 2. Add the Action to a Template

In Bitrix24, add the created action to a workflow template. For details, see [How to Work in the Workflow Designer](https://helpdesk.bitrix24.com/open/23379262/).

1. Go to *Automation > Workflows > Feed Workflows*
2. Open the required workflow card or create a new one
3. Click *Settings > Configure Workflows*
4. Open the template and drag your action from the *Application Actions* block to the required place
5. In the action settings, fill in the fields you created and make sure that *Wait for response* is set to *Yes*
6. Save the template

If the action is not visible in the *Application Actions* block, check the registration using [bizproc.activity.list](../../api-reference/bizproc/bizproc-activity/bizproc-activity-list.md). The response must contain the action code.

```json
{
    "result": [
        "md5_action"
    ]
}
```

When the workflow reaches the action, Bitrix24 sends a POST request to `HANDLER`. The handler receives:

- `event_token` - token of the current wait
- `properties.inputString` - action parameter value
- `auth` - application authorization data
- `workflow_id`, `document_id`, `document_type`, and `code` - launch context

Example of incoming handler data:

```json
{
    "workflow_id": "55c1dc1c3f0d75.78875596",
    "code": "md5_action",
    "document_id": [
        "lists",
        "BizprocDocument",
        "123"
    ],
    "event_token": "55c1dc1c3f0d75.78875596|A51601_82584_96831_81132|hsyUws1j4XiwqPqN45eH66CcQtEvpUIP",
    "properties": {
        "inputString": "Test string"
    },
    "auth": {
        "domain": "example.bitrix24.com",
        "access_token": "put_access_token_here",
        "refresh_token": "put_refresh_token_here"
    }
}
```

Save `event_token` before calling `bizproc.event.send`. Without it, Bitrix24 will not know which waiting action must be completed.

## 3. Return the Result to the Workflow

In the handler, calculate the MD5 hash and call [bizproc.event.send](../../api-reference/bizproc/bizproc-robot/bizproc-event-send.md).

Pass:

- `EVENT_TOKEN` - the `event_token` value received by the handler
- `RETURN_VALUES` - an object with action results. Keys must match the keys from `RETURN_PROPERTIES`
- `LOG_MESSAGE` - a message for the workflow log

{% list tabs %}

- JS

    ```js
    app.post('/bp-md5-handler', async (req, res) => {
        const payload = req.body
        const inputString = String(payload.properties?.inputString ?? '')
        const outputString = crypto
            .createHash('md5')
            .update(inputString)
            .digest('hex')

        const $b24 = makeClient(payload.auth)
        const response = await $b24.actions.v2.call.make({
            method: 'bizproc.event.send',
            params: {
                EVENT_TOKEN: payload.event_token,
                RETURN_VALUES: {
                    outputString,
                },
                LOG_MESSAGE: `Received string: ${inputString}`,
            },
            requestId: 'bizproc-event-send',
        })

        if (!response.isSuccess) {
            res.status(500).json({ error: response.getErrorMessages() })
            return
        }

        res.json({ result: true, outputString })
    })

    app.listen(3000)
    ```

- PHP

    ```php
    <?php
    $payload = $request->request->all();
    $inputString = (string)($payload['properties']['inputString'] ?? '');
    $outputString = md5($inputString);

    $response = $b24->core->call('bizproc.event.send', [
        'EVENT_TOKEN' => $payload['event_token'],
        'RETURN_VALUES' => [
            'outputString' => $outputString,
        ],
        'LOG_MESSAGE' => 'Received string: ' . $inputString,
    ]);

    echo json_encode([
        'result' => $response->getResponseData()->getResult(),
        'outputString' => $outputString,
    ]);
    ```

- Python

    ```python
    @app.post("/bp-md5-handler")
    def bp_md5_handler():
        payload = get_payload()
        auth = payload["auth"]
        properties = payload.get("properties", {})
        input_string = str(properties.get("inputString", ""))
        output_string = hashlib.md5(input_string.encode("utf-8")).hexdigest()

        client, token = make_client(auth)
        result = token.call_method("bizproc.event.send", {
            "EVENT_TOKEN": payload["event_token"],
            "RETURN_VALUES": {
                "outputString": output_string,
            },
            "LOG_MESSAGE": f"Received string: {input_string}",
        })["result"]

        return {
            "result": result,
            "outputString": output_string,
        }
    ```

{% endlist %}

Example of a successful `bizproc.event.send` response:

```json
{
    "result": true
}
```

After a successful call, Bitrix24 closes the waiting action. The `outputString` value can be selected in the *Insert Value* form in subsequent workflow actions.

## Check the Result

Start the workflow with the `MD5 generator` action.

1. Go to *Workflows > Run Workflows*
2. Find the required workflow, open it, and click *Run*
3. Fill in the form fields if required and confirm the launch

After launch, check the handler on the application side. The URL from the `HANDLER` parameter must receive a POST request with `event_token`, `properties.inputString`, and the `auth` block.

After the handler runs, open the workflow log. The log must contain the entry from `LOG_MESSAGE`.

Add a standard action after the custom action to write the value to a variable or document field. In the *Insert Value* form, select the result of the `MD5 generator` action: `MD5`. If the handler returned `outputString`, the input string's MD5 hash will be written to the variable or field.

## Errors and Troubleshooting

If the method returns an error, check the request data.

#|
|| **Code or Indicator** | **Reason and Action** ||
|| `ACCESS_DENIED`, `Application context required` | The action is registered outside an application. Install the application and call `bizproc.activity.add` in its context ||
|| `ACCESS_DENIED`, `Access denied!` | The registration is performed by a non-administrator, or an invalid `EVENT_TOKEN` is passed to `bizproc.event.send` ||
|| `ERROR_ACTIVITY_VALIDATION_FAILURE`, `Wrong properties array!` | `PROPERTIES` or `RETURN_PROPERTIES` is filled in incorrectly. Check parameter keys, field types, and required names ||
|| `ERROR_ACTIVITY_ALREADY_INSTALLED` | An action with this `CODE` is already installed by the application. Change `CODE` or update the action using [bizproc.activity.update](../../api-reference/bizproc/bizproc-activity/bizproc-activity-update.md) ||
|| The result did not appear in the *Insert Value* form | The key in `RETURN_VALUES` does not match the key from `RETURN_PROPERTIES`. In the example, both keys must be `outputString` ||
|| The workflow did not continue after the handler call | The action does not wait for the application response, or the handler did not call `bizproc.event.send`. Check `USE_SUBSCRIPTION: 'Y'` and the successful response `{"result": true}` ||
|#

After fixing a registration error, repeat step 1. After fixing the handler, add the action to the template again if you changed `RETURN_PROPERTIES`, and restart the workflow from step 2.

## Key Points

- `EVENT_TOKEN` is valid for a specific action launch. Do not reuse the token for another workflow or another action
- In `RETURN_VALUES`, pass only keys described in `RETURN_PROPERTIES`. Bitrix24 will not save other values in the action results
- If `USE_SUBSCRIPTION` is explicitly set to `N`, the action will not wait for `bizproc.event.send`, and the workflow will continue without a handler result
- The `AUTH_USER_ID` value specifies whose token is passed to the handler. This user must have the permissions required by the handler for additional REST API calls
- For new CRM automation, it is usually more convenient to use [application Automation rules](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md). The result return mechanism through `bizproc.event.send` remains the same

## Code Example

The code runs the main steps of the scenario: registers an action with the `inputString` input parameter and the `outputString` output result, receives a request from the workflow, calculates the MD5 hash, and returns the result using `bizproc.event.send`.

The scenario has two launch points:

- action registration runs from the installed application
- the handler is hosted at the public HTTPS address from the `HANDLER` parameter

Replace `https://your-domain.example/bp-md5-handler`, `B24_CLIENT_ID`, and `B24_CLIENT_SECRET` with your application values.

{% list tabs %}

- JS

    ```js
    // install-page.js
    // npm install @bitrix24/b24jssdk
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    // Call this function on the application installation or settings page.
    export async function registerActivity() {
        const $b24 = await initializeB24Frame()

        const response = await $b24.actions.v2.call.make({
            method: 'bizproc.activity.add',
            params: {
                CODE: 'md5_action',
                HANDLER: 'https://your-domain.example/bp-md5-handler',
                AUTH_USER_ID: 1,
                USE_SUBSCRIPTION: 'Y',
                NAME: {
                    ru: 'MD5 generator',
                    en: 'MD5 generator',
                },
                DESCRIPTION: {
                    ru: 'The action returns the MD5 hash of the input parameter',
                    en: 'The action returns the MD5 hash of the input parameter',
                },
                PROPERTIES: {
                    inputString: {
                        Name: {
                            ru: 'Input string',
                            en: 'Input string',
                        },
                        Type: 'string',
                        Required: 'Y',
                        Multiple: 'N',
                        Default: '{=Document:NAME}',
                    },
                },
                RETURN_PROPERTIES: {
                    outputString: {
                        Name: {
                            ru: 'MD5',
                            en: 'MD5',
                        },
                        Type: 'string',
                        Multiple: 'N',
                        Default: null,
                    },
                },
            },
            requestId: 'bizproc-activity-add',
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    // bp-md5-handler.js
    // npm install express
    import express from 'express'
    import crypto from 'node:crypto'
    import { B24OAuth } from '@bitrix24/b24jssdk'

    const APP = {
        clientId: process.env.B24_CLIENT_ID,
        clientSecret: process.env.B24_CLIENT_SECRET,
    }

    const app = express()
    app.use(express.json())
    app.use(express.urlencoded({ extended: true }))

    function makeClient(auth) {
        const $b24 = new B24OAuth({
            domain: auth.domain,
            accessToken: auth.access_token,
            refreshToken: auth.refresh_token,
            memberId: auth.member_id,
        }, APP)
        $b24.offClientSideWarning()
        return $b24
    }

    app.post('/bp-md5-handler', async (req, res) => {
        try {
            const payload = req.body
            const inputString = String(payload.properties?.inputString ?? '')
            const outputString = crypto
                .createHash('md5')
                .update(inputString)
                .digest('hex')

            const $b24 = makeClient(payload.auth)
            const response = await $b24.actions.v2.call.make({
                method: 'bizproc.event.send',
                params: {
                    EVENT_TOKEN: payload.event_token,
                    RETURN_VALUES: {
                        outputString,
                    },
                    LOG_MESSAGE: `Received string: ${inputString}`,
                },
                requestId: 'bizproc-event-send',
            })

            if (!response.isSuccess) {
                res.status(500).json({ error: response.getErrorMessages() })
                return
            }

            res.json({ result: true, outputString })
        } catch (error) {
            res.status(500).json({ error: error.message })
        }
    })

    app.listen(3000)
    ```

- PHP

    ```php
    <?php
    // install.php and bp-md5-handler.php
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

    function buildB24(Request $request)
    {
        $appProfile = ApplicationProfile::initFromArray([
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => getenv('B24_CLIENT_ID'),
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => getenv('B24_CLIENT_SECRET'),
            'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'bizproc',
        ]);

        $authToken = AuthToken::initFromEventRequest($request);
        $domain = (string)$request->request->all('auth')['domain'];

        $log = new Logger('bizproc');
        $log->pushHandler(new StreamHandler('php://stdout'));

        return (new ServiceBuilderFactory(new EventDispatcher(), $log))
            ->init($appProfile, $authToken, $domain, DefaultOAuthServerUrl::default());
    }

    function registerActivity($b24): bool
    {
        // Call this function on the application installation or settings page.
        $response = $b24->core->call('bizproc.activity.add', [
            'CODE' => 'md5_action',
            'HANDLER' => 'https://your-domain.example/bp-md5-handler',
            'AUTH_USER_ID' => 1,
            'USE_SUBSCRIPTION' => 'Y',
            'NAME' => [
                'ru' => 'MD5 generator',
                'en' => 'MD5 generator',
            ],
            'DESCRIPTION' => [
                'ru' => 'The action returns the MD5 hash of the input parameter',
                'en' => 'The action returns the MD5 hash of the input parameter',
            ],
            'PROPERTIES' => [
                'inputString' => [
                    'Name' => [
                        'ru' => 'Input string',
                        'en' => 'Input string',
                    ],
                    'Type' => 'string',
                    'Required' => 'Y',
                    'Multiple' => 'N',
                    'Default' => '{=Document:NAME}',
                ],
            ],
            'RETURN_PROPERTIES' => [
                'outputString' => [
                    'Name' => [
                        'ru' => 'MD5',
                        'en' => 'MD5',
                    ],
                    'Type' => 'string',
                    'Multiple' => 'N',
                    'Default' => null,
                ],
            ],
        ]);

        return (bool)$response->getResponseData()->getResult();
    }

    $request = Request::createFromGlobals();
    $b24 = buildB24($request);

    $payload = $request->request->all();
    $inputString = (string)($payload['properties']['inputString'] ?? '');
    $outputString = md5($inputString);

    $response = $b24->core->call('bizproc.event.send', [
        'EVENT_TOKEN' => $payload['event_token'],
        'RETURN_VALUES' => [
            'outputString' => $outputString,
        ],
        'LOG_MESSAGE' => 'Received string: ' . $inputString,
    ]);

    echo json_encode([
        'result' => $response->getResponseData()->getResult(),
        'outputString' => $outputString,
    ]);
    ```

- Python

    ```python
    # app.py
    # pip install b24pysdk flask
    import hashlib
    import os
    from flask import Flask, request
    from b24pysdk import BitrixApp, BitrixToken, Client

    APP = BitrixApp(
        client_id=os.environ["B24_CLIENT_ID"],
        client_secret=os.environ["B24_CLIENT_SECRET"],
    )
    app = Flask(__name__)

    def make_client(auth: dict) -> tuple[Client, BitrixToken]:
        token = BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth.get("refresh_token", ""),
            bitrix_app=APP,
        )
        return Client(token), token

    def register_activity(client: Client) -> bool:
        # Call this function on the application installation or settings page.
        return client.bizproc.activity.add(
            code="md5_action",
            handler="https://your-domain.example/bp-md5-handler",
            auth_user_id=1,
            use_subscription=True,
            name={
                "ru": "MD5 generator",
                "en": "MD5 generator",
            },
            description={
                "ru": "The action returns the MD5 hash of the input parameter",
                "en": "The action returns the MD5 hash of the input parameter",
            },
            properties={
                "inputString": {
                    "Name": {
                        "ru": "Input string",
                        "en": "Input string",
                    },
                    "Type": "string",
                    "Required": "Y",
                    "Multiple": "N",
                    "Default": "{=Document:NAME}",
                },
            },
            return_properties={
                "outputString": {
                    "Name": {
                        "ru": "MD5",
                        "en": "MD5",
                    },
                    "Type": "string",
                    "Multiple": "N",
                    "Default": None,
                },
            },
        ).response.result

    def get_payload() -> dict:
        json_payload = request.get_json(silent=True)
        if json_payload:
            return json_payload

        form = request.form
        return {
            "event_token": form["event_token"],
            "auth": {
                "domain": form["auth[domain]"],
                "access_token": form["auth[access_token]"],
                "refresh_token": form.get("auth[refresh_token]", ""),
            },
            "properties": {
                "inputString": form.get("properties[inputString]", ""),
            },
        }

    @app.post("/bp-md5-handler")
    def bp_md5_handler():
        payload = get_payload()
        properties = payload.get("properties", {})
        input_string = str(properties.get("inputString", ""))
        output_string = hashlib.md5(input_string.encode("utf-8")).hexdigest()

        client, token = make_client(payload["auth"])
        result = token.call_method("bizproc.event.send", {
            "EVENT_TOKEN": payload["event_token"],
            "RETURN_VALUES": {
                "outputString": output_string,
            },
            "LOG_MESSAGE": f"Received string: {input_string}",
        })["result"]

        return {
            "result": result,
            "outputString": output_string,
        }
    ```

{% endlist %}

## Continue Learning

- [Application Actions: Overview of Methods](../../api-reference/bizproc/bizproc-activity/index.md)
- [Add a New Action bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md)
- [Get a List of Actions bizproc.activity.list](../../api-reference/bizproc/bizproc-activity/bizproc-activity-list.md)
- [Return Parameters to an Action or Automation Rule bizproc.event.send](../../api-reference/bizproc/bizproc-robot/bizproc-event-send.md)
- [Write Information to the Workflow Log bizproc.activity.log](../../api-reference/bizproc/bizproc-activity/bizproc-activity-log.md)
