# Get the list of sections entity.section.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `entity.section.get` retrieves a list of sections from the storage (information block sections). It is a list method.

The user must have at least read access permission (**R**) to the storage.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**^*^
[`string`](../../data-types.md) | Required. String identifier of the storage. ||
|| **SORT**
[`unknown`](../../data-types.md) | An array for sorting, in the form `by1`=>`order1`[, `by2`=>`order2` [, ..]], where `by1, ...` is the sorting field, which can take the following values: 
- **ID** - section code;
- **SECTION** - parent section code;
- **NAME** - section name;
- **CODE** - symbolic section code;
- **ACTIVE** - section activity;
- **LEFT_MARGIN** - left boundary;
- **DEPTH_LEVEL** - depth level (starts from 1);
- **SORT** - sorting index;
- **CREATED** - by section creation time;
- **CREATED_BY** - by the identifier of the section creator;
- **MODIFIED_BY** - by the identifier of the user who modified the section;
- **TIMESTAMP_X** - by the time of the last modification.

*order1, ...* - sorting order, which can take the following values:
- **ASC** - ascending;
- **DESC** - descending.
The default value `Array("SORT"=>"ASC")` means that the result will be sorted in ascending order. If an empty array `Array()` is specified, the result will not be sorted. ||
|| **FILTER**
[`unknown`](../../data-types.md) | An array in the form `array("filter_field"=>"value" [, ...])`. `Filter field` can take the following values:
- **ACTIVE** - filter by activity (Y\|N);
- **NAME** - by name (can search by pattern [%_]);
- **CODE** - by symbolic code (by pattern [%_]);
- **SECTION_ID** - by parent section code (if false is specified, root sections will be returned);
- **DEPTH_LEVEL** - by depth level (starts from 1);
- **LEFT_MARGIN**, **RIGHT_MARGIN** - by position in the tree (used when a selection of the subtree is needed);
- **ID** - by section code;
- **TIMESTAMP_X** - by the time of the last modification;
- **DATE_CREATE** - by creation time;
- **MODIFIED_BY** - by the code of the user who modified the section;
- **CREATED_BY** - by creator;
All filterable fields can contain a type of filter check before the name. Optional. By default, records are not filtered. ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

{% include [Parameter notes](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'entity.section.get',
        {
            ENTITY: 'menu_new',
            SORT: {
                'NAME': 'ASC'
            }
        },
        function(result){
            sections = result.data();
        }
    );
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.section.get.json?ENTITY=menu_new&SORT%5BNAME%5D=ASC&auth=9affe382af74d9c5caa588e28096e872
    ```

{% endlist %}

{% include [Examples notes](../../../_includes/examples.md) %}

## Response in case of success

> 200 OK
```json
{
    "result":
    [
        {
            "ID":"219",
            "CODE":null,
            "TIMESTAMP_X":"2013-06-23T10:11:59+02:00",
            "DATE_CREATE":"2013-06-23T10:11:59+02:00",
            "CREATED_BY":"1","MODIFIED_BY":"1",
            "ACTIVE":"Y",
            "SORT":"500",
            "NAME":"Second test section",
            "PICTURE":null,
            "DETAIL_PICTURE":null,
            "DESCRIPTION":null,
            "LEFT_MARGIN":"1",
            "RIGHT_MARGIN":"2",
            "DEPTH_LEVEL":"1",
            "ENTITY":"menu_new",
            "SECTION":null
        },
        {
            "ID":"218",
            "CODE":null,
            "TIMESTAMP_X":"2013-06-23T10:24:46+02:00",
            "DATE_CREATE":"2013-06-23T10:08:54+02:00",
            "CREATED_BY":"1",
            "MODIFIED_BY":"1",
            "ACTIVE":"Y",
            "SORT":"500",
            "NAME":"First test section",
            "PICTURE":null,
            "DETAIL_PICTURE":null,
            "DESCRIPTION":null,
            "LEFT_MARGIN":"3",
            "RIGHT_MARGIN":"4",
            "DEPTH_LEVEL":"1",
            "ENTITY":"menu_new",
            "SECTION":null
        }
    ],
    "total":2
}
```