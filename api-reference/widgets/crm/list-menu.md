# Context Menu Item in the CRM_XXX_LIST_MENU, CRM_DYNAMIC_XXX_LIST_MENU

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement, crm`](../../scopes/permissions.md)

The widget adds its own item to the context menu of an element in the list of CRM objects: [leads](../../crm/leads/index.md), [contacts](../../crm/contacts/index.md), [companies](../../crm/companies/index.md), [deals](../../crm/deals/index.md), [estimates](../../crm/quote/index.md), [new invoices](../../crm/universal/invoice.md), [activities](../../crm/timeline/activities/index.md), and [custom object types](../../crm/universal/index.md).

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

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

### Where to Find It in the Interface

Open a CRM section in list view, click the menu button to the left of an element, and hover over *Bitrix24 Market*. The application item appears in this submenu.

![Context menu item in the deal list](./_images/CRM_DEAL_LIST_MENU.png "Context menu item in the deal list")

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

The example is shown for the `CRM_DEAL_LIST_MENU` placement. Other codes send the same set of data: the `PLACEMENT` value and the object identifier in `PLACEMENT_OPTIONS` change.

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

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context. Along with the universal `URI` key, the context carries the placement’s own key.

{% include [Note on required parameters](../../../_includes/required.md) %}

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
- activity [crm.activity.get](../../crm/timeline/activities/activity-base/crm-activity-get.md)

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
    		"ru": b24.Params{
    			"TITLE": "My deal menu item",
    		},
    		"en": b24.Params{
    			"TITLE": "My deal menu item",
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
|| The item is not visible in the context menu of an element | Open the CRM section in list view and hover over the *Bitrix24 Market* item: the application item is displayed in this submenu ||
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