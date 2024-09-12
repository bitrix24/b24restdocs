# Get a List of Available Sprint Fields tasks.api.scrum.sprint.getFields

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

The method `tasks.api.scrum.sprint.getFields` returns the available fields of the sprint.

## Parameters

No parameters.

## Examples
{% list tabs %}

- JS
    ```js
    BX24.callMethod(
        'tasks.api.scrum.sprint.getFields',
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
    https://your-domain.bitrix24.com/rest/tasks.api.scrum.sprint.getFields
    ```

- cURL (Webhook)
    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -d '{
    }' \
    https://your-domain.bitrix24.com/rest/_USER_ID_/_CODE_/tasks.api.scrum.sprint.getFields
    ```

- PHP
    ```php
    require_once('crest.php'); // connecting CRest PHP SDK

    // executing the request to the REST API
    $result = CRest::call(
    'tasks.api.scrum.sprint.getFields',
    []
    );

    // Processing the response from Bitrix24
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
"result":
{
    "fields":
    {
     "groupId":
     {
        "type": "integer"
     },
     "name":
     {
        "type": "string"
     },
     "sort":
     {
        "type": "integer"
     },
     "createdBy":
     {
        "type": "integer"
     },
     "modifiedBy":
     {
        "type": "integer"
     },
     "dateStart":
     {
        "type": "string"
     },
     "dateEnd":
     {
        "type": "string"
     },
     "status":
     {
        "type": "string"
     }
    }
}
}
```

## Returned Data

#|
|| **Field** `type` | **Description** ||
|| **groupId** `integer` | Identifier of the group (scrum) to which the sprint belongs ||
|| **name** `string` | Name of the sprint ||
|| **sort** `integer` | Sorting ||
|| **createdBy** `integer` | Created by whom ||
|| **modifiedBy** `integer` | Modified by whom ||
|| **dateStart** `string` | Start date of the sprint ||
|| **dateEnd** `string` | End date of the sprint ||
|| **status** `string` | Status of the sprint ||
|#

## Error Handling

The method does not return errors.

{% include [Note on Examples](../../../../_includes/examples.md) %}