# Interacting with Notification Buttons im.notify.confirm

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types not specified
- examples missing

{% endnote %}

{% endif %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.confirm` interacts with notification buttons.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **ID^*^**
[`unknown`](../../data-types.md) | `288` | Identifier of the notification that supports response selection via button clicks | `30` ||
|| **NOTIFY_VALUE^*^**
[`unknown`](../../data-types.md) | `'Y'` | Value of the selected response (button value) | `30` ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

For example, consider the notification:

- the **Accept** button has the value `'Y'`
- the **Decline** button has the value `'N'`

## Examples

{% list tabs %}

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'im.notify.confirm',
            {
                ID: 288,
                NOTIFY_VALUE: 'Y'
            }
        );
        
        const result = response.getData().result;
        console.log(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $notificationId = 123; // Example notification ID
        $isAccept = true; // Example acceptance status

        $result = $serviceBuilder
            ->getIMScope()
            ->notify()
            ->confirm($notificationId, $isAccept);

        if ($result->isSuccess()) {
            print_r($result->getCoreResponse()->getResponseData()->getResult());
        } else {
            print("Confirmation failed.");
        }
    } catch (Throwable $e) {
        print("An error occurred: " . $e->getMessage());
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.notify.confirm',
        {
            ID: 288,
            NOTIFY_VALUE: 'Y'
        },
        res => {
            if (res.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(res.data())
            }
        }
    );
    ```

{% endlist %}

{% include [Note on examples](../../../_includes/examples.md) %}

## Response on Success

```json
{
    "result_message": [
        "Invitation accepted"
    ]
}
```

## Response on Error

```json
{
    "error":"NOTIFY_VALUE_ERROR",
    "error_description":"Notification Value can't be empty"
}
```

### Description of Keys

- `error` – code of the occurred error
- `error_description` – brief description of the occurred error

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ID_ERROR** | Parameter `ID` not provided or is not a number ||
|| **NOTIFY_VALUE_ERROR** | Parameter `NOTIFY_VALUE` not specified or is empty ||
|#