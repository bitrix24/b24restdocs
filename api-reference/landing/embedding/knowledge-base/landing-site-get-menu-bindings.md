# Get Menu Bindings with landing.site.getMenuBindings

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with View permission in the Sites section

The method `landing.site.getMenuBindings` returns Knowledge Base bindings to the menu.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **menuCode**
[`string`](../../../data-types.md) \| [`null`](../../../data-types.md) | Menu code for filtering.

If not provided, bindings for all menus will be returned.

`menuCode` can be obtained:
- in the interface through the "Select Knowledge Base" option: in the URL of the opened frame, the `menuId` parameter contains the menu code (for example, `menuId=crm_switcher:deal`)
- from the result of the method [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) in the `BINDING_ID` field ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Example of obtaining menu bindings, where:
- `menuCode` — menu code for filtering

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "menuCode": "crm_switcher:deal"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getMenuBindings.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "menuCode": "crm_switcher:deal",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getMenuBindings.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.getMenuBindings',
    		{
    			menuCode: 'crm_switcher:deal'
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
                'landing.site.getMenuBindings',
                [
                    'menuCode' => 'crm_switcher:deal',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting menu bindings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getMenuBindings',
        {
            menuCode: 'crm_switcher:deal'
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
        'landing.site.getMenuBindings',
        [
            'menuCode' => 'crm_switcher:deal',
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
    "result": [
        {
            "ENTITY_ID": "39",
            "ENTITY_TYPE": "S",
            "BINDING_ID": "socialnetwork:group_notifications",
            "TITLE": "Knowledge Base",
            "PUBLIC_URL": "https://bitrix24.com/knowledge/knowledge_base/"
        }
    ],
    "time": {
        "start": 1774956995,
        "finish": 1774956995.125436,
        "duration": 0.12543606758117676,
        "processing": 0,
        "date_start": "2026-03-31T14:36:35+02:00",
        "date_finish": "2026-03-31T14:36:35+02:00",
        "operating_reset_at": 1774957595,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | List of menu bindings [more details](#menu-binding-item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Result Item Type {#menu-binding-item}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) | Site identifier ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | Object type:

- `S` — site
- `L` — landing ||
|| **BINDING_ID**
[`string`](../../../data-types.md) | Menu code ||
|| **TITLE**
[`string`](../../../data-types.md) | Name of the bound site ||
|| **PUBLIC_URL**
[`string`](../../../data-types.md) | Public URL of the bound site ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Insufficient permissions."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `TYPE_ERROR` | Data type error | The `menuCode` parameter is passed in an incompatible type ||
|| `ACCESS_DENIED` | Insufficient permissions | The user did not pass general access checks ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-binding-to-menu.md)
- [{#T}](./landing-site-unbinding-from-menu.md)
- [{#T}](./landing-site-get-group-bindings.md)
- [{#T}](./landing-site-binding-to-group.md)
- [{#T}](./index.md)