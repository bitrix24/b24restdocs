# Update Mail Service mailservice.update

> Scope: [`mailservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `mailservice.update` modifies the parameters of an existing mail service.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../data-types.md) | Identifier of the mail service. 

You can obtain the identifier using the [mailservice.list](./mailservice-list.md) method. ||
|| **ACTIVE**
[`string`](../data-types.md) | Service activity status. Acceptable values:
- `Y` — yes
- `N` — no ||
|| **NAME**
[`string`](../data-types.md) | Name of the service ||
|| **SERVER**
[`string`](../data-types.md) | IMAP server address. Stored in lowercase in the database. ||
|| **PORT**
[`integer`](../data-types.md) | IMAP server port ||
|| **ENCRYPTION**
[`string`](../data-types.md) | Secure connection status. Acceptable values:
- `Y` — yes
- `N` — no ||
|| **LINK**
[`string`](../data-types.md) | Web interface address of the service ||
|| **ICON**
[`file`](../data-types.md)
[`integer`](../data-types.md)
[`string`](../data-types.md) | Service logo. You can provide a file or an existing file identifier. ||
|| **SORT**
[`integer`](../data-types.md) | Sort index ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "ID": 31,
        "NAME": "Updated Mail Service",
        "ACTIVE": "N",
        "SERVER": "imap.my2-mail.com",
        "PORT": 993,
        "ENCRYPTION": "Y",
        "LINK": "https://mail.my2-mail.com/",
        "SORT": 600
      }' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/mailservice.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
        "ID": 31,
        "NAME": "Updated Mail Service",
        "ACTIVE": "N",
        "SERVER": "imap.my2-mail.com",
        "PORT": 993,
        "ENCRYPTION": "Y",
        "LINK": "https://mail.my2-mail.com/",
        "SORT": 600,
        "auth": "**put_access_token_here**"
      }' \
      https://**put_your_bitrix24_address**/rest/mailservice.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'mailservice.update',
    		{
    			ID: 31,
    			NAME: 'Updated Mail Service',
    			ACTIVE: 'N',
    			SERVER: 'imap.my2-mail.com',
    			PORT: 993,
    			ENCRYPTION: 'Y',
    			LINK: 'https://mail.my2-mail.com/',
    			SORT: 600
    		}
    	);

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
            ->call(
                'mailservice.update',
                [
                    'ID' => 31,
                    'NAME' => 'Updated Mail Service',
                    'ACTIVE' => 'N',
                    'SERVER' => 'imap.my2-mail.com',
                    'PORT' => 993,
                    'ENCRYPTION' => 'Y',
                    'LINK' => 'https://mail.my2-mail.com/',
                    'SORT' => 600,
                ]
            );

        $result = $response->getResponseData()->getResult();
        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'mailservice.update',
        {
            ID: 31,
            NAME: 'Updated Mail Service',
            ACTIVE: 'N',
            SERVER: 'imap.my2-mail.com',
            PORT: 993,
            ENCRYPTION: 'Y',
            LINK: 'https://mail.my2-mail.com/',
            SORT: 600
        },
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

    $result = CRest::call(
        'mailservice.update',
        [
            'ID' => 31,
            'NAME' => 'Updated Mail Service',
            'ACTIVE' => 'N',
            'SERVER' => 'imap.my2-mail.com',
            'PORT' => 993,
            'ENCRYPTION' => 'Y',
            'LINK' => 'https://mail.my2-mail.com/',
            'SORT' => 600,
        ]
    );

    print_r($result);
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1774008238,
        "finish": 1774008238.539154,
        "duration": 0.539154052734375,
        "processing": 0,
        "date_start": "2026-03-20T15:03:58+02:00",
        "date_finish": "2026-03-20T15:03:58+02:00",
        "operating_reset_at": 1774008838,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` if the service was successfully updated ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Mail service not found"
}
```

{% include notitle [Error Handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_CORE` | Access denied | Insufficient rights to add a mail service ||
|| `400` | `ERROR_CORE` | Mail service ID not specified | Required parameter `ID` not provided ||
|| `400` | `ERROR_CORE` | Mail service not found | Mail service with the specified `ID` not found ||
|| `400` | `ERROR_CORE` | Invalid value for "*field_name*" | An invalid value for the specified field was provided ||
|#

{% include [System Errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./mailservice-add.md)
- [{#T}](./mailservice-get.md)
- [{#T}](./mailservice-list.md)
- [{#T}](./mailservice-delete.md)
- [{#T}](./mailservice-fields.md)