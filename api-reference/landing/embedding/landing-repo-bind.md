# Register an Embedding Location landing.repo.bind

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with View access permission in the Sites section

Registers an embedding location for the current application in the Sites section.

{% note info "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields**^*^
[`object`](../../data-types.md) | Embedding location parameters [(detailed description)](#fields) ||
|#

### The fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT**^*^
[`string`](../../data-types.md) | Embedding location code.

The code depends on where the application item should appear:
- `LANDING_SETTINGS` — an item in the site or page settings menu
- `LANDING_BLOCK_<CODE>` — an editing item for blocks with the specified symbolic code
- `LANDING_BLOCK_*` — an editing item for all blocks

The method trims whitespace from the value and converts the code to uppercase ||
|| **PLACEMENT_HANDLER**^*^
[`string`](../../data-types.md) | Full HTTP or HTTPS address of the embedding location handler.

The address must include the protocol and domain name ||
|| **TITLE**
[`string`](../../data-types.md) | Application item name in the interface.

The default value is an empty string ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

The example registers an application item in the site or page settings menu.

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

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        console.info(response.getData()!.result)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function bindLandingPlacement() {
        try {
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

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          console.info(response.getData().result)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindLandingPlacement)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    fields = {
        "PLACEMENT": "LANDING_SETTINGS",
        "PLACEMENT_HANDLER": "https://your-domain.com/widgets/landing-settings-handler.php",
        "TITLE": "My Settings",
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
        echo 'Error binding landing placement: ' . $e->getMessage();
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
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
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

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
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

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1775203200,
        "finish": 1775203200.764211,
        "duration": 0.7642109394073486,
        "processing": 0,
        "date_start": "2026-04-03T11:00:00+02:00",
        "date_finish": "2026-04-03T11:00:00+02:00",
        "operating_reset_at": 1775203800,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Registration result. Returns `true` if the record was added successfully ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "PLACEMENT_EXIST",
    "error_description": "Such embedding placement already exists"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `MISSING_PARAMS` | Not enough parameters for the call, missing: fields | The `fields` parameter was not passed ||
|| `400` | `TYPE_ERROR` | Invalid call argument type: fields | The value passed in `fields` is not an object ||
|| `400` | `ACCESS_DENIED` | Insufficient permissions. | The user does not have View access permission in the Sites section or did not pass the general access checks for the `landing` module ||
|| `400` | `ACCESS_DENIED` | Only an application can manage embedding locations | The method was called outside the application context, or the `rest` module is unavailable ||
|| `400` | `PLACEMENT_UNKNOWN` | This embedding location is not available for sites | The `PLACEMENT` code does not start with `LANDING_` ||
|| `400` | `PLACEMENT_HANDLER_INVALID` | Invalid embedding location handler address | `PLACEMENT_HANDLER` contains an empty or invalid HTTP or HTTPS address ||
|| `400` | `PLACEMENT_EXIST` | Such embedding placement already exists | The current application already has an embedding location with the specified `PLACEMENT` and `PLACEMENT_HANDLER` ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./settings.md)
- [{#T}](./block.md)
- [{#T}](./landing-repo-unbind.md)
