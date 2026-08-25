# How to Embed a Widget into a CRM Item Tab

> Scope: [`placement`, `crm`](../../../api-reference/scopes/permissions.md)
>
> Who can execute the methods:
>
> - `placement.bind` — administrator
> - `crm.item.get` — any user with permission to read the deal

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A tab in a CRM item card allows you to display an application interface alongside the item's primary data. In this scenario, we will add a tab to a deal card, retrieve the identifier of the open deal, and request its data.

To complete this scenario, we will sequentially execute the following methods:

1. [placement.bind](../../../api-reference/widgets/placement-bind.md) — register a handler for the `CRM_DEAL_DETAIL_TAB` tab
2. [crm.item.get](../../../api-reference/crm/universal/crm-item-get.md) — retrieve deal data using the identifier from `PLACEMENT_OPTIONS`

{% note info "" %}

The original example and additional materials are available in the [Embedding into a CRM Item Card](https://helpdesk.bitrix24.com/courses/index.php?COURSE_ID=268&LESSON_ID=26026) lesson.

{% endnote %}

## How the Scenario Works

The application registers a handler URL using the `placement.bind` method and specifies the `CRM_DEAL_DETAIL_TAB` code. Once the application installation is complete, a new tab appears in the deal card.

When a user opens the tab, Bitrix24 loads the handler in an iframe and passes the invocation context to it. The current deal identifier is sent to `PLACEMENT_OPTIONS.ID`. The handler passes this identifier to `crm.item.get` and displays the retrieved data.

## 1. Prepare the Application

Create an [application](../../../settings/app-installation/index.md) with an interface and add the following permissions:

- `placement` — to register the widget handler
- `crm` — to retrieve deal data

The registration code and the tab handler can be placed in separate files or combined into a single file using different execution branches.

Host the handler page at a public HTTPS address. The examples use the following address:

```text
https://your-domain.example/deal-tab.php
```

The server must allow the page to be opened in an iframe. Check the `X-Frame-Options` header and the `frame-ancestors` directive of the `Content-Security-Policy` header: they must not prohibit embedding the page into Bitrix24.

{% note warning "" %}

The handler URL must be accessible from an external network. Do not use `localhost`, local network addresses, or self-signed SSL certificates.

{% endnote %}

The `placement.bind` method works only within the application context. An incoming webhook is not suitable for registering a tab.

## 2. Register the Tab

Register the handler using the `placement.bind` method. Pass the following parameters:

- `PLACEMENT` — the placement code for `CRM_DEAL_DETAIL_TAB`
- `HANDLER` — the public URL of the page that will open in the tab
- `TITLE` — the tab name
- `LANG_ALL` — localized tab names

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // npm install @bitrix24/b24jssdk
    // App settings page opened in a Bitrix24 iframe
    import { initializeB24Frame } from '@bitrix24/b24jssdk'

    const $b24 = await initializeB24Frame()

    const response = await $b24.actions.v2.call.make({
        method: 'placement.bind',
        params: {
            PLACEMENT: 'CRM_DEAL_DETAIL_TAB',
            HANDLER: 'https://your-domain.example/deal-tab.php',
            TITLE: 'Deal data',
            LANG_ALL: {
                ru: {
                    TITLE: 'Deal data',
                },
                en: {
                    TITLE: 'Deal data',
                },
            },
        },
        requestId: 'placement-bind',
    })

    if (!response.isSuccess) {
        throw new Error(response.getErrorMessages().join('; '))
    }

    console.info('Registered tab')
    ```

- Python

    ```python
    # pip install b24pysdk
    # client is built on the app token — see scenario
    # "How to embed a widget into a lead as a custom field"
    from b24pysdk.errors import BitrixAPIError

    try:
        bitrix_response = client.placement.bind(
            placement="CRM_DEAL_DETAIL_TAB",
            handler="https://your-domain.example/deal-tab.php",
            title="Deal data",
            lang_all={
                "de": {"TITLE": "Deal data"},
                "en": {"TITLE": "Deal data"},
            },
        ).response
        print("Registered tab:", bitrix_response.result)
    except BitrixAPIError as error:
        print(error)
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Exceptions\BaseException;

    // $b24 is built on the app token — see scenario
    // "How to embed a widget into a lead as a custom field"
    try
    {
        $b24->getPlacementScope()->placement()->bind(
            'CRM_DEAL_DETAIL_TAB',
            'https://your-domain.example/deal-tab.php',
            [
                'de' => ['TITLE' => 'Deal data'],
                'en' => ['TITLE' => 'Deal data'],
            ]
        );

        echo 'Registered tab';
    }
    catch (BaseException $exception)
    {
        echo $exception->getMessage();
    }
    ```
{% endlist %}

If the handler is successfully registered, the method will return `true`.

```json
{
    "result": true
}
```

After registration, [complete the application installation](../../../settings/app-installation/installation-finish.md). Until the installation is complete, the tab is unavailable to regular users.

## 3. Handle Tab Opening

When a tab is opened, Bitrix24 sends data to the handler via a POST request. For a deal card, the main parameters look like this:

```text
PLACEMENT=CRM_DEAL_DETAIL_TAB
PLACEMENT_OPTIONS={"ID":"3473"}
```

`PLACEMENT_OPTIONS` is passed as a JSON string. In PHP and Python, convert it into an array or dictionary—for example, using the `json_decode` or `json.loads` function. In B24JsSDK, the property `$b24.placement.options` returns a ready-to-use object, while `$b24.placement.placement` returns the placement code.

#|
|| **Parameter**
`type` | **Description** ||
|| **PLACEMENT**
[`string`](../../../api-reference/data-types.md) | Embedding location code. For the deal tab, `CRM_DEAL_DETAIL_TAB` is received ||
|| **PLACEMENT_OPTIONS**
[`string`](../../../api-reference/data-types.md) | JSON string with the context of the open card ||
|| **ID**
[`string`](../../../api-reference/data-types.md) | Deal identifier within `PLACEMENT_OPTIONS` ||
|| **DOMAIN**
[`string`](../../../api-reference/data-types.md) | Bitrix24 address where the user opened the tab ||
|| **PROTOCOL**
[`string`](../../../api-reference/data-types.md) | Protocol for accessing Bitrix24: `0` — HTTP, `1` — HTTPS ||
|| **AUTH_ID**
[`string`](../../../api-reference/data-types.md) | OAuth token of the user who opened the tab. The PHP handler uses the token to call `crm.item.get` with this user's permissions ||
|#

The full set of request service parameters is described on the [Tabs in CRM Cards](../../../api-reference/widgets/crm/detail-tab.md#what-the-handler-receives) page.

## 4. Retrieve Deal Data

Call `crm.item.get` from the handler. For a deal, pass:

- `entityTypeId: 2` — the identifier for the "Deal" CRM object type
- `id` — the identifier from `PLACEMENT_OPTIONS.ID`

The method is executed with the authorization of the user who opened the tab:

- JS runs inside an iframe — `initializeB24Frame` retrieves authorization from the tab context
- PHP and Python build a client using the request data that Bitrix24 passes to the handler, including the `AUTH_ID` token

{% list tabs %}

- JS

    ```html
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>Deal data</title>
        </head>
        <body>
            <h2 id="deal-title">Loading deal data</h2>
            <div id="deal-stage"></div>

            <script type="module">
                // npm install @bitrix24/b24jssdk
                import { initializeB24Frame } from '@bitrix24/b24jssdk'

                const $b24 = await initializeB24Frame()

                const dealId = Number($b24.placement.options.ID)

                if (
                    $b24.placement.placement !== 'CRM_DEAL_DETAIL_TAB'
                    || !Number.isInteger(dealId)
                    || dealId <= 0
                ) {
                    document.getElementById('deal-title').textContent =
                        'Failed to identify deal'
                } else {
                    const response = await $b24.actions.v2.call.make({
                        method: 'crm.item.get',
                        params: {
                            entityTypeId: 2,
                            id: dealId,
                        },
                        requestId: 'deal-get',
                    })

                    if (!response.isSuccess) {
                        document.getElementById('deal-title').textContent =
                            response.getErrorMessages().join('; ')
                    } else {
                        const deal = response.getData().result.item

                        document.getElementById('deal-title').textContent =
                            deal.title || 'Deal without title'
                        document.getElementById('deal-stage').textContent =
                            'Stage: ' + deal.stageId
                    }
                }
            </script>
        </body>
    </html>
    ```

- Python

    ```python
    # pip install b24pysdk flask
    from flask import Flask, request
    from b24pysdk import BitrixApp, BitrixToken, Client
    from b24pysdk.errors import BitrixAPIError
    import json

    app = Flask(__name__)

    bitrix_app = BitrixApp(
        client_id="local.xxxxxxxx.xxxxxxxx",
        client_secret="yyyyyyyy",
    )

    @app.post("/deal-tab")
    def deal_tab():
        placement = request.form.get("PLACEMENT", "")
        options = json.loads(request.form.get("PLACEMENT_OPTIONS", "{}") or "{}")
        deal_id = int(options.get("ID", 0))

        if placement != "CRM_DEAL_DETAIL_TAB" or deal_id <= 0:
            return "Failed to get call context"

        # Bitrix24 passes the domain and user token to the handler
        client = Client(
            BitrixToken(
                domain=request.args.get("DOMAIN", ""),
                auth_token=request.form.get("AUTH_ID", ""),
                bitrix_app=bitrix_app,
            )
        )

        try:
            deal = client.crm.item.get(
                entity_type_id=2,
                bitrix_id=deal_id,
            ).response.result["item"]
        except BitrixAPIError as error:
            return str(error)

        return f"{deal.get('title', 'Deal without title')} — stage: {deal.get('stageId', '')}"
    ```


- PHP

    ```php
    <?php
    // composer require bitrix24/b24phpsdk:"^3.0"
    require_once 'vendor/autoload.php';

    use Bitrix24\SDK\Core\Credentials\ApplicationProfile;
    use Bitrix24\SDK\Core\Exceptions\BaseException;
    use Bitrix24\SDK\Services\ServiceBuilderFactory;
    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();

    $placement = (string)$request->request->get('PLACEMENT', '');
    $placementOptions = json_decode(
        (string)$request->request->get('PLACEMENT_OPTIONS', '[]'),
        true
    ) ?: [];
    $dealId = (int)($placementOptions['ID'] ?? 0);

    $error = '';
    $deal = null;

    if ($placement !== 'CRM_DEAL_DETAIL_TAB' || $dealId <= 0)
    {
        $error = 'Failed to get call context';
    }
    else
    {
        $appProfile = ApplicationProfile::initFromArray([
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_ID' => 'local.xxxxxxxx.xxxxxxxx',
            'BITRIX24_PHP_SDK_APPLICATION_CLIENT_SECRET' => 'yyyyyyyy',
            'BITRIX24_PHP_SDK_APPLICATION_SCOPE' => 'crm,placement',
        ]);

        try
        {
            // The SDK will automatically take DOMAIN and AUTH_ID from the embedding request
            $b24 = ServiceBuilderFactory::createServiceBuilderFromPlacementRequest(
                $request,
                $appProfile
            );

            $deal = $b24->getCRMScope()->item()->get(2, $dealId)->item();
        }
        catch (BaseException $exception)
        {
            $error = $exception->getMessage();
        }
    }
    ?>
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>Deal data</title>
        </head>
        <body>
            <?php if ($error !== ''): ?>
                <p><?=htmlspecialchars($error)?></p>
            <?php else: ?>
                <h2><?=htmlspecialchars($deal->title ?? 'Deal without title')?></h2>
                <p>Stage: <?=htmlspecialchars($deal->stageId ?? '')?></p>
            <?php endif; ?>
        </body>
    </html>
    ```
{% endlist %}

The method returns a `item` object containing the deal data available to the user whose authorization is used in the request.

```json
{
    "result": {
        "item": {
            "id": 3473,
            "title": "Preparing offer",
            "stageId": "NEW"
        }
    }
}
```

The identifier from `PLACEMENT_OPTIONS` can also be used for other actions: retrieve related contacts and the company, request data from an external system, or display a custom interface for working with the deal.

## 5. Verify the Widget

1. Install the application in a test Bitrix24
2. Ensure that the application installation is complete
3. Open the CRM section
4. Open any deal
5. Find the **Deal Data** tab
6. Verify that the handler displays the name and stage of the opened deal

If the tab does not appear, check the handler registration using the [placement.get](../../../api-reference/widgets/placement-get.md) method. The response must contain the `CRM_DEAL_DETAIL_TAB` code and the handler page URL.

## Other CRM Cards

You can follow the same scenario to add a tab to cards of other objects. Replace the code in `PLACEMENT` and specify the corresponding `entityTypeId` in `crm.item.get`.

#|
|| **CRM Object** | **PLACEMENT** | **entityTypeId** ||
|| Lead | `CRM_LEAD_DETAIL_TAB` | `1` ||
|| Deal | `CRM_DEAL_DETAIL_TAB` | `2` ||
|| Contact | `CRM_CONTACT_DETAIL_TAB` | `3` ||
|| Company | `CRM_COMPANY_DETAIL_TAB` | `4` ||
|| Invoice | `CRM_SMART_INVOICE_DETAIL_TAB` | `31` ||
|#

For codes for commercial proposals and SPAs, see the description of the [CRM_XXX_DETAIL_TAB](../../../api-reference/widgets/crm/detail-tab.md) endpoint.

## Continue Learning

- [How to Embed Widgets in CRM](./index.md)
- [Tab in CRM Card CRM_XXX_DETAIL_TAB](../../../api-reference/widgets/crm/detail-tab.md)
- [Set Widget Handler with placement.bind](../../../api-reference/widgets/placement-bind.md)
- [Retrieve CRM Item with crm.item.get](../../../api-reference/crm/universal/crm-item-get.md)
- [Widget Interface Interaction Methods](../../../api-reference/widgets/ui-interaction/index.md)
