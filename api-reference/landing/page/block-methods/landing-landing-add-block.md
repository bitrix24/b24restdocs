# Adding a New Block to the Page

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples are missing
- success response is absent
- error response is absent
- links to pages that have not yet been created are not provided (3 links from the Sites section)

{% endnote %}

{% endif %}

{% note info "landing.landing.addblock" %}

**Scope**: [`landing`](../../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `landing.landing.addblock` adds a new block to the page. It returns the identifier of the new block or an error.

## Parameters

#|
|| **Method** | **Description** ||
|| **lid**
[`unknown`](../../../data-types.md) | Identifier of the page ||
|| **fields**
[`unknown`](../../../data-types.md) | Array of block fields, where currently only the following values are supported:
- **CODE** - symbolic code of the block. The block code can be obtained from the method [landing.block.getrepository](.). If a block registered by a vendor through [landing.repo.register](.) is being added, the CODE value must be passed as `repo_<ID>`, where `<ID>` is the identifier of that block.
- **AFTER_ID** - after which block (its ID) the new block should be added (if not specified, the block will be added at the beginning)
- **ACTIVE** - activity status of the block (Y/N)
- **CONTENT** - entirely different content of the block (see notes for the method [landing.block.updatecontent](.)) ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.addblock',
    {
        lid: 351,
        fields: {
            CODE: '15.social'
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}