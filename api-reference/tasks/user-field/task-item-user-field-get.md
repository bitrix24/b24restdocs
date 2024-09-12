# Get Custom Task Field by ID task.item.userfield.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- missing 1 example (there should be three examples - curl, js, php)
- no response in case of error
- no response in case of success

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.get` returns a property by ID.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|| **ID**
[`unknown`](../../data-types.md) | Custom field identifier. ||
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
    $request = 'http://your-domain.com/rest/task.item.userfield.get.xml?' . http_build_query($appParams);
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.userfield.get',
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