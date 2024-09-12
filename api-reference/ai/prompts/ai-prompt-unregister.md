# Delete Prompt ai.prompt.unregister

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`ai_admin`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `ai.prompt.unregister` deletes a prompt.

#|
|| **Parameter** | **Description** ||
|| **code^*^**
[`unknown`](../../data-types.md) | Unique code of the prompt. Always has the prefix `rest_`. This code is set once during registration and cannot be changed afterwards. ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```js
    BX24.callMethod(
        'ai.prompt.unregister',
        {
            code: 'rest_joke_wolf'
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

- PHP

    // example for PHP

{% endlist %}

{% include [Example Note](../../../_includes/examples.md) %}

## Success Response

## Error Response

## Typical Use-Cases and Scenarios

- [{#T}](../../../tutorials/ai/add-joke-prompt.md)