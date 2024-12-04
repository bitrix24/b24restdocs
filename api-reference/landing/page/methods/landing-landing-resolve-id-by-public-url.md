# Get Page ID by URL landing.landing.resolveIdByPublicUrl

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.resolveIdByPublicUrl` returns the page ID based on the provided relative URL of the page.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **landingUrl**
[`unknown`](../../../data-types.md) | Relative URL of the page. ||
|| **siteId**
[`unknown`](../../../data-types.md) | Site ID. ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.landing.resolveIdByPublicUrl',
        {
            landingUrl: '/folder/sub/folder/page/',
            siteId: 1817
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