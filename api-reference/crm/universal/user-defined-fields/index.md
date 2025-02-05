# Custom Fields in CRM

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- which object types it applies to, etc. There is some confusion with the sections that needs clarification.
- edits to match the writing standard.

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

Methods for working with custom fields:

#|
|| **Method** | **Description** ||
|| [crm.userfield.fields](./crm-userfield-fields.md) | This method returns the description of fields for custom fields. ||
|| [crm.userfield.types](./crm-userfield-types.md) | This method returns a list of custom field types. ||
|| [crm.userfield.enumeration.fields](./crm-userfield-enumeration-fields.md) | This method returns the description of fields for a custom field of type "enumeration" (list). ||
|| [crm.userfield.settings.fields](./crm-userfield-settings-fields.md) | This method returns the description of settings fields for the custom field type. ||
|#

## Additional
- [Custom Field Settings](../userfieldconfig/index.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)