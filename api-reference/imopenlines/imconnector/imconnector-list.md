# Get the list of connectors imconnector.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- from Sergei's file: please note that this returns the complete list of connected connectors in the current Bitrix24
- parameters are not specified
- required parameters are not indicated
- examples in other languages are missing

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method retrieves the list of connectors.

## Example

{% list tabs %}

- JS

    ```js
    // callListMethod: Retrieves all data at once. Use only for small selections (< 1000 items) due to high memory usage.
    
    try {
      const response = await $b24.callListMethod(
        'imconnector.list',
        {},
        (progress) => { console.log('Progress:', progress) }
      )
      const items = response.getData() || []
      for (const entity of items) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // fetchListMethod: Retrieves data in parts using an iterator. Use it for large data volumes to optimize memory usage.
    
    try {
      const generator = $b24.fetchListMethod('imconnector.list', {}, 'ID')
      for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
      }
    } catch (error) {
      console.error('Request failed', error)
    }
    
    // callMethod: Manually controls pagination through the start parameter. Use it for precise control of request batches. For large datasets, it is less efficient than fetchListMethod.
    
    try {
      const response = await $b24.callMethod('imconnector.list', {}, 0)
      const result = response.getData().result || []
      for (const entity of result) { console.log('Entity:', entity) }
    } catch (error) {
      console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imconnector.list',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            echo 'Error: ' . $result->error()->ex;
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling imconnector.list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod('imconnector.list', {}, function(result) {
        if(result.error())
        {
            console.error(result.error().ex);
        }
        else
        {
            console.log(result.data());
        }
    });
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response on success

List of connectors with names.

```json
{
    "livechat": "Live Chat",
    "whatsappbytwilio": "WhatsApp",
    "viber": "Viber",
    "telegrambot": "Telegram",
    "imessage": "Apple Messages for Business",
    "facebook": "Facebook: Messages",
    "facebookcomments": "Facebook: Comments",
    "fbinstagramdirect": "Instagram Direct",
    "network": "Bitrix24.Network",
    "notifications": "Bitrix24 SMS and WhatsApp",
    "whatsappbyedna": "Edna.com WhatsApp",
    "newcustomconnector": "new super Connector"
}
```

### Possible error codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access ||
|#

