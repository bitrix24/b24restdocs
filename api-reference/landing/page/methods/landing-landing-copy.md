# Copy Page landing.landing.copy

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

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

The method `landing.landing.copy` copies the specified page. It returns the identifier of the new page.

## Parameters

#|
|| **Parameters** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Page identifier. ||
|| **toSiteId**
[`unknown`](../../../data-types.md) | Optional parameter, site identifier. If specified, the copy will occur to the indicated site. ||
|| **toFolderId**
[`unknown`](../../../data-types.md) | Optional parameter, folder identifier. If specified, the copy will occur to the indicated folder (if it exists and access is granted). Otherwise, the copy will occur in the same folder where the page is located, or in the root if the source is also in the root. | 18.7.500 ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.landing.copy',
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