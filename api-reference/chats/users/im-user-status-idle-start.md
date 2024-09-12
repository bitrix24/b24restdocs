# Set Automatic Status "Away" im.user.status.idle.start

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.user.status.idle.start` sets the automatic status to "Away".

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **AGO**
[`unknown`](../../data-types.md) | `10` | How many minutes ago the user went away | `18` ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.user.status.idle.start',
        {
            'AGO': 10
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
        'im.user.status.idle.start',
        Array(
            'AGO' => 10
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Example Notes](../../../_includes/examples.md) %}

## Example Response

```json
{
    "result": true
}
```