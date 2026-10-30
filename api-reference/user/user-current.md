# Get information about the current user user.current

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `user.current` returns the profile of the user whose token is used in the request: for a saved administrator token, this is the administrator; for a token from the application frame, this is the user who opened the application.

{% note info "" %}

The list of Bitrix24 user fields that will be received as a result of the method execution depends on the application/webhook scope. The fields available in each version are listed in the [User Scope Versions](user-scope.md) article.

{% endnote %}

## Method Parameters

No parameters.

To find out which user opened the application in the Bitrix24 frame, call the method with the `AUTH_ID` token from the request to the application on the `DOMAIN` domain.

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
    -d '{"auth":"**put_access_token_here**"}' \
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
      LAST_ACTIVITY_DATE?: string
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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.current().response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
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

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.current", nil)
    if err != nil {
    	return fmt.Errorf("user.current: %w", err)
    }

    var item struct {
    	ID        b24.ID `json:"ID"`
    	Active    bool   `json:"ACTIVE"`
    	Name      string `json:"NAME"`
    	LastName  string `json:"LAST_NAME"`
    	Email     string `json:"EMAIL"`
    	LastLogin string `json:"LAST_LOGIN"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID, item.Active)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": "1",
        "XML_ID": "10001",
        "ACTIVE": true,
        "NAME": "Klaus",
        "LAST_NAME": "Weber",
        "EMAIL": "weber@example.com",
        "LAST_LOGIN": "2026-10-29T15:13:52+01:00",
        "DATE_REGISTER": "2025-10-02T03:00:00+02:00",
        "TIME_ZONE": "Europe/Berlin",
        "IS_ONLINE": "Y",
        "TIMESTAMP_X": "10/27/2026 12:12:19 pm",
        "LAST_ACTIVITY_DATE": "2026-10-29 17:58:08",
        "PERSONAL_GENDER": "M",
        "PERSONAL_BIRTHDAY": "",
        "PERSONAL_PHOTO": "https://cdn-com.bitrix24.com/b1/main/a1b/a1b2c3/avatar.png",
        "PERSONAL_MOBILE": "+4915100000000",
        "WORK_POSITION": "Sales Manager",
        "UF_EMPLOYMENT_DATE": "",
        "UF_DEPARTMENT": [
            1
        ],
        "UF_USR_USER_DEALS": false
    },
    "time": {
        "start": 1793286068,
        "finish": 1793286068.341957,
        "duration": 0.34195709228515625,
        "processing": 0,
        "date_start": "2026-10-29T18:01:08+01:00",
        "date_finish": "2026-10-29T18:01:08+01:00",
        "operating_reset_at": 1793286660,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | User profile [(detailed description)](#user-fields) ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

#### User Fields {#user-fields}

The method returns only the fields allowed by the scope version. The list for each version is in the [User Scope Versions](user-scope.md) article. A field with a `null` value is omitted from the response; empty strings are returned.

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | User ID. A number as a string ||
|| **XML_ID**
[`string`](../data-types.md) | External user code ||
|| **ACTIVE**
[`boolean`](../data-types.md) | Whether the user is active:
- `true` — active
- `false` — dismissed ||
|| **NAME**
[`string`](../data-types.md) | First name ||
|| **LAST_NAME**
[`string`](../data-types.md) | Last name ||
|| **SECOND_NAME**
[`string`](../data-types.md) | Middle name ||
|| **TITLE**
[`string`](../data-types.md) | Salutation ||
|| **EMAIL**
[`string`](../data-types.md) | E-mail ||
|| **LAST_LOGIN**
[`string`](../data-types.md) | Date and time of the last login in ISO 8601 format, for example `2026-10-29T15:13:52+01:00` ||
|| **DATE_REGISTER**
[`string`](../data-types.md) | Registration date in ISO 8601 format ||
|| **TIME_ZONE**
[`string`](../data-types.md) | User time zone, for example `Europe/Berlin` ||
|| **IS_ONLINE**
[`string`](../data-types.md) | Whether the user is online:
- `Y` — online
- `N` — not online ||
|| **TIMESTAMP_X**
[`string`](../data-types.md) | Date and time of the last profile change in the site format, for example `10/27/2026 12:12:19 pm` ||
|| **LAST_ACTIVITY_DATE**
[`string`](../data-types.md) | Date and time of the last activity in the `YYYY-MM-DD HH:MM:SS` format ||
|| **PERSONAL_GENDER**
[`string`](../data-types.md) | Gender:
- `M` — male
- `F` — female
- empty string — not specified ||
|| **PERSONAL_PROFESSION**
[`string`](../data-types.md) | Profession ||
|| **PERSONAL_WWW**
[`string`](../data-types.md) | Personal website ||
|| **PERSONAL_BIRTHDAY**
[`string`](../data-types.md) | Date of birth in ISO 8601 format. An empty string if not specified ||
|| **PERSONAL_PHOTO**
[`string`](../data-types.md) | Profile photo link ||
|| **PERSONAL_ICQ**
[`string`](../data-types.md) | ICQ ||
|| **PERSONAL_PHONE**
[`string`](../data-types.md) | Personal phone ||
|| **PERSONAL_FAX**
[`string`](../data-types.md) | Personal fax ||
|| **PERSONAL_MOBILE**
[`string`](../data-types.md) | Personal mobile ||
|| **PERSONAL_PAGER**
[`string`](../data-types.md) | Pager ||
|| **PERSONAL_STREET**
[`string`](../data-types.md) | Residential street ||
|| **PERSONAL_CITY**
[`string`](../data-types.md) | Residential city ||
|| **PERSONAL_STATE**
[`string`](../data-types.md) | Residential region or territory ||
|| **PERSONAL_ZIP**
[`string`](../data-types.md) | Residential postal code ||
|| **PERSONAL_COUNTRY**
[`string`](../data-types.md) | Residential country code. `0` if the country is not specified ||
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
[`string`](../data-types.md) | Department, a text field. Departments from the company structure are stored in `UF_DEPARTMENT` ||
|| **WORK_WWW**
[`string`](../data-types.md) | Company website ||
|| **WORK_FAX**
[`string`](../data-types.md) | Work fax ||
|| **WORK_PAGER**
[`string`](../data-types.md) | Work pager ||
|| **WORK_STREET**
[`string`](../data-types.md) | Street of the company address ||
|| **WORK_MAILBOX**
[`string`](../data-types.md) | Company P.O. box ||
|| **WORK_CITY**
[`string`](../data-types.md) | Work city ||
|| **WORK_STATE**
[`string`](../data-types.md) | State or region of the company address ||
|| **WORK_ZIP**
[`string`](../data-types.md) | Company postal code ||
|| **WORK_COUNTRY**
[`string`](../data-types.md) | Country code of the company address. `0` if the country is not specified ||
|| **WORK_PROFILE**
[`string`](../data-types.md) | Company's areas of business ||
|| **WORK_LOGO**
[`string`](../data-types.md) | Company logo ||
|| **WORK_NOTES**
[`string`](../data-types.md) | Notes about the workplace ||
|| **UF_SKYPE_LINK**
[`string`](../data-types.md) | Skype chat link ||
|| **UF_ZOOM**
[`string`](../data-types.md) | Zoom ||
|| **UF_EMPLOYMENT_DATE**
[`string`](../data-types.md) | Hiring date in ISO 8601 format. An empty string if not specified ||
|| **UF_TIMEMAN**
[`string`](../data-types.md) | Whether time tracking is enabled for the user: `1` — enabled, any other value — disabled ||
|| **UF_DEPARTMENT**
[`integer[]`](../data-types.md) | IDs of departments from the company structure. Retrieve departments using [department.get](../departments/department-get.md) ||
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
[`string`](../data-types.md) | Facebook ||
|| **UF_TWITTER**
[`string`](../data-types.md) | Twitter ||
|| **UF_SKYPE**
[`string`](../data-types.md) | Microsoft Teams. The field code is kept from Skype ||
|| **UF_DISTRICT**
[`string`](../data-types.md) | District ||
|| **UF_PHONE_INNER**
[`string`](../data-types.md) | Extension number ||
|| **USER_TYPE**
[`string`](../data-types.md) | User type. Main values:
- `employee` — employee
- `extranet` — external extranet user
- `email` — user who writes from email
- `bot` — chatbot ||
|| **UF_USR_\***
`various` | Custom profile fields. The value type depends on the field type: a string, a number, an array, or, for a file, an object with the `id`, `showUrl`, and `downloadData` keys. Fields are created by the [user.userfield.*](./userfields/index.md) methods ||
|#

The `USER_TYPE` field is returned by the [user.get](./user-get.md) and [user.search](./user-search.md) methods. The `user.current` method does not return this field.

## Error Handling

HTTP status: **401**

```json
{
    "error": "INVALID_CREDENTIALS",
    "error_description": "Invalid request credentials"
}
```

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

The method has no parameters, so it has no errors of its own. The method returns only system errors, for example:

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `401` | `INVALID_CREDENTIALS` | Invalid request credentials | Invalid webhook code or authorization token ||
|| `401` | `insufficient_scope` | The request requires higher privileges than provided by the webhook token | The webhook or application has none of the scope versions: `user`, `user_basic`, or `user_brief` ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-add.md)
- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-search.md)
- [{#T}](./user-fields.md)
- [{#T}](../../tutorials/crm/how-to-get-lists/get-activity-list-by-deals.md)