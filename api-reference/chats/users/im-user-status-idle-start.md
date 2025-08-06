# Set Automatic Status "Away" im.user.status.idle.start

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

The method `im.user.status.idle.start` sets the automatic status "Away".

This method was designed for the previous version of the chat. In the current M1 chat version, it works, but the results are not displayed in the interface.

{% note tip "User documentation" %}

- [Bitrix24 Chat: new messenger](https://helpdesk.bitrix24.com/open/25661218/)

{% endnote %}

## Method Parameters

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **AGO**
[`unknown`](../../data-types.md) | `10` | How many minutes ago the user went away | `18` ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

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

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Example Response

```json
{
    "result": true
}
```