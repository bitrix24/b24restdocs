# Get Mail Service Fields mailservice.fields

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `mailservice.fields` returns localized names of the mail service fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/mailservice.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"auth": "**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/mailservice.fields
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod('mailservice.fields', {});
    	console.log(response.getData().result);
    }
    catch (error)
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call('mailservice.fields', []);

        $result = $response->getResponseData()->getResult();
        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'mailservice.fields',
        {},
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call('mailservice.fields', []);
    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": "ID",
        "SITE_ID": "Site",
        "ACTIVE": "Activity",
        "NAME": "Name",
        "SERVER": "Server Address",
        "PORT": "Port",
        "ENCRYPTION": "Secure Connection",
        "LINK": "Web Interface Address",
        "ICON": "Logo",
        "SORT": "Sorting"
    },
    "time": {
        "start": 1710000500.123,
        "finish": 1710000500.200,
        "duration": 0.077,
        "processing": 0.020,
        "date_start": "2024-03-09T10:08:20+02:00",
        "date_finish": "2024-03-09T10:08:20+02:00",
        "operating_reset_at": 1710003600,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Object with field names. The structure of the object is described in detail [below](#fields-map) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#fields-map}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | Name of the identifier field ||
|| **SITE_ID**
[`string`](../data-types.md) | Name of the site field ||
|| **ACTIVE**
[`string`](../data-types.md) | Name of the activity field ||
|| **NAME**
[`string`](../data-types.md) | Name of the service field ||
|| **SERVER**
[`string`](../data-types.md) | Name of the server address field ||
|| **PORT**
[`string`](../data-types.md) | Name of the port field ||
|| **ENCRYPTION**
[`string`](../data-types.md) | Name of the secure connection field ||
|| **LINK**
[`string`](../data-types.md) | Name of the web interface address field ||
|| **ICON**
[`string`](../data-types.md) | Name of the logo field ||
|| **SORT**
[`string`](../data-types.md) | Name of the sorting field ||
|#

## Error Handling

{% include notitle [Error Handling](../../_includes/error-info.md) %}

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mailservice-add.md)
- [{#T}](./mailservice-update.md)
- [{#T}](./mailservice-get.md)
- [{#T}](./mailservice-list.md)
- [{#T}](./mailservice-delete.md)