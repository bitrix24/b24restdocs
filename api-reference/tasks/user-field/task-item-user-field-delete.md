# Delete Custom Field task.item.userfield.delete

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- missing 1 example (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.delete` removes a property.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|| **ID**
[`unknown`](../../data-types.md) | Identifier of the custom field. ||
|#

## Examples

{% list tabs %}

- cURL

    ```http
    $appParams = array(
        'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
        'ID' => 77
    );
    ```

    ```http
    $request = 'http://your-domain.com/rest/task.item.userfield.delete.xml?' . http_build_query($appParams);
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.userfield.delete',
        {
            'auth': 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
            'ID': 77
        },
    
        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}