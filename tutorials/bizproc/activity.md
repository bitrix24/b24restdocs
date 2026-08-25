# How to Add an Action to Create a Smart Invoice Based on a Lead or Deal

> Scope: [`bizproc`, `crm`](../../api-reference/scopes/permissions.md)
>
> Who can execute the methods: to complete the whole scenario, you need a Bitrix24 administrator with permissions to read the lead or deal, create a smart invoice, and change its product rows
>
> - [bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md) and [bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md) - administrator
> - [crm.item.get](../../api-reference/crm/universal/crm-item-get.md) - user with permission to read CRM object items
> - [crm.item.add](../../api-reference/crm/universal/crm-item-add.md) - user with permission to add CRM object items
> - [crm.item.productrow.list](../../api-reference/crm/universal/product-rows/crm-item-productrow-list.md) - user with permission to read the CRM object whose product rows are selected
> - [crm.item.productrow.set](../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md) - user with permission to change the CRM object whose product rows are set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The scenario shows how an application adds a workflow action that receives a lead or deal from the execution context and creates a smart invoice from the CRM object data. The customer and product rows are copied from the lead or deal to the invoice. For a deal, the invoice is also linked to the source deal through the `parentId2` field.

The action can be used in the workflow designer. For CRM automation, use an application Automation rule: the parameter set and handler remain the same, only the registration method changes.

The scenario consists of four steps.

1. Register the action using [bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md)
2. Get `document_id` in the handler and determine what started the action: a lead or a deal
3. Get CRM object data using [crm.item.get](../../api-reference/crm/universal/crm-item-get.md) and product rows using [crm.item.productrow.list](../../api-reference/crm/universal/product-rows/crm-item-productrow-list.md)
4. Create a smart invoice using [crm.item.add](../../api-reference/crm/universal/crm-item-add.md) and copy products using [crm.item.productrow.set](../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md)

## Prepare the Application

`bizproc.activity.add` and `bizproc.robot.add` work only within an [application](../../settings/app-installation/index.md) context. An incoming webhook will not work: the method will return the `ACCESS_DENIED` error with the description `Application context required`.

Before you start, prepare:

- an installed application with the `bizproc` and `crm` scopes
- a public HTTPS handler address, for example `https://your-domain.example/bp-handler`
- the administrator ID for the `AUTH_USER_ID` parameter
- your company ID for the smart invoice field `mycompanyId`

The handler receives application authorization in the request from Bitrix24. Use `auth[domain]`, `auth[access_token]`, and `auth[refresh_token]` values to call CRM methods from the handler.

The scenario uses CRM type identifiers:

#|
|| Object | `entityTypeId` | `ownerType` ||
|| Lead | `1` | `L` ||
|| Deal | `2` | `D` ||
|| Smart invoice | `31` | `SI` ||
|#

{% include [Note on examples](../../_includes/examples.md) %}

## Initialize the SDK in the Handler

The handler receives authorization in the request from Bitrix24. Use `auth` to create an SDK client for CRM method calls.

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    import { B24OAuth } from '@bitrix24/b24jssdk'

    const APP = { clientId: 'local.xxxxxxxx.xxxxxxxx', clientSecret: 'yyyyyyyy' }

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

    const $b24 = makeClient(req.body.auth)
    ```

- Python

    ```python
    # pip install b24pysdk
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
        'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'bizproc,crm',
    ]);

    $authToken = AuthToken::initFromEventRequest($request);
    $domain = (string)$request->request->all('auth')['domain'];

    $log = new Logger('bizproc');
    $log->pushHandler(new StreamHandler('php://stdout'));

    $b24 = (new ServiceBuilderFactory(new EventDispatcher(), $log))
        ->init($appProfile, $authToken, $domain, DefaultOAuthServerUrl::default());
    ```
{% endlist %}

## 1. Register the Action

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
    # pip install b24pysdk

    # client is built on the installed application token.
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
    require_once 'vendor/autoload.php';

    // $b24 is built on the installed application token.
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

    print_r($response->getResponseData()->getResult()); // true
    ```
{% endlist %}

