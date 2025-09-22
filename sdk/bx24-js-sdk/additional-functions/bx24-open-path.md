# Open Path in the Slider BX24.openPath

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

{% endnote %}

{% endif %}

The method `BX24.openPath` opens the specified path within the account in the slider.

{% note warning %}

For security reasons, the method does not work in the mobile application.

{% endnote %}

{% note info %}

Starting from version 22.300, it can open SPAs.

{% endnote %}

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **path**
[`unknown`](../../../api-reference/data-types.md) | The path within the account, which can start with: 
```
^\/(crm\/(deal|lead|contact|company|type)|marketplace|company\/personal\/user\/[0-9]+|workgroups\/group\/[0-9]+)\/
```
 | ||
|| **callback**
[`unknown`](../../../api-reference/data-types.md) | The function is called in 2 cases:
- when there is an error opening
- if the specified path cannot be opened: `{result: "error", errorCode: "PATH_NOT_AVAILABLE"}`
- in the mobile application: `{result: "error", errorCode: "METHOD_NOT_SUPPORTED_ON_DEVICE"}`
- when the slide is closed: `{result: "close"}` | ||
|#

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
                    console.log(result);
                }
            );
        }
    );
</script>
```

{% include [Example Note](../../../_includes/examples.md) %}