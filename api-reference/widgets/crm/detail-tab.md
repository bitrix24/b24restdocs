# Tab in the Detail Form of CRM Entity CRM_XXX_DETAIL_TAB, CRM_DYNAMIC_XXX_DETAIL_TAB

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement, crm`](../../scopes/permissions.md)

The widget adds its own tab to the card of a CRM object: a [lead](../../crm/leads/index.md), [contact](../../crm/contacts/index.md), [company](../../crm/companies/index.md), [deal](../../crm/deals/index.md), [estimate](../../crm/quote/index.md), [new invoice](../../crm/universal/invoice.md), [order](../../sale/order/index.md), or [custom object type](../../crm/universal/index.md).

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#| 
|| **Placement Code** | **Location** ||
|| `CRM_LEAD_DETAIL_TAB` | Tab in the [lead](../../crm/leads/index.md) detail form ||
|| `CRM_DEAL_DETAIL_TAB` | Tab in the [deal](../../crm/deals/index.md) detail form ||
|| `CRM_CONTACT_DETAIL_TAB` | Tab in the [contact](../../crm/contacts/index.md) detail form ||
|| `CRM_COMPANY_DETAIL_TAB` | Tab in the [company](../../crm/companies/index.md) detail form ||
|| `CRM_QUOTE_DETAIL_TAB` | Tab in the [estimate](../../crm/quote/index.md) detail form ||
|| `CRM_SMART_INVOICE_DETAIL_TAB` | Tab in the [invoice](../../crm/universal/invoice.md) detail form ||
|| `CRM_ORDER_DETAIL_TAB` | Tab in the [online store order](../../sale/order/index.md) card ||
|| `CRM_DYNAMIC_XXX_DETAIL_TAB` | Tab in the detail form of a custom object type in CRM. Replace XXX with the numeric identifier of the specific [custom object type](../../crm/universal/index.md). For example, `CRM_DYNAMIC_183_DETAIL_TAB` ||
|#

### Where to Find It in the Interface

Open a CRM object card. The application tab appears in the card tab row. If there are more tabs than fit in the row, it moves under *More*.

![Tab in the deal card](./_images/CRM_DEAL_DETAIL_TAB.png "Tab in the deal card")

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

The example is shown for the `CRM_DEAL_DETAIL_TAB` placement. Other codes send the same set of data: the `PLACEMENT` value and the object identifier in `PLACEMENT_OPTIONS` change.

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 5552d735db7b7b4d5c16dd9c272bfe7d
    [AUTH_ID] => 9d4c7166007e9c94001e30ba00000001f0f107e28b5a4310c7f6d9b3025ea814
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 8c3b9966007e9c94001e30ba00000001f0f107f19c6b3e04d182ac5b73f9052d
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => crm,placement
    [member_id] => d897063e1ce7c5eb9f04b9751eef5915
    [status] => L
    [PLACEMENT] => CRM_DEAL_DETAIL_TAB
    [PLACEMENT_OPTIONS] => {"ID":"8061","URI":"\/crm\/deal\/details\/8061\/?any=details%2F8061%2F&IFRAME=Y&IFRAME_TYPE=SIDE_SLIDER"}
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

- any entity type [crm.item.get](../../crm/universal/crm-item-get.md) specifying entityTypeId = '1' for leads, '2' for deals, and [etc.](../../crm/data-types.md#object_type)
- lead [crm.lead.get](../../crm/leads/crm-lead-get.md)
- deal [crm.deal.get](../../crm/deals/crm-deal-get.md)
- contact [crm.contact.get](../../crm/contacts/crm-contact-get.md)
- company [crm.company.get](../../crm/companies/crm-company-get.md)
- estimate [crm.quote.get](../../crm/quote/crm-quote-get.md)

The object type identifier does not arrive as a separate key. For a custom object type, take it from the `PLACEMENT` parameter value: for the `CRM_DYNAMIC_183_DETAIL_TAB` code, the type identifier is `183`

||
|#

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)

{% endnote %}

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "PLACEMENT": "CRM_DEAL_DETAIL_TAB",
        "HANDLER": "https://your-domain.com/widgets/crm-detail-tab-handler.php",
        "TITLE": "My deal tab",
        "LANG_ALL": {
          "en": {
            "TITLE": "My deal tab"
          },
          "de": {
            "TITLE": "Mein Deal-Tab"
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
          PLACEMENT: 'CRM_DEAL_DETAIL_TAB',
          HANDLER: 'https://your-domain.com/widgets/crm-detail-tab-handler.php',
          TITLE: 'My deal tab',
          LANG_ALL: {
            en: {
              TITLE: 'My deal tab',
            },
            de: {
              TITLE: 'Mein Deal-Tab',
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
      async function bindCrmDealDetailTab() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'CRM_DEAL_DETAIL_TAB',
              HANDLER: 'https://your-domain.com/widgets/crm-detail-tab-handler.php',
              TITLE: 'My deal tab',
              LANG_ALL: {
                en: {
                  TITLE: 'My deal tab',
                },
                de: {
                  TITLE: 'Mein Deal-Tab',
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

      document.addEventListener('DOMContentLoaded', bindCrmDealDetailTab)
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
                    'PLACEMENT' => 'CRM_DEAL_DETAIL_TAB',
                    'HANDLER' => 'https://your-domain.com/widgets/crm-detail-tab-handler.php',
                    'TITLE' => 'My deal tab',
                    'LANG_ALL' => [
                        'en' => [
                            'TITLE' => 'My deal tab',
                        ],
                        'de' => [
                            'TITLE' => 'Mein Deal-Tab',
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
            PLACEMENT: 'CRM_DEAL_DETAIL_TAB',
            HANDLER: 'https://your-domain.com/widgets/crm-detail-tab-handler.php',
            TITLE: 'My deal tab',
            LANG_ALL: {
                en: { TITLE: 'My deal tab' },
                de: { TITLE: 'Mein Deal-Tab' }
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
            'PLACEMENT' => 'CRM_DEAL_DETAIL_TAB',
            'HANDLER' => 'https://your-domain.com/widgets/crm-detail-tab-handler.php',
            'TITLE' => 'My deal tab',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'My deal tab',
                ],
                'de' => [
                    'TITLE' => 'Mein Deal-Tab',
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
    	"PLACEMENT": "CRM_DEAL_DETAIL_TAB",
    	"HANDLER":   "https://your-domain.com/widgets/crm-detail-tab-handler.php",
    	"TITLE":     "My deal tab",
    	"LANG_ALL": b24.Params{
    		"ru": b24.Params{
    			"TITLE": "My deal tab",
    		},
    		"en": b24.Params{
    			"TITLE": "My deal tab",
    		},
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

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./detail-toolbar.md)
- [{#T}](./detail-activity.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../bx24-widget-methods.md)