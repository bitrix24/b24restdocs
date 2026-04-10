# Delete Registered Template landing.demos.unregister

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with View access permission in the Sites section

The method `landing.demos.unregister` deletes a registered template by its code.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code**^*^
[`string`](../../data-types.md) | External code of the template.

The code can be obtained, for example, from the result of the method [landing.demos.getList](./landing-demos-get-list.md) in the `XML_ID` field ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Example of deleting a template, where:
- `code` — code of the template to be deleted

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "ftmlt"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.demos.unregister.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "code": "ftmlt",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.demos.unregister.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.demos.unregister',
    		{
    			code: 'ftmlt'
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
                'landing.demos.unregister',
                [
                    'code' => 'ftmlt',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error unregistering demo: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.demos.unregister',
        {
            code: 'ftmlt'
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
        'landing.demos.unregister',
        [
            'code' => 'ftmlt',
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
        "start": 1774619450,
        "finish": 1774619450.991452,
        "duration": 0.9914519786834717,
        "processing": 0,
        "date_start": "2026-03-27T16:50:50+01:00",
        "date_finish": "2026-03-27T16:50:50+01:00",
        "operating_reset_at": 1774620050,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of the deletion:

- `true` — at least one template with the specified code was found and successfully deleted
- `false` — code not found, empty, or not provided as a string ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

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
|| `MISSING_PARAMS` | Insufficient call parameters, missing: code | Method call without `code` ||
|| `ACCESS_DENIED` | Insufficient permissions | User did not pass general access checks ||
|| `-` | Template deletion error | Failed to delete the template ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-demos-register.md)
- [{#T}](./landing-demos-get-site-list.md)
- [{#T}](./landing-demos-get-page-list.md)
- [{#T}](./landing-demos-get-list.md)
- [{#T}](./localization.md)
- [{#T}](./index.md)