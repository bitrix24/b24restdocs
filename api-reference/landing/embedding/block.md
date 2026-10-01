# Block Editing Item LANDING_BLOCK_<CODE> and LANDING_BLOCK_*

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)

The `LANDING_BLOCK_<CODE>` and `LANDING_BLOCK_*` widgets add an application item to the block actions menu in the page editor.

Use them when the application works with a single block rather than the whole page. For example, it:

- translates the block text and writes the translation back
- checks an image link in the block

If the action applies to the whole site or page, for example, checks meta tags, use [LANDING_SETTINGS](./settings.md).

The embedding location is registered with the [landing.repo.bind](./landing-repo-bind.md) method, not [placement.bind](../../widgets/placement-bind.md). The method works only in the application context; you cannot register the embedding location via a webhook.

{% note info "" %}

The embedding location will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Integrated

#| 
|| **Embedding Location Code** | **Location** ||
|| `LANDING_BLOCK_<CODE>` | Item in the actions menu of blocks of one kind.

For a standard block, `<CODE>` is its symbolic code, for example, `04.1.one_col_fix_with_title`. The [landing.block.getrepository](../block/methods/landing-block-get-repository.md) method returns the codes of standard blocks.

For a block that the application registered with the [landing.repo.register](../user-blocks/landing-repo-register.md) method, `<CODE>` is `repo_<ID>`, where `<ID>` is the block identifier in the repository from the method response. For example, `LANDING_BLOCK_repo_1132` ||
|| `LANDING_BLOCK_*` | Item in the actions menu of all blocks ||
|#

The case of the characters in the code does not matter.

### Where to Find It in the Interface

Open the page in edit mode and hover over the block. A *More* button appears to the right of the *Edit* button — the application item is in its dropdown list. The item label is the `TITLE` value from the registration or, if it is empty, the application name. If the application registered both `<CODE>` and `*` for a block, the list shows both items.

