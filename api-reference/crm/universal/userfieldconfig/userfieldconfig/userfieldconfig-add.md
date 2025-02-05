# Add a New Custom Field userfieldconfig.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- requiredness and parameter types are not specified
- no success response
- no error response
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`userfieldconfig, module scope`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```http
userfieldconfig.add({moduleId: string, field: {}})
```

This method will add a new custom field.

## Parameters

#| 
|| **Parameter** | **Description** ||
|| **moduleId^*^** | String identifier of the module. | ||
|| **field** | List of settings for the new field:

- [entityId^*^](../entity-id.md) - string identifier of the entity. 
- fieldName^*^ - field code. Must be formed according to the template `UF_ + {entity identifier} + _ + {arbitrary string in UPPER_CASE}`. The field code cannot exceed 50 characters. 
- userTypeId^*^ - string identifier of the [field type](../userfieldconfig/userfieldconfig-get-types.md). 
- xmlId - external identifier.
- sort - sort index.
- multiple - multiplicity flag (N or Y), default is N. This flag can only be specified when creating the field.
- mandatory - required flag (N or Y), default is N.
- showFilter - flag to show the field in the filter (N or Y), default is N.
- showInList - flag to show the field in the list (N or Y), default is Y.
- editInList - flag to allow editing the field in the list (N or Y), default is Y.
- isSearchable - flag indicating the presence of the field value [in the full-text index](*key_index) (N or Y), default is N.
- settings - list of additional settings for the field.
- editFormLabel - list of language-dependent field names, where the key is the language identifier and the value is the phrase.
- enum - array of value options for properties of type "list":
    - value^*^ - value of the option
    - def - default value flag (N or Y), default is N. Only one can be the default option
    - sort - sort index. If not specified, it is generated automatically based on the order of value options provided
    - xmlId - external identifier of the option | ||
|#

{% include [Parameter Note](../../../../../_includes/required.md) %}

## Return Value and Example

### Return Value

The method will return the same data as the [userfieldconfig.get](userfieldconfig-get.md) method for the newly created field.

### Examples

Example of a simple request. This request is sufficient to create a field of type "string".

```json
{
    "moduleId": "rpa",
    "field": {
        "entityId": "RPA_1",
        "fieldName": "UF_RPA_1_NEW_REST_STRING",
        "userTypeId": "string"
    }
}
```

Creating a field of type "list"

```json
{
    "moduleId": "rpa",
    "field": {
        "entityId": "RPA_1",
        "fieldName": "UF_RPA_1_NEW_REST_STRING",
        "userTypeId": "enumeration"
    }
}
```

{% include [Example Note](../../../../../_includes/examples.md) %}

## Continue Learning

- [{#T}](../../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-user-field-to-spa.md)

[*key_index]: Only add necessary fields to the search. Building the index takes time when changing each field value, which can significantly slow down operations when there are many such fields.