# About Prompts for CoPilot

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits are needed to meet writing standards

{% endnote %}

{% endif %}

Even though neural networks are quite intelligent creations, it's important to know how to communicate with them effectively. The average user cannot (and essentially should not) know how to do this. This is where so-called pre-prompts come into play.

{% note tip " " %}

**Pre-prompts** are texts that supplement the user’s request.

{% endnote %}

Proper emphasis in such a pre-prompt guides the neural network on what exactly it needs to do, even if the user has not articulated their thoughts very clearly. A more practical example is when a user simply presses the "translate" button, and we need to create a pre-prompt for accurate translation behind the scenes.

In this section of the documentation, we will explain what pre-prompts are, how to write them, and how to register them via REST.

#|
|| **Method** | **Description** ||
|| [ai.prompt.register](./ai-prompt-register.md) | Adds a prompt ||
|| [ai.prompt.unregister](./ai-prompt-unregister.md) | Removes a prompt || 
|#