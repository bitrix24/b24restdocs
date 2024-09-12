# Introduction

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Describe what interactive blocks are and what types exist?

{% endnote %}

{% endif %}

In Bitrix.Sites, there is an option to create interactive blocks of several types. Generally, this requires:

- ensuring the appropriate layout;
- connecting a JS extension in the block's manifest;
- specifying editable nodes or the block's subtype depending on the type.

This section describes how to connect each type of interactive block.

Of course, you have the option to create any of these types of blocks according to your own rules. In that case, they will not be interactive in the editor, and you will need to write client scripts yourself. In the case of documented capabilities, the system will handle everything for you.