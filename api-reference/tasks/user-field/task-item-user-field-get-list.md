# Get a list of custom fields task.item.userfield.getlist

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

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.getlist` returns a list of properties.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|| **ORDER**
[`unknown`](../../data-types.md) | Array for sorting the result. The array format is `array('sort field'=>'sort direction' [, ...])`. ||
|| **FILTER**
[`unknown`](../../data-types.md) | Array for filtering the result in the format `array('filter field'=>'filter value' [, ...])`. Required parameter. ||
|#

## Examples

{% list tabs %}

- cURL

    ```http    
    $appParams = array(
    'auth' => 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa',
    'ORDER' => array('ID' => 'asc'),
    'FILTER' => array('USER_TYPE_ID' => 'string')
    );
    ```

    ```http    
    $request = 'http://your-domain.com/rest/task.item.userfield.getlist.xml?' . http_build_query($appParams);
    ```

- JS

    ```js
    BX24.callMethod(
        "task.item.userfield.getlist",
        {
            order:
            {
                "ID": "ASC"
            },
            filter:
            {
                "EDIT_IN_LIST": "Y"
            }
        },
        function(result)
        {
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}