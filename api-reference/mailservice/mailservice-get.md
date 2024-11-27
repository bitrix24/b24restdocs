# Get Parameters of the Mail Service mailservice.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing descriptions of parameter types
- no response examples

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.get` returns the parameters of the specified mail service.

## Parameters

#|
||  **Parameter** / **Type**| **Description** | **Available since** ||
|| **ID**
[`unknown`](../data-types.md) | Identifier of the mail service | ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "mailservice.get",
        {
            'ID': 10
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}