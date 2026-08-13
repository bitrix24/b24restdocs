# Button in the Automation Rules Designer CRM_XXX_ROBOT_DESIGNER_TOOLBAR

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement, crm`](../../scopes/permissions.md)

The widget adds its own button to the automation rules designer, where the automation of CRM objects is configured: [leads](../../crm/leads/index.md), [deals](../../crm/deals/index.md), [new invoices](../../crm/universal/invoice.md), and [custom object types](../../crm/universal/index.md).

The placement code is specified in the `PLACEMENT` parameter of the [placement.bind](../placement-bind.md) method.

{% note info "" %}

The widget is not displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#| 
|| **Placement Code** | **Location** ||
|| `CRM_LEAD_ROBOT_DESIGNER_TOOLBAR` | Dropdown menu item of the top button in the [lead](../../crm/leads/index.md) ||
|| `CRM_DEAL_ROBOT_DESIGNER_TOOLBAR` | Dropdown menu item of the top button in the [deal](../../crm/deals/index.md) ||
|| `CRM_SMART_INVOICE_ROBOT_DESIGNER_TOOLBAR` | Dropdown menu item of the top button in the [new invoices](../../crm/universal/invoice.md) ||
|| `CRM_DYNAMIC_XXX_ROBOT_DESIGNER_TOOLBAR` | Button in the automation rules designer of a custom CRM object type. Replace XXX with the numeric identifier of the specific [custom entity type](../../crm/universal/index.md). For example, `CRM_DYNAMIC_183_ROBOT_DESIGNER_TOOLBAR` ||
|#

### Where to Find It in the Interface

Open a CRM list and click *Automation rules*. The application button appears on the right in the header of the *Sales automation* window.

Do not confuse this placement with [`TASK_ROBOT_DESIGNER_TOOLBAR`](../task/robot-designer-toolbar.md) and [`SONET_GROUP_ROBOT_DESIGNER_TOOLBAR`](../workgroups/robot-designer-toolbar.md): those belong to task automation and require different scopes.

![Button in the deal automation rules designer](./_images/CRM_DEAL_ROBOT_DESIGNER_TOOLBAR.png "Button in the deal automation rules designer")

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

The example is shown for the `CRM_DEAL_ROBOT_DESIGNER_TOOLBAR` placement. Other codes send the same set of data: only the `PLACEMENT` value changes.

```php

Array
(
    [DOMAIN] => xxx.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => en
    [APP_SID] => 5f23711fbf9257553e256254309cc1f5
    [AUTH_ID] => c3807166007e9c94001e30ba00000001f0f1076e1b0c3f84a25d97be4610c72
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => b26f9966007e9c94001e30ba00000001f0f1077ac4d15e293fb680da1c295f3
    [SERVER_ENDPOINT] => https://oauth.bitrix.info/rest/
    [APPLICATION_TOKEN] => ec1b2074a9d3f5c81b6e40d27a95cf38
    [APPLICATION_SCOPE] => crm,bizproc,placement
    [member_id] => d897063e1ce7c5eb9f04b9751eef5915
    [status] => L
    [PLACEMENT] => CRM_DEAL_ROBOT_DESIGNER_TOOLBAR
    [PLACEMENT_OPTIONS] => {"URI":"\/crm\/deal\/automation\/0\/"}
)

```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../_includes/widget_data.md) %}

### PLACEMENT_OPTIONS

The `PLACEMENT_OPTIONS` value is passed as a JSON string with the call context.

The placement has no keys of its own — the context carries only the universal `URI` key. The pipeline identifier does not arrive as a separate parameter, but it can be taken from the path in `URI`. For example, for the `/crm/deal/automation/0/` value the pipeline identifier is `0`.

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
        "PLACEMENT": "CRM_DEAL_ROBOT_DESIGNER_TOOLBAR",
        "HANDLER": "https://your-domain.com/widgets/crm-robot-designer-handler.php",
        "TITLE": "My automation designer button",
        "LANG_ALL": {
          "en": {
            "TITLE": "My automation designer button"
          },
          "de": {
            "TITLE": "Meine Automatisierungs-Schaltfläche"
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
          PLACEMENT: 'CRM_DEAL_ROBOT_DESIGNER_TOOLBAR',
          HANDLER: 'https://your-domain.com/widgets/crm-robot-designer-handler.php',
          TITLE: 'My automation designer button',
          LANG_ALL: {
            en: {
              TITLE: 'My automation designer button',
            },
            de: {
              TITLE: 'Meine Automatisierungs-Schaltfläche',
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
      async function bindCrmDealRobotDesignerToolbar() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'CRM_DEAL_ROBOT_DESIGNER_TOOLBAR',
              HANDLER: 'https://your-domain.com/widgets/crm-robot-designer-handler.php',
              TITLE: 'My automation designer button',
              LANG_ALL: {
                en: {
                  TITLE: 'My automation designer button',
                },
                de: {
                  TITLE: 'Meine Automatisierungs-Schaltfläche',
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

      document.addEventListener('DOMContentLoaded', bindCrmDealRobotDesignerToolbar)
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
                    'PLACEMENT' => 'CRM_DEAL_ROBOT_DESIGNER_TOOLBAR',
                    'HANDLER' => 'https://your-domain.com/widgets/crm-robot-designer-handler.php',
                    'TITLE' => 'My automation designer button',
                    'LANG_ALL' => [
                        'en' => [
                            'TITLE' => 'My automation designer button',
                        ],
                        'de' => [
                            'TITLE' => 'Meine Automatisierungs-Schaltfläche',
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
            PLACEMENT: 'CRM_DEAL_ROBOT_DESIGNER_TOOLBAR',
            HANDLER: 'https://your-domain.com/widgets/crm-robot-designer-handler.php',
            TITLE: 'My automation designer button',
            LANG_ALL: {
                en: { TITLE: 'My automation designer button' },
                de: { TITLE: 'Meine Automatisierungs-Schaltfläche' }
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
            'PLACEMENT' => 'CRM_DEAL_ROBOT_DESIGNER_TOOLBAR',
            'HANDLER' => 'https://your-domain.com/widgets/crm-robot-designer-handler.php',
            'TITLE' => 'My automation designer button',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'My automation designer button',
                ],
                'de' => [
                    'TITLE' => 'Meine Automatisierungs-Schaltfläche',
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
    	"PLACEMENT": "CRM_DEAL_ROBOT_DESIGNER_TOOLBAR",
    	"HANDLER":   "https://your-domain.com/widgets/crm-robot-designer-handler.php",
    	"TITLE":     "My automation designer button",
    	"LANG_ALL": b24.Params{
    		"ru": b24.Params{
    			"TITLE": "My automation designer button",
    		},
    		"en": b24.Params{
    			"TITLE": "My automation designer button",
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
|| The `TASK_ROBOT_DESIGNER_TOOLBAR` or `SONET_GROUP_ROBOT_DESIGNER_TOOLBAR` code is registered | These placements belong to task and workgroup automation and require different scopes. For CRM, take the code from the table at the beginning of the page ||
|| The widget is registered but does not appear in the interface | Complete the [application installation](../../../settings/app-installation/installation-finish.md) and reload the page ||
|#

Other registration error codes are listed in the "Possible Error Codes" section of the [placement.bind](../placement-bind.md) page.

## Continue Your Exploration

- [{#T}](./index.md)
- [{#T}](./funnels-toolbar.md)
- [{#T}](./list-toolbar.md)
- [{#T}](../placement-bind.md)
- [{#T}](../ui-interaction/index.md)
- [{#T}](../../../settings/interactivity/index.md)
- [{#T}](../bx24-widget-methods.md)