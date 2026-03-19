# Publish Page landing.landing.publication

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "publication" access permission for the site

The method `landing.landing.publication` publishes the page and makes it active.

If the page is located in a folder, the method will publish that folder and all parent folders. After this, the site will become active.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **lid***
[`integer`](../../../data-types.md) | Page identifier.

The page identifier can be obtained using the method [landing.landing.getList](./landing-landing-get-list.md), as well as from the results of the methods [landing.landing.add](./landing-landing-add.md), [landing.landing.addByTemplate](./landing-landing-add-by-template.md), and [landing.landing.copy](./landing-landing-copy.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.landing.publication.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "lid": 351,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.landing.publication.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.landing.publication',
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
                'landing.landing.publication',
                [
                    'lid' => 351,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error publishing page: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.publication',
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
        'landing.landing.publication',
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
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773794655,
        "finish": 1773794655.622698,
        "duration": 0.6226980686187744,
        "processing": 0,
        "date_start": "2026-03-18T03:44:15+01:00",
        "date_finish": "2026-03-18T03:44:15+01:00",
        "operating_reset_at": 1773795255,
        "operating": 0.2789781093597412
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Publication result. Returns `true` on success ||
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

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `lid` is missing ||
|| `LANDING_NOT_EXIST` | Page not found: the `lid` contains the identifier of a non-existent, deleted, or inaccessible page ||
|| `PUBLIC_PAGE_REACHED` | The tariff plan has a limit on the number of published pages ||
|| `LANDING_PAYMENT_FAILED` | The page was added from an application, a subscription to Bitrix24 Marketplace is required for publication ||
|| `LANDING_PAYMENT_FAILED_BLOCK` | The page contains a block from an application, a subscription to Bitrix24 Marketplace is required for publication ||
|| `PUBLIC_SITE_REACHED` | The tariff plan has a limit on the number of created or published sites ||
|| `PUBLIC_SITE_REACHED_FREE` | Site publication is temporarily available only on paid plans ||
|| `PUBLIC_HTML_DISALLOWED[...]` | The tariff plan has a restriction on adding custom HTML code. In square brackets, the method returns the type of object and its identifier: `S<site_id>` for a site or `L<landing_id>` for a page ||
|| `PHONE_NOT_CONFIRMED` | Phone number confirmation is required for publication ||
|| `EMAIL_NOT_CONFIRMED` | E-mail confirmation is required for publication ||
|| `URLCHECKER_FAIL` | Malicious content detected on the page ||
|| `LICENSE_EXPIRED` | Your product license has expired ||
|| `TYPE_ERROR` | Data type error in method call parameters ||
|| `SYSTEM_ERROR` | Internal error while executing the method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-landing-get-public-url.md)
- [{#T}](./landing-landing-move.md)
- [{#T}](./landing-landing-unpublic.md)
- [{#T}](./landing-landing-update.md)