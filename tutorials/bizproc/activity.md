# How to Add an Action to Create a Smart Invoice Based on a Lead or Deal

> Scope: [`bizproc`, `crm`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the whole scenario, you need a Bitrix24 administrator with permissions to read the lead or deal, create a smart invoice, and change its product rows
>
> - [bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md) and [bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md) - administrator
> - [crm.lead.get / crm.deal.get](../../api-reference/crm/rest-v3/items/crm-entity-get-rest-v3.md) - user with permission to read CRM object items
> - [crm.smartinvoice.add](../../api-reference/crm/rest-v3/items/crm-entity-add-rest-v3.md) - user with permission to add CRM object items
> - [crm.lead.productrow.list / crm.deal.productrow.list](../../api-reference/crm/rest-v3/product-rows/crm-entity-product-row-list-rest-v3.md) - user with permission to read the CRM object whose product rows are selected
> - [crm.smartinvoice.productrow.replace](../../api-reference/crm/rest-v3/product-rows/crm-entity-product-row-replace-rest-v3.md) - user with permission to change the CRM object whose product rows are set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The scenario shows how an application adds a workflow action that receives a lead or deal from the execution context and creates a smart invoice from the CRM object data. The customer and product rows are copied from the lead or deal to the invoice. For a deal, the invoice is also linked to the source deal through the `relatedDealId` field.

The action can be used in the workflow designer. For CRM automation, use an application Automation rule: the parameter set and handler remain the same, only the registration method changes.

The scenario consists of four steps.

1. Register the action using [bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md)
2. Get `document_id` in the handler and determine what started the action: a lead or a deal
3. Get CRM object data using [crm.lead.get / crm.deal.get](../../api-reference/crm/rest-v3/items/crm-entity-get-rest-v3.md) and product rows using [crm.lead.productrow.list / crm.deal.productrow.list](../../api-reference/crm/rest-v3/product-rows/crm-entity-product-row-list-rest-v3.md)
4. Create a smart invoice using [crm.smartinvoice.add](../../api-reference/crm/rest-v3/items/crm-entity-add-rest-v3.md) and copy products using [crm.smartinvoice.productrow.replace](../../api-reference/crm/rest-v3/product-rows/crm-entity-product-row-replace-rest-v3.md)

Deployment order: combine the functions from steps 2–4 and deploy the handler at the `HANDLER` URL, then register the action from step 1, and only then start the workflow.

## Prepare the Application

`bizproc.activity.add` and `bizproc.robot.add` work only within an [application](../../settings/app-installation/index.md) context. An inbound webhook will not work: the method will return the `ACCESS_DENIED` error with the description `Access denied! Application context required`.

Before you start, prepare:

- an application with the `bizproc` and `crm` scopes
- a public HTTPS handler address, for example `https://your-domain.example/bp-handler`
- the administrator ID for the `AUTH_USER_ID` parameter
- an [ONAPPINSTALL](../../api-reference/common/events/on-app-install.md) event handler prepared before installing the application: in the PHP and Python examples, it registers the action from step 1. The [ONAPPINSTALL](../../api-reference/common/events/on-app-install.md) article describes how to connect the installation handler. If the application is already installed without this handler, reinstall it
- installation request validation. The installation handler URL is public, so before saving tokens and registering the action, validate the request using the instructions for a [local](../../settings/app-installation/local-apps/installation-callback.md) or [mass-market](../../settings/app-installation/mass-market-apps/installation-callback.md) application
- application storage for `application_token`: after successfully validating the installation request, save `application_token` together with `domain` and implement a function that returns the value by `domain`. The action handler calls it before calling CRM methods
- the `B24_CLIENT_ID` and `B24_CLIENT_SECRET` environment variables, SDK dependencies, and Express for Node.js or Flask for Python
- your company ID for the smart invoice field `myCompanyId`. You can get it using [crm.mycompany.list](../../api-reference/crm/rest-v3/items/crm-entity-list-rest-v3.md) with `select: ["id", "title"]`

The object type is specified by the REST 3.0 method name:

#|
|| Object | Methods ||
|| Lead | `crm.lead.get`, `crm.lead.productrow.list` ||
|| Deal | `crm.deal.get`, `crm.deal.productrow.list` ||
|| Smart invoice | `crm.smartinvoice.add`, `crm.smartinvoice.productrow.replace` ||
|#

{% include [Note on examples](../../_includes/examples.md) %}

## 1. Register the Action

Register the action once from the installed application, for example on the application settings page. The JS example runs in the application iframe through `initializeB24Frame()`. In PHP and Python, create the `$b24` and `client` variables using the installed application tokens, for example from the [ONAPPINSTALL](../../api-reference/common/events/on-app-install.md) event: its `auth` fields arrive in the same format as in the handler request in step 2. In the examples, replace the `HANDLER` URL, `AUTH_USER_ID`, and the default `mycompany_id` value with your application and Bitrix24 data.

Pass a code that is unique within the application in `CODE`. In `HANDLER`, specify the public URL where Bitrix24 will send data when the action runs. In `PROPERTIES`, describe the parameters that the administrator will fill in the workflow designer.

In the example, the action receives two parameters:

- `invoice_title` - smart invoice title
- `mycompany_id` - your company ID

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    const response = await $b24.actions.v2.call.make({
        method: 'bizproc.activity.add',
        params: {
            CODE: 'create_smart_invoice',
            HANDLER: 'https://your-domain.example/bp-handler',
            AUTH_USER_ID: 1,
            NAME: 'Create smart invoice',
            DESCRIPTION: 'Creates a smart invoice from lead or deal data',
            PROPERTIES: {
                invoice_title: {
                    Name: 'Invoice title',
                    Type: 'string',
                    Required: 'Y',
                    Default: 'Invoice for CRM document',
                },
                mycompany_id: {
                    Name: 'Your company ID',
                    Type: 'int',
                    Required: 'Y',
                    Default: '1',
                },
            },
            FILTER: {
                INCLUDE: [
                    ['crm', 'CCrmDocumentDeal'],
                    ['crm', 'CCrmDocumentLead'],
                ],
            },
        },
        requestId: 'bizproc-activity-add',
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    console.info(response.getData().result) // true
    ```

- Python

    ```python
    # pip install b24pysdk flask
    # Runs in the ONAPPINSTALL handler after validating the installation request
    # and saving application_token. make_client and form_group are from step 2
    from flask import request

    auth = form_group(request.form, "auth")
    client, token = make_client(auth)

    result = client.bizproc.activity.add(
        code="create_smart_invoice",
        handler="https://your-domain.example/bp-handler",
        auth_user_id=1,
        name="Create smart invoice",
        description="Creates a smart invoice from lead or deal data",
        properties={
            "invoice_title": {
                "Name": "Invoice title",
                "Type": "string",
                "Required": "Y",
                "Default": "Invoice for CRM document",
            },
            "mycompany_id": {
                "Name": "Your company ID",
                "Type": "int",
                "Required": "Y",
                "Default": "1",
            },
        },
        filter={
            "INCLUDE": [
                ["crm", "CCrmDocumentDeal"],
                ["crm", "CCrmDocumentLead"],
            ],
        },
    ).response.result

    print(result)  # True
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    // Runs in the ONAPPINSTALL handler after validating the installation request and saving application_token.
    // Action registration uses REST 2, so an SDK client is required
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Credentials\ApplicationProfile;
    use Bitrix24\SDK\Core\Credentials\AuthToken;
    use Bitrix24\SDK\Core\Credentials\DefaultOAuthServerUrl;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Psr\Log\NullLogger;
    use Symfony\Component\EventDispatcher\EventDispatcher;
    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();
    $appProfile = ApplicationProfile::initFromArray([
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => getenv('B24_CLIENT_ID'),
        'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => getenv('B24_CLIENT_SECRET'),
        'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'bizproc,crm',
    ]);
    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), new NullLogger()))->init(
        $appProfile,
        AuthToken::initFromEventRequest($request),
        (string)$request->request->all('auth')['domain'],
        DefaultOAuthServerUrl::default()
    );

    // The typed activity()->add method accepts extended DTOs,
    // so for a short example we call the method directly through the SDK core.
    $response = $b24->core->call('bizproc.activity.add', [
        'CODE' => 'create_smart_invoice',
        'HANDLER' => 'https://your-domain.example/bp-handler',
        'AUTH_USER_ID' => 1,
        'NAME' => 'Create smart invoice',
        'DESCRIPTION' => 'Creates a smart invoice from lead or deal data',
        'PROPERTIES' => [
            'invoice_title' => [
                'Name' => 'Invoice title',
                'Type' => 'string',
                'Required' => 'Y',
                'Default' => 'Invoice for CRM document',
            ],
            'mycompany_id' => [
                'Name' => 'Your company ID',
                'Type' => 'int',
                'Required' => 'Y',
                'Default' => '1',
            ],
        ],
        'FILTER' => [
            'INCLUDE' => [
                ['crm', 'CCrmDocumentDeal'],
                ['crm', 'CCrmDocumentLead'],
            ],
        ],
    ]);

    print_r($response->getResponseData()->getResult()[0]); // true: the SDK returns a scalar result as an array
    ```
{% endlist %}

If you need to add an Automation rule for CRM automation, replace `bizproc.activity.add` with [bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md). The `CODE`, `HANDLER`, `AUTH_USER_ID`, `NAME`, `DESCRIPTION`, `PROPERTIES`, and `FILTER` parameters are used the same way.

Example of a successful response:

```json
{
    "result": true
}
```

After registration, the action will appear in the workflow designer for leads and deals. When the workflow reaches the action, Bitrix24 will send a request to `HANDLER`.

## 2. Parse Handler Data

Bitrix24 sends a POST request to `HANDLER` in `application/x-www-form-urlencoded` format. The request contains action parameters in `properties`, document identifiers in `document_id`, and authorization in `auth`. For a lead, `document_id` contains a string such as `LEAD_456`; for a deal, `DEAL_123`.

The code in steps 2–4 is organized into functions. In [Assemble the Handler](#handler), they are called in order within a single request handler.

### Create a Client

Use the `auth` fields to create a client for CRM method calls in steps 3 and 4 through [REST 3.0](../../api-reference/rest-v3.md#call) at `/rest/api/`. Action registration continues to use REST 2. In JS, CRM is called through `actions.v3.call.make`. In PHP, the example sends an HTTP request to `/rest/api/` directly, with the OAuth token in the `auth` field of the JSON body. Store the application `client_id` and `client_secret` in server environment variables.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24OAuth } from '@bitrix24/b24jssdk'

    const APP = { clientId: process.env.B24_CLIENT_ID, clientSecret: process.env.B24_CLIENT_SECRET }

    function makeClient(auth) {
        const $b24 = new B24OAuth({
            applicationToken: auth.application_token,
            userId: Number(auth.user_id),
            memberId: auth.member_id,
            accessToken: auth.access_token,
            refreshToken: auth.refresh_token,
            expires: Number(auth.expires),
            expiresIn: Number(auth.expires_in),
            scope: auth.scope,
            domain: auth.domain,
            clientEndpoint: auth.client_endpoint,
            serverEndpoint: auth.server_endpoint,
            status: auth.status,
        }, APP)
        $b24.offClientSideWarning()
        return $b24
    }
    ```

