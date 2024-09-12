# Working with Site Types and Scopes

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

## Site Types

Sites can be of the following types.

- Main:
  - PAGE (from Home Page) - regular sites.
  - STORE - shops.
  - SMN - sites used in the Sites24 section in the administrative area of the BUS.

- Additional:
  - KNOWLEDGE – knowledge bases.
  - GROUP – knowledge bases of social network groups.

Currently, the extension of types is not supported.

## Scopes

In addition to the separation function at the component level, there is also a distinction based on permissions, known as scopes.

If you are working with **main types**, no action is needed.
If working with **additional types**, you need to set the scope before starting. In the case of REST, this can be done by passing an additional parameter **scope**.

## Example

The example provides a method for retrieving a list of pages, but the rule applies to any other method, including working with permissions and changes to entities.

```js
BX24.callMethod(
    'landing.landing.getList',
    {
        params: {
            select: [
                'ID', 'TITLE'
            ],
            filter: {
                TITLE: '%services%',
                SITE_ID: 205
            },
            order: {
                ID: 'DESC'
            }
        },
        scope: 'knowledge'
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

{% include [Footnote on examples](../../_includes/examples.md) %}