# Chat Widgets

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards

{% endnote %}

{% endif %}

Within the chat, there are several embedding points:

- [IM_TEXTAREA](./textarea.md) – an application for the panel above the input field (this format has existed before – it generates content while typing a message);
- [IM_SIDEBAR](./sidebar.md) – an application for the sidebar (you can create applications that add additional scenarios for the chat – for example, a separate Drive for the chat or a knowledge base);
- [IM_CONTEXT_MENU](./context-menu.md) – an application for opening the context menu of a message within the chat, embedding in the "Create content based on" item (the equivalent is "Create task" or "Create meeting" based on the message);

Up to version `im 25.1600.0`: 
- [IM_SMILES_SELECTOR](./smile-selector.md) – an application to enhance the capabilities of emojis and Giphy (there can be custom sources for images or emojis).