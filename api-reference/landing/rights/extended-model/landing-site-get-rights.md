# Get Access Permissions for landing.site.getRights

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "view" permission for the "Sites and Stores" section

The method `landing.site.getRights` retrieves the list of permissions for the current user for the specified site.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) | Identifier of the site.

The site identifier can be obtained using the method [landing.site.getList](../../site/landing-site-get-list.md) or from the result of the method [landing.site.add](../../site/landing-site-add.md)
||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 645
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getRights.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 645,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getRights.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.site.getRights',
            {
                id: 645
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
                'landing.site.getRights',
                [
                    'id' => 645,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting site rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getRights',
        {
            id: 645
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
        'landing.site.getRights',
        [
            'id' => 645,
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
        "read",
        "edit",
        "sett"
    ],
    "time": {
        "start": 1774765200,
        "finish": 1774765200.411258,
        "duration": 0.4112579822540283,
        "processing": 0,
        "date_start": "2026-03-29T10:00:00+01:00",
        "date_finish": "2026-03-29T10:00:00+01:00",
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
[`string[]`](../../../data-types.md) | Permission codes for the current user for the specified site.

The array may contain one or more values:
`denied` - access to the site is denied for the current user or one of their access groups
`read` - view the site
`edit` - modify the site pages
`sett` - change site settings
`public` - publish the site
`delete` - move the site to the trash and restore it from the trash

If different permissions are set for the user, `denied` may be returned along with granting codes.

If the site is not found or is inaccessible to the current user, the method returns an empty array ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: id"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | The user does not have access to the "Sites and Stores" section ||
|| `FEATURE_NOT_AVAIL` | Permission settings are not available on the current plan. To work with permissions, switch to a different plan ||
|| `MISSING_PARAMS` | The required parameter `id` is missing ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-set-rights.md)
- [{#T}](../landing-role-is-enabled.md)
- [{#T}](../landing-role-enable.md)