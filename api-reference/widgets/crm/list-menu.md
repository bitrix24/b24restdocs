# Context Menu Item in the CRM_XXX_LIST_MENU, CRM_DYNAMIC_XXX_LIST_MENU

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement, crm`](../../scopes/permissions.md)

The widget adds its own item to the context menu of an element in the list of CRM objects: [leads](../../crm/leads/index.md), [contacts](../../crm/contacts/index.md), [companies](../../crm/companies/index.md), [deals](../../crm/deals/index.md), [estimates](../../crm/quote/index.md), [new invoices](../../crm/universal/invoice.md), [activities](../../crm/timeline/activities/index.md), and [custom object types](../../crm/universal/index.md).

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## How the Placement Works

1. The application registers a handler with the [placement.bind](../placement-bind.md) method, passing a code from the [table](#codes) in the `PLACEMENT` parameter. The method works only on behalf of an application; the placement cannot be registered with a webhook. A widget can be registered by an administrator or by an employee with the permission to create applications
2. An item with the name from `TITLE` appears in the context menu of the element. If no name is passed, the application name is shown
3. When the user clicks the item, Bitrix24 opens the handler in a frame. The handler receives a POST request with the [call data](#handler-data), including the element identifier in `PLACEMENT_OPTIONS.ID`
4. The handler calls REST on behalf of the current user with the token from `AUTH_ID`: for example, it retrieves the deal with the [crm.item.get](../../crm/universal/crm-item-get.md) method, passing `entityTypeId = 2` and the received `ID`

## When to Use

- open the history of data exchange between the selected deal and an external system in the application
- show a form for sending the data of a specific contact to an external service

For actions on the list as a whole, use [CRM_XXX_LIST_TOOLBAR](./list-toolbar.md). To open the application from the element form, choose [CRM_XXX_DETAIL_TAB](./detail-tab.md).

## Where the Widget Is Embedded {#codes}

#| 
|| **Placement Code** | **Location** ||
|| `CRM_LEAD_LIST_MENU` | Context menu item for [lead](../../crm/leads/index.md) ||
|| `CRM_CONTACT_LIST_MENU` | Context menu item for [contact](../../crm/contacts/index.md) ||
|| `CRM_COMPANY_LIST_MENU` | Context menu item for [company](../../crm/companies/index.md) ||
|| `CRM_DEAL_LIST_MENU` | Context menu item for [deal](../../crm/deals/index.md) ||
|| `CRM_SMART_INVOICE_LIST_MENU` | Context menu item for [new invoice](../../crm/universal/invoice.md) ||
|| `CRM_QUOTE_LIST_MENU` | Context menu item for [estimate](../../crm/quote/index.md) ||
|| `CRM_ACTIVITY_LIST_MENU` | Context menu item for [CRM activity](../../crm/timeline/activities/index.md) ||
|| `CRM_DYNAMIC_XXX_LIST_MENU` | Context menu item for custom CRM object type. Instead of XXX, specify the numeric identifier of the specific [custom object type](../../crm/universal/index.md). For example, `CRM_DYNAMIC_183_LIST_MENU` || 
|#

### Where to Find It in the Interface {#interface}

Open a CRM section in list view, click the menu button to the left of an element, and hover over *Bitrix24 Market*. The application item appears in this submenu.

![Context menu item in the deal list](./_images/CRM_DEAL_LIST_MENU.png "Context menu item in the deal list")

## What the Handler Receives {#handler-data}

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

The example is shown for the `CRM_DEAL_LIST_MENU` placement.

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => da79481fc89aea5a929b58815fbca926
    [AUTH_ID] => 83fd7166007e9c94001e30ba00000001f0f107a52e6ad7ef9a1b8c3d5e2f4061
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 737c9966007e9c94001e30ba00000001f0f1072b8d4e6f01a3c57d9e8b2f4160
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => crm,placement
    [member_id] => d897063e1ce7c5eb9f04b9751eef5915
    [status] => L
    [PLACEMENT] => CRM_DEAL_LIST_MENU
    [PLACEMENT_OPTIONS] => {"ID":"8061","URI":"\/crm\/deal\/list\/"}
)

```

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context.

{% include [Note on required parameters](../../../_includes/required.md) %}

#| 
|| **Parameter** | **Description** ||
|| **ID*** 
[`string`](../../data-types.md) | Identifier of the CRM object for which the widget was opened.

