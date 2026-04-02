# Get a List of Special Pages from landing.syspage.get

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" access permission for the site

The method `landing.syspage.get` returns a list of special pages for the site.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the site.

The site identifier can be obtained using the [landing.site.getList](../../site/landing-site-get-list.md) method or from the result of the [landing.site.add](../../site/landing-site-add.md) method ||
|| **active**
[`boolean`](../../../data-types.md) | Return only pages that are not deleted and are active.

The filter is considered enabled if `true` is passed. By default, it is `false` ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1390,
        "active": true
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.syspage.get.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 1390,
        "active": true,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.syspage.get.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.syspage.get',
    		{
    			id: 1390,
    			active: true
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
                'landing.syspage.get',
                [
                    'id' => 1390,
                    'active' => true,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting special pages: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.syspage.get',
        {
            id: 1390,
            active: true
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
        'landing.syspage.get',
        [
            'id' => 1390,
            'active' => true,
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
    "result": {
        "mainpage": {
            "TYPE": "mainpage",
            "LANDING_ID": "711",
            "TITLE": "Classic Store. Clothing",
            "DELETED": "N",
            "ACTIVE": "Y"
        },
        "catalog": {
            "TYPE": "catalog",
            "LANDING_ID": "713",
            "TITLE": "Catalog",
            "DELETED": "N",
            "ACTIVE": "Y"
        },
        "personal": {
            "TYPE": "personal",
            "LANDING_ID": "717",
            "TITLE": "Personal Section",
            "DELETED": "N",
            "ACTIVE": "Y"
        },
        "cart": {
            "TYPE": "cart",
            "LANDING_ID": "2261",
            "TITLE": "Cart",
            "DELETED": "N",
            "ACTIVE": "Y"
        },
        "order": {
            "TYPE": "order",
            "LANDING_ID": "721",
            "TITLE": "Order Checkout",
            "DELETED": "N",
            "ACTIVE": "Y"
        },
        "compare": {
            "TYPE": "compare",
            "LANDING_ID": "749",
            "TITLE": "Comparison",
            "DELETED": "N",
            "ACTIVE": "Y"
        },
        "payment": {
            "TYPE": "payment",
            "LANDING_ID": "751",
            "TITLE": "Order Payment",
            "DELETED": "N",
            "ACTIVE": "Y"
        }
    },
    "time": {
        "start": 1774349441,
        "finish": 1774349441.726663,
        "duration": 0.7266631126403809,
        "processing": 0,
        "date_start": "2026-03-24T13:50:41+01:00",
        "date_finish": "2026-03-24T13:50:41+01:00",
        "operating_reset_at": 1774350041,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`any`](../../../data-types.md) | The result of the request execution.

The method returns an object with special pages grouped by type code [(detailed description)](#result).

If the site is not found or the user does not have "read" access permission for the site, the method will return `null`.

If access to the site is available but no special pages are assigned, the method will return an empty object `{}`. The same response will be returned if the filter `active = true` is enabled and no pages match it ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **type code**
[`object`](../../../data-types.md) | Object binding of the special page [(detailed description)](#syspage-item)

Key of the `result` object.

Possible values:
`mainpage` — main page
`catalog` — main catalog page
`personal` — personal section
`cart` — cart
`order` — order checkout
`payment` — payment page
`compare` — comparison page
`feedback` — feedback page ||
|#

#### Special Page Binding Object {#syspage-item}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE**
[`string`](../../../data-types.md) | Code of the special page type.

The value matches the key in the `result` object ||
|| **LANDING_ID**
[`string`](../../../data-types.md) | Identifier of the page designated as special for the site.

Returned as a string in the response ||
|| **TITLE**
[`string`](../../../data-types.md) | Title of the page ||
|| **DELETED**
[`string`](../../../data-types.md) | Flag indicating if the page is deleted.

Possible values:
`Y` — page is deleted
`N` — page is not deleted ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Flag indicating if the page is active.

Possible values:
`Y` — page is active
`N` — page is inactive ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Insufficient call parameters, missing: id"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | The required parameter `id` was not provided ||
|| `ACCESS_DENIED` | Access to the method call is denied: the user does not have access to the method or the `landing` module ||
|| `TYPE_ERROR` | Data type error in the `id` parameter ||
|| `SYSTEM_ERROR` | Internal error during method execution ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-syspage-set.md)
- [{#T}](./landing-syspage-get-special-page.md)
- [{#T}](./landing-syspage-delete-for-landing.md)
- [{#T}](./landing-syspage-delete-for-site.md)