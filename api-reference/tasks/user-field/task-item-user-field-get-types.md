# Get a list of available data types task.item.userfield.gettypes

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- one example is missing (there should be three examples - curl, js, php)
- no response in case of success
- no response in case of error
- add a note that other types cannot be added to tasks

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`task`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `task.item.userfield.gettypes` returns all available data types in the system.

Custom fields in tasks support the following data types:
- `string` — string
- `double` — number
- `date` — date
- `boolean` — yes/no 

## Parameters

#|
|| **Parameter** / **Type** | **Description** ||
|| **auth**
[`unknown`](../../data-types.md) | Authorization token. ||
|#

## Examples

{% list tabs %}

- cURL

    ```http
    $request = 'http://your-domain.com/rest/task.item.userfield.gettypes.xml?' . http_build_query($appParams);
    ```

- JS

    ```js
    BX24.callMethod(
        'task.item.userfield.gettypes',
        {'auth': 'q21g8vhcqmxdrbhqlbd2wh6ev1debppa'},

        function(result)
        {
            console.info(result.data());
            console.log(result);
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}