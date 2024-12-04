# Delete action bizproc.activity.delete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes the specified action for workflows that was previously added by the application.

{% note warning "Attention!" %}

In addition to deleting using the method `bizproc.activity.delete`, it is important to understand that when your application is deleted, as well as when the application is updated on a specific Bitrix24, all actions for workflows related to your application are automatically deleted!

{% endnote %}

#|
|| **Parameter** | **Description**                         ||
|| **CODE**^*^     | Symbolic identifier of the application action.||
|#

## Examples

{% list tabs %}

- JS

    ```javascript
    var params = {
        code: 'md5'
    };

    BX24.callMethod(
        'bizproc.activity.delete',
        params,
        function(result) {
            if(result.error())
                alert('Error: ' + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```
    
{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}