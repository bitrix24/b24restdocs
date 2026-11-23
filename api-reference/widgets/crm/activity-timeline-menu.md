# Context Menu Item for a Timeline Record in CRM_XXX_ACTIVITY_TIMELINE_MENU, CRM_DYNAMIC_XXX_ACTIVITY_TIMELINE_MENU

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement, crm`](../../scopes/permissions.md)

The widget adds an item to the context menu of a timeline record in a CRM object card: a [lead](../../crm/leads/index.md), [deal](../../crm/deals/index.md), [estimate](../../crm/quote/index.md), [new invoice](../../crm/universal/invoice.md), or [custom object type](../../crm/universal/index.md).

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## How the Placement Works

1. The application registers a handler with the [placement.bind](../placement-bind.md) method, passing a code from the [table](#codes) in `PLACEMENT`. The method works only on behalf of an application; the placement cannot be registered with a webhook. A widget can be registered by an administrator or by an employee with the permission to create applications
2. In the web interface, the application item appears in the *Extensions* submenu of a timeline record. Extensions are added only to records that have a context menu
3. The user selects the item, and Bitrix24 opens the handler in a frame with the [call data](#handler-data)
4. The handler can call REST on behalf of the current user with the token from `AUTH_ID`: for example, retrieve the CRM object by `ENTITY_ID` or the associated activity by `ASSOCIATED_ENTITY_ID`

## When to Use

- open external system data for a CRM object by `ENTITY_ID`
- show the details of the associated activity, retrieved with [crm.activity.get](../../crm/timeline/activities/activity-base/crm-activity-get.md), if `ASSOCIATED_ENTITY_TYPE_ID` is `6`
- link a history record to a record in an external system by `TIMELINE_ITEM_ID`, if it is passed

For an action on the card as a whole, use the [top menu item](./detail-toolbar.md); for a button above the timeline, use [CRM_XXX_DETAIL_ACTIVITY](./detail-activity.md).

## Widget Placement {#codes}

#| 
|| **Placement Code** | **Location** ||
|| `CRM_LEAD_ACTIVITY_TIMELINE_MENU` | Context menu item for a timeline record in a [lead](../../crm/leads/index.md) ||
|| `CRM_DEAL_ACTIVITY_TIMELINE_MENU` | Context menu item for a timeline record in a [deal](../../crm/deals/index.md) ||
|| `CRM_QUOTE_ACTIVITY_TIMELINE_MENU` | Context menu item for a timeline record in an [estimate](../../crm/quote/index.md) ||
|| `CRM_SMART_INVOICE_ACTIVITY_TIMELINE_MENU` | Context menu item for a timeline record in a [new invoice](../../crm/universal/invoice.md). The placement is available if new invoices are enabled ||
|| `CRM_DYNAMIC_XXX_ACTIVITY_TIMELINE_MENU` | Context menu item for a timeline record in custom CRM object types. Replace XXX with the numeric identifier of the specific [custom object type](../../crm/universal/index.md). For example, `CRM_DYNAMIC_183_ACTIVITY_TIMELINE_MENU` ||
|#

### Where to Find It in the Interface {#interface}

Open a CRM object card, find a timeline record with a context menu, click *•••* in the lower right corner of the record, and hover over *Extensions*. The application item appears in this submenu.

![Context menu item for a timeline record in the deal card](./_images/CRM_DEAL_ACTIVITY_TIMELINE_MENU.png "Context menu item for a timeline record in the deal card")

## What the Handler Receives {#handler-data}

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

The example is shown for the `CRM_DEAL_ACTIVITY_TIMELINE_MENU` placement on a scheduled activity.

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 4e0ec6b5f934a6af21bd9719f1d5444c
    [AUTH_ID] => b26f7166007e9c94001e30ba00000001f0f1075d0a9b2e73f14c86ad25e0b39
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => a15e9966007e9c94001e30ba00000001f0f10769b3c04d182ea75f0cb384e12
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => crm,placement
    [member_id] => d897063e1ce7c5eb9f04b9751eef5915
    [status] => L
    [PLACEMENT] => CRM_DEAL_ACTIVITY_TIMELINE_MENU
    [PLACEMENT_OPTIONS] => {"ENTITY_ID":"8061","ASSOCIATED_ENTITY_ID":"8097","ASSOCIATED_ENTITY_TYPE_ID":"6","URI":"\/crm\/deal\/details\/8061\/?any=details%2F8061%2F"}
)

```

`APPLICATION_SCOPE` contains the scopes of the example application. The placement itself is registered in the `crm` scope.

After parsing, the `PLACEMENT_OPTIONS` string from this example looks like this:

```json
{
    "ENTITY_ID": "8061",
    "ASSOCIATED_ENTITY_ID": "8097",
    "ASSOCIATED_ENTITY_TYPE_ID": "6",
    "URI": "/crm/deal/details/8061/?any=details%2F8061%2F"
}
```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The context always contains the CRM object identifier in `ENTITY_ID`. The set of the remaining keys depends on the timeline record:

