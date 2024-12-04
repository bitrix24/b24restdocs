# Add Widget to Start Page: the Vibe landing.repowidget.register

> Scope: [`landing`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.repowidget.register` adds a widget for the Start page: the Vibe. It returns an error or the `ID` of the added widget.

When adding, a check is performed. If a widget with the code `code` has already been registered previously, its content will be updated. Widgets that are already placed on the Vibes will be automatically updated in case of content changes.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../data-types.md) | Unique code for the widget. It is highly recommended to use a unique prefix for your widgets to avoid the risk of code collisions with widgets from other developers ||
|| **fields***
[`object`](../data-types.md) | Field values for creating the widget ||
|#

### Parameter fields {#anchor-fields}

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NAME**
[`string`](../data-types.md) | Widget name ||
|| **PREVIEW**
[`string`](../data-types.md) | URL of the widget cover image for the widget selection slider ||
|| **DESCRIPTION**
[`string`](../data-types.md) | Widget description ||
|| **CONTENT**
[`string`](../data-types.md) | Widget markup using Vue constructs ||
|| **SECTIONS**
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
- `widgets_video` — Video ||
|| **WIDGET_PARAMS**
[`object`](../data-types.md) | [Parameters](#anchor-widget-params) for the Vue template engine. If absent, the block will remain as regular HTML code with `{{}}` ||
|| **ACTIVE**
[`char`](../data-types.md) | Widget activity. Accepts values: 

- `Y` - widget is active and available
- `N` - widget is inactive and unavailable ||
|| **SITE_TEMPLATE_ID**
[`string`](../data-types.md) | Binding the widget to a specific site template. **Only for on-premise Bitrix24!** ||
|#

#### Parameter WIDGET_PARAMS  {#anchor-widget-params}

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **rootNode***
[`string`](../data-types.md) | Selector for the root element in the markup that will be turned into a Vue component. The root element must be the only element in the provided template; all other markup will be cleared ||
|| **lang**
[`string`](../data-types.md) | Array of language phrases used in constructs `{{$Bitrix.Loc.getMessage('W_EMPTY')}}` ||
|| **handler***
[`string`](../data-types.md) | Address of the [external handler](./index.md#anchor-handler) to which requests will be sent.

**Important**: The handler must be accessible from the external network! Check the handler's availability with special services.

 ||
|| **style**
[`string`](../data-types.md) | Address of styles for the widget. Styles can also be set inline in the markup via binding `:style="{borderBottom: '1px solid red'}"` ||
|| **data***
[`object`](../data-types.md) | Demo data for the widget that will be used to showcase the widget in the Vibe templates in [Bitrix24 Market](../../market/index.md).

If you are developing a widget for a specific Bitrix24 and do not plan to publish it in the Market, you can specify any array as the parameter value; it will not be used anyway.

However, if you are preparing a mass-market solution with a widget, pay maximum attention to the demo data - they will be displayed in the preview slider of the Vibe template! Obviously, the structure of the demo data should correspond to what your handler `handler` would return in normal widget usage.
 ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- JS

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
            PREVIEW: 'https://my-app.com/vibe_preview.jpg',
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

- PHP

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
            'PREVIEW' => 'https://my-app.com/vibe_preview.jpg', 
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

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-repowidget-unregister.md)
- [{#T}](./landing-repowidget-get-list.md)
- [{#T}](./landing-repowidget-debug.md)