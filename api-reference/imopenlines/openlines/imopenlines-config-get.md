# Get Open Line by Id imopenlines.config.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves an open line by its ID.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CONFIG_ID***
[`unknown`](../../data-types.md) | Line ID ||
|| **WITH_QUEUE**
[`unknown`](../../data-types.md) | Display with the list of line operators (Y/N, default: Y) ||
|| **SHOW_OFFLINE**
[`unknown`](../../data-types.md) | Show the list along with operators who are offline (Y/N, default: Y) ||
|#

## Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```js
    //imopenlines.config.get
    function configGet()
    {
        var params = {
            CONFIG_ID:    1,
            WITH_QUEUE: 'Y',
            SHOW_OFFLINE: 'Y'
        };
        BX24.callMethod(
            'imopenlines.config.get',
            params,
            function (result) {
                if (result.error())
                    alert("Error: " + result.error());
                else
                    alert("Success: " + result.data());
            }
        );
    }
    ```

- PHP

    // example for PHP

{% endlist %}