A lead, deal, contact, company, estimate, new invoice, or custom object type item can be retrieved with the [crm.item.get](../../crm/universal/crm-item-get.md) method by passing the corresponding [entityTypeId](../../crm/data-types.md#object_type). For an activity, use [crm.activity.get](../../crm/timeline/activities/activity-base/crm-activity-get.md).

The object type identifier does not arrive as a separate key. For a custom object type, take it from the `PLACEMENT` parameter value: for the `CRM_DYNAMIC_183_LIST_MENU` code, the type identifier is `183`

||
|#

## OPTIONS When Registering via placement.bind

This placement does not support the `OPTIONS` parameters. The values passed are not retained: the [placement.get](../placement-get.md) method returns an empty array for such a registration.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "CRM_DEAL_LIST_MENU",
        "HANDLER": "https://your-domain.com/widgets/crm-list-menu-handler.php",
        "TITLE": "My deal menu item",
        "LANG_ALL": {
          "en": {
            "TITLE": "My deal menu item"
          },
          "de": {
            "TITLE": "Mein Deal-Menüelement"
          }
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
          PLACEMENT: 'CRM_DEAL_LIST_MENU',
          HANDLER: 'https://your-domain.com/widgets/crm-list-menu-handler.php',
          TITLE: 'My deal menu item',
          LANG_ALL: {
            en: {
              TITLE: 'My deal menu item',
            },
            de: {
              TITLE: 'Mein Deal-Menüelement',
            },
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
      async function bindCrmDealListMenu() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'CRM_DEAL_LIST_MENU',
              HANDLER: 'https://your-domain.com/widgets/crm-list-menu-handler.php',
              TITLE: 'My deal menu item',
              LANG_ALL: {
                en: {
                  TITLE: 'My deal menu item',
                },
                de: {
                  TITLE: 'Mein Deal-Menüelement',
                },
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

      document.addEventListener('DOMContentLoaded', bindCrmDealListMenu)
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
                    'PLACEMENT' => 'CRM_DEAL_LIST_MENU',
                    'HANDLER' => 'https://your-domain.com/widgets/crm-list-menu-handler.php',
                    'TITLE' => 'My deal menu item',
                    'LANG_ALL' => [
                        'en' => [
                            'TITLE' => 'My deal menu item',
                        ],
                        'de' => [
                            'TITLE' => 'Mein Deal-Menüelement',
                        ],
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
            PLACEMENT: 'CRM_DEAL_LIST_MENU',
            HANDLER: 'https://your-domain.com/widgets/crm-list-menu-handler.php',
            TITLE: 'My deal menu item',
            LANG_ALL: {
                en: { TITLE: 'My deal menu item' },
                de: { TITLE: 'Mein Deal-Menüelement' }
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
            'PLACEMENT' => 'CRM_DEAL_LIST_MENU',
            'HANDLER' => 'https://your-domain.com/widgets/crm-list-menu-handler.php',
            'TITLE' => 'My deal menu item',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'My deal menu item',
                ],
                'de' => [
                    'TITLE' => 'Mein Deal-Menüelement',
                ],
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
    	"PLACEMENT": "CRM_DEAL_LIST_MENU",
    	"HANDLER":   "https://your-domain.com/widgets/crm-list-menu-handler.php",
    	"TITLE":     "My deal menu item",
    	"LANG_ALL": b24.Params{
    		"en": b24.Params{
    			"TITLE": "My deal menu item",
    		},
    		"de": b24.Params{
    			"TITLE": "Mein Deal-Menüelement",
    		},
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

#|
|| **Mistake** | **Solution** ||
|| `placement.bind` returns `WRONG_AUTH_TYPE` with the description `Application context required` | Register the placement on behalf of an application. A placement cannot be bound with a webhook ||
|| `placement.bind` returns `ERROR_PLACEMENT_NOT_FOUND` | The code is assembled for an object type that this placement does not support, or the application has not been granted the `crm` scope. Check the code against the table at the beginning of the page ||
|| The widget is registered but does not appear in the interface | Complete the [application installation](../../../settings/app-installation/installation-finish.md) and reload the page ||
|| The item is not visible in the context menu of an element | Check the path in the [Where to Find It in the Interface](#interface) section ||
|#

Other registration error codes are listed in the "Possible Error Codes" section of the [placement.bind](../placement-bind.md) page.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./list-toolbar.md)
- [{#T}](./detail-tab.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../bx24-widget-methods.md)