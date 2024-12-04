# Update Robot Fields bizproc.robot.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- corrections needed for writing standards
- missing parameters or fields
- parameter types not specified
- parameter requirements not specified
- no response in case of success
- no response in case of error

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method updates the robot fields. The **FIELDS** array receives the same parameters used in [bizproc.robot.add](./bizproc-robot-add.md).

## Examples

{% list tabs %}

- JS

    ```js
    function updateRobot1()
    {
        var params = {
            'CODE': 'hash',
            'FIELDS': {
                'DOCUMENT_TYPE': '',
                'FILTER': ''
            },
        };
        BX24.callMethod(
            'bizproc.robot.update',
            params,
            function(result)
            {
                if(result.error())
                    alert("Error: " + result.error());
                else
                    alert("Success: " + result.data());
            }
        );
    }
    ```

- PHP (B24PhpSdk)

    ```php
    try {
        $result = $serviceBuilder
            ->getBizProcScope()
            ->robot()
            ->update(
                'robot_code',
                'https://example.com/handler',
                1,
                ['en' => 'Localized Name'],
                true,
                ['property1' => 'value1'],
                false,
                ['returnProperty1']
            );

        // Process the result
        if ($result->isSuccess()) {
            print_r($result->getCoreResponse()->getResponseData()->getResult());
        } else {
            print("Update failed.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```
        
{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}