- Python

    ```python
    # pip install b24pysdk
    import os
    from b24pysdk import BitrixApp, BitrixToken, Client, Config

    V3_METHODS = {
        "crm.lead.get", "crm.deal.get", "crm.smartinvoice.add",
        "crm.lead.productrow.list", "crm.deal.productrow.list",
        "crm.smartinvoice.productrow.replace",
    }

    APP = BitrixApp(
        client_id=os.environ["B24_CLIENT_ID"],
        client_secret=os.environ["B24_CLIENT_SECRET"],
    )

    def make_client(auth: dict) -> tuple[Client, BitrixToken]:
        # b24pysdk settings are stored separately for each thread,
        # so enable REST 3.0 methods in the request thread
        Config().configure(api_v3_methods=Config().api_v3_methods | V3_METHODS)
        token = BitrixToken(
            domain=auth["domain"],
            auth_token=auth["access_token"],
            refresh_token=auth.get("refresh_token", ""),
            bitrix_app=APP,
        )
        return Client(token), token

    def form_group(form, name: str) -> dict:
        """Collect form fields in name[key] format into a dictionary"""
        prefix = f"{name}["
        return {k[len(prefix):-1]: v for k, v in form.items() if k.startswith(prefix)}
    ```

- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Symfony\Component\HttpFoundation\Request;

    function makeClient(Request $request): Closure
    {
        $auth = $request->request->all('auth');
        return static function (string $method, array $params) use ($auth): array {
            $curl = curl_init('https://' . $auth['domain'] . '/rest/api/' . $method);
            curl_setopt_array($curl, [
                CURLOPT_POST => true,
                CURLOPT_RETURNTRANSFER => true,
                CURLOPT_TIMEOUT => 30,
                CURLOPT_HTTPHEADER => ['Content-Type: application/json'],
                CURLOPT_POSTFIELDS => json_encode($params + ['auth' => $auth['access_token']], JSON_THROW_ON_ERROR),
            ]);
            $body = curl_exec($curl);
            $error = curl_error($curl);
            curl_close($curl);
            if ($body === false) {
                throw new RuntimeException($error);
            }
            $data = json_decode($body, true, 512, JSON_THROW_ON_ERROR);
            if (isset($data['error'])) {
                throw new RuntimeException($data['error']['message']);
            }
            return $data['result'];
        };
    }
    ```

{% endlist %}

### Determine the Source Object

From `document_id`, get:

- the type segment for the method name: `lead` for a lead or `deal` for a deal
- the numeric ID of the source CRM object. If it is not a positive integer, the function stops processing

{% list tabs %}

- JS

    ```js
    function parseDocumentId(documentId) {
        const values = Array.isArray(documentId) ? documentId : [documentId]
        const types = { DEAL_: 'deal', LEAD_: 'lead' }

        for (const value of values.map(String)) {
            for (const [prefix, type] of Object.entries(types)) {
                const id = Number(value.slice(prefix.length))
                if (value.startsWith(prefix) && Number.isInteger(id) && id > 0) {
                    return { type, id }
                }
            }
        }

        throw new Error('The action was started neither from a lead nor from a deal')
    }
    ```

- Python

    ```python
    def parse_document_id(document_id: list[str]) -> dict:
        types = {"DEAL_": "deal", "LEAD_": "lead"}

        for value in document_id:
            for prefix, entity_type in types.items():
                suffix = value[len(prefix):]
                if value.startswith(prefix) and suffix.isdigit() and int(suffix) > 0:
                    return {"type": entity_type, "id": int(suffix)}

        raise ValueError("The action was started neither from a lead nor from a deal")
    ```

- PHP

    ```php
    <?php
    function parseDocumentId(array $documentId): array
    {
        $types = ['DEAL_' => 'deal', 'LEAD_' => 'lead'];

        foreach ($documentId as $value) {
            foreach ($types as $prefix => $type) {
                $suffix = substr((string)$value, strlen($prefix));
                if (str_starts_with((string)$value, $prefix) && ctype_digit($suffix) && (int)$suffix > 0) {
                    return ['type' => $type, 'id' => (int)$suffix];
                }
            }
        }

        throw new RuntimeException('The action was started neither from a lead nor from a deal');
    }
    ```

{% endlist %}

## 3. Get the CRM Object and Products

Call [crm.lead.get / crm.deal.get](../../api-reference/crm/rest-v3/items/crm-entity-get-rest-v3.md) and pass the `id` from `document_id`. In `select`, specify `companyId`, `contactsId`, and `currencyId`: without `select`, the method returns only `id`.

Get products using [crm.lead.productrow.list / crm.deal.productrow.list](../../api-reference/crm/rest-v3/product-rows/crm-entity-product-row-list-rest-v3.md). The filter is an array of conditions `[["ownerId", "=", id]]`; the type is specified by the method name. Explicitly request the product row fields. Fetch pages through `pagination.page`, starting at `1`, until fewer than `pagination.limit` rows are returned. The response contains neither a total count nor `next`.

{% list tabs %}

- JS

    ```js
    async function callMethod($b24, method, params) {
        const response = await $b24.actions.v3.call.make({ method, params, requestId: method })
        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }
        return response.getData().result
    }

    async function loadSource($b24, source) {
        const { item } = await callMethod($b24, `crm.${source.type}.get`, {
            id: source.id,
            select: ['companyId', 'contactsId', 'currencyId'],
        })
        const sourceRows = []
        for (let page = 1; ; page++) {
            const { items } = await callMethod($b24, `crm.${source.type}.productrow.list`, {
                filter: [['ownerId', '=', source.id]],
                select: ['productId', 'productName', 'price', 'quantity', 'discountTypeId',
                    'discountRate', 'discountSum', 'taxRate', 'taxIncluded', 'measureCode', 'sort'],
                order: { sort: 'asc', id: 'asc' },
                pagination: { limit: 50, page },
            })
            sourceRows.push(...items)
            if (items.length < 50) break
        }
        return { sourceItem: item, sourceRows }
    }
    ```

- Python

    ```python
    def load_source(token, source: dict) -> tuple[dict, list[dict]]:
        source_item = token.call_method(f"crm.{source['type']}.get", {
            "id": source["id"],
            "select": ["companyId", "contactsId", "currencyId"],
        }, prefer_version=3)["result"]["item"]
        source_rows = []
        page = 1
        while True:
            items = token.call_method(f"crm.{source['type']}.productrow.list", {
                "filter": [["ownerId", "=", source["id"]]],
                "select": ["productId", "productName", "price", "quantity", "discountTypeId",
                           "discountRate", "discountSum", "taxRate", "taxIncluded", "measureCode", "sort"],
                "order": {"sort": "asc", "id": "asc"},
                "pagination": {"limit": 50, "page": page},
            }, prefer_version=3)["result"]["items"]
            source_rows.extend(items)
            if len(items) < 50:
                break
            page += 1
        return source_item, source_rows
    ```

- PHP

    ```php
    <?php
    function loadSource($b24, array $source): array
    {
        $sourceItem = $b24('crm.' . $source['type'] . '.get', [
            'id' => $source['id'],
            'select' => ['companyId', 'contactsId', 'currencyId'],
        ])['item'];
        $sourceRows = [];
        $page = 1;
        do {
            $items = $b24('crm.' . $source['type'] . '.productrow.list', [
                'filter' => [['ownerId', '=', $source['id']]],
                'select' => ['productId', 'productName', 'price', 'quantity', 'discountTypeId',
                    'discountRate', 'discountSum', 'taxRate', 'taxIncluded', 'measureCode', 'sort'],
                'order' => ['sort' => 'asc', 'id' => 'asc'],
                'pagination' => ['limit' => 50, 'page' => $page++],
            ])['items'];
            $sourceRows = array_merge($sourceRows, $items);
        } while (count($items) === 50);
        return [$sourceItem, $sourceRows];
    }
    ```

{% endlist %}

In the shortened `get` response, the customer and currency fields are in `result.item`. Save them to create the invoice. Prepare the products from `result.items` for copying. The following are shortened live responses with identifiers replaced.

```json
{"result":{"item":{"companyId":null,"contactsId":[],"currencyId":"RUB"}}}
```

Shortened product page response with `select: ["productName", "price", "quantity"]`:

```json
{"result":{"items":[{"productName":"Product A","price":100,"quantity":2},{"productName":"Product B","price":250,"quantity":1}]}}
```

## 4. Create a Smart Invoice and Copy Products

Create a smart invoice using [crm.smartinvoice.add](../../api-reference/crm/rest-v3/items/crm-entity-add-rest-v3.md). The type is already specified by the method name.

In `fields`, pass:

- `title` - smart invoice title from the action parameter `invoice_title`
- `companyId` - company ID from the source CRM object
- `contactsId` - array of contact IDs from the source CRM object; `contactId` is read-only
- `currencyId` - currency of the source CRM object, so product prices remain in the same currency
- `myCompanyId` - your company ID from the action parameter `mycompany_id`. If it is not a positive integer, the function stops processing before creating the smart invoice
- `relatedDealId` - deal ID if the action was started from a deal

Then pass product rows to the created smart invoice using [crm.smartinvoice.productrow.replace](../../api-reference/crm/rest-v3/product-rows/crm-entity-product-row-replace-rest-v3.md). In `ownerId`, pass `result.id` from the creation response; in `items`, pass the complete set of rows. The method replaces the entire set: an empty `items` array deletes all products. A single call can pass no more than 1000 rows.

{% list tabs %}

- JS

    ```js
    function prepareProductRows(rows) {
        return rows.map((row, index) => ({
            productId: row.productId,
            productName: row.productName,
            price: row.price,
            quantity: row.quantity,
            discountTypeId: row.discountTypeId,
            discountRate: row.discountRate,
            discountSum: row.discountSum,
            taxRate: row.taxRate,
            taxIncluded: row.taxIncluded,
            measureCode: row.measureCode,
            sort: row.sort || (index + 1) * 10,
        }))
    }

    async function createInvoice($b24, source, properties, sourceItem, sourceRows) {
        const myCompanyId = Number(properties.mycompany_id)
        if (!Number.isInteger(myCompanyId) || myCompanyId <= 0) {
            throw new Error('The mycompany_id parameter must be a positive integer')
        }

        if (sourceRows.length > 1000) throw new Error('An invoice can contain no more than 1000 rows')

        const fields = {
            title: properties.invoice_title || 'Invoice for CRM document',
            companyId: sourceItem.companyId || null,
            contactsId: sourceItem.contactsId || [],
            myCompanyId,
        }

        if (sourceItem.currencyId) {
            fields.currencyId = sourceItem.currencyId
        }

        if (source.type === 'deal') {
            fields.relatedDealId = source.id
        }

        const invoiceResult = await callMethod($b24, 'crm.smartinvoice.add', {
            fields,
        })
        const invoiceId = invoiceResult.id
        console.info(`Created smart invoice ${invoiceId}`)

        if (sourceRows.length > 0) {
            await callMethod($b24, 'crm.smartinvoice.productrow.replace', {
                ownerId: invoiceId,
                items: prepareProductRows(sourceRows),
                select: ['id'],
            })
        }

        return invoiceId
    }
    ```

- Python

    ```python
    def prepare_product_rows(rows: list[dict]) -> list[dict]:
        return [
            {
                "productId": row.get("productId"),
                "productName": row.get("productName"),
                "price": row.get("price"),
                "quantity": row.get("quantity", 1),
                "discountTypeId": row.get("discountTypeId"),
                "discountRate": row.get("discountRate"),
                "discountSum": row.get("discountSum"),
                "taxRate": row.get("taxRate"),
                "taxIncluded": row.get("taxIncluded"),
                "measureCode": row.get("measureCode"),
                "sort": row.get("sort", (index + 1) * 10),
            }
            for index, row in enumerate(rows)
        ]

    def create_invoice(token, source: dict, properties: dict, source_item: dict, source_rows: list[dict]) -> int:
        mycompany_id = str(properties.get("mycompany_id", ""))
        if not mycompany_id.isdigit() or int(mycompany_id) <= 0:
            raise ValueError("The mycompany_id parameter must be a positive integer")

        if len(source_rows) > 1000:
            raise ValueError("An invoice can contain no more than 1000 rows")

        fields = {
            "title": properties.get("invoice_title", "Invoice for CRM document"),
            "companyId": source_item.get("companyId"),
            "contactsId": source_item.get("contactsId") or [],
            "myCompanyId": int(mycompany_id),
        }

        if source_item.get("currencyId"):
            fields["currencyId"] = source_item["currencyId"]

        if source["type"] == "deal":
            fields["relatedDealId"] = source["id"]

        invoice = token.call_method("crm.smartinvoice.add", {
            "fields": fields,
        }, prefer_version=3)["result"]
        print(f"Created smart invoice {invoice['id']}")

        if source_rows:
            token.call_method("crm.smartinvoice.productrow.replace", {
                "ownerId": invoice["id"],
                "items": prepare_product_rows(source_rows),
                "select": ["id"],
            }, prefer_version=3)

        return invoice["id"]
    ```

- PHP

    ```php
    <?php
    function prepareProductRows(array $rows): array
    {
        $preparedRows = [];

        foreach (array_values($rows) as $index => $row) {
            $preparedRows[] = [
                'productId' => $row['productId'] ?? null,
                'productName' => $row['productName'] ?? null,
                'price' => $row['price'] ?? null,
                'quantity' => $row['quantity'] ?? 1,
                'discountTypeId' => $row['discountTypeId'] ?? null,
                'discountRate' => $row['discountRate'] ?? null,
                'discountSum' => $row['discountSum'] ?? null,
                'taxRate' => $row['taxRate'] ?? null,
                'taxIncluded' => $row['taxIncluded'] ?? null,
                'measureCode' => $row['measureCode'] ?? null,
                'sort' => $row['sort'] ?? (($index + 1) * 10),
            ];
        }

        return $preparedRows;
    }

    function createInvoice($b24, array $source, array $properties, array $sourceItem, array $sourceRows): int
    {
        $myCompanyId = (string)($properties['mycompany_id'] ?? '');
        if (!ctype_digit($myCompanyId) || (int)$myCompanyId <= 0) {
            throw new InvalidArgumentException('The mycompany_id parameter must be a positive integer');
        }

        if (count($sourceRows) > 1000) {
            throw new InvalidArgumentException('An invoice can contain no more than 1000 rows');
        }

        $fields = [
            'title' => $properties['invoice_title'] ?? 'Invoice for CRM document',
            'companyId' => $sourceItem['companyId'] ?? null,
            'contactsId' => $sourceItem['contactsId'] ?? [],
            'myCompanyId' => (int)$myCompanyId,
        ];

        if (!empty($sourceItem['currencyId'])) {
            $fields['currencyId'] = $sourceItem['currencyId'];
        }

        if ($source['type'] === 'deal') {
            $fields['relatedDealId'] = $source['id'];
        }

        $invoice = $b24('crm.smartinvoice.add', ['fields' => $fields]);
        error_log('Created smart invoice ' . $invoice['id']);

        if ($sourceRows !== []) {
            $b24('crm.smartinvoice.productrow.replace', [
                'ownerId' => $invoice['id'],
                'items' => prepareProductRows($sourceRows),
                'select' => ['id'],
            ]);
        }

        return (int)$invoice['id'];
    }
    ```

{% endlist %}

Shortened `crm.smartinvoice.add` response: save `result.id` to copy products.

```json
{"result":{"id":128}}
```

The `crm.smartinvoice.productrow.replace` response with `select: ["id"]` contains the current row IDs in `result.items`:

```json
{"result":{"items":[{"id":201},{"id":202}]}}
```

## Assemble the Handler {#handler}

The handler calls the functions from steps 2–4 in order. The `HANDLER` URL is public, so the handler first compares `auth[application_token]` with the value the application saved from the [ONAPPINSTALL](../../api-reference/common/events/on-app-install.md) event. If they do not match, it responds with `403` without calling CRM methods. The saved value is returned by `getSavedAppToken` in JS and PHP and `get_saved_app_token` in Python: implement this function using your storage. The function accepts a Bitrix24 domain and returns its saved `application_token`, or an empty string if there is no record. Do not log the token. Deploy the handler at the `HANDLER` URL before starting the workflow.

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

    app.post('/bp-handler', async (req, res) => {
        const auth = req.body.auth || {}
        if (!sameToken(auth.application_token, await getSavedAppToken(auth.domain))) {
            return res.sendStatus(403)
        }

        res.sendStatus(200) // Bitrix24 does not wait for the action result

        try {
            const $b24 = makeClient(auth)
            const source = parseDocumentId(req.body.document_id)
            const properties = req.body.properties || {}
            const { sourceItem, sourceRows } = await loadSource($b24, source)
            await createInvoice($b24, source, properties, sourceItem, sourceRows)
        } catch (error) {
            console.error('Handler error:', error.message)
        }
    })

    app.listen(3000)
    ```

