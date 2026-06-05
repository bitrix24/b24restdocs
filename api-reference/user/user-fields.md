# Get user fields user.fields

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `user.fields` method allows you to retrieve a list of user field names. The method returns a standard list of fields; the use of custom fields is not supported.

{% note info "" %}

The list of Bitrix24 user fields that will be retrieved as a result of the method execution depends on the scope of the application/webhook. Details regarding access to user data can be found in the [article](index.md).

{% endnote %}

No parameters.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.fields
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/user.fields
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UserFieldsResult = {
      [field: string]: string
    }

    try {
      const response = await $b24.actions.v2.call.make<UserFieldsResult>({
        method: 'user.fields',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('User fields:', result)
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
      async function fetchUserFields() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'user.fields',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('User fields:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchUserFields)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'user.fields',
                []
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
        echo 'Error fetching user fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "user.fields",
        {},
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
        'user.fields',
        []
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
        "result":{
            "ID":"ID",
            "XML_ID":"External code",
            "ACTIVE":"Activity",
            "NAME":"First Name",
            "LAST_NAME":"Last Name",
            "SECOND_NAME":"Middle Name",
            "TITLE":"User List",
            "EMAIL":"E-Mail",
            "LAST_LOGIN":"Last Authorization",
            "DATE_REGISTER":"Registration Date",
            "TIME_ZONE":"TIME_ZONE",
            "IS_ONLINE":"IS_ONLINE",
            "TIMESTAMP_X":"TIMESTAMP_X",
            "LAST_ACTIVITY_DATE":"LAST_ACTIVITY_DATE",
            "PERSONAL_GENDER":"Gender",
            "PERSONAL_PROFESSION":"Profession",
            "PERSONAL_WWW":"Homepage",
            "PERSONAL_BIRTHDAY":"Date of Birth",
            "PERSONAL_PHOTO":"Photo",
            "PERSONAL_ICQ":"ICQ",
            "PERSONAL_PHONE":"Personal Phone",
            "PERSONAL_FAX":"Fax",
            "PERSONAL_MOBILE":"Personal Mobile",
            "PERSONAL_PAGER":"Pager",
            "PERSONAL_STREET":"Residential Street",
            "PERSONAL_CITY":"Residential City",
            "PERSONAL_STATE":"State / Region",
            "PERSONAL_ZIP":"Zip Code",
            "PERSONAL_COUNTRY":"Country",
            "PERSONAL_MAILBOX":"P.O. Box",
            "PERSONAL_NOTES":"Additional Notes",
            "WORK_PHONE":"Company Phone",
            "WORK_COMPANY":"Company",
            "WORK_POSITION":"Job Title",
            "WORK_DEPARTMENT":"Department",
            "WORK_WWW":"Company Website",
            "WORK_FAX":"WORK_FAX",
            "WORK_PAGER":"WORK_PAGER",
            "WORK_STREET":"WORK_STREET",
            "WORK_MAILBOX":"WORK_MAILBOX",
            "WORK_CITY":"Work City",
            "WORK_STATE":"WORK_STATE",
            "WORK_ZIP":"WORK_ZIP",
            "WORK_COUNTRY":"WORK_COUNTRY",
            "WORK_PROFILE":"WORK_PROFILE",
            "WORK_LOGO":"WORK_LOGO",
            "WORK_NOTES":"WORK_NOTES",
            "UF_SKYPE_LINK":"Skype Chat Link",
            "UF_ZOOM":"Zoom",
            "UF_EMPLOYMENT_DATE":"Hire Date",
            "UF_TIMEMAN":"Time Tracking",
            "UF_DEPARTMENT":"Units",
            "UF_INTERESTS":"Interests",
            "UF_SKILLS":"Skills",
            "UF_WEB_SITES":"Other Websites",
            "UF_XING":"Xing",
            "UF_LINKEDIN":"LinkedIn",
            "UF_TWITTER":"Twitter",
            "UF_SKYPE":"Skype",
            "UF_DISTRICT":"District",
            "UF_PHONE_INNER":"Extension",
            "USER_TYPE":"USER_TYPE"
        },
        "time":{
            "start":1721719975.764591,
            "finish":1721719975.786585,
            "duration":0.02199411392211914,
            "processing":0.0005099773406982422,
            "date_start":"2024-07-23T07:32:55+00:00",
            "date_finish":"2024-07-23T07:32:55+00:00",
            "operating":0
        }
    }
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response, which contains custom fields ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-add.md)
- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-current.md)
- [{#T}](./user-search.md)