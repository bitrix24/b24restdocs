# Create a Custom Field for Estimates crm.quote.userfield.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing (should include three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.quote.userfield.add` creates a new custom field for estimates.

The system limitation on the field name is 20 characters. The custom field name is always prefixed with `UF_CRM_`, meaning the actual length of the name is 13 characters.

#|
||  **Parameter** / **Type**| **Description** ||
|| **fields**
[`unknown`](../../data-types.md) | Set of fields - an array of the form `array("field"=>"value"[, ...])`, containing the description of the custom field. A complete description of the fields can be obtained by calling the method [crm.userfield.fields](../universal/user-defined-fields/crm-userfield-fields.md). 
||
|#

## Example

```js
BX24.callMethod(
    "crm.quote.userfield.add",
    {
        fields:
        {
            "FIELD_NAME": "MY_STRING",
            "EDIT_FORM_LABEL": "My String",
            "LIST_COLUMN_LABEL": "My String",
            "USER_TYPE_ID": "string",
            "XML_ID": "MY_STRING",
            "SETTINGS": { "DEFAULT_VALUE": "Hello, World!" }
        }
    },
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```

{% include [Footnote on examples](../../../_includes/examples.md) %}