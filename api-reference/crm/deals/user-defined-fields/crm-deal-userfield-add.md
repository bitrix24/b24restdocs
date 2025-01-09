# Create a New Custom Field for Deals crm.deal.userfield.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter mandatory status is not indicated
- examples (in other languages) are missing
- success response is missing
- error response is missing
- text needs adjustments to fit the overall format
- link to the yet-to-be-created page is not specified

{% endnote %}

{% endif %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.deal.userfield.add` creates a new custom field for deals.

The system limit for the field name is 20 characters. The custom field name is always prefixed with UF_CRM_, meaning the actual length of the name is 13 characters.

It returns an ID that can later be identified using the method [crm.deal.userfield.list](./crm-deal-userfield-list.md).

## Parameters

#|
|| **Parameter** | **Description** ||
|| **fields** | A set of [fields](*fields) – an array in the form `array("field"=>"value"[, ...])`, containing the description of the custom field. It includes the `LIST` field, which contains a set of list values for custom fields of type List. This is specified when creating/updating the field. Each value is an array with the following fields:
- **VALUE** - the value of the list item. This field is mandatory when creating a new item.
- **SORT** - sorting.
- **DEF** - if equal to `Y`, the list item is the default value. For multiple fields, several `DEF=Y` are allowed. For non-multiple fields, the first one will be considered default.
- **XML_ID** - external code of the value. This parameter is only considered when updating existing list item values.
- **ID** - identifier of the value. If specified, it is considered an update of an existing list item value, not the creation of a new one. It only makes sense when calling the `*.userfield.update` methods.
- **DEL** - if equal to `Y`, the existing list item will be deleted. This is applied if the ID parameter is filled. ||
|#

A complete description of the fields can be obtained by calling the method [crm.userfield.fields](../../universal/user-defined-fields/crm-userfield-fields.md).

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.deal.userfield.add",
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

- JS

    ```js
    BX24.callMethod(
        "crm.deal.userfield.add",
        {
            fields:
            {
                "FIELD_NAME": "MY_LIST",
                "EDIT_FORM_LABEL": "My List",
                "LIST_COLUMN_LABEL": "My List",
                "USER_TYPE_ID": "enumeration",
                "LIST": [ { "VALUE": "Item #1" },
                    { "VALUE": "Item #2" },
                    { "VALUE": "Item #3" },
                    { "VALUE": "Item #4" },
                    { "VALUE": "Item #5" } ],
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

- PHP (B24PhpSdk)

    ```php
    try {
        $userfieldItemFields = [
            'FIELD_NAME' => 'Test Field',
            'USER_TYPE_ID' => 'string',
            'XML_ID' => 'test_field_1',
            'SORT' => '100',
            'MULTIPLE' => 'N',
            'MANDATORY' => 'N',
            'SHOW_FILTER' => 'Y',
            'SHOW_IN_LIST' => 'Y',
            'EDIT_IN_LIST' => 'Y',
            'IS_SEARCHABLE' => 'Y',
            'EDIT_FORM_LABEL' => 'Test Field Label',
            'LIST_COLUMN_LABEL' => 'Test Field List Label',
            'LIST_FILTER_LABEL' => 'Test Field Filter Label',
            'ERROR_MESSAGE' => 'Error occurred',
            'HELP_MESSAGE' => 'Help message for Test Field',
            'LIST' => '',
            'SETTINGS' => '',
        ];

        $result = $serviceBuilder
            ->getCRMScope()
            ->dealUserfield()
            ->add($userfieldItemFields);

        print($result->getId());
    } catch (Throwable $e) {
        print('Error: ' . $e->getMessage());
    }
    ```

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}

[*fields]: Currently:
- ENTITY_ID
- USER_TYPE_ID
- FIELD_NAME
- LIST_FILTER_LABEL
- LIST_COLUMN_LABEL
- EDIT_FORM_LABEL
- ERROR_MESSAGE
- HELP_MESSAGE
- MULTIPLE
- MANDATORY
- SHOW_FILTER
- SETTINGS
- LIST