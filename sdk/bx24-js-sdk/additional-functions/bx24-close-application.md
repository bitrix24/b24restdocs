# Close the Window with the Application BX24.closeApplication

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The method `BX24.closeApplication` sends a command to close the pop-up window with the application.

This method is recommended for use in integrations such as `CRM_*_LIST_MENU` from the [Widgets](https://apidocs.bitrix24.com/api-reference/widgets/index.html) section. For example, you can add a button that closes the application window.

```js
void BX24.closeApplication([Function callback])
```

## Parameters

#| 
|| **Name** 
`type` | **Description** ||
|| **callback** 
`function` | A callback function that is executed after the close window command is sent ||
|#

## Code Example

{% include [Example Footnote](../../../_includes/examples.md) %}

A unified example for [BX24.openApplication](./bx24-open-application.md) and `BX24.closeApplication`:

```php
<script src="//api.bitrix24.com/api/v1/"></script>
<?php
$placementOptions = array();
if (array_key_exists('PLACEMENT_OPTIONS', $_REQUEST))
{
    $placementOptions = json_decode($_REQUEST['PLACEMENT_OPTIONS'], true);
}

if (!isset($placementOptions['opened']))
{
?>
    <span onclick="openApplication()">Open</span>
<?php
}
else
{
?>
    <span onclick="closeApplication()">Close</span>
<?php
}
?>
<script>
    function openApplication()
    {
        BX24.openApplication(
            { opened: true },
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

### Example with Slider

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

setTimeout(function () {
    BX24.closeApplication();
}, 15000);
```

## Response Handling

The method does not return data (`void`).

## Continue Learning

- [{#T}](./bx24-open-application.md)
- [{#T}](./bx24-open-path.md)