# Publish the site folder landing.site.publicationFolder

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.publicationFolder` publishes a site folder. Permissions to publish the site folder are required.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **folderId**
[`unknown`](../../data-types.md) | Folder identifier. | ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'landing.site.publicationFolder',
        {
            id: 737
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



{% include [Footnote about examples](../../../_includes/examples.md) %}