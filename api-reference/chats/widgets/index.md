# Widgets for the New Chat

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits are needed to meet writing standards

{% endnote %}

{% endif %}

{% note info %}

Currently, **New Chat** is available in the cloud version of *Bitrix24* to all partners on [NFR licenses](*NFR). [Learn more about NFR...](https://partners.bitrix24.com/)

{% endnote %}

The previous embedding format for chat applications was implemented before the creation of a unified Rest Placement format for the entire product (a place for [embedding](../../widgets/index.md) Rest applications).

After the release of the Rest Placement format, developers needed to learn two different implementation formats for the same task of "embedding into the product" ([old format](../outdated/index.md) for chats and new one for other entities), and maintaining multiple options in a single application is quite challenging.

Therefore, in the New Chat, which will be available to clients later in 2023, a unified Rest Placement format for the entire product has been implemented.

Within the New Chat, there are several embedding points:

- [IM_NAVIGATION](./im-navigation.md) – application for the left navigation menu (essentially, this is an application within the chat environment without embedding directly into the chat);
- [IM_TEXTAREA](./im-textarea.md) – application for the panel above the input field (this format existed before – it generates content while composing a message);
- [IM_SIDEBAR](./im-sidebar.md) – application for the sidebar (you can create applications that add additional scenarios for the chat – for example, a separate drive for the chat or a knowledge base);
- [IM_CONTEXT_MENU](./im-context-menu.md) – application for opening the context menu of a message within the chat, embedding into the "Create content based on" item (an analogy is "Create task" or "Create meeting" based on the message);
- [IM_SMILES_SELECTOR](./im-smiles-selector.md) – application for enhancing the capabilities of emojis and Giphy (here you can have your own sources of images or emojis).

[*NFR]: **NFR** (not for resale) – a free license for a certain period, intended for familiarization with the program and testing deployments of the program.