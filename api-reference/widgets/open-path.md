# Open Standard Bitrix24 Pages from Application Widgets

To avoid creating your own interfaces for displaying information about CRM entities, tasks, etc., and to show users the standard and familiar Bitrix24 pages, you can use the method [openPath](../bx24-js-sdk/additional-functions/bx24-open-path.md).

This significantly simplifies development and speeds up the application creation process. In your application, you can display lists of deals, tasks, contacts, companies, etc., based on your required business logic. By clicking on a list item, you can open the standard entity page in Bitrix24 using the `openPath` method.

We strongly recommend using this method in your application interfaces, as it enhances the user experience, making your solutions closely resemble the standard Bitrix24 interfaces.

Keep in mind that the `openPath` method does not provide the ability to retrieve data from the standard Bitrix24 pages. It merely opens them in a slider and allows you to track the moment the slider closes, so you can refresh the data in your interface. However, in conjunction with [event handling](../events/index.md) and the [interactive interaction](../interactivity/index.md) mechanism between the back-end and front-end parts of the application, you can implement data updates in your interface after the standard sliders close.

## Example:

```js
<script src="//api.bitrix24.com/api/v1/"></script>
<script>
    BX24.init(
        function()
        {
            BX24.openPath(
                '/crm/deal/details/5/',
                function(result)
                {
                    // callback function, called when the slider closes
                    console.log(result);
                }
            );
        }
    );
</script>
```