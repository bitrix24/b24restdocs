# Navigation and Title

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet writing standards

{% endnote %}

{% endif %}

For this type of block, there is no need to specify any special parameters in the manifest. Any block can serve as a navigation chain or title. It is sufficient to have markers in the block's content:

## Marker: \#title\#

This is replaced with the title of the current page, which primarily depends on the page's name but can be overridden by components within the page blocks or an external solution.

Example of a block with a title:

```html
<section class="landing-block g-pt-20 g-pb-20">
    <div class="landing-title-container container g-font-size-12">
        #title#
    </div>
</section>
```

## Marker: \#breadcrumb\#

This marker is replaced with a navigation chain – a sequence of links from the main page to the current one. Typically, the real benefit of the chain appears only in the case of dynamic content and a branched structure.

There is no way to influence the layout of the sequence of links directly in the cloud version.

Example of a block with a navigation chain:

```html
<section class="landing-block g-pt-20 g-pb-20">
    <div class="landing-breadcrumb-container container g-font-size-12">
        #breadcrumb#
    </div>
</section>
```