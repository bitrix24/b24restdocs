# Add Widget for Vibe landing.repowidget.register

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../scopes/permissions.md)
>
> Who can execute the method: any user when called from an application; an administrator when called via a webhook

The method `landing.repowidget.register` adds a widget for the Vibe.

If the application has already registered a widget with this `code`, the method updates its content. Instances placed on Vibe pages are updated automatically.

Register the widget from an application: the handler of a widget registered via a webhook is not called. For details, see the [overview of methods](./index.md).

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../data-types.md) | Widget code, unique within the application. Widgets of different applications with the same code do not conflict ||
|| **fields***
[`object`](../data-types.md) | Field values for creating the widget [(detailed description)](#anchor-fields) ||
|#

### Parameter fields {#anchor-fields}

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME***
[`string`](../data-types.md) | Widget name ||
|| **PREVIEW***
[`string`](../data-types.md) | URL of the widget cover image for the widget selection slider ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Widget description ||
|| **CONTENT***
[`string`](../data-types.md) | Widget markup using Vue constructs ||
|| **SECTIONS***
[`string`](../data-types.md) | Code of the section where the widget will be added. List of available sections:

- `widgets_company_life` — Company Life
- `widgets_new_employees` — New Employees
- `widgets_team` — Team
- `widgets_automation` — Automation
- `widgets_events` — Meetings and Events
- `widgets_profile` — Employee Profile
- `widgets_tasks` — Tasks and Projects
- `widgets_sales` — Sales and Clients
- `widgets_hr` — HR
- `widgets_other` — Other
- `widgets_separators` — Transitions and Separators
- `widgets_text` — Text
- `widgets_image` — Images
- `widgets_video` — Video
- `widgets_tiles` — Buttons and Links
- `widgets_columns` — Columns
- `widgets_text_image` — Text and Images ||
|| **WIDGET_PARAMS***
[`object`](../data-types.md) | [Parameters](#anchor-widget-params) for the Vue template engine ||
|| **ACTIVE**
[`char`](../data-types.md) | Widget activity. Accepts values:

- `Y` — widget is active and available
- `N` — widget is inactive and unavailable

Default: `Y` ||
|| **SITE_TEMPLATE_ID**
[`string`](../data-types.md) | Binding the widget to a specific site template. **Only for on-premise Bitrix24!** ||
|#

#### Parameter WIDGET_PARAMS  {#anchor-widget-params}

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **rootNode***
[`string`](../data-types.md) | Selector for the root element in the `CONTENT` markup that will be turned into a Vue component. All markup must be inside the root element. If the selector does not find the element in `CONTENT`, the widget is not rendered ||
|| **lang**
[`object`](../data-types.md) | Language phrases for `{{$Bitrix.Loc.getMessage('W_EMPTY')}}` constructs in the format `{"language code": {"PHRASE_CODE": "text"}}`, for example, `{"de": {"W_EMPTY": "Keine Daten"}, "en": {"W_EMPTY": "No data"}}`.

The widget uses the phrases for the user's interface language; if there are none, it uses `en` ||
|| **handler***
[`string`](../data-types.md) | Address of the [external handler](./index.md#anchor-handler) to which requests will be sent.

Address requirements:

- an absolute URL accessible from the external network
- the `https` protocol for a new or changed address
- not a loopback or local network address

If the address does not meet the requirements, the method returns the `WIDGET_HANDLER_INVALID` error. Requirements for the handler response are described in the section [Handler Request and Response](./index.md#handler-request) ||
|| **style**
[`string`](../data-types.md) | Address of styles for the widget. Styles can also be set inline in the markup via the binding `:style="{borderBottom: '1px solid red'}"` ||
|| **demoData***
[`object`](../data-types.md) | Demo data for the widget. It is displayed in the Vibe template preview slider in the [Market](../../market/index.md), so its structure must match the response of the `handler`.

If the widget will not be published in the Market, pass any object ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    const content = '<div class="my-app-w-container"><!-- Vue template --></div>'

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'landing.repowidget.register',
        params: {
          code: 'my_widget',
          fields: {
            NAME: 'My widget',
            PREVIEW: 'https://my-app.com/vibe_preview.jpg',
            CONTENT: content,
            SECTIONS: 'widgets_company_life',
            WIDGET_PARAMS: {
              rootNode: '.my-app-w-container',
              lang: {
                ru: {
                  W_TITLE: 'People and their ages',
                  W_EMPTY: 'No data',
                },
                en: {
                  W_TITLE: 'People and their ages',
                  W_EMPTY: 'Empty',
                },
              },
              handler: 'https://my-app.com/vibe.php',
              style: 'https://my-app.com/vibe.css',
              demoData: {
                desc: 'Just a test widget',
                count: 420,
                persons: [
                  { name: 'Person 1', age: 21 },
                  { name: 'Person 2', age: 42 },
                  { name: 'Person 3', age: 123 },
                ],
              },
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
        console.info('Registered widget ID:', result)
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
      async function registerVibeWidget() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const content = '<div class="my-app-w-container"><!-- Vue template --></div>'

          const response = await $b24.actions.v2.call.make({
            method: 'landing.repowidget.register',
            params: {
              code: 'my_widget',
              fields: {
                NAME: 'My widget',
                PREVIEW: 'https://my-app.com/vibe_preview.jpg',
                CONTENT: content,
                SECTIONS: 'widgets_company_life',
                WIDGET_PARAMS: {
                  rootNode: '.my-app-w-container',
                  lang: {
                    ru: {
                      W_TITLE: 'People and their ages',
                      W_EMPTY: 'No data',
                    },
                    en: {
                      W_TITLE: 'People and their ages',
                      W_EMPTY: 'Empty',
                    },
                  },
                  handler: 'https://my-app.com/vibe.php',
                  style: 'https://my-app.com/vibe.css',
                  demoData: {
                    desc: 'Just a test widget',
                    count: 420,
                    persons: [
                      { name: 'Person 1', age: 21 },
                      { name: 'Person 2', age: 42 },
                      { name: 'Person 3', age: 123 },
                    ],
                  },
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
          console.info('Registered widget ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', registerVibeWidget)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    content = '<div class="my-app-w-container"><!-- Vue template --></div>'

    try:
        bitrix_response = client.landing.repowidget.register(
            code="my_widget",
            fields={
                "NAME": "My widget",
                "PREVIEW": "https://my-app.com/vibe_preview.jpg",
                "CONTENT": content,
                "SECTIONS": "widgets_company_life",
                "WIDGET_PARAMS": {
                    "rootNode": ".my-app-w-container",
                    "lang": {
                        "ru": {
                            "W_TITLE": "People and their ages",
                            "W_EMPTY": "No data",
                        },
                        "en": {
                            "W_TITLE": "People and their ages",
                            "W_EMPTY": "Empty",
                        },
                    },
                    "handler": "https://my-app.com/vibe.php",
                    "style": "https://my-app.com/vibe.css",
                    "demoData": {
                        "desc": "Some people...",
                        "persons": [
                            {
                                "name": "Person 1",
                                "age": 21,
                            },
                            {
                                "name": "Person 2",
                                "age": 42,
                            },
                            {
                                "name": "Person 3",
                                "age": 123,
                            },
                        ],
                    },
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
                'landing.repowidget.register',
                [
                    'code'    => 'my_widget',
                    'fields'  => [
                        'NAME'         => 'My widget',
                        'PREVIEW'      => 'https://my-app.com/main_preview.jpg',
                        'CONTENT'      => $content,
                        'SECTIONS'     => 'widgets_company_life',
                        'WIDGET_PARAMS' => [
                            'rootNode' => '.my-app-w-container',
                            'lang'     => [
                                'de' => [
                                    'W_TITLE' => 'People and their ages',
                                    'W_EMPTY' => 'Empty',
                                ],
                                'en' => [
                                    'W_TITLE' => 'People and their ages',
                                    'W_EMPTY' => 'Empty',
                                },
                            ],
                            'handler'   => 'https://my-app.com/vibe.php',
                            'style'     => 'https://my-app.com/vibe.css',
                            'demoData'  => [
                                'desc'    => 'Just a test widget',
                                'count'   => 420,
                                'persons' => [
                                    ['name' => 'Person 1', 'age' => 21],
                                    ['name' => 'Person 2', 'age' => 42],
                                    ['name' => 'Person 3', 'age' => 123],
                                ],
                            ],
                        ],
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        echo 'Success: ' . print_r($result, true);
        // Your required data processing logic
        processData($result);
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error registering repowidget: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const content = `
        <div class="my-app-w-container">
            <h2 class="w-title" :style="{borderBottom: '1px solid red'}">
                {{$Bitrix.Loc.getMessage('W_TITLE')}}
            </h2>
            
            <h3>Description: not_var{{desc}}</h3>
            
            <div v-for="(value) in persons">
                <p>
                    <span class="w-name">not_var{{value.name}}</span>:
                    <span class="w-age">not_var{{value.age}}</span>
                </p>
            </div>
            
            <div v-if="persons == null">
                {{$Bitrix.Loc.getMessage('W_EMPTY')}}
            </div>
            
            <h4>Just a number not_var{{count}}</h4>
            
            <div class="w-buttons">
                <button @click="fetch">Get data (without parameters)</button>
                <button @click="fetch({param: 'a'})">Data for parameter 'a'</button>
                <button @click="fetch({param: 'b'})">Data for parameter 'b'</button>
                <button @click="openApplication({param1: '1', param2: 'false'})">Open application</button>
                <button @click="openPath('/crm')">Open local address in slider</button>
            </div>
        </div>
    `;

    const data = {
        code: 'my_widget',
        fields: {
            NAME: 'My widget',
            PREVIEW: 'https://my-app.com/main_preview.jpg',
            CONTENT: content,
            SECTIONS: 'widgets_company_life',
            WIDGET_PARAMS: {
                rootNode: '.my-app-w-container',
                lang: {
                    de: {
                        W_TITLE: 'People and their ages',
                        W_EMPTY: 'Empty',
                    },
                    en: {
                        W_TITLE: 'People and their ages',
                        W_EMPTY: 'Empty',
                    },
                },
                handler: 'https://my-app.com/vibe.php',
                style: 'https://my-app.com/vibe.css',
                demoData: {
                    desc: 'Just a test widget',
                    count: 420,
                    persons: [
                        {'name': 'Person 1', 'age': 21},
                        {'name': 'Person 2', 'age': 42},
                        {'name': 'Person 3', 'age': 123},
                    ],
                },
            },
        },
    };

    BX24.callMethod(
        'landing.repowidget.register',
        data,
        (result) =>
        {
            if (result.error())
            {
                console.error(result.error());

                return;
            }

            console.info(result.data());
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $content = <<<'HTML'
        <div class="my-app-w-container">
            <h2 class="w-title" :style="{borderBottom: '1px solid red'}">
                {{$Bitrix.Loc.getMessage('W_TITLE')}}
            </h2>
            
            <h3>Description: not_var{{desc}}</h3>
            
            <div v-for="(value) in persons">
                <p>
                    <span class="w-name">not_var{{value.name}}</span>: 
                    <span class="w-age">not_var{{value.age}}</span>
                </p>
            </div>
            
            <div v-if="persons == null">
                {{$Bitrix.Loc.getMessage('W_EMPTY')}}
            </div>
            
            <h4>Just a number not_var{{count}}</h4>
            
            <div class="w-buttons">
                <button @click="fetch">Get data (without parameters)</button>
                <button @click="fetch({param: 'a'})">Data for parameter 'a'</button>
                <button @click="fetch({param: 'b'})">Data for parameter 'b'</button>
                <button @click="openApplication({param1: '1', param2: 'false'})">Open application</button>
                <button @click="openPath('/crm')">Open local address in slider</button>
            </div>
        </div>
    HTML;

    $data = [
        'code' => 'my_widget',
        'fields' => [
            'NAME' => 'My widget', 
            'PREVIEW' => 'https://my-app.com/main_preview.jpg', 
            'CONTENT' => $content,  // Vue markup extracted into a separate variable for convenience
            'SECTIONS' => 'widgets_company_life', 
            'WIDGET_PARAMS' => [
                'rootNode' => '.my-app-w-container',
                'lang' => [
                    'de' => [
                        'W_TITLE' => 'People and their ages',
                        'W_EMPTY' => 'Empty!',
                    ],
                    'en' => [
                        'W_TITLE' => 'People and their ages',
                        'W_EMPTY' => 'Empty!',
                    ],
                ],
                'handler' => 'https://my-app.com/vibe.php',
                'style' => 'https://my-app.com/vibe.css',
                'demoData' => [
                    'desc' => 'Just a test widget',
                    'count' => 420,
                    'persons' => [
                        [
                            'name' => 'Person 1',
                            'age' => 21,
                        ],
                        [
                            'name' => 'Person 2',
                            'age' => 42,
                        ],
                        [
                            'name' => 'Person 3',
                            'age' => 123,
                        ],
                    ],
                ],
            ],
        ],
    ];

    $result = CRest::call(
        'landing.repowidget.register',
        $data
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": 10,
    "time": {
        "start": 1713949410.036288,
        "finish": 1713949411.632775,
        "duration": 1.596487045288086,
        "processing": 0.6458539962768555,
        "date_start": "2024-04-24T11:03:30+02:00",
        "date_finish": "2024-04-24T11:03:31+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the added widget ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"REQUIRED_FIELD_NO_EXISTS",
    "error_description":"The required field is missing: CONTENT"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `REQUIRED_FIELD_NO_EXISTS` | The required field is missing: CONTENT | A required field is not passed in the `fields` parameter: `NAME`, `PREVIEW`, `CONTENT`, `SECTIONS`, or `WIDGET_PARAMS`. The field name is substituted into the error text ||
|| `400` | `REQUIRED_PARAM_NO_EXISTS` | The required widget parameter is missing: handler | A required parameter is not passed in the `WIDGET_PARAMS` parameter: `rootNode`, `handler`, or `demoData`. The parameter name is substituted into the error text ||
|| `400` | `WIDGET_HANDLER_INVALID` | Invalid handler URL in parameter WIDGET_PARAMS.handler | The `handler` address does not meet the requirements from the [parameter](#anchor-widget-params) description ||
|| `400` | `CONTENT_IS_BAD` | The contents of the block were marked as unsafe | The `CONTENT` markup failed the security check. You can check it with the [landing.repo.checkContent](../landing/user-blocks/landing-repo-check-content.md) method ||
|| `400` | `ACCESS_DENIED` | — | The method was called via a webhook by a user who is not an administrator ||
|| `400` | `MISSING_PARAMS` | Some of the call parameters were missing: fields | The `code` or `fields` parameter was not passed ||
|| `400` | `TYPE_ERROR` | Invalid type of the call argument: fields | The `fields` parameter was not passed as an object ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-repowidget-get-list.md)
- [{#T}](./landing-repowidget-unregister.md)
- [{#T}](./landing-repowidget-debug.md)
- [{#T}](./index.md)
