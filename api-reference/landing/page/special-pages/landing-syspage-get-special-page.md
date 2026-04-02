# Get the URL of the special page using landing.syspage.getSpecialPage

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" access permission for the site

The method `landing.syspage.getSpecialPage` returns the URL of the special page for the site.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **siteId***
[`integer`](../../../data-types.md) | The site identifier.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) or from the result of the method [landing.site.add](../../site/landing-site-add.md) ||
|| **type***
[`string`](../../../data-types.md) | The code of the special page that the method looks for in the site's bindings. 

The method searches for an exact match of the string, without trimming spaces and without normalizing the case.

Possible values:
- `mainpage` — main page
- `catalog` — main catalog page
- `personal` — personal section
- `cart` — shopping cart
- `order` — order placement
- `payment` — payment page
- `compare` — comparison page
- `feedback` — feedback page

The method [landing.syspage.set](./landing-syspage-set.md) removes spaces from the edges of the `type` value when saving. Therefore, in `landing.syspage.getSpecialPage`, the `type` parameter should be passed without extra spaces. Otherwise, the method may not find the required page.

If a number, `true`, `false`, or `null` is passed instead of a string, the method will return an empty string `""`. If an array is passed, the request will result in an error ||
|| **additional**
[`object`](../../../data-types.md) | Additional URL parameters [(detailed description)](#additional).

Each key-value pair is added to the final URL of the found page. By default, it is an empty object ||
|#

### Additional Parameter {#additional}

The `additional` parameter adds extra parameters to the URL of the found page. It does not affect which page the method returns; the page is still determined solely by `siteId` and `type`.

Pass `additional` as an object in the format `{"<parameter_name>": "<value>"}`. All passed parameters will be added to the URL of the found page. If the page is not found, the parameter is not applied. If the URL already contains a parameter with that name, its value will be replaced.

Example:

```json
{
    "siteId": 1390,
    "type": "personal",
    "additional": {
        "SECTION": "private",
        "utm_source": "newsletter"
    }
}
```

Result:

```text
https://b24-test.bitrix24.shop/personalsection/?SECTION=private&utm_source=newsletter
```

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 1390,
        "type": "personal",
        "additional": {
          "SECTION": "private"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.syspage.getSpecialPage.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 1390,
        "type": "personal",
        "additional": {
          "SECTION": "private"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.syspage.getSpecialPage.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.syspage.getSpecialPage',
    		{
    			siteId: 1390,
    			type: 'personal',
    			additional: {
    				SECTION: 'private'
    			}
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
                'landing.syspage.getSpecialPage',
                [
                    'siteId' => 1390,
                    'type' => 'personal',
                    'additional' => [
                        'SECTION' => 'private',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting special page URL: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.syspage.getSpecialPage',
        {
            siteId: 1390,
            type: 'personal',
            additional: {
                SECTION: 'private'
            }
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
        'landing.syspage.getSpecialPage',
        [
            'siteId' => 1390,
            'type' => 'personal',
            'additional' => [
                'SECTION' => 'private',
            ],
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
    "result": "https://btest.bitrix24.shop/personalsection/?SECTION=private",
    "time": {
        "start": 1774359422,
        "finish": 1774359422.588757,
        "duration": 0.5887570381164551,
        "processing": 0,
        "date_start": "2026-03-24T16:37:02+01:00",
        "date_finish": "2026-03-24T16:37:02+01:00",
        "operating_reset_at": 1774360022,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`string`](../../../data-types.md) \| `null` | URL of the special page.

If `additional` is passed and the page is found, the parameters will be added to the query string.

The method does not check if the page is active or marked as deleted. It returns `null` if the site is not found or if the user does not have "read" access permission for the site `siteId`.

An empty string `""` is returned in two cases:
- if no binding is found for the passed `type`,
- if `type` is passed as something other than a string or an array ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: type"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `siteId`, `type`, or both parameters are missing ||
|| `ACCESS_DENIED` | Access to the method call is denied: insufficient rights to work with the Sites and Stores section ||
|| `ERROR_ARGUMENT` | An incorrect data type was passed in the `siteId`, `type`, or `additional` parameter ||
|| `SYSTEM_ERROR` | An internal error occurred during the execution of the method ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-syspage-set.md)
- [{#T}](./landing-syspage-get.md)
- [{#T}](./landing-syspage-delete-for-landing.md)
- [{#T}](./landing-syspage-delete-for-site.md)