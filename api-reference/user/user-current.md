# Get information about the current user user.current

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `user.current` retrieves information about the [current](*current_key*) user.

{% note info "" %}

The list of Bitrix24 user fields that will be received as a result of the method execution depends on the application/webhook scope. Details about access to user data can be found in the [article](index.md).

{% endnote %}

The method has no parameters. However, by making a REST request to the `DOMAIN` domain using data from `$_REQUEST` and adding `AUTH_ID` to the request for Bitrix24 access, you can determine which user opened the page in the Bitrix24 context.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.current
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/user.current
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type UserCurrentResult = {
      ID: string
      ACTIVE: boolean
      NAME: string
      LAST_NAME: string
      EMAIL: string
      LAST_LOGIN: ISODate | ''
      DATE_REGISTER: ISODate | ''
      IS_ONLINE: string
      LAST_ACTIVITY_DATE: string | null
      PERSONAL_GENDER: string
      PERSONAL_BIRTHDAY: string
      WORK_POSITION: string
      UF_EMPLOYMENT_DATE: string
      UF_DEPARTMENT: number[]
    }

    try {
      const response = await $b24.actions.v2.call.make<UserCurrentResult>({
        method: 'user.current',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Current user:', result.ID, result.NAME, result.LAST_NAME)
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
      async function getCurrentUser() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'user.current',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Current user:', result.ID, result.NAME, result.LAST_NAME)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getCurrentUser)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'user.current',
                []
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Current user data: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting current user data: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "user.current",
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
        'user.current',
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
            "ID":"3",
            "ACTIVE":true,
            "NAME":"John",
            "LAST_NAME":"Smith",
            "EMAIL":"test@gmail.com",
            "LAST_LOGIN":"2024-07-23T08:07:26+00:00",
            "DATE_REGISTER":"2024-07-22T00:00:00+00:00",
            "IS_ONLINE":"Y",
            "LAST_ACTIVITY_DATE":"2024-07-23 08:08:50",
            "PERSONAL_GENDER":"",
            "PERSONAL_BIRTHDAY":"",
            "WORK_POSITION":"",
            "UF_EMPLOYMENT_DATE":"",
            "UF_DEPARTMENT":[1]
        },
        "time":{
            "start":1721722262.960948,
            "finish":1721722262.985244,
            "duration":0.024296045303344727,
            "processing":0.0012989044189453125,
            "date_start":"2024-07-23T08:11:02+00:00",
            "date_finish":"2024-07-23T08:11:02+00:00",
            "operating":0
        }
    }
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response, which contains information about the user ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-add.md)
- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-search.md)
- [{#T}](./user-fields.md)
- [{#T}](../../tutorials/crm/how-to-get-lists/get-activity-list-by-deals.md)

[*key_current]: The user whose token you used during the REST call. If you are using a saved admin token, `administrator` will be returned. If you are using a token that arrives in a POST request to the application frame, it will be the user who entered the application.