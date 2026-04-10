# Bind Knowledge Base to Menu landing.site.bindingToMenu

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with View permission in the Sites section and Placement permission in the Extensions section of the Knowledge Base

The method `landing.site.bindingToMenu` binds the Knowledge Base to the specified menu.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../../data-types.md) | Identifier of the Knowledge Base site.

`id` can be obtained, for example, from the method [landing.site.getList](../../site/landing-site-get-list.md) in the `ID` field ||
|| **menuCode**^*^
[`string`](../../../data-types.md) | Menu code.

`menuCode` can be obtained:
- in the interface through the "Select Knowledge Base" option: in the URL of the opened frame, the `menuId` parameter contains the menu code (for example, `menuId=crm_switcher:deal`)
- from the result of the method [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) in the `BINDING_ID` field ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of binding the Knowledge Base to a menu, where:
- `id` — identifier of the Knowledge Base site
- `menuCode` — menu code

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 31,
        "menuCode": "crm_switcher:deal"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.bindingToMenu.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 31,
        "menuCode": "crm_switcher:deal",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.bindingToMenu.json"
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'landing.site.bindingToMenu',
    		{
    			id: 31,
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
                'landing.site.bindingToMenu',
                [
                    'id' => 31,
                    'menuCode' => 'crm_switcher:deal',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error binding site to menu: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.bindingToMenu',
        {
            id: 31,
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
        'landing.site.bindingToMenu',
        [
            'id' => 31,
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
    "result": true,
    "time": {
        "start": 1774952664,
        "finish": 1774952665.017161,
        "duration": 1.0171608924865723,
        "processing": 0,
        "date_start": "2026-03-31T13:24:24+03:00",
        "date_finish": "2026-03-31T13:24:25+03:00",
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
[`boolean`](../../../data-types.md) | Binding result:

- `true` — binding completed
- `false` — binding not completed ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: menuCode"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MISSING_PARAMS` | Not enough call parameters, missing: menuCode | Method call without `menuCode` ||
|| `MISSING_PARAMS` | Not enough call parameters, missing: id | Method call without `id` ||
|| `TYPE_ERROR` | Bitrix\\Landing\\PublicAction\\Site::bindingToMenu(): Argument #1 ($id) must be of type int, string given | Values provided are incompatible with the method signature ||
|| `ACCESS_DENIED` | Insufficient permissions | User did not pass general access checks ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-unbinding-from-menu.md)
- [{#T}](./landing-site-get-menu-bindings.md)
- [{#T}](./landing-site-binding-to-group.md)
- [{#T}](./landing-site-get-group-bindings.md)
- [{#T}](./index.md)