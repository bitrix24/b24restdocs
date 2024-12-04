# Mark the page as undeleted landing.landing.markUnDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.markUnDelete` marks the page as undeleted.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **$lid**
[`unknown`](../../../data-types.md) | Page identifier. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.landing.markUnDelete',
        {
            lid: 1688
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}