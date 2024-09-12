# Publish the website folder landing.site.publicationFolder

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.publicationFolder` publishes the website folder. Permissions to publish the website folder are required.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **folderId**
[`unknown`](../../data-types.md) | The identifier of the folder. | ||
|#

## Example

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

{% include [Footnote on examples](../../../_includes/examples.md) %}