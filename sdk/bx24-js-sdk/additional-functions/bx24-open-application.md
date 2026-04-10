# Open the Popup Window BX24.openApplication

The method `BX24.openApplication` opens a popup window with an application frame. You can pass parameters and a close handler to the application being opened.

```js
void BX24.openApplication([Object params], [Function closeCallback], [Object settings])
```

## Parameters

#| 
|| **Name**
`type` | **Description** ||
|| **params**
`object` | An object of parameters that will be passed to the opened application ||
|| **closeCallback**
`function` | A function that will be called after the popup window is closed ||
|| **settings**
`object` | Additional window settings. Keys from `settings` are automatically added to `params` with the prefix `bx24_` ||
|#

### Settings Parameter {#settings}

#| 
|| **Name**
`type` | **Description** ||
|| **width**
`integer` | Width of the slider. Passed as `bx24_width` ||
|| **label**
`object` | Parameters for the label. Passed as `bx24_label` ||
|| **title**
`string` | Page title. Passed as `bx24_title` ||
|| **leftBoundary**
`integer` | Left margin of the slider. Passed as `bx24_leftBoundary`. Not used simultaneously with `width` ||
|#

> In some contexts of opening the window, the parameters `bx24_label.bgColor` and `bx24_label.text` may not apply. Meanwhile, `bx24_label.color` may affect the color of the window's interface elements, such as the close icon.

## Code Example

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
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-close-application.md)
- [{#T}](./bx24-open-path.md)