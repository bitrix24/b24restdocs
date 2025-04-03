# Add Custom Field user.userfield.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples in other languages are missing
- success response is missing
- error response is missing
 
{% endnote %}

{% endif %}

> Scope: [`user.userfield`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a custom field.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **USER_TYPE_ID** | Type of the custom field. Possible values:
- `string` - string;
- `integer` - integer;
- `double` - number;
- `date` - date;
- `datetime` - date with time;
- `boolean` - Yes / No;
- `file` - file;
- `enumeration` - list;
- `url` - link;
- `address` - Google map address;
- `money` - money;
- `iblock_section` - Binding to an information block section;
- `iblock_element` - Binding to an information block element;
- `employee` - Binding to a user;
- `crm` - Binding to a CRM entity;
- `crm_status` - Binding to a CRM directory.

Some types have their own additional settings. ||
|#
{% include [Note on parameters](../../../_includes/required.md) %}

## Example

{% list tabs %}

- PHP

    ```php
    CRest::call(
        'user.userfield.add',
        [
            'fields' => [
                'FIELD_NAME' => 'MY_TEST_FIELD_STR3',
                'USER_TYPE_ID' => 'string',
                'XML_ID' => 'MY_TEST_FIELD_STR_xml',
                'MULTIPLE' => 'Y',
                'SHOW_FILTER' => 'Y',
                'SORT' => 100,
                'LIST_FILTER_LABEL' => 'Title',
                'LIST_COLUMN_LABEL' => 'List Title',
                'EDIT_FORM_LABEL' => 'Title',
                'ERROR_MESSAGE' => 'Title',
                'HELP_MESSAGE' => 'Title',
                'SETTINGS' => [
                    'DEFAULT_VALUE' => 'value'
                ]
            ],
        ]
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}

Instead of

```
'LIST_FILTER_LABEL',
'LIST_COLUMN_LABEL',
'EDIT_FORM_LABEL',
'ERROR_MESSAGE',
'HELP_MESSAGE',
```

you can specify the key `'LABEL'`, which will fill all the above keys.