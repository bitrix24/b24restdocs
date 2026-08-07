# Button Above the Timeline of the CRM_XXX_DETAIL_ACTIVITY, CRM_DYNAMIC_XXX_DETAIL_ACTIVITY Card

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement, crm`](../../scopes/permissions.md)

The widget adds its own button to the panel above the timeline of a CRM object card: a [lead](../../crm/leads/index.md), [contact](../../crm/contacts/index.md), [company](../../crm/companies/index.md), [deal](../../crm/deals/index.md), [estimate](../../crm/quote/index.md), [new invoice](../../crm/universal/invoice.md), [order](../../sale/order/index.md), or [custom object type](../../crm/universal/index.md).

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

Extended capabilities of the button above the timeline are described in the article [Additional Placement Features in CRM_XXX_DETAIL_ACTIVITY](./detail-activity-area.md).

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#|
|| **Placement Code** | **Location** ||
|| `CRM_LEAD_DETAIL_ACTIVITY` | Button above the timeline of [lead](../../crm/leads/index.md) ||
|| `CRM_CONTACT_DETAIL_ACTIVITY` | Button above the timeline of [contact](../../crm/contacts/index.md) ||
|| `CRM_COMPANY_DETAIL_ACTIVITY` | Button above the timeline of [company](../../crm/companies/index.md) ||
|| `CRM_DEAL_DETAIL_ACTIVITY` | Button above the timeline of [deal](../../crm/deals/index.md) ||
|| `CRM_QUOTE_DETAIL_ACTIVITY` | Button above the timeline of [estimate](../../crm/quote/index.md) ||
|| `CRM_SMART_INVOICE_DETAIL_ACTIVITY` | Button above the timeline of [invoices](../../crm/universal/invoice.md) ||
|| `CRM_ORDER_DETAIL_ACTIVITY` | Button above the timeline of an [online store order](../../sale/order/index.md) ||
|| `CRM_DYNAMIC_XXX_DETAIL_ACTIVITY` | Button above the timeline of custom CRM object type. Instead of XXX, specify the numeric identifier of the specific [custom object type](../../crm/universal/index.md). For example, `CRM_DYNAMIC_183_DETAIL_ACTIVITY` ||
|#

### Where to Find It in the Interface

Open a CRM object card and click *More* in the panel above the timeline — the row with the *Activity*, *Comment*, and *Message* buttons. The application item appears in this menu.

![Button above the timeline of the deal card](./_images/CRM_DEAL_DETAIL_ACTIVITY.png "Button above the timeline of the deal card")

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

The example is shown for the `CRM_DEAL_DETAIL_ACTIVITY` placement. Other codes send the same set of data: the `PLACEMENT` value and the object identifier in `PLACEMENT_OPTIONS` change.

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 79adf5ce0f12cdb9e4137fd8ea6741bf
    [AUTH_ID] => a15e7166007e9c94001e30ba00000001f0f1073c7d9e0512b8a46f0ed37c951
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 904d9966007e9c94001e30ba00000001f0f10748e2b1d3906f57ca2b81de640
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => crm,placement
    [member_id] => d897063e1ce7c5eb9f04b9751eef5915
    [status] => L
    [PLACEMENT] => CRM_DEAL_DETAIL_ACTIVITY
    [PLACEMENT_OPTIONS] => {"ID":"8061","URI":"\/crm\/deal\/details\/8061\/?any=details%2F8061%2F"}
)

```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context. Along with the universal `URI` key, the context carries the object identifier.

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Parameter** | **Description** ||
|| **ID*** 
[`string`](../../data-types.md) | Identifier of the CRM object for which the widget was opened.

It can be used to retrieve additional information using the corresponding methods:

