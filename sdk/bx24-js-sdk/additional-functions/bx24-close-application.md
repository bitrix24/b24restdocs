# Close the window with the application BX24.closeApplication

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing
- response on success is missing
- response on error is missing

{% endnote %}

{% endif %}

```js
void BX24.closeApplication();
```

The method `BX24.closeApplication` closes the currently open modal window with the application (opened either through [BX24.openApplication](./bx24-open-application.md) or via the modal window of the embedding handler `CRM_*_LIST_MENU`).

It is recommended for use in `CRM_*_LIST_MENU`, for example, to display a close button. (By default, users have no way to return to the CRM other than closing the popup window by clicking the cross in the corner of the window.)

## Example

A unified example for [BX24.openApplication](./bx24-open-application.md) and BX24.closeApplication.

```php
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

{% include [Note on examples](../../../_includes/examples.md) %}