- Python

    ```python
    # pip install flask
    import hmac
    from flask import Flask, request

    app = Flask(__name__)

    @app.post("/bp-handler")
    def bp_handler():
        form = request.form
        saved = get_saved_app_token(form.get("auth[domain]", ""))
        if not saved or not hmac.compare_digest(form.get("auth[application_token]", ""), saved):
            return "", 403

        try:
            client, token = make_client(form_group(form, "auth"))
            source = parse_document_id(list(form_group(form, "document_id").values()))
            properties = form_group(form, "properties")
            source_item, source_rows = load_source(token, source)
            create_invoice(token, source, properties, source_item, source_rows)
        except Exception as error:
            app.logger.error("Handler error: %s", error)

        return "", 200
    ```

- PHP

    ```php
    <?php
    // bp-handler.php
    // Save the functions from steps 2–4 and getSavedAppToken in bp-functions.php with a single opening <?php tag
    require_once __DIR__ . '/vendor/autoload.php';
    require_once __DIR__ . '/bp-functions.php';

    $request = Symfony\Component\HttpFoundation\Request::createFromGlobals();
    $auth = $request->request->all('auth');

    $saved = getSavedAppToken($auth['domain'] ?? '');
    if ($saved === '' || !hash_equals($saved, (string)($auth['application_token'] ?? ''))) {
        http_response_code(403);
        exit;
    }

    try {
        $b24 = makeClient($request);
        $source = parseDocumentId($request->request->all('document_id'));
        $properties = $request->request->all('properties');
        [$sourceItem, $sourceRows] = loadSource($b24, $source);
        createInvoice($b24, $source, $properties, $sourceItem, $sourceRows);
    } catch (Throwable $e) {
        error_log('Handler error: ' . $e->getMessage());
    }

    http_response_code(200);
    ```

