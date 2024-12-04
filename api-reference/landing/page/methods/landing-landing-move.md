# Move Page to Another Site / Folder landing.landing.move

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
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

The method `landing.landing.move` moves a page to another site and/or folder.

## Parameters

#|
|| **Parameters** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the page to be moved. ||
|| **toSiteId**
[`unknown`](../../../data-types.md) | Identifier of the site to which the page should be moved. Write permissions for this site are required. ||
|| **toFolderId**
[`unknown`](../../../data-types.md) | Identifier of the site folder to which the page should be moved. The folder must be within the specified site. To move to the root of the site, this parameter should be omitted. (to move within the current site - omit the toSiteId parameter as well). ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.landing.move',
        {
            lid: 11262,
            toSiteId: 1817,
            toFolderId: 737
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