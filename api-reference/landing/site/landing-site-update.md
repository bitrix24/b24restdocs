# Update Site landing.site.update

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify settings" access permission for the site

The method `landing.site.update` updates the parameters of the site.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Site identifier.

The site identifier can be obtained using the [landing.site.getList](./landing-site-get-list.md) method or from the result of the [landing.site.add](./landing-site-add.md) method. ||
|| **fields***
[`object`](../../data-types.md) | A set of fields to update the site [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../data-types.md) | Site title, up to `255` characters long. ||
|| **CODE**
[`string`](../../data-types.md) | Symbolic code of the site. When an empty string is passed, the code is generated from `TITLE` or from the string `site`. 

The code cannot contain `/`, and a code consisting solely of digits receives the prefix `site`. The maximum length of the code is `253` characters. 

If the code is already taken in the domain, a numeric suffix is automatically added to it. ||
|| **TYPE**
[`string`](../../data-types.md) | Type of the site. Supported types are `PAGE`, `STORE`, `SMN`, `KNOWLEDGE`, `GROUP`, `MAINPAGE`. 

This is the internal type of the landing page, which is associated with the internal `scope`. The `scope` parameter is not passed in `landing.site.update` [(detailed description)](../types.md) ||
|| **DOMAIN_ID**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Domain of the site. Usually, the domain name is passed. For sites of types `PAGE`, `STORE`, `SMN`, if the parameter is not passed or an empty string is passed, the parameter value is ignored. For `GROUP`, `KNOWLEDGE`, and `MAINPAGE`, the parameter is usually not passed.

For 1C-Bitrix: Site Management, pass the identifier of an existing domain. The domain name as a string is not supported, and the method will return an error. ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the site, up to `255` characters long. ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier of the site. ||
|| **LANDING_ID_INDEX**
[`integer`](../../data-types.md) | Identifier of the site page that will be the main one. ||
|| **LANDING_ID_404**
[`integer`](../../data-types.md) | Identifier of the 404 error page. ||
|| **LANDING_ID_503**
[`integer`](../../data-types.md) | Identifier of the 503 error page. ||
|#

Page identifiers for `LANDING_ID_INDEX`, `LANDING_ID_404`, and `LANDING_ID_503` can be obtained using the [landing.landing.getList](../page/methods/landing-landing-get-list.md) method.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 206,
        "fields": {
          "TITLE": "Support portal",
          "CODE": "support-portal",
          "DESCRIPTION": "Updated site description",
          "LANDING_ID_INDEX": 987
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.update.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 206,
        "fields": {
          "TITLE": "Support portal",
          "CODE": "support-portal",
          "DESCRIPTION": "Updated site description",
          "LANDING_ID_INDEX": 987
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.update.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.update',
    		{
    			id: 206,
    			fields: {
    				TITLE: 'Support portal',
    				CODE: 'support-portal',
    				DESCRIPTION: 'Updated site description',
    				LANDING_ID_INDEX: 987
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
                'landing.site.update',
                [
                    'id' => 206,
                    'fields' => [
                        'TITLE' => 'Support portal',
                        'CODE' => 'support-portal',
                        'DESCRIPTION' => 'Updated site description',
                        'LANDING_ID_INDEX' => 987,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating site: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.update',
        {
            id: 206,
            fields: {
                TITLE: 'Support portal',
                CODE: 'support-portal',
                DESCRIPTION: 'Updated site description',
                LANDING_ID_INDEX: 987
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
        'landing.site.update',
        [
            'id' => 206,
            'fields' => [
                'TITLE' => 'Support portal',
                'CODE' => 'support-portal',
                'DESCRIPTION' => 'Updated site description',
                'LANDING_ID_INDEX' => 987,
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
    "result": true,
    "time": {
        "start": 1773288229,
        "finish": 1773288229.999823,
        "duration": 0.9998230934143066,
        "processing": 0,
        "date_start": "2026-03-12T07:03:49+01:00",
        "date_finish": "2026-03-12T07:03:49+01:00",
        "operating_reset_at": 1773288829,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | `true` if the site was successfully updated. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough parameters provided, missing: id"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `id` or `fields` is missing. ||
|| `ACCESS_DENIED` | Insufficient rights to modify the site or specific fields. ||
|| `DOMAIN_NOT_FOUND` | A non-existent domain was specified. ||
|| `DOMAIN_IS_INCORRECT` | An incorrect format of the domain name was provided. ||
|| `DOMAIN_EXIST_TRASH` | The domain is already linked to a site in the trash. ||
|| `DOMAIN_DISABLE` | It is not allowed to use a prohibited domain name in Bitrix24. ||
|| `DOMAIN_EXIST` | The domain is already occupied. ||
|| `CODE_IS_NOT_UNIQUE` | The site code is not unique within the domain. ||
|| `SLASH_IS_NOT_ALLOWED` | The character `/` was passed in `fields.CODE`. ||
|| `CONTROLLER_ERROR_BADRESPONSE` | Unrecognized response from the external domain registration service. ||
|| `CONTROLLER_ERROR_BADLICENSE` | License error in the external domain registration service. ||
|| `CONTROLLER_ERROR_<ERROR_CODE>` | Error from the external domain registration service with code `<ERROR_CODE>`. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-add.md)
- [{#T}](./landing-site-get-list.md)
- [{#T}](./landing-site-publication.md)
- [{#T}](./landing-site-delete.md)