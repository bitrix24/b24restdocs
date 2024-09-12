# Copying a Page

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

{% note info "landing.landing.copy" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.copy` duplicates the specified page. It returns the identifier of the new page.

## Parameters

#|
|| **Parameters** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the page. ||
|| **toSiteId**
[`unknown`](../../../data-types.md) | Optional parameter, identifier of the site. If specified, the copy will be made to the indicated site. ||
|| **toFolderId**
[`unknown`](../../../data-types.md) | Optional parameter, identifier of the folder. If specified, the copy will be made to the indicated folder (if it exists and access is granted). Otherwise, the copy will be made to the same folder where the page is located, or to the root if the source is also in the root. | 18.7.500 ||
|#

## Examples

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

{% include [Example Notes](../../../../_includes/examples.md) %}