# About REST API in Websites

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits are needed to meet writing standards

{% endnote %}

{% endif %}

To create a full-fledged website through REST or to make changes to an existing one, you need to understand that REST mimics user interaction logic. For example, to start modifying a block, you must first add it if it’s not already on the page, and to make the changes visible, you need to publish the page. But let’s summarize the steps briefly.

So, "to create a website," you need just a few things:

1. Create a website or choose one from the existing options. In the end, you will have an identifier for the website you are working with. ([Methods](./site/index.md) for working with the website.)
2. Now it’s time for the page. Similarly, create a page or select from existing ones. ([Methods](./page/index.md) for working with the page.)
3. Blocks. Blocks are the molecules of websites (nodes are the atoms). You should have a good understanding of what a [block](./block/index.md) is and what its [manifest](./block/manifest.md) is. You can work with blocks in the context of the page (adding, moving, deleting) using [these methods](./page/block-methods/index.md). However, to work with a specific block, use [these methods](./block/methods/index.md).
4. Don’t forget, after all actions, the page needs to be [published](./page/methods/landing-landing-publication.md).
5. If you need more blocks, you can always [register new ones](./demos/index.md).

This is the essential framework you need to work with blocks. Of course, there are many more methods available, and they are quite specific to cover most of your use cases.

**Good luck!**