# Get a set of available custom field types for the module moduleId userfieldconfig.getTypes

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- no response in case of error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`userfieldconfig, module scope`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

## Description

```js
userfieldconfig.getTypes({moduleId: string})
```

The method will return a set of available custom field types for the module moduleId.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **moduleId^*^** | Module identifier.  | ||
|#

{% include [Parameter notes](../../../../../_includes/required.md) %}

## Examples

Example response

```json
{
    "types": {
        "employee": {
            "userTypeId": "employee",
            "description": "Link to employee"
        },
        "money": {
            "userTypeId": "money",
            "description": "Money"
        },
        "string": {
            "userTypeId": "string",
            "description": "String"
        },
        "integer": {
            "userTypeId": "integer",
            "description": "Integer"
        }
    }
}
```

{% include [Example notes](../../../../../_includes/examples.md) %}


{% if build == 'dev' %}

Possible values:
- `string` - String
- `integer` - Integer
- `double` - Number
- `boolean` - Yes/No
- `datetime` - Date/Time
- `date` - Date
- `money` - Money
- `url` - Link
- `address` - Address
- `enumeration` - List
- `file` - File
- `employee` - Link to employee
- `crm_status` - Link to CRM directory
- `iblock_section` - Link to information block sections
- `iblock_element` - Link to information block elements
- `crm` - Link to CRM entities
- `disk_file` - File (Drive)
- `disk_version` - File version (Drive)
- `video` - Video
- `hlblock` - Link to highload block elements
- `mail_message` - E-mail
- `resourcebooking` - Resource booking
- `string_formatted` - Template
- `vote` - Survey
- `url_preview` - Link content
- [Custom field types](../../../universal/user-defined-fields/userfield-type.md)

{% endif %}