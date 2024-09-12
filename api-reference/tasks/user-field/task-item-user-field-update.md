# Update Custom Field of Task task.item.userfield.update

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

{% note info "task.item.userfield.update" %}

**Scope**: [`task`](../../scopes/permissions.md) | **Who can execute the method**: `any user`

{% endnote %}

The method `task.item.userfield.update` is used to edit the properties' parameters.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|| **ID**
[`unknown`](../../data-types.md) | Identifier of the custom field. ||
|| **DATA**
[`unknown`](../../data-types.md) | Array `array('field'=>'value', ...)`. Contains the values of the parameters being edited. ||
|#

## Examples

{% list tabs %}

- cURL

    ```http
    $appParams = array(
        'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
        'ID' => 77,
        'DATA' => array('XML_ID' => 'new_external_id')
    );
    ```

    ```http
    $request = 'http://your-domain.com/rest/task.item.userfield.update.xml?' . http_build_query($appParams);
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.userfield.update',
        {
            'auth': 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
            'ID': 77,
            'DATA': {'XML_ID': 'new_external_id'}
        },

        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Examples Note](../../../_includes/examples.md) %}