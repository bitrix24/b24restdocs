# Add Comment task.commentitem.add

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

The method `task.commentitem.add` creates a new comment for a task. It returns the identifier of the added comment.

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **TASKID^*^**
[`unknown`](../../data-types.md) | Identifier of the task. ||
|| **FIELDS^*^**
[`unknown`](../../data-types.md) | Array of data fields for the task (`POST_MESSAGE` — required field). ||
|#

{% include [Parameter Note](../../../_includes/required.md) %}

{% note info %}

The order of parameters in the request must be followed. If it is violated, the request will be executed with errors.

{% endnote %}

### Fields

#|
|| **Field** / **Type** | **Description** ||
|| **AUTHOR_ID**
[`unknown`](../../data-types.md) | Identifier of the user on whose behalf the comment is created. ||
|| **AUTHOR_NAME**
[`unknown`](../../data-types.md) | Name of the user (optional). ||
|| **AUTHOR_EMAIL**
[`unknown`](../../data-types.md) | E-mail of the user (optional). ||
|| **USE_SMILES**
[`unknown`](../../data-types.md) | (Y\|N) — whether to parse comments for smiles. ||
|| **POST_MESSAGE**
[`unknown`](../../data-types.md) | Text of the message. ||
|| **UF_FORUM_MESSAGE_DOC**
[`unknown`](../../data-types.md) | Array of files from the disk to attach in the form `['n123', ...]` ||
|#

## Example

```js
// Adding a new comment with the text "HELLO" for the task with ID=13
BX24.callMethod(
    'task.commentitem.add',
    [13, {'POST_MESSAGE': 'HELLO'}],
    function(result){
        console.info(result.data());
        console.log(result);
    }
);
```
{% include [Example Note](../../../_includes/examples.md) %}