# Register a Widget Handler placement.bind

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`placement`, `depending on the placement`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `placement.bind` adds a handler for the widget placement.

The method works only in an application context. Calling it through a webhook returns `WRONG_AUTH_TYPE`.

It can be called at any time during the application's operation; however, it is often more convenient to register your widgets during the [application installation](../../settings/app-installation/index.md).

Until the application installation is complete, the registered widgets are not displayed in the Bitrix24 interface — neither to regular users nor to administrators.
[Check the application installation](../../settings/app-installation/installation-finish.md).

## Method Parameters {#params}

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **PLACEMENT***
[`string`](../data-types.md) | Placement code. The method converts it to uppercase. The code must be available to the application under its scopes ||
|| **HANDLER***
[`string`](../data-types.md) | Widget handler URL. Specify an address with the `http` or `https` scheme and a host name containing a dot ||
|| **TITLE**
[`string`](../data-types.md) | Name of the widget in the interface. Depending on the placement, this may be the name of a tab in a form, a menu item, etc. ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Description of the widget in the interface. Not used in practice ||
|| **GROUP_NAME**
[`string`](../data-types.md) | Allows grouping UI elements for multiple handlers of the same widget type. For example, several dropdown items in the [top button of the CRM card](./crm/detail-toolbar.md). Supported only by certain types of widgets ||
|| **LANG_ALL**
[`object`](../data-types.md) | Object with `TITLE`, `DESCRIPTION`, and `GROUP_NAME` for the specified languages. If a nonempty `LANG_ALL` is passed, the method uses it instead of the corresponding top-level parameters. Users whose Bitrix24 interface uses one of these languages see localized names, descriptions, and groups:

```json
{
    "en": {
        "TITLE": "title",
        "DESCRIPTION": "description",
        "GROUP_NAME": "group"
    },
    "de": {
        "TITLE": "Titel",
        "DESCRIPTION": "Beschreibung",
        "GROUP_NAME": "Gruppe"
    }
}
```

||
|| **OPTIONS**
[`object`](../data-types.md) | Additional display parameters for the widget. Specific values depend on the placement. Currently used in widgets for messengers, in the widget [`PAGE_BACKGROUND_WORKER`](./universal/background-worker.md), and in the widget [CRM_XXX_DETAIL_ACTIVITY](../widgets/crm/detail-activity-area.md)

||
|| **ICON**
[`object`](../data-types.md) | Handler image. Pass `fileData` as an array containing the file name and its Base64-encoded content. The method stores the file as the registered handler's icon ||
|| **USER_ID**
[`integer`](../data-types.md) | Identifier of the Bitrix24 user for whom the registered widget will be available. Possible values can be obtained using the [user.get](../user/user-get.md) method.

Currently, this parameter is only supported by the widget [`PAGE_BACKGROUND_WORKER`](./universal/background-worker.md).

If you pass a positive `USER_ID` for a placement that does not support per-user registration, the method returns `ERROR_PLACEMENT_USER_MODE`.

