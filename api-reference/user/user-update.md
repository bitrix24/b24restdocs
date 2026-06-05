# Update user data user.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`user`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The `user.update` method updates user data. This is only possible on behalf of a user with user invitation rights.

## Method Parameters

{% include [Note on parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../data-types.md) | User identifier ||
|| **ACTIVE**
[`boolean`](../data-types.md) | User activity flag. Possible values: 
- `Y` — employee is active,
- `N` — employee is dismissed ||
|| **EMAIL**
[`string`](../data-types.md) | User E-mail ||
|| **NAME**
[`string`](../data-types.md) | First name ||
|| **LAST_NAME**
[`string`](../data-types.md) | Last name ||
|| **SECOND_NAME**
[`string`](../data-types.md) | Middle name ||
|| **PERSONAL_GENDER**
[`string`](../data-types.md) | Gender ||
|| **PERSONAL_PROFESSION**
[`string`](../data-types.md) | Profession ||
|| **PERSONAL_WWW**
[`string`](../data-types.md) | Homepage ||
|| **PERSONAL_BIRTHDAY**
[`string`](../data-types.md) | Date of birth ||
|| **PERSONAL_PHOTO**
[`array`](../data-types.md) | Photo, pass an array containing the file name and a [Base64](../files/how-to-upload-files.md) string ||
|| **PERSONAL_ICQ**
[`string`](../data-types.md) | ICQ ||
|| **PERSONAL_PHONE**
[`string`](../data-types.md) | Personal phone ||
|| **PERSONAL_FAX**
[`string`](../data-types.md) | Fax ||
|| **PERSONAL_MOBILE**
[`string`](../data-types.md) | Personal mobile ||
|| **PERSONAL_PAGER**
[`string`](../data-types.md) | Pager ||
|| **PERSONAL_STREET**
[`string`](../data-types.md) | Residential street ||
|| **PERSONAL_CITY**
[`string`](../data-types.md) | Residential city ||
|| **PERSONAL_STATE**
[`string`](../data-types.md) | Region / territory ||
|| **PERSONAL_ZIP**
[`string`](../data-types.md) | Postal code ||
|| **PERSONAL_COUNTRY**
[`string`](../data-types.md) | Country ||
|| **PERSONAL_MAILBOX**
[`string`](../data-types.md) | P.O. Box ||
|| **PERSONAL_NOTES**
[`string`](../data-types.md) | Additional notes ||
|| **WORK_PHONE**
[`string`](../data-types.md) | Company phone ||
|| **WORK_COMPANY**
[`string`](../data-types.md) | Company ||
|| **WORK_POSITION**
[`string`](../data-types.md) | Job title ||
|| **WORK_DEPARTMENT**
[`string`](../data-types.md) | Department ||
|| **WORK_WWW**
[`string`](../data-types.md) | Company website ||
|| **WORK_FAX**
[`string`](../data-types.md) | WORK_FAX ||
|| **WORK_PAGER**
[`string`](../data-types.md) | WORK_PAGER ||
|| **WORK_STREET**
[`string`](../data-types.md) | WORK_STREET ||
|| **WORK_MAILBOX**
[`string`](../data-types.md) | WORK_MAILBOX ||
|| **WORK_CITY**
[`string`](../data-types.md) | Work city ||
|| **WORK_STATE**
[`string`](../data-types.md) | WORK_STATE ||
|| **WORK_ZIP**
[`string`](../data-types.md) | WORK_ZIP ||
|| **WORK_COUNTRY**
[`string`](../data-types.md) | WORK_COUNTRY ||
|| **WORK_PROFILE**
[`string`](../data-types.md) | WORK_PROFILE ||
|| **WORK_LOGO**
[`array`](../data-types.md) | WORK_LOGO ||
|| **WORK_NOTES**
[`string`](../data-types.md) | WORK_NOTES ||
|| **UF_SKYPE_LINK**
[`string`](../data-types.md) | Skype chat link ||
|| **UF_ZOOM**
[`string`](../data-types.md) | Zoom ||
|| **UF_DEPARTMENT**
[`string`](../data-types.md) | Department ||
|| **UF_INTERESTS**
[`string`](../data-types.md) | Interests ||
|| **UF_SKILLS**
[`string`](../data-types.md) | Skills ||
|| **UF_WEB_SITES**
[`string`](../data-types.md) | Other websites ||
|| **UF_XING**
[`string`](../data-types.md) | Xing ||
|| **UF_LINKEDIN**
[`string`](../data-types.md) | LinkedIn ||
|| **UF_FACEBOOK**
[`string`](../data-types.md) | Facebook** ||
|| **UF_TWITTER**
[`string`](../data-types.md) | Twitter ||
|| **UF_SKYPE**
[`string`](../data-types.md) | Skype ||
|| **UF_DISTRICT**
[`string`](../data-types.md) | District ||
|| **UF_PHONE_INNER**
[`string`](../data-types.md) | Extension number ||
|#

\
**Belongs to Meta Platforms, Inc., which is recognized as extremist and banned in the Russian Federation.*

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "ID": 1,
        "NAME": "Administrator",
        "LAST_NAME": "SomeLastName"
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.update
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "ID": 1,
        "NAME": "Administrator",
        "LAST_NAME": "SomeLastName",
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UserUpdateResult = boolean

    try {
      const response = await $b24.actions.v2.call.make<UserUpdateResult>({
        method: 'user.update',
        params: {
          ID: 1,
          NAME: 'Administrator',
          LAST_NAME: 'SomeLastName',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User updated successfully:', result)
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
      async function updateUser() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'user.update',
            params: {
              ID: 1,
              NAME: 'Administrator',
              LAST_NAME: 'SomeLastName',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User updated successfully:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateUser)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'user.update',
                [
                    'ID'       => 1,
                    'NAME'     => 'Administrator',
                    'LAST_NAME' => 'SomeLastName',
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating user: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "user.update",
        {
            "ID": 1,
            "NAME": "Administrator",
            "LAST_NAME": "SomeLastName"
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'user.update',
        [
            'ID' => 1,
            'NAME' => 'Administrator',
            'LAST_NAME' => 'SomeLastName'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
    {
        "result": true,
        "time": {
            "start": 1721807581.02493,
            "finish": 1721807581.20039,
            "duration": 0.17546010017395,
            "processing": 0.133708000183105,
            "date_start": "2024-07-24T07:53:01+00:00",
            "date_finish": "2024-07-24T07:53:01+00:00",
            "operating": 0.133685827255249
        }
    }
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Success status ||
|| **time**
[`time`](../data-types.md) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "access_denied"
}
```

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error message** | **Description** ||
|| `ERROR_CORE` | access_denied | Invalid user `ID` passed ||
|| `ERROR_CORE` | access_denied | User does not have permission to call the method ||
|| `ERROR_CORE` |  | Invalid user `ID` passed||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-add.md)
- [{#T}](./user-get.md)
- [{#T}](./user-current.md)
- [{#T}](./user-search.md)
- [{#T}](./user-fields.md)