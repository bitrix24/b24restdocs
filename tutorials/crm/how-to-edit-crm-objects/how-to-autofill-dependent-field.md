# How to Automatically Fill a Dependent CRM Field After the Main Field Changes

> Scope: [`crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the strictest of the listed permissions is required — permission to modify items of a CRM object
>
> - [event.bind](../../../api-reference/events/event-bind.md) — any application user
> - [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — a user with permission to read items of a CRM object
> - [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) — a user with permission to modify items of a CRM object

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A dependent field can be filled automatically after a deal is saved. For example, a manager selects a service in the Service field, and the application writes the list of documents for that service to the Documents field.

The scenario does not change the card interface during editing: it does not show or hide fields, make them required, or rebuild card sections. The application receives an event after saving, reads the deal, checks the main field value, and updates the dependent field.

The scenario consists of four steps.

1. Subscribe the application to the [onCrmDealUpdate](../../../api-reference/crm/deals/events/on-crm-deal-update.md) event using [event.bind](../../../api-reference/events/event-bind.md)
2. Retrieve deal field values using [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md)
3. Check the main field value in the handler code
4. Write a value to the dependent field using [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md)

## Before You Start

- A local application with OAuth authorization and the `crm` scope
- A public HTTPS URL for the event handler
- Completed application installation: events are not sent until installation is complete
- The saved `application_token` of the application for checking incoming events
- Two custom fields in the deal: the main field whose value the application checks and the dependent field that the application fills. In the example, the main Service field has the List type, and the dependent Documents field has the Text type. If the document list is short, you can use the String type
- Permissions of the user under whom the handler runs to read and modify deals

In the examples, replace:

- `https://your-domain.example/crm-deal-update` — event handler URL
- `B24_CLIENT_ID` — application ID
- `B24_CLIENT_SECRET` — application secret key
- `B24_APPLICATION_TOKEN` — `application_token` of the installed application
- `UF_CRM_SERVICE` — the Service field
- `UF_CRM_DOCUMENTS` — the Documents field
- `102` and `103` — IDs of the Service list values

Custom field IDs and list value IDs are different in every Bitrix24. You can find them in custom field settings or retrieve them using [crm.deal.userfield.list](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-list.md) and [crm.deal.userfield.get](../../../api-reference/crm/deals/user-defined-fields/crm-deal-userfield-get.md).

The fragments in steps 2–4 show separate operations inside the handler. The full handler code is in the [Code Example](#full-example) section.

## 1. Subscribe the Application to Deal Updates

The [event.bind](../../../api-reference/events/event-bind.md) method registers an event handler. In the `event` parameter, pass the event code `ONCRMDEALUPDATE`; in `handler`, pass the public HTTPS URL of the handler.

The method works only in the application context. An incoming webhook is not suitable for registering an event using `event.bind`.

In the examples below, `$b24` for JS, `$b24` for PHP, and `client` for Python are already initialized clients with the application OAuth token. Retrieving, storing, and refreshing OAuth tokens are described in [Full OAuth 2.0 Authorization Protocol](../../../settings/oauth/index.md).

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    const handlerUrl = 'https://your-domain.example/crm-deal-update'

    const response = await $b24.actions.v2.call.make({
        method: 'event.bind',
        params: {
            event: 'ONCRMDEALUPDATE',
            handler: handlerUrl,
        },
        requestId: 'bind-crm-deal-update',
    })

    console.log(response.getData().result)
    ```

- PHP

    ```php
    <?php
    $handlerUrl = 'https://your-domain.example/crm-deal-update';

    // event.bind has no SDK wrapper, so call the method directly
    $response = $b24->core->call('event.bind', [
        'event' => 'ONCRMDEALUPDATE',
        'handler' => $handlerUrl,
    ]);

    print_r($response->getResponseData()->getResult());
    ```

- Python

    ```python
    handler_url = "https://your-domain.example/crm-deal-update"

    response = client.event.bind(
        event="ONCRMDEALUPDATE",
        handler=handler_url,
    ).response

    print(response.result)
    ```

{% endlist %}

A successful registration returns `true`.

```json
{
    "result": true
}
```

## 2. Retrieve Deal Field Values

When the deal changes, Bitrix24 sends a POST request to the handler URL. The [onCrmDealUpdate](../../../api-reference/crm/deals/events/on-crm-deal-update.md) event passes only the deal ID in `data.FIELDS.ID`. Field values are not included in the event, so the handler must request the deal using [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md).

The request body is sent as `application/x-www-form-urlencoded`. The example below shows the structure in JSON format.

```json
{
    "event": "ONCRMDEALUPDATE",
    "data": {
        "FIELDS": {
            "ID": "759"
        }
    },
    "auth": {
        "domain": "some-domain.bitrix24.com",
        "access_token": "put_access_token_here",
        "refresh_token": "put_refresh_token_here",
        "member_id": "a223c6b3710f85df22e9377d6c4f7553",
        "application_token": "51856fefc120afa4b628cc82d3935cce"
    }
}
```

In the example, pass the following values to `crm.item.get`:

- `entityTypeId` — `2`, the Deal object type
- `id` — deal ID from the event
- `useOriginalUfNames` — `Y`, to read custom fields by names such as `UF_CRM_SERVICE`

{% list tabs %}

- JS

    ```js
    import express from 'express'
    import { B24OAuth } from '@bitrix24/b24jssdk'

    const app = express()
    app.use(express.urlencoded({ extended: true }))

    const APP = {
        clientId: process.env.B24_CLIENT_ID,
        clientSecret: process.env.B24_CLIENT_SECRET,
    }
    const APPLICATION_TOKEN = process.env.B24_APPLICATION_TOKEN

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

    app.post('/crm-deal-update', async (req, res) => {
        const auth = req.body.auth
        const dealId = Number(req.body.data?.FIELDS?.ID)
        if (!APPLICATION_TOKEN || auth?.application_token !== APPLICATION_TOKEN) {
            res.sendStatus(403)
            return
        }

        const $b24 = makeClient(auth)
        const response = await $b24.actions.v2.call.make({
            method: 'crm.item.get',
            params: {
                entityTypeId: 2,
                id: dealId,
                useOriginalUfNames: 'Y',
            },
            requestId: `deal-${dealId}-get`,
        })

        const deal = response.getData().result.item
        console.log(deal.UF_CRM_SERVICE)

        res.sendStatus(200)
    })

    app.listen(3000)
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
    $auth = $request->request->all('auth');
    $applicationToken = getenv('B24_APPLICATION_TOKEN');

    if (!$applicationToken || (($auth['application_token'] ?? '') !== $applicationToken)) {
        http_response_code(403);
        return;
    }

    $appProfile = ApplicationProfile::initFromArray([
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => getenv('B24_CLIENT_ID'),
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => getenv('B24_CLIENT_SECRET'),
        'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'crm',
    ]);

    $authToken = AuthToken::initFromEventRequest($request);
    $domain = (string)$auth['domain'];

    $log = new Logger('crm-dependent-fields');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->init($appProfile, $authToken, $domain, DefaultOAuthServerUrl::default());

    $eventData = $request->request->all('data');
    $dealId = (int)$eventData['FIELDS']['ID'];

    // Pass the useOriginalUfNames parameter through a direct SDK core call
    $response = $b24->core->call('crm.item.get', [
        'entityTypeId' => 2,
        'id' => $dealId,
        'useOriginalUfNames' => 'Y',
    ]);
    $deal = $response->getResponseData()->getResult()['item'];

    echo $deal['UF_CRM_SERVICE'];
    ```

- Python

    ```python
    # pip install flask b24pysdk
    import os
    from flask import Flask, request
    from b24pysdk import BitrixApp, BitrixToken

    app = Flask(__name__)

    APP = BitrixApp(
        client_id=os.environ["B24_CLIENT_ID"],
        client_secret=os.environ["B24_CLIENT_SECRET"],
    )
    APPLICATION_TOKEN = os.environ["B24_APPLICATION_TOKEN"]

    def make_token(auth: dict) -> BitrixToken:
        return BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth.get("refresh_token", ""),
            bitrix_app=APP,
        )

    def get_nested(prefix: str) -> dict:
        result = {}
        for key, value in request.form.items():
            if key.startswith(prefix + "[") and key.endswith("]"):
                path = key[len(prefix) + 1 : -1].split("][")
                cursor = result
                for part in path[:-1]:
                    cursor = cursor.setdefault(part, {})
                cursor[path[-1]] = value
        return result

    @app.post("/crm-deal-update")
    def handle_deal_update():
        auth = get_nested("auth")
        event_data = get_nested("data")
        deal_id = int(event_data["FIELDS"]["ID"])
        if auth.get("application_token") != APPLICATION_TOKEN:
            return "", 403

        token = make_token(auth)
        # Pass the useOriginalUfNames parameter through a direct SDK call
        deal = token.call_method("crm.item.get", {
            "entityTypeId": 2,
            "id": deal_id,
            "useOriginalUfNames": "Y",
        })["result"]["item"]

        print(deal["UF_CRM_SERVICE"])
        return "", 200
    ```

{% endlist %}

The method response contains the deal and custom fields. The example includes only the fields required for the scenario. The `UF_CRM_DOCUMENTS` field is empty because the dependent field has not been updated yet.

```json
{
    "result": {
        "item": {
            "id": 759,
            "title": "Deal #759",
            "UF_CRM_SERVICE": "102",
            "UF_CRM_DOCUMENTS": ""
        }
    }
}
```

Save the `UF_CRM_SERVICE` value: it is needed to check the condition.

## 3. Check the Main Field Value

The dependency rules are defined by the application code. In the example, `102` and `103` are IDs of list options in the Service field. The application maps each option to its own list of documents.

Below is a fragment of handler logic. Full JS, PHP, and Python implementations are available in the [Code Example](#full-example) section.

```js
const documentsByService = {
    102: 'Passport, application, contract',
    103: 'Tax ID, power of attorney, statement',
}

const serviceId = deal.UF_CRM_SERVICE
const documents = documentsByService[serviceId]

if (!documents) {
    return
}
```

If the Service field is empty or the selected value is not in `documentsByService`, the handler finishes without updating the deal. Add all list values for which the Documents field must be filled to the `documentsByService` object.

If the dependent field already contains the required value, finish the handler without calling `crm.item.update`. This protects the application from unnecessary deal updates and repeated events.

```js
if (deal.UF_CRM_DOCUMENTS === documents) {
    return
}
```

## 4. Update the Dependent Field

The [crm.item.update](../../../api-reference/crm/universal/crm-item-update.md) method updates only the fields passed in the `fields` object. Pass the deal ID from the event and the new dependent field value.

In the example, pass the following values:

- `entityTypeId` — `2`, the Deal object type
- `id` — deal ID from the event
- `fields.UF_CRM_DOCUMENTS` — the new dependent field value
- `useOriginalUfNames` — `Y`, to use a field name such as `UF_CRM_DOCUMENTS`

{% list tabs %}

- JS

    ```js
    await $b24.actions.v2.call.make({
        method: 'crm.item.update',
        params: {
            entityTypeId: 2,
            id: dealId,
            fields: {
                UF_CRM_DOCUMENTS: documents,
            },
            useOriginalUfNames: 'Y',
        },
        requestId: `deal-${dealId}-update-documents`,
    })
    ```

- PHP

    ```php
    <?php
    // Pass the useOriginalUfNames parameter through a direct SDK core call
    $b24->core->call('crm.item.update', [
        'entityTypeId' => 2,
        'id' => $dealId,
        'fields' => [
            'UF_CRM_DOCUMENTS' => $documents,
        ],
        'useOriginalUfNames' => 'Y',
    ]);
    ```

- Python

    ```python
    # Pass the useOriginalUfNames parameter through a direct SDK call
    token.call_method("crm.item.update", {
        "entityTypeId": 2,
        "id": deal_id,
        "fields": {
            "UF_CRM_DOCUMENTS": documents,
        },
        "useOriginalUfNames": "Y",
    })
    ```

{% endlist %}

A successful response contains the updated deal. The example includes only the fields that confirm the dependent field change.

```json
{
    "result": {
        "item": {
            "id": 759,
            "title": "Deal #759",
            "UF_CRM_SERVICE": "102",
            "UF_CRM_DOCUMENTS": "Passport, application, contract"
        }
    }
}
```

## Check the Result

Open the deal card in CRM. If the Service field contains a value mapped to a document list by the application, the Documents field will be filled after the deal is saved and the event is processed.

You can check the result through REST using [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md).

```json
{
    "entityTypeId": 2,
    "id": 759,
    "useOriginalUfNames": "Y"
}
```

The scenario is complete if the response contains the `UF_CRM_DOCUMENTS` field with a value matching `UF_CRM_SERVICE`.

## Errors and Diagnostics

If a method returns an error, check the request data.

#|
|| **Code** | **Reason and Action** ||
|| `ERROR_EVENT_NOT_FOUND` | An incorrect event code was passed to `event.bind`. For a deal, specify `ONCRMDEALUPDATE` ||
|| `ACCESS_DENIED` | The user does not have permission to read or modify the deal. Check the permissions of the user under whom the handler runs ||
|| `NOT_FOUND` | The deal was not found or is not available to the user. Check `data.FIELDS.ID` from the event ||
|| `CRM_FIELD_ERROR_VALUE_NOT_VALID` | A value of an unsuitable type was passed to the dependent field. Check the custom field type and value format ||
|#

If the event does not arrive, check that the application is installed, installation is complete, the handler is available over HTTPS, and the event is registered using `event.bind`.

If the event arrives but the field does not change, check:

- the Service field code and the Documents field code
- IDs of the Service list values
- whether the selected value exists in the `documentsByService` object
- the `useOriginalUfNames` parameter value
- the condition that skips the update if `UF_CRM_DOCUMENTS` is already equal to the required value
- the `B24_APPLICATION_TOKEN` value if the handler returns `403`

## Key Considerations

- The scenario runs after the deal is saved. It does not change the card interface when a value is selected
- The `ONCRMDEALUPDATE` event reports only the deal ID, not the list of changed fields. That is why the handler always reads the deal using `crm.item.get`
- Updating the dependent field also triggers a deal update event. Before calling `crm.item.update`, compare the current and new values
- If it is important not to lose an update while the handler is temporarily unavailable, use [offline events](../../../api-reference/events/offline-events.md). The application will be able to retrieve accumulated events from the queue
- For smart processes, use the [onCrmDynamicItemUpdate](../../../api-reference/crm/universal/events/on-crm-dynamic-item-update.md) event. The event will include the item `ID` and `ENTITY_TYPE_ID`; pass them to `crm.item.get` and `crm.item.update`
- For leads, contacts, and companies, use the corresponding object events: [onCrmLeadUpdate](../../../api-reference/crm/leads/events/on-crm-lead-update.md), [onCrmContactUpdate](../../../api-reference/crm/contacts/events/on-crm-contact-update.md), [onCrmCompanyUpdate](../../../api-reference/crm/companies/events/on-crm-company-update.md)
- Check `application_token` to make sure the request came from Bitrix24. See the detailed explanation in [Secure Event Handling](../../../api-reference/events/safe-event-handlers.md)
- OAuth tokens are not sent to the handler if the update was made by an automation rule, workflow, or agent. For reliable background processing, store the tokens of the user who installed the application

## Code Example {#full-example}

The code goes through all four steps: receives the deal update event, reads field values, selects a document list by service, and updates the dependent field.

Replace the application parameters, `application_token`, handler URL, custom field codes, and list value IDs.

{% list tabs %}

- JS

    ```js
    // npm install express @bitrix24/b24jssdk
    import express from 'express'
    import { B24OAuth } from '@bitrix24/b24jssdk'

    const app = express()
    app.use(express.urlencoded({ extended: true }))

    const APP = {
        clientId: process.env.B24_CLIENT_ID,
        clientSecret: process.env.B24_CLIENT_SECRET,
    }
    const APPLICATION_TOKEN = process.env.B24_APPLICATION_TOKEN

    const ENTITY_TYPE_ID = 2
    const SERVICE_FIELD = 'UF_CRM_SERVICE'
    const DOCUMENTS_FIELD = 'UF_CRM_DOCUMENTS'
    const DOCUMENTS_BY_SERVICE = {
        102: 'Passport, application, contract',
        103: 'Tax ID, power of attorney, statement',
    }

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

    async function call($b24, method, params, requestId) {
        const response = await $b24.actions.v2.call.make({ method, params, requestId })
        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }
        return response.getData().result
    }

    app.post('/crm-deal-update', async (req, res) => {
        try {
            const auth = req.body.auth
            const dealId = Number(req.body.data?.FIELDS?.ID)
            if (!auth || !dealId) {
                res.sendStatus(400)
                return
            }
            if (!APPLICATION_TOKEN || auth.application_token !== APPLICATION_TOKEN) {
                res.sendStatus(403)
                return
            }

            const $b24 = makeClient(auth)
            const { item: deal } = await call($b24, 'crm.item.get', {
                entityTypeId: ENTITY_TYPE_ID,
                id: dealId,
                useOriginalUfNames: 'Y',
            }, `deal-${dealId}-get`)

            const serviceId = deal[SERVICE_FIELD]
            const documents = DOCUMENTS_BY_SERVICE[serviceId]
            if (!documents || deal[DOCUMENTS_FIELD] === documents) {
                res.sendStatus(200)
                return
            }

            await call($b24, 'crm.item.update', {
                entityTypeId: ENTITY_TYPE_ID,
                id: dealId,
                fields: {
                    [DOCUMENTS_FIELD]: documents,
                },
                useOriginalUfNames: 'Y',
            }, `deal-${dealId}-update-documents`)

            res.sendStatus(200)
        } catch (error) {
            console.error(error)
            res.sendStatus(500)
        }
    })

    app.listen(3000)
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
    $auth = $request->request->all('auth');
    $applicationToken = getenv('B24_APPLICATION_TOKEN');

    if (!$applicationToken || (($auth['application_token'] ?? '') !== $applicationToken)) {
        http_response_code(403);
        return;
    }

    $appProfile = ApplicationProfile::initFromArray([
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => getenv('B24_CLIENT_ID'),
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => getenv('B24_CLIENT_SECRET'),
        'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'crm',
    ]);

    $authToken = AuthToken::initFromEventRequest($request);
    $domain = (string)$auth['domain'];

    $log = new Logger('crm-dependent-fields');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->init($appProfile, $authToken, $domain, DefaultOAuthServerUrl::default());

    $entityTypeId = 2;
    $serviceField = 'UF_CRM_SERVICE';
    $documentsField = 'UF_CRM_DOCUMENTS';
    $documentsByService = [
        '102' => 'Passport, application, contract',
        '103' => 'Tax ID, power of attorney, statement',
    ];

    try {
        $eventData = $request->request->all('data');
        $dealId = (int)$eventData['FIELDS']['ID'];

        // Pass the useOriginalUfNames parameter through a direct SDK core call
        $resultDeal = $b24->core->call('crm.item.get', [
            'entityTypeId' => $entityTypeId,
            'id' => $dealId,
            'useOriginalUfNames' => 'Y',
        ]);
        $deal = $resultDeal->getResponseData()->getResult()['item'];

        $serviceId = (string)$deal[$serviceField];
        $documents = $documentsByService[$serviceId] ?? null;

        if ($documents === null || $deal[$documentsField] === $documents) {
            http_response_code(200);
            return;
        }

        // Pass the useOriginalUfNames parameter through a direct SDK core call
        $b24->core->call('crm.item.update', [
            'entityTypeId' => $entityTypeId,
            'id' => $dealId,
            'fields' => [
                $documentsField => $documents,
            ],
            'useOriginalUfNames' => 'Y',
        ]);

        http_response_code(200);
    } catch (\Throwable $e) {
        error_log($e->getMessage());
        http_response_code(500);
    }
    ```

- Python

    ```python
    # pip install flask b24pysdk
    import os
    from flask import Flask, request
    from b24pysdk import BitrixApp, BitrixToken

    app = Flask(__name__)

    APP = BitrixApp(
        client_id=os.environ["B24_CLIENT_ID"],
        client_secret=os.environ["B24_CLIENT_SECRET"],
    )
    APPLICATION_TOKEN = os.environ["B24_APPLICATION_TOKEN"]

    ENTITY_TYPE_ID = 2
    SERVICE_FIELD = "UF_CRM_SERVICE"
    DOCUMENTS_FIELD = "UF_CRM_DOCUMENTS"
    DOCUMENTS_BY_SERVICE = {
        "102": "Passport, application, contract",
        "103": "Tax ID, power of attorney, statement",
    }

    def make_token(auth: dict) -> BitrixToken:
        return BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth.get("refresh_token", ""),
            bitrix_app=APP,
        )

    def get_nested(prefix: str) -> dict:
        result = {}
        for key, value in request.form.items():
            if key.startswith(prefix + "[") and key.endswith("]"):
                path = key[len(prefix) + 1 : -1].split("][")
                cursor = result
                for part in path[:-1]:
                    cursor = cursor.setdefault(part, {})
                cursor[path[-1]] = value
        return result

    @app.post("/crm-deal-update")
    def handle_deal_update():
        try:
            auth = get_nested("auth")
            event_data = get_nested("data")
            deal_id = int(event_data["FIELDS"]["ID"])
            if auth.get("application_token") != APPLICATION_TOKEN:
                return "", 403

            token = make_token(auth)
            # Pass the useOriginalUfNames parameter through a direct SDK call
            deal = token.call_method("crm.item.get", {
                "entityTypeId": ENTITY_TYPE_ID,
                "id": deal_id,
                "useOriginalUfNames": "Y",
            })["result"]["item"]

            service_id = str(deal[SERVICE_FIELD])
            documents = DOCUMENTS_BY_SERVICE.get(service_id)

            if documents is None or deal[DOCUMENTS_FIELD] == documents:
                return "", 200

            # Pass the useOriginalUfNames parameter through a direct SDK call
            token.call_method("crm.item.update", {
                "entityTypeId": ENTITY_TYPE_ID,
                "id": deal_id,
                "fields": {
                    DOCUMENTS_FIELD: documents,
                },
                "useOriginalUfNames": "Y",
            })

            return "", 200
        except Exception as error:
            print(error)
            return "", 500
    ```

{% endlist %}

## Continue Learning

- [{#T}](../../../api-reference/events/index.md)
- [{#T}](../../../api-reference/events/event-bind.md)
- [{#T}](../../../api-reference/events/safe-event-handlers.md)
- [{#T}](../../../api-reference/crm/deals/events/on-crm-deal-update.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-get.md)
- [{#T}](../../../api-reference/crm/universal/crm-item-update.md)
- [{#T}](../../../api-reference/crm/deals/user-defined-fields/index.md)
