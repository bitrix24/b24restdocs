# Change the Value of the Custom Field userfieldconfig.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- No response in case of an error
- No examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`userfieldconfig, module scope`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
userfieldconfig.update({moduleId: string, id: number, field: {}})
```

This method changes the value of a field.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **moduleId^*^** | String identifier of the module.  | ||
|| **id^*^** | Identifier of the field settings.  | ||
|| **field** | List of settings for the new field. Similar to the userfieldconfig.add method, but:
- **fieldName** - cannot be changed
- **userTypeId** - cannot be changed
- **entityId** - cannot be changed
- **editInList** - cannot be changed
- **multiple** - cannot be changed
- **isSearchable** - changing this flag will not trigger automatic rebuilding of the search index. Rebuilding occurs when entities to which the fields are linked are changed
- **enum** - complete list of all value options for the "list" type property. For this field to be considered, **userTypeId** must be present in fields 
  - id - identifier of the option. Must be present if the option needs to be updated| ||
|#

{% include [Parameter Note](../../../../../_includes/required.md) %}

{% note info "" %}

Be careful when working with value options for lists.

{% endnote %}

## Return Value and Example

The method will return the same data as the userfieldconfig.get method for the modified field.

### Examples

Updating flags and language phrases, without changing value options

```json
{
    "moduleId": "rpa",
    "id": 170,
    "field": {
        "mandatory": "Y",
        "editFormLabel": {
            "de": "Neuer Feldname"
        }    
    }
}
```

Example of removing all value options.

```json
{
    "moduleId": "rpa",
    "id": 170,
    "field": {
        "userTypeId": "enumeration",
        "enum": [
            [""]
        ]    
    }
}
```

Example of partially updating value options.

```json
{
    "moduleId": "rpa",
    "id": 170,
    "field": {
        "userTypeId": "enumeration",
        "enum": [
            {
                "id": 29,
                "value": "Old value"
            },
            {
                "id": 30,
                "value": "Updated value"
            },
            {
                "value": "New value"
            }
        ]    
    }
}
```

In this example:
- The value option with id=29 will remain unchanged
- The value of the option with id=30 will change
- A new option "New value" will be added
- All other options will be removed. If only the id of the option is passed in the request without the value, the option will be deleted.

{% include [Example Note](../../../../../_includes/examples.md) %}