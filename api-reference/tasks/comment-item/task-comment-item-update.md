# Update comment task.commentitem.update

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing
- add a description that access permission can be checked using a special method

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.commentitem.update` updates the comment data. Authorization via oauth and obtaining an auth code is required.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Task identifier. ||
|| **ITEMID^*^**
[`unknown`](../../data-types.md) | Comment identifier. ||
|| **FIELDS^*^**
[`unknown`](../../data-types.md) | Array of data fields for the comment (`POST_MESSAGE` — required field). ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

{% note info %}

Maintaining the order of parameters in the request is mandatory. If violated, the request will be executed with errors.

{% endnote %}

## Example

{% list tabs %}

- JS

    ```js
    // Update the comment with ID=1205, setting the text to "HI"

    BX24.callMethod(
        'task.commentitem.update',
        [13, 1205, {'POST_MESSAGE': 'HI'}],
        function(result){
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}