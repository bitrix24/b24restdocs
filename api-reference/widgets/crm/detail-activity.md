# Button Above the Timeline of the CRM_XXX_DETAIL_ACTIVITY, CRM_DYNAMIC_XXX_DETAIL_ACTIVITY Card

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement, crm`](../../scopes/permissions.md)

The widget adds its own button to the panel above the timeline of a CRM object card: a [lead](../../crm/leads/index.md), [contact](../../crm/contacts/index.md), [company](../../crm/companies/index.md), [deal](../../crm/deals/index.md), [estimate](../../crm/quote/index.md), [new invoice](../../crm/universal/invoice.md), [order](../../sale/order/index.md), or [custom object type](../../crm/universal/index.md).

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## How the Placement Works

1. The application registers a handler with the [placement.bind](../placement-bind.md) method, passing a code from the [table](#codes) in `PLACEMENT`. The method works only on behalf of an application; the placement cannot be registered with a webhook. A widget can be registered by an administrator or by an employee with the permission to create applications
2. An application item appears in the *More* menu above the timeline
3. The user clicks the item: by default, Bitrix24 opens the handler in a frame in a slider. With `OPTIONS.useBuiltInInterface = 'Y'`, the handler is loaded in a hidden frame and calls `setLayout`, passing the [interface structure](./detail-activity-area.md#LayoutDto). Bitrix24 renders the interface from this structure. The call context is described in the [What the Handler Receives](#handler-data) section
4. The handler uses the object identifier from `PLACEMENT_OPTIONS` to retrieve data via REST with the `AUTH_ID` token of the current user

## When to Use

The placement suits actions next to activities and comments. For example:

- open a form for logging a request in an external system
- prepare a message to the client based on deal data

Choose the regular slider for the application's own markup and the built-in interface for a form based on the interface structure. For ongoing viewing of related data, use a [card tab](./detail-tab.md); for an action in the top menu, use the [card menu item](./detail-toolbar.md).

## Where the Widget Is Embedded {#codes}

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

In read-only mode, clicking the application item does not open the widget.

![Button above the timeline of the deal card](./_images/CRM_DEAL_DETAIL_ACTIVITY.png "Button above the timeline of the deal card")

## What the Handler Receives {#handler-data}

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

The example is shown for a regular slider opening in the `CRM_DEAL_DETAIL_ACTIVITY` placement. The built-in interface receives a different set of `PLACEMENT_OPTIONS` keys, described below.

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

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string. A regular slider opening passes `ID`; with `useBuiltInInterface = 'Y'`, it passes `entityTypeId`, `entityId`, and `useBuiltInInterface = 'Y'`.

#|
|| **Parameter** | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Identifier of the CRM object or order for a regular slider opening.

For a lead, deal, contact, company, estimate, new invoice, or custom object type item, use [crm.item.get](../../crm/universal/crm-item-get.md), specifying `entityTypeId` and `id`.

For an order, use [sale.order.get](../../sale/order/sale-order-get.md), specifying `id`. To call `sale.*` methods, the application needs the `sale` scope.

For a regular slider opening, the object type identifier does not arrive as a separate key. For a custom object type, take it from the `PLACEMENT` parameter value: for the `CRM_DYNAMIC_183_DETAIL_ACTIVITY` code, the type identifier is `183`

||
|| **entityTypeId**
[`integer`](../../data-types.md) | Identifier of the [CRM object type](../../crm/data-types.md#object_type) or order. Passed in the built-in interface ||
|| **entityId**
[`integer`](../../data-types.md) | Identifier of the CRM object or order. Passed in the built-in interface; the object data can be retrieved with the same methods as for `ID` ||
|| **useBuiltInInterface**
[`string`](../../data-types.md) | Built-in interface flag. Passed with the `Y` value in this mode ||
|#

Clicking *Learn more* in the welcome notification opens a separate slider: its context contains `entityTypeId`, `entityId`, `newUserNotification = 'Y'`, and `bx24_width = 800`.

## OPTIONS When Registering via placement.bind

For the `CRM_XXX_DETAIL_ACTIVITY` placements, the `placement.bind` method supports the `OPTIONS` parameters. They switch the widget to the built-in Bitrix24 interface instead of the application's own markup and configure the welcome notification.

#|
|| **Parameter**
`type` | **Description** ||
|| **useBuiltInInterface**
[`string`](../../data-types.md) | Use the built-in Bitrix24 interface: `Y` to enable, `N` to disable. `N` by default. When set to `Y`, the interface is built from the [interface structure](./detail-activity-area.md#LayoutDto) ||
|| **newUserNotificationTitle**
[`string`](../../data-types.md) | Title of the notification for a new user ||
|| **newUserNotificationText**
[`string`](../../data-types.md) | Text of the notification for a new user ||
|#

To show the notification, specify a non-empty title and text. The notification is shown in a saved card to a user who has not viewed it yet, provided that notifications are not disabled and the button is available in the interface.

## Code Examples

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

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
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
    		"en": b24.Params{
    			"TITLE": "My timeline button",
    		},
    		"de": b24.Params{
    			"TITLE": "Meine Timeline-Schaltfläche",
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

    // The response arrives as json.RawMessage — unmarshal it into the response
    // shape of the placement.bind method, see "Response Handling" on its page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Common Mistakes

Registration errors and how to fix them are described on the [placement.bind](../placement-bind.md) page.

## Continue Your Learning

- [{#T}](./index.md)
- [{#T}](./detail-activity-area.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../bx24-widget-methods.md)