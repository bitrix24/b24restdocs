# Marking Block as Undeleted

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

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

{% note info "landing.landing.markundeletedblock" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.markundeletedblock` restores a block that has been marked as deleted.

## Parameters

#|
|| **Method** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the page. ||
|| **block**
[`unknown`](../../../data-types.md) | Identifier of the block. ||
|#

## Example

```js
BX24.callMethod(
    'landing.landing.markundeletedblock',
    {
        lid: 627,
        block: 11923
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}