# Update Open Line imopenlines.config.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the open line.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CONFIG_ID***
[`unknown`](../../data-types.md) | Line ID ||
|| **PARAMS**
[`unknown`](../../data-types.md) | Array of parameters to update (optional). A list of possible fields can be found in the description of the method [imopenlines.config.add](./imopenlines-config-add.md) ||
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
    //imopenlines.config.update
    function configUpdate()
    {
        var params = {
            CONFIG_ID: 1,
            PARAMS: {
                LINE_NAME: 'New line name',
                ...
            }
        };
        BX24.callMethod(
            'imopenlines.config.update',
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