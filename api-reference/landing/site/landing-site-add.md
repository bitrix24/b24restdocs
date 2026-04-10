# Add site landing.site.add

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: a user with "create" access permission for sites  

The method `landing.site.add` creates a new site and returns the identifier of the created site.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../data-types.md) | Internal scope of landings. It is not related to the REST scope `landing` in the method name. 
For `GROUP`, `KNOWLEDGE`, and `MAINPAGE`, the value of `scope` must match the value of `fields.TYPE` [(detailed description)](#type-scope) ||
|| **fields***
[`object`](../../data-types.md) | Set of fields for the new site [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE***
[`string`](../../data-types.md) | Title of the site, up to `255` characters ||
|| **CODE***
[`string`](../../data-types.md) | Symbolic code of the site. If an empty string is passed, the code will be generated from the `TITLE` field. For codes that consist only of digits, a prefix `site` will be automatically added. 

If such a value already exists in the domain, a numeric suffix will be added, for example, `code2` or `code3` ||
|| **TYPE**
[`string`](../../data-types.md) | Type of the site. Default is `PAGE`. Supported types are `PAGE`, `STORE`, `SMN`, `KNOWLEDGE`, `GROUP`, `MAINPAGE`. 

The value must correspond to the `scope` parameter [(detailed description)](#type-scope) ||
|| **DOMAIN_ID**
[`integer`](../../data-types.md) \| [`string`](../../data-types.md) | Domain of the site. The value depends on the platform.

For Bitrix24, specify the domain name of the site. If the parameter is not filled or an empty string is passed, the domain will be generated automatically based on the system template.

For 1C-Bitrix: Site Management, you need to pass the identifier of an already existing domain. Domain name as a string is not supported, the method will return an error ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | Description of the site, up to `255` characters ||
|| **XML_ID**
[`string`](../../data-types.md) | External identifier, up to `255` characters ||
|| **ADDITIONAL_FIELDS**
[`object`](../../data-types.md) | Additional fields of the site, stored separately after creation ||
|#

### Correspondence between TYPE and scope {#type-scope}

Site types and rules for selecting the `scope` parameter are described in the article [Working with site types and scopes](../types.md).

#|
|| **fields.TYPE** | **scope in request** | **When to use** ||
|| `PAGE`, `STORE`, `SMN` | do not pass | Site or store when the `scope` parameter is not passed ||
|| `GROUP` | `GROUP` | Group site ||
|| `KNOWLEDGE` | `KNOWLEDGE` | Knowledge base ||
|| `MAINPAGE` | `MAINPAGE` | Main page or vibe ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "scope": "KNOWLEDGE",
        "fields": {
          "TITLE": "Company Knowledge Base",
          "CODE": "",
          "TYPE": "KNOWLEDGE",
          "DESCRIPTION": "Knowledge base site"
        }
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.add.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "scope": "KNOWLEDGE",
        "fields": {
          "TITLE": "Company Knowledge Base",
          "CODE": "",
          "TYPE": "KNOWLEDGE",
          "DESCRIPTION": "Knowledge base site"
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.add.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.add',
    		{
                scope: 'KNOWLEDGE',
    			fields: {
    				TITLE: 'Company Knowledge Base',
    				CODE: '',
    				TYPE: 'KNOWLEDGE',
    				DESCRIPTION: 'Knowledge base site'
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
                'landing.site.add',
                [
                    'scope' => 'KNOWLEDGE',
                    'fields' => [
                        'TITLE' => 'Company Knowledge Base',
                        'CODE' => '',
                        'TYPE' => 'KNOWLEDGE',
                        'DESCRIPTION' => 'Knowledge base site',
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding site: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.add',
        {
            scope: 'KNOWLEDGE',
            fields: {
                TITLE: 'Company Knowledge Base',
                CODE: '',
                TYPE: 'KNOWLEDGE',
                DESCRIPTION: 'Knowledge base site'
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
        'landing.site.add',
        [
            'scope' => 'KNOWLEDGE',
            'fields' => [
                'TITLE' => 'Company Knowledge Base',
                'CODE' => '',
                'TYPE' => 'KNOWLEDGE',
                'DESCRIPTION' => 'Knowledge base site',
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

HTTP status: **200**

```json
{
    "result": 159,
    "time": {
        "start": 1773161828,
        "finish": 1773161828.444154,
        "duration": 0.4441540241241455,
        "processing": 0,
        "date_start": "2026-03-10T19:57:08+01:00",
        "date_finish": "2026-03-10T19:57:08+01:00",
        "operating_reset_at": 1773162428,
        "operating": 0.14034819602966309
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../data-types.md) | Identifier of the created site ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "DOMAIN_IS_INCORRECT",
    "error_description": "The site address is entered incorrectly. You can only use the following characters: \"a-z\", \"0-9\", \"-\", \".\"."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Access to create a site is denied: insufficient rights to create a site or an unavailable `TYPE` for the current scope is passed ||
|| `SITE_LIMIT_REACHED` | Site limit reached: the tariff or license does not allow creating another site ||
|| `DOMAIN_NOT_FOUND` | Domain does not exist ||
|| `DOMAIN_IS_INCORRECT` | The site address is entered incorrectly: an incorrect format of the domain name is passed ||
|| `DOMAIN_EXIST_TRASH` | The domain is already linked to a site in the trash: first unlink the domain from the site in the trash ||
|| `DOMAIN_DISABLE` | The word bitrix cannot be used in the domain: restriction for domains in Bitrix24 ||
|| `DOMAIN_EXIST` | Such a domain already exists: the domain is already taken ||
|| `SLASH_IS_NOT_ALLOWED` | Slash is not allowed in the site address: the character `/` is passed in `fields.CODE` ||
|| `CONTROLLER_ERROR_BADRESPONSE` | Unrecognized response from the registration service: error from the external domain registration service ||
|| `CONTROLLER_ERROR_BADLICENSE` | License expired or key is incorrect ||
|| `CONTROLLER_ERROR_<ERROR_CODE>` | Error from the external domain registration service: the error code depends on the response from the domain registration controller ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-update.md)
- [{#T}](./landing-site-get-list.md)
- [{#T}](./landing-site-delete.md)