{% endlist %}

## Check the Result

1. Deploy the handler from [Assemble the Handler](#handler) at the `HANDLER` URL
2. Open the workflow designer for leads or deals, add the *Create smart invoice* action to the template, fill in *Invoice title* and *Your company ID*, and save the template
3. Start the workflow for a lead or deal with products

Open the smart invoice card. It should contain the title, customer, your company, currency, and products from the source lead or deal.

Through REST 3.0, check the invoice using [crm.smartinvoice.get](../../api-reference/crm/rest-v3/items/crm-entity-get-rest-v3.md): pass the `id` from the *Created smart invoice …* log entry and `select: ["title", "companyId", "contactsId", "myCompanyId", "currencyId", "relatedDealId"]`. For a deal, `relatedDealId` must match the source `id`. Check products using [crm.smartinvoice.productrow.list](../../api-reference/crm/rest-v3/product-rows/crm-entity-product-row-list-rest-v3.md) with `filter: [["ownerId", "=", id]]` and `select: ["productName", "price", "quantity"]`.

## Error Diagnostics

If a method returns an error, check the request data.

- `ACCESS_DENIED`, `Access denied! Application context required` - the action or automation rule is registered outside an application. Install the application and call the method in its context
- `ACCESS_DENIED`, `Access denied!` - registration is performed by a non-administrator
- `ERROR_ACTIVITY_VALIDATION_FAILURE`, `Wrong property key (...)!` or `Empty property NAME (...)!` - a parameter key in `PROPERTIES` must start with a Latin letter and contain only Latin letters, digits, and underscores, and the parameter must have a nonempty `Name`. `Wrong properties array!` means that `PROPERTIES` was not passed as an object
- `ERROR_ACTIVITY_VALIDATION_FAILURE`, `Wrong activity FILTER!` - `FILTER` was not passed as an object. Pass an object with the `INCLUDE` key, as in step 1
- `ERROR_ACTIVITY_ALREADY_INSTALLED`, `Activity or Robot already installed!` - an action with this `CODE` is already registered by the application. Change `CODE` or update the action using [bizproc.activity.update](../../api-reference/bizproc/bizproc-activity/bizproc-activity-update.md)
- `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` when calling CRM methods - the user the action runs as does not have permissions to read the source CRM object, create a smart invoice, or change invoice product rows
- `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` - the object was not found. Check the source object `id` or the invoice `ownerId`
- REST 3.0 validation error - check field names in `select` and `fields`, a positive `myCompanyId`, the `filter` condition array, and the `pagination` object
- empty `result.items` - the source lead or deal has no product rows, so the smart invoice will be created without products
- the handler responded with `200`, but there is no invoice - a `200` response does not confirm invoice creation: Bitrix24 does not wait for the result. Check the handler log. The *Created smart invoice* message confirms only invoice creation and contains its ID. If it is followed by *Handler error*, the failure occurred while copying products: check the invoice product rows separately
- the handler responded with `403` - no saved `application_token` for this application was found for the request domain, or the token does not match. Check the storage and start the workflow again
- the handler did not receive a request - check that the action is added to the template, the workflow has reached it, and `HANDLER` is publicly accessible over HTTPS

After fixing a registration error, repeat step 1. After fixing the handler, restart the workflow for the lead or deal. If the invoice was created but products were not copied, do not restart the workflow: it will create a second invoice. Fix the cause and repeat `crm.smartinvoice.productrow.replace` for the existing invoice: the handler logs its ID immediately after `crm.smartinvoice.add`. The method replaces the entire set of invoice rows, so pass the complete product list, including rows that were already copied.

## Important Notes

- For a lead, the scenario copies the customer and products to the smart invoice but does not link the invoice to the source lead
- Running the action again creates a new smart invoice. If duplicates are unacceptable, store the link between the CRM object and the created invoice in a CRM field or in an external system
- `AUTH_USER_ID` specifies the default user in the *Run as* field in the action settings. Bitrix24 passes the token of the user from this field to the handler. If the field is empty, it passes the token of the `AUTH_USER_ID` user. This user must have permissions to read the source CRM object, create a smart invoice, and change its product rows
- The `FILTER` parameter limits action availability in the designer, but it does not replace CRM user permission checks
- The action is registered without `USE_SUBSCRIPTION`, so by default it does not wait for the handler response: the workflow proceeds to the next action after the request is queued. Waiting can be enabled in the action settings with the *Wait for response* field. If subsequent actions need the invoice ID, register the action with `USE_SUBSCRIPTION: 'Y'` and `RETURN_PROPERTIES` and return the value using [bizproc.event.send](../../api-reference/bizproc/bizproc-robot/bizproc-event-send.md)

## Continue Learning

- [Add a new action bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md)
- [Register a new Automation rule bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md)
- [Get a Lead or Deal in REST 3.0](../../api-reference/crm/rest-v3/items/crm-entity-get-rest-v3.md)
- [Create a Smart Invoice in REST 3.0](../../api-reference/crm/rest-v3/items/crm-entity-add-rest-v3.md)
- [Get Product Rows in REST 3.0](../../api-reference/crm/rest-v3/product-rows/crm-entity-product-row-list-rest-v3.md)
- [Replace Product Rows in REST 3.0](../../api-reference/crm/rest-v3/product-rows/crm-entity-product-row-replace-rest-v3.md)
