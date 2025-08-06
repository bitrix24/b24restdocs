# Set User Status im.user.status.set

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `im.user.status.set` sets the user's status.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **STATUS^*^**
[`unknown`](../../data-types.md) | `online` | New user status | 18 ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

The following statuses are available:

- `online` – Online
- `dnd` – Do Not Disturb
- `away` – Away

{% note info "" %}

The status `away` was used in the previous version of the chat. In the current version of chat M1, it is not displayed in the interface.
[Bitrix24 Chat: new messenger](https://helpdesk.bitrix24.com/open/25661218/)

{% endnote %}

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```javascript
    BX24.callMethod(
        'im.user.status.set',
        {
            STATUS: 'online'
        },
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
        'im.user.status.set',
        Array(
            'STATUS' => 'online',
        ),
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