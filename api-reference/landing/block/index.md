# Entity Blocks

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

A block is HTML code accompanied by a [manifest file](./manifest.md). Here is an example of the simplest block:

```html
<section class="landing-block">
    <div class="text-center g-color-gray-dark-v3 g-pa-10">
        <div class="g-width-600 mx-auto">
            <div class="landing-block-node-text g-font-size-12 ">
                <p>&copy; 2017 All rights reserved. Developed by
                <a href="#" class="landing-block-node-link g-color-primary">Bitrix24</a></p>
            </div>
        </div>
    </div>
</section>
```

It is important to remember that when blocks are rendered (both in editing mode and viewing mode), they are wrapped in a special wrapper `<div id="anchor" class="block-wrapper block-code">...</div>`, where:

- **anchor** – the anchor of the block, if not changed by the user, will be in the format block123, where 123 is the block ID;
- **block-wrapper** – a common class for all blocks;
- **block-code** – a class that depends on the block code, where **code** is the actual code of the block (converted to a safe format).

{% note warning %}

In editing mode, copies of all blocks are created before they are published. Accessing blocks by ID should refer to the draft versions of the blocks.

{% endnote %}

You can order the layout of new blocks from a specialist or use the additional blocks offered by the [vendor](https://htmlstream.com/preview/unify-v2.6/unify-main/shortcodes/index.html).