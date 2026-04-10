# Get Public URL of the Page landing.landing.getpublicurl

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with access to the "Sites and Stores" section and "view" permission for the site

The method `landing.landing.getpublicurl` returns the complete public URL of the page.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the [landing.landing.getList](./landing-landing-get-list.md) method, as well as from the results of the [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), and [landing.landing.copy](./landing-landing-copy.md) methods ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.getpublicurl.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.getpublicurl.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.landing.getpublicurl',
            {
                lid: 351
            }
        );

        const result = response.getData().result;
        console.info(result);
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
                'landing.landing.getpublicurl',
                [
                    'lid' => 351,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . $result;
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting landing public URL: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.getpublicurl',
        {
            lid: 351
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.landing.getpublicurl',
        [
            'lid' => 351,
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo $result['result'];
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": "https://b24-r8rseo.bitrix24site.com/spring-sale/",
    "time": {
        "start": 1773784381,
        "finish": 1773784381.945904,
        "duration": 0.945904016494751,
        "processing": 0,
        "date_start": "2026-03-18T00:53:01+01:00",
        "date_finish": "2026-03-18T00:53:01+01:00",
        "operating_reset_at": 1773784981,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../../data-types.md) | Complete public URL of the page.

For the main page of the site and the index page of a folder, the method returns the address without the `CODE` of the page. If the `RULE` field is filled for the page, the address is also returned without adding the `CODE`.

More about the `CODE` and `RULE` fields can be found in the section [Fields of the Page Object](../fields.md) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "LANDING_NOT_EXIST",
    "error_description": "Landing not found"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid` is missing ||
|| `LANDING_NOT_EXIST` | Page not found: the `lid` contains an identifier of a non-existent, deleted, or inaccessible page ||
|| `ACCESS_DENIED` | Insufficient permissions to call the method ||
|| `TYPE_ERROR` | Data type error in the method call parameters ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-copy.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-get-preview.md)
- [{#T}](./landing-landing-resolve-id-by-public-url.md)
- [{#T}](./landing-landing-update.md)