- any object type [crm.item.get](../../crm/universal/crm-item-get.md) specifying entityTypeId = '1' for leads, '2' for deals, and [etc.](../../crm/data-types.md#object_type)
- lead [crm.lead.get](../../crm/leads/crm-lead-get.md)
- deal [crm.deal.get](../../crm/deals/crm-deal-get.md)
- contact [crm.contact.get](../../crm/contacts/crm-contact-get.md)
- company [crm.company.get](../../crm/companies/crm-company-get.md)
- estimate [crm.quote.get](../../crm/quote/crm-quote-get.md)

The object type identifier does not arrive as a separate key. For a custom object type, take it from the `PLACEMENT` parameter value: for the `CRM_DYNAMIC_183_DETAIL_ACTIVITY` code, the type identifier is `183`

||
|#

## OPTIONS When Registering via placement.bind

For the `CRM_XXX_DETAIL_ACTIVITY` placements, the `placement.bind` method supports the `OPTIONS` parameters. They switch the widget to the built-in Bitrix24 interface instead of the application's own markup and configure the welcome notification.

{% include [Footnote on Required Parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type` | **Description** ||
|| **useBuiltInInterface**
[`boolean`](../../data-types.md) | Use the built-in Bitrix24 interface, `N` by default. When set to `Y`, the interface is built from the [LayoutDto](./detail-activity-area.md#LayoutDto) structure ||
|| **newUserNotificationTitle**
[`string`](../../data-types.md) | Title of the notification for a new user ||
|| **newUserNotificationText**
[`string`](../../data-types.md) | Text of the notification for a new user. Clicking *Learn more* opens a slider with the `newUserNotification=Y` context and a width of `800px` ||
|#

### Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "CRM_DEAL_DETAIL_ACTIVITY",
        "HANDLER": "https://your-domain.com/widgets/crm-detail-activity-handler.php",
        "TITLE": "My timeline button",
        "LANG_ALL": {
          "en": {
            "TITLE": "My timeline button"
          },
          "de": {
            "TITLE": "Meine Timeline-Schaltfläche"
          }
        },
        "OPTIONS": {
          "useBuiltInInterface": "Y",
          "newUserNotificationTitle": "Meet the new app",
          "newUserNotificationText": "The app helps you work with deals"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/placement.bind
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'placement.bind',
        params: {
          PLACEMENT: 'CRM_DEAL_DETAIL_ACTIVITY',
          HANDLER: 'https://your-domain.com/widgets/crm-detail-activity-handler.php',
          TITLE: 'My timeline button',
          LANG_ALL: {
            en: {
              TITLE: 'My timeline button',
            },
            de: {
              TITLE: 'Meine Timeline-Schaltfläche',
            },
          },
          OPTIONS: {
            useBuiltInInterface: 'Y',
            newUserNotificationTitle: 'Meet the new app',
            newUserNotificationText: 'The app helps you work with deals',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Placement bound successfully:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function bindCrmDealDetailActivity() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'CRM_DEAL_DETAIL_ACTIVITY',
              HANDLER: 'https://your-domain.com/widgets/crm-detail-activity-handler.php',
              TITLE: 'My timeline button',
              LANG_ALL: {
                en: {
                  TITLE: 'My timeline button',
                },
                de: {
                  TITLE: 'Meine Timeline-Schaltfläche',
                },
              },
              OPTIONS: {
                useBuiltInInterface: 'Y',
                newUserNotificationTitle: 'Meet the new app',
                newUserNotificationText: 'The app helps you work with deals',
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Placement bound successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindCrmDealDetailActivity)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.bind',
                [
                    'PLACEMENT' => 'CRM_DEAL_DETAIL_ACTIVITY',
                    'HANDLER' => 'https://your-domain.com/widgets/crm-detail-activity-handler.php',
                    'TITLE' => 'My timeline button',
                    'LANG_ALL' => [
                        'en' => [
                            'TITLE' => 'My timeline button',
                        ],
                        'de' => [
                            'TITLE' => 'Meine Timeline-Schaltfläche',
                        ],
                    ],
                    'OPTIONS' => [
                        'useBuiltInInterface' => 'Y',
                        'newUserNotificationTitle' => 'Meet the new app',
                        'newUserNotificationText' => 'The app helps you work with deals',
                    ],
                ]
            );

        $result = $response->getResponseData()->getResult();
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error binding placement: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'placement.bind',
        {
            PLACEMENT: 'CRM_DEAL_DETAIL_ACTIVITY',
            HANDLER: 'https://your-domain.com/widgets/crm-detail-activity-handler.php',
            TITLE: 'My timeline button',
            LANG_ALL: {
                en: { TITLE: 'My timeline button' },
                de: { TITLE: 'Meine Timeline-Schaltfläche' }
            },
            OPTIONS: {
                useBuiltInInterface: 'Y',
                newUserNotificationTitle: 'Meet the new app',
                newUserNotificationText: 'The app helps you work with deals'
            }
        },
        function(result) {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'CRM_DEAL_DETAIL_ACTIVITY',
            'HANDLER' => 'https://your-domain.com/widgets/crm-detail-activity-handler.php',
            'TITLE' => 'My timeline button',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'My timeline button',
                ],
                'de' => [
                    'TITLE' => 'Meine Timeline-Schaltfläche',
                ],
            ],
            'OPTIONS' => [
                'useBuiltInInterface' => 'Y',
                'newUserNotificationTitle' => 'Meet the new app',
                'newUserNotificationText' => 'The app helps you work with deals',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "placement.bind", b24.Params{
    	"PLACEMENT": "CRM_DEAL_DETAIL_ACTIVITY",
    	"HANDLER":   "https://your-domain.com/widgets/crm-detail-activity-handler.php",
    	"TITLE":     "My timeline button",
    	"LANG_ALL": b24.Params{
    		"ru": b24.Params{
    			"TITLE": "My timeline button",
    		},
    		"en": b24.Params{
    			"TITLE": "My timeline button",
    		},
    	},
    	"OPTIONS": b24.Params{
    		"useBuiltInInterface":      "Y",
    		"newUserNotificationTitle": "Meet the new app",
    		"newUserNotificationText":  "The app helps you work with deals",
    	},
    })
    if err != nil {
    	return fmt.Errorf("placement.bind: %w", err)
    }

    // The response arrives as json.RawMessage — unmarshal it
    // into a struct matching the response shape shown below on this page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Continue Your Learning

- [{#T}](./index.md)
- [{#T}](./detail-activity-area.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../bx24-widget-methods.md)