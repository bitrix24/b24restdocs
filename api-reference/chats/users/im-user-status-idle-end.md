# Disable Automatic Status "Away" im.user.status.idle.end

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- revisions needed for writing standards
- parameter types not specified
- examples missing
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.user.status.idle.end` disables the automatic status "Away".

No parameters.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.user.status.idle.end',
        {},
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.user.status.idle.start',
        Array(),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": true
}
```