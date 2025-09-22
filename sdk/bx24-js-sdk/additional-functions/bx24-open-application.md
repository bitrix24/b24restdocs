# Open Popup BX24.openApplication

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent
- the parameter list should probably be split into separate tables

{% endnote %}

{% endif %}

```js
void BX24.openApplication([
    Object parameters[
    Function closeCallback
    ]
]);
```

When the method `BX24.openApplication` is called, a popup window will open with the application frame. The application will receive data from the parameters parameter. The closeCallback handler will be invoked when the popup window is closed. The method can control the size, title, and label of the slider.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **parameters**
[`unknown`](../../../api-reference/data-types.md) | An object with parameters that will be passed to the opened application as a JSON string ||
|| **closeCallback**
[`unknown`](../../../api-reference/data-types.md) | The application close handler ||
|| **bx24_width**
[`unknown`](../../../api-reference/data-types.md) | The width of the slider ||
|| **bx24_label**
[`unknown`](../../../api-reference/data-types.md) | The title of the label ||
|| **bx24_title**
[`unknown`](../../../api-reference/data-types.md) | The title of the page ||
|| **bx24_leftBoundary**
[`unknown`](../../../api-reference/data-types.md) | The slider spans the full width with a left margin. Cannot be used simultaneously with bx24_width. ||
|#

For placements `CRM_*_LIST_MENU`, it is blocked.

## Examples

A unified example for BX24.openApplication and [BX24.closeApplication](./bx24-close-application.md)

```js
<script src="//api.bitrix24.com/api/v1/"></script>
<?
// parsing input data
$placementOptions = array();
if(array_key_exists('PLACEMENT_OPTIONS', $_REQUEST))
{
    $placementOptions = json_decode($_REQUEST['PLACEMENT_OPTIONS'], true);
}

// if the application is not opened, display the open button; otherwise, display the close button
if(!isset($placementOptions['opened']))
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
                'opened': true // data passed to the opened application
            },
            function()
            {
                // this handler will be triggered when the application is closed
                alert('Application closed!')
            }
        );

        setTimeout(closeApplication, 15000); // automatically close after 15 seconds
    }

    function closeApplication()
    {
        BX24.closeApplication();
    }
</script>
```

Example with slider

```js
BX24.openApplication(
    {
        'opened': true,
        'bx24_width': 450,// int
        'bx24_label': {
            'bgColor':'pink', // aqua/green/orange/brown/pink/blue/grey/violet
            'text': 'my task',
            'color': '#07ff0e',
        },
        'bx24_title': 'my title', // str
        //'bx24_leftBoundary': 300, //int
    },
    function()
    {
        console.log('Application closed!')
    }
);
```

{% include [Note on examples](../../../_includes/examples.md) %}