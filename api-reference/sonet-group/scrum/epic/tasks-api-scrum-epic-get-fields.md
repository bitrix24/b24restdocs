# Get a List of Available Epic Fields tasks.api.scrum.epic.getFields

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing
- response in case of success is missing
 
{% endnote %}

{% endif %}

> Scope: [`task`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `tasks.api.scrum.epic.getFields` returns the available fields of the epic.

## Parameters

No parameters.

## Examples
{% list tabs %}

- JS
    ```js
    BX24.callMethod(
        'tasks.api.scrum.epic.getFields',
        {},
        function(res)
        {
            console.log(res);
        }
    );
    ```

- cURL (oAuth)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    auth=YOUR_ACCESS_TOKEN
    }' \
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.epic.getFields
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.epic.getFields
    ```

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing the request to the REST API
    $result = CRest::call(
    'tasks.api.scrum.epic.getFields',
    []
    );

    // Handling the response from Bitrix24
    if ($result['error']) {
    echo 'Error: '.$result['error_description'];
    } else {
    print_r($result['result']);
    }
    ```
{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
  "fields":
  {
    "name":
    {
      "type": "string"
    },
    "description":
    {
      "type": "string"
    },
    "groupId":
    {
      "type": "integer"
    },
    "color":
    {
      "type": "string"
    },
    "files":
    {
      "type": "array"
    },
    "createdBy":
    {
      "type": "integer"
    },
    "modifiedBy":
    {
      "type": "integer"
    }
  }
}
```

## Returned Data

#|
|| **Field** `type` | **Description** ||
|| **name** `string` | Epic name ||
|| **description** `string` | Epic description ||
|| **groupId** `integer` | Identifier of the group (scrum) to which the epic belongs ||
|| **color** `string` | Epic color ||
|| **files** `array` | Array of files attached to the epic ||
|| **createdBy** `integer` | Created by ||
|| **modifiedBy** `integer` | Modified by ||
|#

## Error Handling

The method does not return errors.

{% include [Examples Note](../../../../_includes/examples.md) %}