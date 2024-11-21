# Set Read Notification Cancellation im.notify.read

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.read` sets the cancellation of read notifications.

## Parameters

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `17` | Notification identifier | 18 ||
|| **ONLY_CURRENT**
[`unknown`](../../data-types.md) | `N` | Read only the specified notification | 18 ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

- If the parameter `ONLY_CURRENT` is passed as `Y`, the read mark will be set only for the specified `ID`. Otherwise, the mark will be set for notifications equal to or greater than the specified `ID`.

## Examples

{% list tabs %}

- cURL

    // example for cURL

- JS

    ```js
    BX24.callMethod(
        'im.notify.read',
        {
            'ID': 17,
        },
        function(result){
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    {% include [Explanation about restCommand](../_includes/rest-command.md) %}

    ```php
    $result = restCommand(
        'im.notify.read',
        Array(
            'ID' => '17'
        ),
        $_REQUEST[
            "auth"
        ]
    );    
    ```

- B24-PHP-SDK

    ```php       
    try {
        $notificationIds = [1, 2, 3]; // Example notification IDs
        $result = $serviceBuilder
            ->getIMScope()
            ->notify()
            ->markMessagesAsUnread($notificationIds);
        if ($result->isSuccess()) {
            print_r($result->getCoreResponse()->getResponseData()->getResult());
        } else {
            print("Failed to mark messages as unread.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on Success

```json
{
    "result": true
}        
```