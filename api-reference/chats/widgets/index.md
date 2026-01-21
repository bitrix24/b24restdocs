# Chat Widgets

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet the writing standard

{% endnote %}

{% endif %}

Within the chat, there are several embedding points:

- [IM_TEXTAREA](./im-textarea.md) – an application for the panel above the input field (this format existed before – it generates content while typing a message);
- [IM_SIDEBAR](./im-sidebar.md) – an application for the sidebar (you can create applications that add additional scenarios for the chat – for example, a separate Drive for the chat or a knowledge base);
- [IM_CONTEXT_MENU](./im-context-menu.md) – an application for opening the context menu of a message within the chat, embedding in the "Create content based on" item (analogous to "Create task" or "Create meeting" based on the message);

Before version `im 25.1600.0`: 
- [IM_SMILES_SELECTOR](./im-smiles-selector.md) – an application for enhancing the capabilities of emojis and Giphy (there can be custom sources for images or emojis).