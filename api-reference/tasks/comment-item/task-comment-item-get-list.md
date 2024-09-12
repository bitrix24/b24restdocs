# Get List of Comments task.commentitem.getlist

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed to meet writing standards
- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

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
[`unknown`](../../data-types.md) | Array for sorting the result. The sorting field can take the following values:
- `ID` — comment identifier; 
- `AUTHOR_ID` — comment author's identifier; 
- `AUTHOR_NAME` — author's name;
- `AUTHOR_EMAIL` — author's email address; 
- `POST_DATE` — date of comment publication. 

The sorting direction can take the following values:
- `asc` — ascending; 
- `desc` — descending.

By default, it is sorted in descending order by comment identifier. ||
|| **FILTER**
[`unknown`](../../data-types.md) | Array in the form `{"filter_field": "filter_value" [, ...]}`. The filter field can take the following values: 
- `ID` — comment identifier; 
- `AUTHOR_ID` — comment author's identifier; 
- `AUTHOR_NAME` — author's name; 
- `POST_DATE` — date of comment publication.

Before the name of the filter field, you can specify the type of filtering:
- "!" — not equal; 
- "<" — less than; 
- "<=" — less than or equal to;  
- ">" — greater than;  
- ">=" — greater than or equal to.  

"filter_value" — a single value or an array.

By default, records are not filtered. ||
|#

{% include [Parameter Notes](../../../_includes/required.md) %}

{% note info %}

It is mandatory to follow the order of parameters in the request. If this order is violated, the request will be executed with errors.

{% endnote %}

## Example

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
{% include [Example Notes](../../../_includes/examples.md) %}