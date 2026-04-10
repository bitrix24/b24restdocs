# Get a List of Included Areas for the Site landing.template.getSiteRef

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: user with "view" access permission for the site

The method `landing.template.getSiteRef` retrieves a list of included areas associated with the site.

The response includes only the bindings saved for the site. Bindings configured for individual pages of this site are not returned by the method. To obtain them, use [landing.template.getLandingRef](./landing-template-get-landing-ref.md).

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the site.

The site identifier can be obtained using the [landing.site.getList](../site/landing-site-get-list.md) method ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 157
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.template.getSiteRef.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 157,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.template.getSiteRef.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.template.getSiteRef',
            {
                id: 157
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
                'landing.template.getSiteRef',
                [
                    'id' => 157,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting site refs: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.template.getSiteRef',
        {
            id: 157
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
        'landing.template.getSiteRef',
        [
            'id' => 157,
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

If included areas are configured for the site, the method will return an object containing a list of these bindings. For each area, the identifier of the associated page is specified.

If no bindings for included areas are saved for the site, the method returns `true`.

```json
{
    "result": {
        "1": 735,
        "2": 731
    },
    "time": {
        "start": 1774879192,
        "finish": 1774879192.923787,
        "duration": 0.9237871170043945,
        "processing": 0,
        "date_start": "2026-03-30T16:59:52+02:00",
        "date_finish": "2026-03-30T16:59:52+02:00",
        "operating_reset_at": 1774879792,
        "operating": 0
    }
}
```

```json
{
    "result": true,
    "time": {
        "start": 1774765200,
        "finish": 1774765200.411258,
        "duration": 0.4112579822540283,
        "processing": 0,
        "date_start": "2026-03-29T10:00:00+02:00",
        "date_finish": "2026-03-29T10:00:00+02:00",
        "operating_reset_at": 1774765800,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) \| [`boolean`](../../data-types.md) | Bindings of the included areas of the site in the format `{"<AREA_ID>": <PAGE_ID>}`.

If bindings are saved for the site, the method returns an object. If there are no bindings, the method returns `true` [(detailed description)](#result) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **<AREA_ID>**
[`integer`](../../data-types.md) | Identifier of the included area of the template. The value for this key contains the identifier of the page assigned to this area ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ENTITY_NOT_FOUND",
    "error_description": "Entity not found"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameter `id` not provided ||
|| `ENTITY_NOT_FOUND` | Site not found or unavailable ||
|| `ACCESS_DENIED` | Insufficient rights to view the site ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-template-get-landing-ref.md)
- [{#T}](./landing-template-get-list.md)
- [{#T}](./landing-template-set-landing-ref.md)
- [{#T}](./landing-template-set-site-ref.md)