||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"PLACEMENT":"PLACEMENT_CODE","HANDLER":"http://myapp.com/handler/?type=1","OPTIONS":{"errorHandlerUrl":"http://myapp.com/error/"},"TITLE":"title","DESCRIPTION":"description","GROUP_NAME":"group","LANG_ALL":{"en":{"TITLE":"title","DESCRIPTION":"description","GROUP_NAME":"group"},"de":{"TITLE":"Titel","DESCRIPTION":"Beschreibung","GROUP_NAME":"Gruppe"}},"auth":"**put_access_token_here**"}' \
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
          PLACEMENT: 'PLACEMENT_CODE',
          HANDLER: 'http://myapp.com/handler/?type=1',
          OPTIONS: {
            errorHandlerUrl: 'http://myapp.com/error/',
          },
          TITLE: 'title',
          DESCRIPTION: 'description',
          GROUP_NAME: 'group',
          LANG_ALL: {
            en: {
              TITLE: 'title',
              DESCRIPTION: 'description',
              GROUP_NAME: 'group',
            },
            ru: {
              TITLE: 'title',
              DESCRIPTION: 'description',
              GROUP_NAME: 'group',
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
        console.info('Handler registered:', result)
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
      async function bindPlacement() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'placement.bind',
            params: {
              PLACEMENT: 'PLACEMENT_CODE',
              HANDLER: 'http://myapp.com/handler/?type=1',
              OPTIONS: {
                errorHandlerUrl: 'http://myapp.com/error/',
              },
              TITLE: 'title',
              DESCRIPTION: 'description',
              GROUP_NAME: 'group',
              LANG_ALL: {
                en: {
                  TITLE: 'title',
                  DESCRIPTION: 'description',
                  GROUP_NAME: 'group',
                },
                ru: {
                  TITLE: 'title',
                  DESCRIPTION: 'description',
                  GROUP_NAME: 'group',
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
          console.info('Handler registered:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindPlacement)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.placement.bind(
            placement="PLACEMENT_CODE",
            handler="http://myapp.com/handler/?type=1",
            options={
                "errorHandlerUrl": "http://myapp.com/error/",
            },
            title="title",
            description="description",
            group_name="group",
            lang_all={
                "en": {
                    "TITLE": "title",
                    "DESCRIPTION": "description",
                    "GROUP_NAME": "group",
                },
                "ru": {
                    "TITLE": "title",
                    "DESCRIPTION": "description",
                    "GROUP_NAME": "group",
                },
            },
        ).response
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
                'placement.bind',
                [
                    'PLACEMENT' => 'PLACEMENT_CODE',
                    'HANDLER' => 'http://myapp.com/handler/?type=1',
                    'OPTIONS' => [
                        'errorHandlerUrl' => 'http://myapp.com/error/'
                    ],
                    'TITLE' => 'title',
                    'DESCRIPTION' => 'description',
                    'GROUP_NAME' => 'group',
                    'LANG_ALL' => [
                        'en' => [
                            'TITLE' => 'title',
                            'DESCRIPTION' => 'description',
                            'GROUP_NAME' => 'group',
                        ],
                        'de' => [
                            'TITLE' => 'Titel',
                            'DESCRIPTION' => 'Beschreibung',
                            'GROUP_NAME' => 'Gruppe',
                        ]
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
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
        "placement.bind",
        { 
            "PLACEMENT": "PLACEMENT_CODE",
            "HANDLER": "http://myapp.com/handler/?type=1",
            "OPTIONS": {
                "errorHandlerUrl": "http://myapp.com/error/"
            },
            "TITLE": "title",
            "DESCRIPTION": "description",
            "GROUP_NAME": "group",
            "LANG_ALL": {
                "en": {
                    "TITLE": "title",
                    "DESCRIPTION": "description",
                    "GROUP_NAME": "group",
                },
                "de": {
                    "TITLE": "Titel",
                    "DESCRIPTION": "Beschreibung",
                    "GROUP_NAME": "Gruppe",
                }
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'placement.bind',
        [
            'PLACEMENT' => 'PLACEMENT_CODE',
            'HANDLER' => 'http://myapp.com/handler/?type=1',
            'OPTIONS' => [
                'errorHandlerUrl' => 'http://myapp.com/error/'
            ],
            'TITLE' => 'title',
            'DESCRIPTION' => 'description',
            'GROUP_NAME' => 'group',
            'LANG_ALL' => [
                'en' => [
                    'TITLE' => 'title',
                    'DESCRIPTION' => 'description',
                    'GROUP_NAME' => 'group'
                ],
                'de' => [
                    'TITLE' => 'Titel',
                    'DESCRIPTION' => 'Beschreibung',
                    'GROUP_NAME' => 'Gruppe'
                ]
            ]
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
    	"PLACEMENT": "PLACEMENT_CODE",
    	"HANDLER":   "http://myapp.com/handler/?type=1",
    	"OPTIONS": b24.Params{
    		"errorHandlerUrl": "http://myapp.com/error/",
    	},
    	"TITLE":       "title",
    	"DESCRIPTION": "description",
    	"GROUP_NAME":  "group",
    	"LANG_ALL": b24.Params{
    		"en": b24.Params{
    			"TITLE":       "title",
    			"DESCRIPTION": "description",
    			"GROUP_NAME":  "group",
    		},
    		"ru": b24.Params{
    			"TITLE":       "Titel",
    			"DESCRIPTION": "Beschreibung",
    			"GROUP_NAME":  "Gruppe",
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("placement.bind: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

{% note tip "Typical use-cases and scenarios" %}

- [{#T}](../../tutorials/crm/crm-widgets/widget-as-detail-tab.md)
- [{#T}](../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md)

{% endnote %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the handler was registered successfully. On error, the method returns an error code and description ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**, **200**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "The value of an argument 'TITLE' must be of type string",
    "argument": "TITLE"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Status** ||
|| `ERROR_PLACEMENT_MAX_COUNT` | The registration limit for the placement was exceeded, for example for `PAGE_BACKGROUND_WORKER` or [`REST_APP_URI`](./universal/app-url.md) | 400 ||
|| `ERROR_PLACEMENT_USER_MODE` | `USER_ID` was passed for a placement that does not support per-user registration | 400 ||
|| `EMPTY_ERROR_HANDLER_URL` | The required `OPTIONS[errorHandlerUrl]` parameter was not passed when registering the `PAGE_BACKGROUND_WORKER` widget | 200 ||
|| `ERROR_ARGUMENT` | A required parameter is missing or has an invalid type. The name of the invalid parameter is returned in `argument` | 400 ||
|| `ERROR_PLACEMENT_NOT_FOUND` | The `PLACEMENT` code is unavailable to the application under its scopes | 400 ||
|| `ERROR_WRONG_HANDLER_URL` | The `HANDLER` URL has no host name containing a dot, for example `localhost` | 400 ||
|| `ERROR_UNSUPPORTED_PROTOCOL` | The `HANDLER` URL uses a scheme other than `http` or `https` | 400 ||
|| `WRONG_AUTH_TYPE` | The method was called outside an application context, for example through a webhook | 403 ||
|| `ACCESS_DENIED` | The caller does not have administrator permissions | 403 ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Widget Handler

Pass the widget handler URL in `HANDLER`. After a successful `placement.bind` call, Bitrix24 registers the handler for the selected placement.

{% note warning "" %}

When registering the handler, the method checks for the `http` or `https` scheme and a dot in the host name. `localhost` fails this check. Ensure Bitrix24 can reach the handler from the external network: the method does not check server availability.

{% endnote %}

For most placements, Bitrix24 calls the handler with a POST request. The request contains application authorization and placement context. For example, a deal card tab receives the deal ID in `PLACEMENT_OPTIONS`. Some placements call the handler differently or do not pass data.

Request format, data fields, and exceptions are described on the [placement pages](./placements.md).

## Continue Learning

- [{#T}](./placements.md)
- [{#T}](./placement-list.md)
- [{#T}](./placement-unbind.md)
- [{#T}](./ui-interaction/index.md)