If you need to add an Automation rule for CRM automation, replace `bizproc.activity.add` with [bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md). The `CODE`, `HANDLER`, `AUTH_USER_ID`, `NAME`, `DESCRIPTION`, `PROPERTIES`, and `FILTER` parameters are used the same way.

Example of a successful response:

```json
{
    "result": true
}
```

After registration, the action will appear in the workflow designer for leads and deals. When the workflow reaches this action, Bitrix24 will call `HANDLER`.

## 2. Parse Handler Data

Bitrix24 passes action parameters to the handler in `properties` and document identifiers in `document_id`. For a lead, the value contains a string like `LEAD_456`; for a deal, `DEAL_123`.

From `document_id`, save:

- `entityTypeId` of the source CRM object: `1` for a lead or `2` for a deal
- `ownerType` of the source CRM object: `L` for a lead or `D` for a deal
- numeric ID of the source CRM object

{% list tabs %}

- JS

    ```js
    function parseDocumentId(documentId) {
        const values = Array.isArray(documentId) ? documentId : [documentId]
        const deal = values.find((value) => String(value).startsWith('DEAL_'))
        const lead = values.find((value) => String(value).startsWith('LEAD_'))

        if (deal) {
            return { entityTypeId: 2, ownerType: 'D', id: Number(deal.slice(5)) }
        }

        if (lead) {
            return { entityTypeId: 1, ownerType: 'L', id: Number(lead.slice(5)) }
        }

        throw new Error('The action was started neither from a lead nor from a deal')
    }

    const source = parseDocumentId(req.body.document_id)
    const properties = req.body.properties || {}
    ```

- Python

    ```python
    def parse_document_id(document_id: list[str]) -> dict:
        for value in document_id:
            if value.startswith("DEAL_"):
                return {"entityTypeId": 2, "ownerType": "D", "id": int(value[5:])}

            if value.startswith("LEAD_"):
                return {"entityTypeId": 1, "ownerType": "L", "id": int(value[5:])}

        raise ValueError("The action was started neither from a lead nor from a deal")

    payload = request.json
    source = parse_document_id(payload.get("document_id", []))
    properties = payload.get("properties", {})
    ```


- PHP

    ```php
    <?php
    function parseDocumentId(array $documentId): array
    {
        foreach ($documentId as $value) {
            if (str_starts_with((string)$value, 'DEAL_')) {
                return ['entityTypeId' => 2, 'ownerType' => 'D', 'id' => (int)substr((string)$value, 5)];
            }

            if (str_starts_with((string)$value, 'LEAD_')) {
                return ['entityTypeId' => 1, 'ownerType' => 'L', 'id' => (int)substr((string)$value, 5)];
            }
        }

        throw new RuntimeException('The action was started neither from a lead nor from a deal');
    }

    $source = parseDocumentId((array)($_REQUEST['document_id'] ?? []));
    $properties = $_REQUEST['properties'] ?? [];
    ```
{% endlist %}

## 3. Get the CRM Object and Products

Call [crm.item.get](../../api-reference/crm/universal/crm-item-get.md) to get fields of the source lead or deal. Pass the value obtained while parsing `document_id` to `entityTypeId`, and pass the numeric object ID to `id`.

Get product rows using [crm.item.productrow.list](../../api-reference/crm/universal/product-rows/crm-item-productrow-list.md). In the filter, pass `=ownerType` and `=ownerId` of the source CRM object.

{% list tabs %}

- JS

    ```js
    async function callMethod($b24, method, params) {
        const response = await $b24.actions.v2.call.make({
            method,
            params,
            requestId: method,
        })

        if (!response.isSuccess) {
            throw new Error(response.getErrorMessages().join('; '))
        }

        return response.getData().result
    }

    const sourceItemResult = await callMethod($b24, 'crm.item.get', {
        entityTypeId: source.entityTypeId,
        id: source.id,
    })
    const sourceItem = sourceItemResult.item

    const sourceRowsResult = await callMethod($b24, 'crm.item.productrow.list', {
        filter: {
            '=ownerType': source.ownerType,
            '=ownerId': source.id,
        },
    })
    const sourceRows = sourceRowsResult.productRows
    ```

