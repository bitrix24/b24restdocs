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

The original example and additional materials are available in the [Embedding into a CRM Item Card](https://dev.bitrixsoft.com/learning/course/index.php?COURSE_ID=266&LESSON_ID=25544) lesson.

{% endnote %}

## How the Scenario Works

The application registers a handler URL using the `placement.bind` method and specifies the `CRM_DEAL_DETAIL_TAB` code. Once the application installation is complete, a new tab appears in the deal card.

When a user opens the tab, Bitrix24 loads the handler in an iframe and passes the call context to it. The current deal identifier is received in `PLACEMENT_OPTIONS.ID`. The handler passes this identifier to `crm.item.get` and displays the retrieved data.

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

The `placement.bind` method works only within the context of an application. An incoming webhook is not suitable for registering a tab.

## 2. Register the Tab

Choose one example option: BX24.js or PHP CRest.

Register the handler using the `placement.bind` method. Pass the following parameters:

- `PLACEMENT` — the placement code for `CRM_DEAL_DETAIL_TAB`
- `HANDLER` — the public URL of the page that will open in the tab
- `TITLE` — the tab name
- `LANG_ALL` — localized tab names

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.callMethod(
        'placement.bind',
        {
            PLACEMENT: 'CRM_DEAL_DETAIL_TAB',
            HANDLER: 'https://your-domain.example/deal-tab.php',
            TITLE: 'Deal data',
            LANG_ALL: {
                de: {
                    TITLE: 'Transaction data',
                },
                en: {
                    TITLE: 'Deal data',
                },
            },
        },
        (result) => {
            if (result.error())
            {
                console.error(result.error() + ': ' + result.error_description());
                return;
            }

            console.info('Tab registered');
        }
    );
    ```

- PHP CRest

    ```php
    <?php
    require_once('crest.php');

    $result = CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'CRM_DEAL_DETAIL_TAB',
            'HANDLER' => 'https://your-domain.example/deal-tab.php',
            'TITLE' => 'Deal data',
            'LANG_ALL' => [
                'de' => [
                    'TITLE' => 'Transaction data',
                ],
                'en' => [
                    'TITLE' => 'Deal data',
                ],
            ],
        ]
    );

    if (!empty($result['error']))
    {
        echo $result['error'] . ': ' . $result['error_description'];
    }
    else
    {
        echo 'Tab registered';
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

`PLACEMENT_OPTIONS` is passed as a JSON string. In PHP, convert it to an array using the `json_decode` function. In the JavaScript SDK, the `BX24.getPlacementOptions()` method returns a ready-to-use object.

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

The full set of request service parameters is described on the [CRM Card Tab](../../../api-reference/widgets/crm/detail-tab.md#what-the-handler-receives) page.

## 4. Retrieve Deal Data

Call `crm.item.get` from the handler. For a deal, pass:

- `entityTypeId: 2` — the identifier for the "Deal" CRM object type
- `id` — the identifier from `PLACEMENT_OPTIONS.ID`

Choose one handler option:

- JavaScript calls the method via BX24.js using the authorization of the user who opened the tab
- PHP performs an OAuth request using the `AUTH_ID` token, which Bitrix24 passes for the user who opened the tab

{% list tabs %}

- JS

    ```html
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>Deal Data</title>
            <script src="https://api.bitrix24.com/api/v1/"></script>
        </head>
        <body>
            <h2 id="deal-title">Loading deal data</h2>
            <div id="deal-stage"></div>

            <script>
                BX24.init(() => {
                    const placementOptions = BX24.getPlacementOptions();
                    const dealId = Number(placementOptions.ID);

                    if (
                        BX24.getPlacement() !== 'CRM_DEAL_DETAIL_TAB'
                        || !Number.isInteger(dealId)
                        || dealId <= 0
                    )
                    {
                        document.getElementById('deal-title').textContent =
                            'Could not determine transaction';
                        return;
                    }

                    BX24.callMethod(
                        'crm.item.get',
                        {
                            entityTypeId: 2,
                            id: dealId,
                        },
                        (result) => {
                            if (result.error())
                            {
                                document.getElementById('deal-title').textContent =
                                    result.error_description();
                                return;
                            }

                            const deal = result.data().item;

                            document.getElementById('deal-title').textContent =
                                deal.title || 'Unnamed transaction';
                            document.getElementById('deal-stage').textContent =
                                'Stage: ' + deal.stageId;
                        }
                    );
                });
            </script>
        </body>
    </html>
    ```

- PHP (OAuth)

    ```php
    <?php
    $placement = $_POST['PLACEMENT'] ?? '';
    $placementOptions = isset($_POST['PLACEMENT_OPTIONS'])
        ? json_decode($_POST['PLACEMENT_OPTIONS'], true)
        : [];
    $placementOptions = is_array($placementOptions) ? $placementOptions : [];
    $dealId = (int)($placementOptions['ID'] ?? 0);
    $domain = (string)($_POST['DOMAIN'] ?? '');
    $authId = (string)($_POST['AUTH_ID'] ?? '');
    $protocol = ($_POST['PROTOCOL'] ?? '1') === '0' ? 'http' : 'https';
    $deal = [];
    $error = '';

    if (
        $placement !== 'CRM_DEAL_DETAIL_TAB'
        || $dealId <= 0
        || $domain === ''
        || $authId === ''
        || !preg_match('/^[a-z0-9.-]+(?::\d+)?$/i', $domain)
    )
    {
        $error = 'Could not get call context';
    }
    else
    {
        $curl = curl_init($protocol . '://' . $domain . '/rest/crm.item.get.json');

        curl_setopt_array(
            $curl,
            [
                CURLOPT_POST => true,
                CURLOPT_RETURNTRANSFER => true,
                CURLOPT_TIMEOUT => 30,
                CURLOPT_POSTFIELDS => http_build_query([
                    'entityTypeId' => 2,
                    'id' => $dealId,
                    'auth' => $authId,
                ]),
            ]
        );

        $response = curl_exec($curl);

        if ($response === false)
        {
            $error = curl_error($curl);
        }

        curl_close($curl);

        if ($error === '')
        {
            $result = json_decode($response, true);

            if (!is_array($result))
            {
                $error = 'Could not parse Bitrix24 response';
            }
            elseif (!empty($result['error']))
            {
                $error = $result['error_description'] ?? $result['error'];
            }
            else
            {
                $deal = $result['result']['item'] ?? [];
            }
        }
    }
    ?>
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>Deal Data</title>
        </head>
        <body>
            <?php if ($error !== ''): ?>
                <p><?=htmlspecialchars($error)?></p>
            <?php else: ?>
                <h2><?=htmlspecialchars($deal['title'] ?? 'Unnamed transaction')?></h2>
                <p>Stage: <?=htmlspecialchars($deal['stageId'] ?? '')?></p>
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
            "title": "Preparing proposal",
            "stageId": "NEW"
        }
    }
}
```

The identifier from `PLACEMENT_OPTIONS` can also be used for other actions: retrieving linked contacts and the company, requesting data from an external system, or displaying a custom interface for working with the deal.

## 5. Verify the Widget

1. Install the application in a test Bitrix24 environment
2. Ensure that the application installation is complete
3. Open the CRM section
4. Open any deal
5. Locate the **Deal Data** tab
6. Verify that the handler displays the name and stage of the opened deal

If the tab does not appear, check the handler registration using the [placement.get](../../../api-reference/widgets/placement-get.md) method. The response must include code `CRM_DEAL_DETAIL_TAB` and the handler page URL.

## Other CRM Cards

You can add a tab to cards of other objects using the same scenario. Replace the code in `PLACEMENT` and specify the corresponding `entityTypeId` in `crm.item.get`.

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
- [CRM Card Tab CRM_XXX_DETAIL_TAB](../../../api-reference/widgets/crm/detail-tab.md)
- [Set a Widget Handler with placement.bind](../../../api-reference/widgets/placement-bind.md)
- [Retrieve a CRM Item with crm.item.get](../../../api-reference/crm/universal/crm-item-get.md)
- [Widget Interface Interaction Methods](../../../api-reference/widgets/ui-interaction/index.md)
