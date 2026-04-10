# Menu Item in Site Settings and LANDING_SETTINGS Page

> Scope: [`landing`](../../scopes/permissions.md)

The `LANDING_SETTINGS` widget adds an application item to the site or page settings menu in edit mode.

For embedding in the `landing` section, the internal method of the `landing.repo.bind` module is used instead of [placement.bind](../../widgets/placement-bind.md).

{% note info "" %}

The embedding will not be displayed in the interface until the application installation is complete. [Check the application installation](../../../settings/app-installation/installation-finish.md)

{% endnote %}

## Where the Widget is Embedded

#| 
|| **Widget Code** | **Location** ||
|| `LANDING_SETTINGS` | Item in the site or page settings menu ||
|#

### Where to Find It in the Interface

Open the site or page in edit mode. In the upper right corner, go to *Site Capabilities > Settings (⚙️)*. The application item with `PLACEMENT=LANDING_SETTINGS` appears as the last item in the left slider menu.

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
    [PLACEMENT] => LANDING_SETTINGS
    [PLACEMENT_OPTIONS] => {"SITE_ID":"30","LID":"30"}
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

For `LANDING_SETTINGS`, the following keys are passed in the context:

- `SITE_ID` — the identifier of the site where the widget is opened
- `LID` — the identifier of the page from which the widget was called in edit mode

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

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.repo.bind',
            {
                fields: {
                    PLACEMENT: 'LANDING_SETTINGS',
                    PLACEMENT_HANDLER: 'https://your-domain.com/widgets/landing-settings-handler.php',
                    TITLE: 'My Settings'
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
                        'PLACEMENT' => 'LANDING_SETTINGS',
                        'PLACEMENT_HANDLER' => 'https://your-domain.com/widgets/landing-settings-handler.php',
                        'TITLE' => 'My Settings',
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

{% endlist %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./landing-repo-unbind.md)
- [{#T}](../../widgets/ui-interaction/index.md)
- [{#T}](../../widgets/bx24-widget-methods.md)