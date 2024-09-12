# AI in Bitrix24

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not to be exported to prod_" %}

- edits are needed to meet writing standards
- links to pages that have not yet been created are not specified.
- from Sergey's file: describe scenarios that can be integrated

{% endnote %}

{% endif %}

{% note info "" %}

**Scope**: [`ai_admin`](../scopes/permissions.md) | **Who can execute the method**: `administrator`

{% endnote %}

REST methods available when working with the ai module. These methods provide capabilities for working with custom AI services, including their registration, viewing the list, and deletion, as well as enabling and disabling request logging for debugging.

#|
|| **Method** | **Description** ||
|| [ai.engine.register](./ai-engine-register.md) | Method for adding a custom service. ||
|| [ai.engine.list](./ai-engine-list.md) | Method for retrieving the list of engines. ||
|| [ai.engine.unregister](./ai-engine-unregister.md) | Method for removing an engine. ||
|#