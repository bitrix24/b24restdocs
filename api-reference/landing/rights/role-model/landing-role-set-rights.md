# Set Role Permissions for the Site List landing.role.setRights

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator or user with "full access" permission to the "Sites and Stores" section

The method `landing.role.setRights` sets role permissions for sites. You can specify separate permissions for each site, while others will have default permissions. The new set of permissions completely replaces the previous one.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | The identifier of the role for which permissions need to be updated.

You can obtain the identifier using the [landing.role.getList](./landing-role-get-list.md) method.

If you pass the identifier of a non-existent role, the method will not return a separate error. ||
|| **rights***
[`object`](../../../data-types.md) \| [`array`](../../../data-types.md) | An object in the following format:

```json
{
    "0": ["read"],
    "<siteId>": ["read", "edit", "sett"]
}
```

where:
- `0` — default permission for sites without separate settings
- `<siteId>` — site identifier

The list of available permission codes is described [below](#right-codes), and the structure of the object is in the parameter table [rights](#rights).

The method completely replaces previously saved role permissions for sites. ||
|| **additional**
[`string[]`](../../../data-types.md) | Additional capabilities of the role.

**Possible values:**

- `menu24` — show the "Sites and Stores" menu item for the role
- `create` — allow creating new sites

If the parameter is not passed, the current additional capabilities of the role will remain unchanged. ||
|#

### Parameter rights {#rights}

#|
|| **Name**
`type` | **Description** ||
|| **`0`**
[`string[]`](../../../data-types.md) | Default permissions for the role for all sites that do not have separate settings.

Available permission codes are described [below](#right-codes). ||
|| **`<siteId>`**
[`string[]`](../../../data-types.md) | Role permissions for the site with the specified identifier.

The key is the site identifier, and the value is an array of permission codes. If a site with that identifier is not found, the entry will be skipped without an error.

You can obtain the site identifier using the [landing.site.getList](../../site/landing-site-get-list.md) method or from the result of the [landing.site.add](../../site/landing-site-add.md) method.

For each site, pass an array of permission codes. If a different value is passed instead of an array, the entry for that site will be skipped without an error. ||
|#

### Permission Codes {#right-codes}

#|
|| **Code** | **Description** ||
|| `denied` | Access to the site is denied. ||
|| `read` | View the site. ||
|| `edit` | Modify site pages. ||
|| `sett` | Change site settings. ||
|| `public` | Publish. ||
|| `delete` | Move to trash and restore from trash. ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 11,
        "rights": {
          "0": ["read"],
          "66": ["read", "edit", "sett"],
          "71": ["denied"]
        },
        "additional": ["menu24", "create"]
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.role.setRights.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 11,
        "rights": {
          "0": ["read"],
          "66": ["read", "edit", "sett"],
          "71": ["denied"]
        },
        "additional": ["menu24", "create"],
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.role.setRights.json"
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'landing.role.setRights',
            {
                id: 11,
                rights: {
                    0: ['read'],
                    66: ['read', 'edit', 'sett'],
                    71: ['denied']
                },
                additional: ['menu24', 'create']
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
                'landing.role.setRights',
                [
                    'id' => 11,
                    'rights' => [
                        0 => ['read'],
                        66 => ['read', 'edit', 'sett'],
                        71 => ['denied'],
                    ],
                    'additional' => ['menu24', 'create'],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting role rights: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.role.setRights',
        {
            id: 11,
            rights: {
                0: ['read'],
                66: ['read', 'edit', 'sett'],
                71: ['denied']
            },
            additional: ['menu24', 'create']
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
        'landing.role.setRights',
        [
            'id' => 11,
            'rights' => [
                0 => ['read'],
                66 => ['read', 'edit', 'sett'],
                71 => ['denied'],
            ],
            'additional' => ['menu24', 'create'],
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
        "start": 1775071662,
        "finish": 1775071663.148474,
        "duration": 1.1484739780426025,
        "processing": 0,
        "date_start": "2026-04-01T22:27:42+02:00",
        "date_finish": "2026-04-01T22:27:43+02:00",
        "operating_reset_at": 1775072263,
        "operating": 0.1147608757019043
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | The result of the call.

The method returns `true` if the request was processed without access or system errors.

The value `true` does not guarantee that permissions were recorded for each provided site. If a site is not found or the format of one of the entries is incorrect, that entry will be skipped without an error.

After the call, check the saved set of permissions using the [landing.role.getRights](./landing-role-get-rights.md) method. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough parameters for the call, missing: rights"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Not enough permissions to work with the "Sites and Stores" section. ||
|| `IS_NOT_ADMIN` | The method requires administrator rights or "full access" permission to the "Sites and Stores" section. ||
|| `FEATURE_NOT_AVAIL` | Permission management in the "Sites and Stores" section is not available on the current plan. ||
|| `MISSING_PARAMS` | The required parameter `id` or `rights` is missing. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-role-get-list.md)
- [{#T}](./landing-role-get-rights.md)
- [{#T}](./landing-role-set-access-codes.md)