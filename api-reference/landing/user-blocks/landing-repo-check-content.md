# Check Content for Dangerous Substrings landing.repo.checkContent

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with View access permission in the Sites section

The method `landing.repo.checkContent` checks content through a sanitizer.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **content**^*^
[`string`](../../data-types.md) | Content to be checked ||
|| **splitter**
[`string`](../../data-types.md) | A delimiter that marks dangerous fragments in `content`.

Default: `#SANITIZE#` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of content checking, where:
- `content` — HTML to be checked
- `splitter` — marker string for dangerous fragments

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "content": "<div style=\"color:red\" onclick=\"alert(1)\"><iframe src=\"//evil.com\"></iframe></div>",
        "splitter": "#AAA#"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.repo.checkContent.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "content": "<div style=\"color:red\" onclick=\"alert(1)\"><iframe src=\"//evil.com\"></iframe></div>",
        "splitter": "#AAA#",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.repo.checkContent.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.repo.checkContent',
    		{
    			content: '<div style="color:red" onclick="alert(1)"><iframe src="//evil.com"></iframe></div>',
    			splitter: '#AAA#'
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
                'landing.repo.checkContent',
                [
                    'content' => '<div style="color:red" onclick="alert(1)"><iframe src="//evil.com"></iframe></div>',
                    'splitter' => '#AAA#',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error checking content: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.repo.checkContent',
        {
            content: '<div style="color:red" onclick="alert(1)"><iframe src="//evil.com"></iframe></div>',
            splitter: '#AAA#'
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
        'landing.repo.checkContent',
        [
            'content' => '<div style="color:red" onclick="alert(1)"><iframe src="//evil.com"></iframe></div>',
            'splitter' => '#AAA#',
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
        "is_bad": true,
        "content": "\u003Cdiv style=\u0022color:red\u0022 oncl#AAA#ick=\u0022alert(1)\u0022\u003E\u003Cifr#AAA#ame src=\u0022\/\/evil.com\u0022\u003E\u003C\/iframe\u003E\u003C\/div\u003E"
    },
    "time": {
        "start": 1774952664,
        "finish": 1774952665.017161,
        "duration": 1.0171608924865723,
        "processing": 0,
        "date_start": "2026-03-31T13:24:24+02:00",
        "date_finish": "2026-03-31T13:24:25+02:00",
        "operating_reset_at": 1774953265,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Result of the check [more details](#result-data) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Type result {#result-data}

#|
|| **Name**
`type` | **Description** ||
|| **is_bad**
[`boolean`](../../data-types.md) | Indicator of dangerous fragments in the content ||
|| **content**
[`string`](../../data-types.md) | Content after being processed by the sanitizer ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "The value of an argument 'content' has an invalid type",
    "argument": "content"
}
```

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Insufficient permissions."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MISSING_PARAMS` | Not enough parameters for the call, missing: content | Method call without `content` ||
|| `ERROR_ARGUMENT` | The value of an argument 'content' has an invalid type | Parameter `content` passed in an incorrect type ||
|| `ACCESS_DENIED` | Insufficient permissions | User did not pass general access checks ||
|| `insufficient_scope` | Token lacks sufficient scope | Token does not contain `landing` scope ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-repo-register.md)
- [{#T}](./landing-repo-get-list.md)
- [{#T}](./landing-repo-unregister.md)
- [{#T}](./index.md)