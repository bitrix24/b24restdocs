# Set Role Permissions for the Site List landing.role.setRights

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator or user with "full access" permission to the "Sites and Stores" section

The method `landing.role.setRights` sets role permissions for sites. You can specify separate permissions for each site, while others will have default permissions. The new set of permissions completely replaces the previous one.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | The section the role belongs to. This parameter is not related to the REST scope `landing` in the method name.

The values `GROUP`, `KNOWLEDGE`, and `MAINPAGE` correspond to the types of sites described in the article [Working with Site Types and Scopes](../../types.md).

Possible values:
`GROUP` - roles for group sites
`KNOWLEDGE` - roles for knowledge bases
`MAINPAGE` - roles for the main page or vibe

If the parameter is not provided, the method works with roles for sites and online stores. For a role from another section, the method returns the error `ROLE_SCOPE_MISMATCH` ||
|| **id***
[`integer`](../../../data-types.md) | The identifier of the role for which permissions need to be updated.

You can obtain the identifier using the [landing.role.getList](./landing-role-get-list.md) method.

If you pass the identifier of a non-existent role, the method returns the error `ROLE_SCOPE_MISMATCH`. ||
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

- `menu24` — show the menu item of the section for the role
- `create` — allow creating new sites, knowledge bases, or pages in the section

The codes depend on the section specified in the `scope` parameter. For knowledge bases, group sites, and the main page, the codes have a prefix — `knowledge_`, `group_`, and `vibe_` respectively. For example, for a knowledge base you need to pass `knowledge_create`.

The method does not save codes that belong to another section.

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'landing.role.setRights',
        params: {
          id: 11,
          rights: {
            0: ['read'],
            66: ['read', 'edit', 'sett'],
            71: ['denied'],
          },
          additional: ['menu24', 'create'],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Rights set successfully:', result)
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function setRoleRights() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.role.setRights',
            params: {
              id: 11,
              rights: {
                0: ['read'],
                66: ['read', 'edit', 'sett'],
                71: ['denied'],
              },
              additional: ['menu24', 'create'],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Rights set successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setRoleRights)
    </script>
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "landing.role.setRights", b24.Params{
    	"id": 11,
    	"rights": b24.Params{
    		"0":  []string{"read"},
    		"66": []string{"read", "edit", "sett"},
    		"71": []string{"denied"},
    	},
    	"additional": []string{"menu24", "create"},
    })
    if err != nil {
    	return fmt.Errorf("landing.role.setRights: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
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
|| `ROLE_SCOPE_MISMATCH` | The role does not belong to the section specified in the `scope` parameter. The method returns this error both for a role from another section and for a role that does not exist. ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-role-get-list.md)
- [{#T}](./landing-role-get-rights.md)
- [{#T}](./landing-role-set-access-codes.md)