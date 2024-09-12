# Get a List of User Field Settings userfieldconfig.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- no response in case of an error
- no examples in other languages
  
{% endnote %}

{% endif %}

> Scope: [`userfieldconfig, module scope`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

```http
userfieldconfig.list({moduleId: string, select: ?{}, order: ?{}, filter: ?{}, start: number = 0})
```

The method will return a list of user field settings.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **moduleId^*^** | String identifier of the module. | ||
|| **select**| Array of fields to display. By default, all fields are shown except for list options and language phrases. To get phrases in the list, you need to pass the language identifier with the key **language**. | ||
|| **order** | List to determine the display order, where the key is the field name and the value is ASC or DESC. | ||
|| **filter** | List for filtering. | ||
|#

{% include [Parameter Note](../../../../../_includes/required.md) %}

## Examples

**Example Request**

Find all multiple user field settings from the *rpa* module, sorted in descending order by *id*, with all fields and language phrases for the *en* language.

```json
{
    "moduleId": "rpa",
    "select": {
        "0": "*",
        "language": "en"
    },
    "order": {
        "id": "DESC"
    },
    "filter": {
        "multiple": "Y"
    }
}
```

The response format for language phrases is slightly different, as it contains phrases for only one language.

```json
{
    "fields": [
        {
            "id": "165",
            "entityId": "RPA_1",
            "fieldName": "UF_RPA_1_1585069397",
            "userTypeId": "file",
            "xmlId": null,
            "sort": "100",
            "multiple": "Y",
            "mandatory": "N",
            "showFilter": "E",
            "showInList": "Y",
            "editInList": "Y",
            "isSearchable": "Y",
            "settings": {
                "SIZE": 20,
                "LIST_WIDTH": 0,
                "LIST_HEIGHT": 0,
                "MAX_SHOW_SIZE": 0,
                "MAX_ALLOWED_SIZE": 0,
                "EXTENSIONS": []
            },
            "languageId": {
                "en": "en"
            },
            "editFormLabel": {
                "en": "Multiple File"
            },
            "listColumnLabel": null,
            "listFilterLabel": null,
            "errorMessage": null,
            "helpMessage": null
        },
        {
            ...
        }
    ]
}
```

{% include [Example Note](../../../../../_includes/examples.md) %}