- Python

    ```python
    source_item = token.call_method("crm.item.get", {
        "entityTypeId": source["entityTypeId"],
        "id": source["id"],
    })["result"]["item"]

    source_rows = token.call_method("crm.item.productrow.list", {
        "filter": {
            "=ownerType": source["ownerType"],
            "=ownerId": source["id"],
        },
    })["result"]["productRows"]
    ```


- PHP

    ```php
    <?php
    $sourceItem = $b24->core
        ->call('crm.item.get', [
            'entityTypeId' => $source['entityTypeId'],
            'id' => $source['id'],
        ])
        ->getResponseData()
        ->getResult()['item'];

    $sourceRows = $b24->core
        ->call('crm.item.productrow.list', [
            'filter' => [
                '=ownerType' => $source['ownerType'],
                '=ownerId' => $source['id'],
            ],
        ])
        ->getResponseData()
        ->getResult()['productRows'];
    ```
{% endlist %}

From the `crm.item.get` response, save `item.companyId`, `item.contactId`, or `item.contactIds`. These fields are needed for the smart invoice customer. From the `crm.item.productrow.list` response, save the `productRows` array: it must be prepared and passed to the smart invoice.

## 4. Create a Smart Invoice and Copy Products

Create a smart invoice using [crm.item.add](../../api-reference/crm/universal/crm-item-add.md). Pass `31` in `entityTypeId`.

In `fields`, pass:

- `title` - smart invoice title from the action parameter `invoice_title`
- `companyId` - company ID from the source CRM object
- `contactId` - contact ID from the source CRM object
- `contactIds` - array of contact IDs from the source CRM object, if it exists
- `mycompanyId` - your company ID from the action parameter `mycompany_id`
- `parentId2` - deal ID if the action was started from a deal

