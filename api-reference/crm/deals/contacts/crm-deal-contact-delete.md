# Remove Contact from Specified Deal crm.deal.contact.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.contact.delete` removes a contact from the specified deal.

#|
|| **Parameter** | **Description** ||
|| **id**^*^ | Identifier of the deal. ||
|| **fields** | An object with the following [fields](./crm-deal-contact-fields.md): 
- **CONTACT_ID** - identifier of the contact (required field) ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}