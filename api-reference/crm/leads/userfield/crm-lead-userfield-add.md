# Add Field crm.lead.userfield.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- edits needed to meet writing standards
- parameter requirements are not specified
- examples are missing (in other languages)
- success response is missing
- error response is missing
- link to the yet-to-be-created page is not provided

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.lead.userfield.add` creates a new custom field for leads.

The system limit on the field name is 20 characters. The prefix `UF_CRM_` is always added to the custom field name, meaning the actual length of the name is 13 characters.

#|
|| **Parameter** | **Description** ||
|| **fields** | Set of fields – an array of the form `array("field"=>"value"[, ...])`, containing the description of the custom field. ||
|| **LIST** | Contains a set of list values for custom fields of type List. Specified when creating/updating the field. Each value is an array with the fields: 
- **VALUE** - the value of the list item. This field is required when creating a new item.
- **SORT** - sorting. 
- **DEF** - if equal to `Y`, the list item is the default value. For multiple fields, several `DEF=Y` are allowed. For non-multiple fields, the first will be considered default. 
- **XML_ID** - external code of the value. This parameter is considered only when updating already existing values of the list item. 
- **ID** - identifier of the value. If specified, it is considered an update of an existing list item value, not the creation of a new one. It only makes sense when calling the `*.userfield.update` methods. 
- **DEL** - if equal to Y, the existing list item will be deleted. Used if the ID parameter is filled. ||
|#

A complete description of the fields can be obtained by calling the method [crm.userfield.fields](.).

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.lead.userfield.add",
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

{% endlist %}

Example of creating a list type field

{% list tabs %}

- JS
  
    ```js
    BX24.callMethod(
        "crm.lead.userfield.add",
        {
            fields:
            {
                "FIELD_NAME": "MY_LIST",
                "EDIT_FORM_LABEL": "My List",
                "LIST_COLUMN_LABEL": "My List",
                "USER_TYPE_ID": "enumeration",
                "LIST": [ { "VALUE": "Item #1" }, { "VALUE": "Item #2" }, { "VALUE": "Item #3" }, { "VALUE": "Item #4" }, { "VALUE": "Item #5" } ],
                "XML_ID": "MY_LIST",
                "SETTINGS": { "LIST_HEIGHT": 3 }
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

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}