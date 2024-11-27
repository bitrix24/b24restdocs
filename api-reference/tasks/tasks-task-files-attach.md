# Attach Files to Task tasks.task.files.attach

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success
 
{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly

{% endnote %}

> Scope: [`task`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.task.files.attach` is used to attach a file uploaded to the drive to a task.

#|
|| **Parameter** / **Type** | **Description** ||
|| **taskId**
[`unknown`](../data-types.md) | Task identifier. ||
|| **fileId**
[`unknown`](../data-types.md) | Identifier of the file uploaded to the drive. ||
|| **params**
[`unknown`](../data-types.md) | Array of additional parameters, empty by default. Currently not used. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'tasks.task.files.attach',
        {
            taskId: 1,
            fileId: 1065,
        },
        function(res) {
            console.log(res.answer.result);
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../_includes/examples.md) %}