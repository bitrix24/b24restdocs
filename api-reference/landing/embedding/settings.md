# Menu Item in Site Settings and LANDING_SETTINGS Page

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)

The `LANDING_SETTINGS` widget adds an application item to the site or page settings menu in edit mode.

Use it when the application works with an entire site or page. For example, when it:

- checks the page meta tags and headings before publishing
- sends the page for external moderation or approval

If the action applies to a single block, use [LANDING_BLOCK_<CODE> or LANDING_BLOCK_*](./block.md).

The embedding location is registered with the [landing.repo.bind](./landing-repo-bind.md) method, not [placement.bind](../../widgets/placement-bind.md). The method works only in the application context; the embedding location cannot be registered through a webhook.

{% note info "" %}

The embedding location will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#| 
|| **Embedding Location Code** | **Location** ||
|| `LANDING_SETTINGS` | Item in the site or page settings menu ||
|#

### Where to Find It in the Interface

Open the site or page in edit mode. In the upper right corner, go to *Site Capabilities > Settings*. Application items appear at the end of the left slider menu, with newer items above older ones. The item label is the `TITLE` value from the registration: if it is empty, the item appears without a label, and the application name is not substituted.

Site settings and page settings open in the same slider, so the application has one item for both sections. The item is visible to everyone who opens the settings slider, even without permission to change the settings.

Application items are not displayed in the settings of the Main Page, a site of type `VIBE`.

## What the Handler Receives

Data is sent in a POST request: some parameters come in the handler URL query string, the rest in the request body {.b24-info}

```php
Array
(
    [DOMAIN] => example.bitrix24.com
    [PROTOCOL] => 1
    [LANG] => de
    [APP_SID] => 0123456789abcdef0123456789abcdef
    [APPLICATION_SCOPE] => crm,placement,landing
    [APPLICATION_TOKEN] => xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    [AUTH_ID] => 6061e72600631fcd00005a4b00000001f0f1076700000000f69dd5fc643d9ce2fdbc1
    [AUTH_EXPIRES] => 3600
    [REFRESH_ID] => 50e00aa340631fcd00005a4b00000001f0f1071111116580a5b83c2de639ef28c12
    [SERVER_ENDPOINT] => https://oauth.bitrix24.info/rest/
    [member_id] => abcdef1234567890abcdef1234567890
    [status] => F
    [PLACEMENT] => LANDING_SETTINGS
    [PLACEMENT_OPTIONS] => {"SITE_ID":"12","LID":"30"}
)
```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../../widgets/_includes/widget_data.md) %}

### PLACEMENT_OPTIONS {#placement-options}

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

For `LANDING_SETTINGS`, the following keys are passed in the context:

#| 
|| **Key**
`type` | **Description** ||
|| **SITE_ID**
[`string`](../../data-types.md) | Identifier of the site whose settings the widget is opened in ||
|| **LID**
[`string`](../../data-types.md) | Identifier of the page whose editor the settings were opened from. If the settings were opened without reference to a page, `0` is passed ||
|| **URI**
[`string`](../../data-types.md) | Path with the query string of the page from which the widget was opened. If the page address cannot be determined, the key is not passed ||
|#

The handler retrieves site and page data by the identifiers from `PLACEMENT_OPTIONS`:

- the site — with the [landing.site.getList](../site/landing-site-get-list.md) method filtered by `ID`, and its additional fields — with the [landing.site.getadditionalfields](../site/landing-site-get-additional-fields.md) method
- the page — with the [landing.landing.getList](../page/methods/landing-landing-get-list.md) method filtered by `ID`, and its meta tags and other additional fields — with the [landing.landing.getadditionalfields](../page/methods/landing-landing-get-additional-fields.md) method

If the settings are opened for a Knowledge Base or a group site, pass the `scope` parameter to these methods, otherwise they will not find the site. The values are described in [Working with Site Types and Scopes](../types.md).

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "fields": {
          "PLACEMENT": "LANDING_SETTINGS",
          "PLACEMENT_HANDLER": "https://your-domain.com/widgets/landing-settings-handler.php",
          "TITLE": "My Settings"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/landing.repo.bind
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
        method: 'landing.repo.bind',
        params: {
          fields: {
            PLACEMENT: 'LANDING_SETTINGS',
            PLACEMENT_HANDLER: 'https://your-domain.com/widgets/landing-settings-handler.php',
            TITLE: 'My Settings',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Landing settings bound:', result)
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
      async function bindLandingSettings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.repo.bind',
            params: {
              fields: {
                PLACEMENT: 'LANDING_SETTINGS',
                PLACEMENT_HANDLER: 'https://your-domain.com/widgets/landing-settings-handler.php',
                TITLE: 'My Settings',
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
          console.info('Landing settings bound:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindLandingSettings)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "PLACEMENT": "LANDING_SETTINGS",
        "PLACEMENT_HANDLER": "https://your-domain.com/widgets/landing-settings-handler.php",
        "TITLE": "My settings",
    }

    try:
        bitrix_response = client.landing.repo.bind(fields=fields).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.repo.bind',
                [
                    'fields' => [
                        'PLACEMENT' => 'LANDING_SETTINGS',
                        'PLACEMENT_HANDLER' => 'https://your-domain.com/widgets/landing-settings-handler.php',
                        'TITLE' => 'My Settings',
                    ],
                ]
            );

        $result = $response->getResponseData()->getResult();
        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error binding landing settings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.bind',
        {
            fields: {
                PLACEMENT: 'LANDING_SETTINGS',
                PLACEMENT_HANDLER: 'https://your-domain.com/widgets/landing-settings-handler.php',
                TITLE: 'My Settings'
            }
        },
        function(result)
        {
            if (result.error()) {
                console.error(result.error());
            } else {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.repo.bind',
        [
            'fields' => [
                'PLACEMENT' => 'LANDING_SETTINGS',
                'PLACEMENT_HANDLER' => 'https://your-domain.com/widgets/landing-settings-handler.php',
                'TITLE' => 'My Settings',
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
    res, err := client.Core().Call(ctx, "landing.repo.bind", b24.Params{
    	"fields": b24.Params{
    		"PLACEMENT":         "LANDING_SETTINGS",
    		"PLACEMENT_HANDLER": "https://your-domain.com/widgets/landing-settings-handler.php",
    		"TITLE":             "My Settings",
    	},
    })
    if err != nil {
    	return fmt.Errorf("landing.repo.bind: %w", err)
    }

    // The response arrives as json.RawMessage. On success, result = true.
    // The response shape is described on the landing.repo.bind method page.
    fmt.Printf("%s\n", res.Result)
    ```

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./landing-repo-bind.md)
- [{#T}](./landing-repo-unbind.md)
- [{#T}](../page/methods/landing-landing-get-additional-fields.md)
- [{#T}](../site/landing-site-get-additional-fields.md)
- [{#T}](./block.md)
- [{#T}](../../widgets/bx24-widget-methods.md)