- for a scheduled activity, `ASSOCIATED_ENTITY_ID` and `ASSOCIATED_ENTITY_TYPE_ID` may be passed, as in the example above
- for a configurable history record, the context may also contain `TYPE_ID`, `TYPE_CATEGORY_ID`, and `TIMELINE_ITEM_ID`

Check that a key is present before using it. Keys without a value are not included in the context.

#| 
|| **Parameter** | **Description** ||
|| **ENTITY_ID*** 
[`string`](../../data-types.md) | The identifier of the CRM object for which the widget was opened.

To retrieve the object data, use the [crm.item.get](../../crm/universal/crm-item-get.md) method with the object identifier and its [type](../../crm/data-types.md#object_type).

The object type identifier does not arrive as a separate key. For a custom object type, take it from the `PLACEMENT` parameter value: for the `CRM_DYNAMIC_183_ACTIVITY_TIMELINE_MENU` code, the type identifier is `183`

||
|| **ASSOCIATED_ENTITY_ID**
[`string`](../../data-types.md) | The identifier of the object associated with the timeline record. If it is an activity, retrieve it with the [crm.activity.get](../../crm/timeline/activities/activity-base/crm-activity-get.md) method

||
|| **ASSOCIATED_ENTITY_TYPE_ID**
[`string`](../../data-types.md) | The identifier of the [associated object type](../../crm/data-types.md#object_type). For an activity, the value is `6`

||
|| **TYPE_ID**
[`string`](../../data-types.md) | The type of the history record in the timeline. Not passed for a scheduled activity

||
|| **TYPE_CATEGORY_ID**
[`string`](../../data-types.md) | The category of the history record in the timeline. Not passed for a scheduled activity

||
|| **TIMELINE_ITEM_ID**
[`string`](../../data-types.md) | The identifier of the history record in the timeline. Not passed for a scheduled activity

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
        "PLACEMENT": "CRM_DEAL_ACTIVITY_TIMELINE_MENU",
        "HANDLER": "https://your-domain.com/widgets/crm-timeline-menu-handler.php",
        "TITLE": "My activity menu item",
        "LANG_ALL": {
          "en": {
            "TITLE": "My activity menu item"
          },
          "de": {
            "TITLE": "Mein Aktivitäten-Menüelement"
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
          PLACEMENT: 'CRM_DEAL_ACTIVITY_TIMELINE_MENU',
          HANDLER: 'https://your-domain.com/widgets/crm-timeline-menu-handler.php',
          TITLE: 'My activity menu item',
          LANG_ALL: {
            en: {
              TITLE: 'My activity menu item',
            },
            de: {
              TITLE: 'Mein Aktivitäten-Menüelement',
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
      async function bindCrmDealActivityTimelineMenu() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'CRM_DEAL_ACTIVITY_TIMELINE_MENU',
              HANDLER: 'https://your-domain.com/widgets/crm-timeline-menu-handler.php',
              TITLE: 'My activity menu item',
              LANG_ALL: {
                en: {
                  TITLE: 'My activity menu item',
                },
                de: {
                  TITLE: 'Mein Aktivitäten-Menüelement',
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

      document.addEventListener('DOMContentLoaded', bindCrmDealActivityTimelineMenu)
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
                    'PLACEMENT' => 'CRM_DEAL_ACTIVITY_TIMELINE_MENU',
                    'HANDLER' => 'https://your-domain.com/widgets/crm-timeline-menu-handler.php',
                    'TITLE' => 'My activity menu item',
                    'LANG_ALL' => [
                        'en' => [
                            'TITLE' => 'My activity menu item',
                        ],
                        'de' => [
                            'TITLE' => 'Mein Aktivitäten-Menüelement',
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
            PLACEMENT: 'CRM_DEAL_ACTIVITY_TIMELINE_MENU',
            HANDLER: 'https://your-domain.com/widgets/crm-timeline-menu-handler.php',
            TITLE: 'My activity menu item',
            LANG_ALL: {
                en: { TITLE: 'My activity menu item' },
                de: { TITLE: 'Mein Aktivitäten-Menüelement' }
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
            'PLACEMENT' => 'CRM_DEAL_ACTIVITY_TIMELINE_MENU',
            'HANDLER' => 'https://your-domain.com/widgets/crm-timeline-menu-handler.php',
            'TITLE' => 'My activity menu item',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'My activity menu item',
                ],
                'de' => [
                    'TITLE' => 'Mein Aktivitäten-Menüelement',
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
    	"PLACEMENT": "CRM_DEAL_ACTIVITY_TIMELINE_MENU",
    	"HANDLER":   "https://your-domain.com/widgets/crm-timeline-menu-handler.php",
    	"TITLE":     "My activity menu item",
    	"LANG_ALL": b24.Params{
    		"en": b24.Params{
    			"TITLE": "My activity menu item",
    		},
    		"de": b24.Params{
    			"TITLE": "Mein Aktivitäten-Menüelement",
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
|#

Other registration error codes are listed in the "Possible Error Codes" section of the [placement.bind](../placement-bind.md) page.

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./detail-activity.md)
- [{#T}](./detail-tab.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../bx24-widget-methods.md)