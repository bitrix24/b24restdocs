# Get a list of open lines imopenlines.config.list.get

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- missing parameters or fields
- missing examples
- missing success response
- missing error response

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method retrieves a list of open lines.

## Method Parameters

#|
|| **Name**
`Type` | **Description** ||
|| **PARAMS**
[`array`](../../data-types.md) | Array of parameters for selection (select, order, filter) (optional). A list of available fields can be found in the method description [imopenlines.config.add](./imopenlines-config-add.md) ||
|| **OPTIONS**
[`array`](../../data-types.md) | Array of additional options (optional). Currently includes only the field 'QUEUE' => 'Y'/'N' — queue of responsible employees ||
|#

## Examples

{% include [Example notes](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```js
    //imopenlines.config.list.get
    function configListGet()
    {
        var params = {
            PARAMS: {
                select: {
                    'ID',
                    ...
                },
                order: {
                    ID: 'ASC',
                    ...
                },
                filter: {
                    ID: 1,
                    ...
                }
            },
            OPTIONS: {
                QUEUE: 'Y'
            }
        };
        BX24.callMethod(
            'imopenlines.config.list.get',
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