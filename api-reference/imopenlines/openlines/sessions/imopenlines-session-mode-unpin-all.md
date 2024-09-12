# Unpin All Your Dialogs imopenlines.session.mode.unpinAll

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- no response in case of an error

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Unpins all dialogs from the current operator. Returns an array of identifiers for the unpinned sessions.

## Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```js
    BX24.callMethod(
        'imopenlines.session.mode.unpinAll',
        {},
        function(result)
        {
            if(result.error())
            {
                console.warn(result.error().ex);
                return false;
            }
            console.log(result.data());
        }
    );
    ```

- PHP

    // example for PHP

{% endlist %}

## Response on Success

```json
{
    "result":[
        1652,
        1653
    ]
}
```