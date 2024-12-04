# Get site folders landing.site.getFolders

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getFolders` retrieves the site's folders.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **siteId**
[`unknown`](../../data-types.md) | Site identifier. 

{% note warning %}

Write access permission is required for the specified site. 

{% endnote %}

| ||
|| **filter**
[`unknown`](../../data-types.md) | Optional filter. Can accept the following fields:
- ACTIVE – folder activity (Y/N). By default, it is created as inactive;
- DELETED – folder deleted (Y/N). By default, non-deleted folders are returned;
- PARENT_ID – identifier of the parent folder;
- TITLE – folder title;
- INDEX_ID – identifier of the folder's index page;
- CODE – symbolic code of the folder;
- CREATED_BY_ID – identifier of the user who created the folder;
- MODIFIED_BY_ID – identifier of the user who modified the folder; | ||
|#

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.site.getFolders',
        {
            siteId: 1817,
            filter: {
                TITLE: 'Modified folder'
            }
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

{% include [Footnote on examples](../../../_includes/examples.md) %}