The item is visible to everyone who opens the page in the editor: permissions to modify the block are not checked when the item is displayed. If the application action modifies the block, check permissions in the handler.

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
    [PLACEMENT] => LANDING_BLOCK_*
    [PLACEMENT_OPTIONS] => {"ID":"996","CODE":"43.4.cover_with_price_text_button_bgimg","LID":"30","URI":"\/sites\/site\/12\/view\/30\/"}
)
```

{% include [Note on required parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../../widgets/_includes/widget_data.md) %}

The part of the code after `LANDING_BLOCK_` in the `PLACEMENT` parameter arrives in lowercase: `LANDING_BLOCK_*`, `LANDING_BLOCK_04.1.one_col_fix_with_title`, or `LANDING_BLOCK_repo_1132`.

### PLACEMENT_OPTIONS {#placement-options}

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

For `LANDING_BLOCK_<CODE>` and `LANDING_BLOCK_*`, the same keys are passed in the context:

#|
|| **Key**
`type` | **Description** ||
|| **ID**
[`string`](../../data-types.md) | Block identifier on the page. It is accepted by the methods of the [Blocks](../block/index.md) section, for example, [landing.block.getbyid](../block/methods/landing-block-get-by-id.md). This is not the block identifier in the repository from the `LANDING_BLOCK_repo_<ID>` code ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the block, for example, `04.1.one_col_fix_with_title` or `repo_1132` ||
|| **LID**
[`string`](../../data-types.md) | Identifier of the page where the block is opened ||
|| **URI**
[`string`](../../data-types.md) | Path with the query string of the page from which the widget is opened. If the page address cannot be determined, the key is not passed ||
|#

## Code Examples

{% include [Note on Examples](../../../_includes/examples.md) %}

The examples register an item for the standard block `04.1.one_col_fix_with_title`. To make the item appear for all blocks, pass `LANDING_BLOCK_*` in `PLACEMENT`; the other fields stay the same.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "fields": {
          "PLACEMENT": "LANDING_BLOCK_04.1.one_col_fix_with_title",
          "PLACEMENT_HANDLER": "https://your-domain.com/widgets/landing-block-handler.php",
          "TITLE": "My Widget for the Block"
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
            PLACEMENT: 'LANDING_BLOCK_04.1.one_col_fix_with_title',
            PLACEMENT_HANDLER: 'https://your-domain.com/widgets/landing-block-handler.php',
            TITLE: 'My Widget for the Block',
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Binding result:', result)
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
      async function bindLandingBlock() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.repo.bind',
            params: {
              fields: {
                PLACEMENT: 'LANDING_BLOCK_04.1.one_col_fix_with_title',
                PLACEMENT_HANDLER: 'https://your-domain.com/widgets/landing-block-handler.php',
                TITLE: 'My Widget for the Block',
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
          console.info('Binding result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindLandingBlock)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "PLACEMENT": "LANDING_BLOCK_04.1.one_col_fix_with_title",
        "PLACEMENT_HANDLER": "https://your-domain.com/widgets/landing-block-handler.php",
        "TITLE": "My Widget for the Block",
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
                        'PLACEMENT' => 'LANDING_BLOCK_04.1.one_col_fix_with_title',
                        'PLACEMENT_HANDLER' => 'https://your-domain.com/widgets/landing-block-handler.php',
                        'TITLE' => 'My Widget for the Block',
                    ],
                ]
            );

        $result = $response->getResponseData()->getResult();
        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error binding landing block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.bind',
        {
            fields: {
                PLACEMENT: 'LANDING_BLOCK_04.1.one_col_fix_with_title',
                PLACEMENT_HANDLER: 'https://your-domain.com/widgets/landing-block-handler.php',
                TITLE: 'My Widget for the Block'
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
                'PLACEMENT' => 'LANDING_BLOCK_04.1.one_col_fix_with_title',
                'PLACEMENT_HANDLER' => 'https://your-domain.com/widgets/landing-block-handler.php',
                'TITLE' => 'My Widget for the Block',
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
    		"PLACEMENT":         "LANDING_BLOCK_04.1.one_col_fix_with_title",
    		"PLACEMENT_HANDLER": "https://your-domain.com/widgets/landing-block-handler.php",
    		"TITLE":             "My Widget for the Block",
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

## How to Update a Block from the Application {#refresh-block}

If the application has changed the block content, for example, with the [landing.block.updatenodes](../block/methods/landing-block-update-nodes.md) method, the editor keeps showing the old version. To redraw the block, call the `refreshBlock` command of the [BX24.placement.call](../../widgets/ui-interaction/bx24-placement-call.md) method from the widget frame.

The command works only inside an open `LANDING_BLOCK_<CODE>` or `LANDING_BLOCK_*` widget: it is executed by the page editor, and there is no separate REST method for it.

Pass `id` to the command — the block identifier on the page, as a string or a number. Take it from the `ID` key in [PLACEMENT_OPTIONS](#placement-options).

The callback function fires after the editor reloads the block. If there is no block with this `id` on the open page, the command does nothing and the callback function is not called.

{% list tabs %}

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Block ID from PLACEMENT_OPTIONS.ID
    const options = $b24.placement.options as { ID: string }

    await $b24.placement.call('refreshBlock', { id: Number(options.ID) })
    console.info('Block refreshed')
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        // Block ID from PLACEMENT_OPTIONS.ID
        const blockId = Number($b24.placement.options.ID)

        await $b24.placement.call('refreshBlock', { id: blockId })
        console.info('Block refreshed')
      })
    </script>
    ```

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            // Block ID from PLACEMENT_OPTIONS.ID
            var blockId = Number(BX24.placement.info().options.ID);

            BX24.placement.call('refreshBlock', { id: blockId }, function () {
                console.log('Block refreshed');
            });
        });
    });
    ```

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./landing-repo-bind.md)
- [{#T}](./landing-repo-unbind.md)
- [{#T}](./settings.md)
- [{#T}](../block/methods/landing-block-update-nodes.md)
- [{#T}](../block/methods/landing-block-get-repository.md)
- [{#T}](../user-blocks/landing-repo-register.md)
- [{#T}](../../widgets/ui-interaction/bx24-placement-call.md)
- [{#T}](../../widgets/bx24-widget-methods.md)
