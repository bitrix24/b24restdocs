# Get Information About the User's Set Status im.user.status.get

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

The method `im.user.status.get` retrieves information about the user's set status.

No parameters.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```javascript
    BX24.callMethod(
        'im.user.status.get',
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
        'im.user.status.get',
        Array(),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in Case of Success

```json
{
    "result": "dnd"
}
```

### Description of the Result

- `online` – Online
- `dnd` – Do Not Disturb
- `away` – Away

{% note info "" %}

The status `away` was used in the previous version of the chat. In the current version of chat M1, it is not displayed in the interface.
[Bitrix24 Chat: new messenger](https://helpdesk.bitrix24.com/open/25661218/)

{% endnote %}