Then pass product rows to the created smart invoice using [crm.item.productrow.set](../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md). In `ownerType`, specify `SI`; in `ownerId`, specify the smart invoice ID from the `crm.item.add` response.

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

    const fields = {
        title: properties.invoice_title || 'Invoice for CRM document',
        companyId: sourceItem.companyId || 0,
        contactId: sourceItem.contactId || 0,
        contactIds: sourceItem.contactIds || [],
        mycompanyId: Number(properties.mycompany_id),
    }

    if (source.entityTypeId === 2) {
        fields.parentId2 = source.id
    }

    const invoiceResult = await callMethod($b24, 'crm.item.add', {
        entityTypeId: 31,
        fields,
    })
    const invoiceId = invoiceResult.item.id

    if (sourceRows.length > 0) {
        await callMethod($b24, 'crm.item.productrow.set', {
            ownerType: 'SI',
            ownerId: invoiceId,
            productRows: prepareProductRows(sourceRows),
        })
    }

    console.info(`Created smart invoice ${invoiceId}`)
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

    fields = {
        "title": properties.get("invoice_title", "Invoice for CRM document"),
        "companyId": int(source_item.get("companyId") or 0),
        "contactId": int(source_item.get("contactId") or 0),
        "contactIds": source_item.get("contactIds") or [],
        "mycompanyId": int(properties.get("mycompany_id") or 0),
    }

    if source["entityTypeId"] == 2:
        fields["parentId2"] = source["id"]

    invoice = token.call_method("crm.item.add", {
        "entityTypeId": 31,
        "fields": fields,
    })["result"]["item"]

    if source_rows:
        token.call_method("crm.item.productrow.set", {
            "ownerType": "SI",
            "ownerId": invoice["id"],
            "productRows": prepare_product_rows(source_rows),
        })

    print(f"Created smart invoice {invoice['id']}")
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

    $fields = [
        'title' => $properties['invoice_title'] ?? 'Invoice for CRM document',
        'companyId' => (int)($sourceItem['companyId'] ?? 0),
        'contactId' => (int)($sourceItem['contactId'] ?? 0),
        'contactIds' => $sourceItem['contactIds'] ?? [],
        'mycompanyId' => (int)($properties['mycompany_id'] ?? 0),
    ];

    if ($source['entityTypeId'] === 2) {
        $fields['parentId2'] = $source['id'];
    }

    $invoice = $b24->core
        ->call('crm.item.add', [
            'entityTypeId' => 31,
            'fields' => $fields,
        ])
        ->getResponseData()
        ->getResult()['item'];

    if ($sourceRows !== []) {
        $b24->core->call('crm.item.productrow.set', [
            'ownerType' => 'SI',
            'ownerId' => $invoice['id'],
            'productRows' => prepareProductRows($sourceRows),
        ]);
    }

    echo 'Created smart invoice ' . $invoice['id'];
    ```
{% endlist %}

Example of a successful `crm.item.add` response:

```json
{
    "result": {
        "item": {
            "id": 128,
            "entityTypeId": 31,
            "title": "Invoice for CRM document"
        }
    }
}
```

Save `result.item.id`: this is the ID of the created smart invoice. It must be passed to the `ownerId` parameter of `crm.item.productrow.set`.

## Check the Result

Open the smart invoice card. It should contain the title, customer, your company, and products from the source lead or deal.

You can check the result through REST using [crm.item.get](../../api-reference/crm/universal/crm-item-get.md). Pass `entityTypeId = 31` and the `id` from the `crm.item.add` response.

## Error Diagnostics

If a method returns an error, check the request data.

- `ACCESS_DENIED`, `Application context required` - the action or Automation rule is registered outside an application. Install the application and call the method in its context
- `ACCESS_DENIED`, `Access denied!` - registration is performed by a non-administrator
- `ERROR_ACTIVITY_VALIDATION_FAILURE`, `Wrong properties array!` - `PROPERTIES` or `RETURN_PROPERTIES` are filled incorrectly
- `ERROR_ACTIVITY_VALIDATION_FAILURE`, `Wrong activity DOCUMENT_TYPE!` - `DOCUMENT_TYPE` or the `FILTER` rule is specified incorrectly
- `ACCESS_DENIED` when calling CRM methods - the user from `AUTH_USER_ID` does not have permissions to read the source CRM object, create a smart invoice, or change invoice product rows
- `OWNER_NOT_FOUND` - an incorrect smart invoice `ownerType` or `ownerId` was passed to `crm.item.productrow.set`
- empty `productRows` - the source lead or deal has no product rows, so the smart invoice will be created without products

## Important Notes

- For a deal, the smart invoice is linked to the source deal through the `parentId2` field
- For a lead, the scenario copies the customer and products to the smart invoice. A separate link between a smart invoice and a lead through `parentId1` is not described in the smart invoices article
- Running the action again creates a new smart invoice. If duplicates are unacceptable, store the link between the CRM object and the created invoice in a CRM field or in an external system
- The `AUTH_USER_ID` value determines whose token Bitrix24 passes to the handler. This user must have permissions to read the source CRM object, create a smart invoice, and change its product rows
- The `FILTER` parameter limits action availability in the designer, but it does not replace CRM user permission checks

## Continue Learning

- [Add a new action bizproc.activity.add](../../api-reference/bizproc/bizproc-activity/bizproc-activity-add.md)
- [Register a new Automation rule bizproc.robot.add](../../api-reference/bizproc/bizproc-robot/bizproc-robot-add.md)
- [Get a CRM item crm.item.get](../../api-reference/crm/universal/crm-item-get.md)
- [Create a new CRM item crm.item.add](../../api-reference/crm/universal/crm-item-add.md)
- [Get CRM object product rows crm.item.productrow.list](../../api-reference/crm/universal/product-rows/crm-item-productrow-list.md)
- [Save CRM object product rows crm.item.productrow.set](../../api-reference/crm/universal/product-rows/crm-item-productrow-set.md)
