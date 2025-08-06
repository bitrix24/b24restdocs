# Disable automatic status "Away" im.user.status.idle.end

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.user.status.idle.end` disables the automatic status "Away".

This method was designed for the previous version of the chat. In the current version of chat M1, it works, but the results are not displayed in the interface.

{% note tip "User documentation" %}

- [Bitrix24 Chat: new messenger](https://helpdesk.bitrix24.com/open/25661218/)

{% endnote %}

## Method parameters

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

## Response in case of success

```json
{
    "result": true
}
```