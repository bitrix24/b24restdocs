# Get the list of comments task.commentitem.getlist

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- revisions needed for writing standards
- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.commentitem.getlist` returns a list of comments for a task.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ORDER**
[`unknown`](../../data-types.md) | Array for sorting the result. The field for sorting can take the following values:
- `ID` — comment identifier; 
- `AUTHOR_ID` — comment author's identifier; 
- `AUTHOR_NAME` — author's name;
- `AUTHOR_EMAIL` — author's email address; 
- `POST_DATE` — comment publication date. 

The sorting direction can take the following values:
- `asc` — ascending; 
- `desc` — descending.

By default, it is filtered in descending order by comment identifier. ||
|| **FILTER**
[`unknown`](../../data-types.md) | An array of the form `{"filter_field": "filter value" [, ...]}`. The filter field can take the following values: 
- `ID` — comment identifier; 
- `AUTHOR_ID` — comment author's identifier; 
- `AUTHOR_NAME` — author's name; 
- `POST_DATE` — comment publication date.

Before the name of the filter field, you can specify the type of filtering:
- "!" — not equal; 
- "<" — less than; 
- "<=" — less than or equal to;  
- ">" — greater than;  
- ">=" — greater than or equal to.  

"filter values" — a single value or an array.

By default, records are not filtered. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

{% note info %}

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'task.commentitem.getlist',
        [1, {'ID': 'asc'}, {'>AUTHOR_ID': 2}],
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}