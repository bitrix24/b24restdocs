# Delete Registered Robot bizproc.provider.delete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a registered robot.

#|
|| Parameters  | Description ||
|| **CODE** | Robot identifier. ||
|#

## Examples

{% list tabs %}

- JS

    ```javascript
    var params = {
        'CODE': 'robot'
    };

    BX24.callMethod(
        'bizproc.robot.delete',
        params,
        function(result) {
            if(result.error())
                alert('Error: ' + result.error());
            else
                alert("Successfully: " + result.data());
        }
    );
    ```

- PHP (B24PhpSdk)

    ```php
    try {
        $robotCode = 'your_robot_code_here'; // Replace with the actual robot code
        $result = $serviceBuilder
            ->getBizProcScope()
            ->robot()
            ->delete($robotCode);

        if ($result->isSuccess()) {
            print("Robot deleted successfully.");
        } else {
            print("Failed to delete robot.");
        }
    } catch (Throwable $e) {
        print("Error: " . $e->getMessage());
    }
    ```

{% endlist %}

{% include [Examples note](../../../_includes/examples.md) %}