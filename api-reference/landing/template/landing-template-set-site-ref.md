# Set Included Areas for the Site landing.template.setSiteRef

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `landing.template.setSiteRef` sets the included areas for the site within a specific template (the site or page must already be linked to the template via the TPL_ID field). It will return *true* on success or an error.

## Parameters

#|
|| **Method** | **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Site identifier. ||
|| **data**
[`unknown`](../../data-types.md) | Array of data to set (if the array is empty or not provided, the included areas will be reset). The keys of the array are the area identifiers, and the values are the identifiers of the pages to be set as the area. ||
|#

## Examples

```js
BX24.callMethod(
    'landing.template.setSiteRef',
    {
        id: 557,
        data: {
            1: 614,
            2: 615,
            3: 616
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

{% include [Footnote on examples](../../../_includes/examples.md) %}