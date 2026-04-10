# Editing the LANDING_BLOCK_* Integration Point

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)

The widget `LANDING_BLOCK_<CODE>` adds an application item next to the block editing actions in the page editor.

For integration within the `landing` section, the internal method of the `landing.repo.bind` module is used instead of [placement.bind](../../widgets/placement-bind.md).

The integration code depends on the block code and is specified in the format `LANDING_BLOCK_<CODE>`.

If you need to integrate the application into a custom block registered by you, specify the block identifier instead of `<CODE>`. For example, for a block with the identifier `1132`, use the code `LANDING_BLOCK_repo_1132`. The case of the characters in the code is not important.

{% note info "" %}

The integration will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Integrated

#| 
|| **Integration Code** | **Location** ||
|| `LANDING_BLOCK_<CODE>` | Edit item for a specific block ||
|| `LANDING_BLOCK_*` | Edit item for all blocks ||
|#

### Where to Find It in the Interface

Open the page in edit mode and hover over the block. In the standard block actions, to the right of the *Edit* button, there is a *More* button. The application item with `PLACEMENT=LANDING_BLOCK_<CODE>` or `PLACEMENT=LANDING_BLOCK_*` appears in the dropdown list of the *More* button.

## What the Handler Receives

Data is transmitted as a POST request {.b24-info}

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
    [PLACEMENT_OPTIONS] => {"ID":"996","CODE":"43.4.cover_with_price_text_button_bgimg","LID":"30"}
)
```

{% include [Note on Required Parameters](../../../_includes/required.md) %}

{% include notitle [Description of Standard Data](../../widgets/_includes/widget_data.md) %}

### Additional Data

#| 
|| **Parameter** `type` | **Description** ||
|| **APPLICATION_SCOPE** [`string`](../../data-types.md) | List of scopes available to the application ||
|| **APPLICATION_TOKEN** [`string`](../../data-types.md) | Application token for secure event handling ||
|| **SERVER_ENDPOINT** [`string`](../../data-types.md) | Bitrix24 authorization server address needed for updating OAuth 2.0 tokens ||
|#

### PLACEMENT_OPTIONS

The value of `PLACEMENT_OPTIONS` is passed as a JSON string with the context of the call.

For `LANDING_BLOCK_<CODE>`, the following keys are passed in the context:

- `ID` — block identifier
- `CODE` — symbolic code of the block
- `LID` — identifier of the page where the block is opened

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
          "PLACEMENT": "LANDING_BLOCK_04.1.one_col_fix_with_title",
          "PLACEMENT_HANDLER": "https://your-domain.com/widgets/landing-block-handler.php",
          "TITLE": "My Widget for the Block"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/landing.repo.bind
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.repo.bind',
            {
                fields: {
                    PLACEMENT: 'LANDING_BLOCK_04.1.one_col_fix_with_title',
                    PLACEMENT_HANDLER: 'https://your-domain.com/widgets/landing-block-handler.php',
                    TITLE: 'My Widget for the Block'
                }
            }
        );

        const result = response.getData().result;
        if (result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
    catch (error)
    {
        console.error('Error:', error);
    }
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
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
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

{% endlist %}

### Registering a Widget for All Blocks

If the application needs a single handler for all blocks, use the code `LANDING_BLOCK_*`.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "fields": {
          "PLACEMENT": "LANDING_BLOCK_*",
          "PLACEMENT_HANDLER": "https://your-domain.com/widgets/landing-block-handler.php",
          "TITLE": "My Widget for the Block"
        },
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/landing.repo.bind
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.repo.bind',
            {
                fields: {
                    PLACEMENT: 'LANDING_BLOCK_*',
                    PLACEMENT_HANDLER: 'https://your-domain.com/widgets/landing-block-handler.php',
                    TITLE: 'My Widget for the Block'
                }
            }
        );

        const result = response.getData().result;
        if (result.error())
            console.error(result.error());
        else
            console.info(result.data());
    }
    catch (error)
    {
        console.error('Error:', error);
    }
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
                        'PLACEMENT' => 'LANDING_BLOCK_*',
                        'PLACEMENT_HANDLER' => 'https://your-domain.com/widgets/landing-block-handler.php',
                        'TITLE' => 'My Widget for the Block',
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
        echo 'Error binding landing block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.bind',
        {
            fields: {
                PLACEMENT: 'LANDING_BLOCK_*',
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
                'PLACEMENT' => 'LANDING_BLOCK_*',
                'PLACEMENT_HANDLER' => 'https://your-domain.com/widgets/landing-block-handler.php',
                'TITLE' => 'My Widget for the Block',
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## How to Update a Block from the Application

After working with the block, the application can update it using the `refreshBlock` command of the [BX24.placement.call](../../widgets/ui-interaction/bx24-placement-call.md) method.

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"PLACEMENT":"refreshBlock","PARAMS":{"id":123}}' \
      "https://**put_your_bitrix24_address**/rest/placement.call?auth=**put_access_token_here**"
    ```

- JS

    ```js
    try
    {
        await $b24.callMethod(
            'placement.call',
            {
                type: 'refreshBlock',
                params: {
                    id: 123
                }
            }
        );

        console.log('Block successfully updated');
    }
    catch (error)
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'placement.call',
                [
                    'PLACEMENT' => 'refreshBlock',
                    'PARAMS' => [
                        'id' => 123,
                    ]
                ]
            );

        $result = $response->getResponseData()->getResult();
        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating block: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.placement.call(
        'refreshBlock',
        {
            id: 123
        },
        function()
        {
            console.log('Block successfully updated');
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.call',
        [
            'PLACEMENT' => 'refreshBlock',
            'PARAMS' => [
                'id' => 123,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./landing-repo-unbind.md)
- [{#T}](../../widgets/ui-interaction/bx24-placement-call.md)
- [{#T}](../../widgets/bx24-widget-methods.md)