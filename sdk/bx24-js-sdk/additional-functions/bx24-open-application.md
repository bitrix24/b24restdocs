# Open the Popup Window BX24.openApplication

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The method `BX24.openApplication` opens a popup window with an application frame. You can pass parameters and a close handler to the application being opened.

The method works only inside an application frame in Bitrix24. Call it after initializing the library in the [BX24.init](../system-functions/bx24-init.md) handler. The method does not require its own scope: it controls the interface and does not access the REST API.

```js
BX24.openApplication(params?: object, closeCallback?: callable, settings?: object): void;
```

## Method Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **params**
`object` | Optional parameter. Arbitrary parameters for the opened application. The application developer determines the object keys and values. The data is passed in the `PLACEMENT_OPTIONS` request parameter as JSON ||
|| **closeCallback**
`callable` | Optional parameter. A function called without arguments after the popup window is closed [(see details)](#close-callback) ||
|| **settings**
`object` | Optional parameter. Additional window settings. Keys from `settings` are automatically added to `params` with the prefix `bx24_` ||
|#

### params Parameter {#params}

The `params` object passes data from the source application to the application opened in the popup window. For example, `{ opened: true }` is passed as `{"opened":true}`.

In the opened application, read the `PLACEMENT_OPTIONS` request parameter and parse it from JSON. In PHP, you can do this as follows:

```php
$params = [];
if (array_key_exists('PLACEMENT_OPTIONS', $_REQUEST))
{
    $params = json_decode($_REQUEST['PLACEMENT_OPTIONS'], true);
}
```

### Settings Parameter {#settings}

#| 
|| **Name**
`type` | **Description** ||
|| **width**
`integer` | Optional parameter. Width of the slider. Passed as `bx24_width` ||
|| **label**
`object` | Optional parameter. Window label parameters. Passed as `bx24_label` ||
|| **title**
`string` | Optional parameter. Page title. Passed as `bx24_title` ||
|| **leftBoundary**
`integer` | Optional parameter. Left margin of the slider. Passed as `bx24_leftBoundary`. Not used simultaneously with `width` ||
|#

#### label Parameter {#label}

#|
|| **Name**
`type` | **Description** ||
|| **bgColor**
`string` | Optional parameter. Label background color in CSS format, for example, `pink` or `#ff69b4` ||
|| **text**
`string` | Optional parameter. Label text ||
|| **color**
`string` | Optional parameter. Text and window interface element color in CSS format, for example, `#07ff0e` ||
|#

{% note warning "" %}

In some contexts of opening the window, the parameters `bx24_label.bgColor` and `bx24_label.text` may not apply. Meanwhile, `bx24_label.color` may affect the color of the window's interface elements, such as the close icon.

{% endnote %}

## Code Examples

{% include [Example Note](../../../_includes/examples.md) %}

A unified example for `BX24.openApplication` and [BX24.closeApplication](./bx24-close-application.md):

```php
<script src="//api.bitrix24.com/api/v1/"></script>
<?
$placementOptions = array();
if (array_key_exists('PLACEMENT_OPTIONS', $_REQUEST))
{
    $placementOptions = json_decode($_REQUEST['PLACEMENT_OPTIONS'], true);
}

if (!isset($placementOptions['opened']))
{
?>
    <span onclick="openApplication()">Open</span>
<?
}
else
{
?>
    <span onclick="closeApplication()">Close</span>
<?
}
?>
<script>
    function openApplication()
    {
        BX24.openApplication(
            {
                opened: true
            },
            function()
            {
                alert('Application closed!');
            }
        );

        setTimeout(closeApplication, 15000);
    }

    function closeApplication()
    {
        BX24.closeApplication();
    }
</script>
```

### Slider Example

```js
BX24.init(() => {
    BX24.openApplication(
        { opened: true },
        function () {
            console.log('Application closed');
        },
        {
            width: 450,
            label: {
                bgColor: 'pink',
                text: 'my task',
                color: '#07ff0e'
            },
            title: 'my title'
        }
    );
});
```

## Response Handling {#close-callback}

The method does not return data (`void`). If you pass a `closeCallback` function, it is called without arguments after the popup window is closed. The handler only reports that the window was closed and does not contain the result of the opened application's operation.

## Error Handling

The method does not return error codes. The `closeCallback` function does not receive a result or error object.

## Continue Learning

- [{#T}](./bx24-close-application.md)
- [{#T}](./bx24-open-path.md)
