# Get user fields user.fields

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `user.fields` method returns the codes and names of user profile fields: standard fields and custom fields with the `UF_USR_` prefix.

{% note info "" %}

The list of Bitrix24 user fields that will be retrieved as a result of the method execution depends on the scope of the application/webhook. The fields available in each version are listed in the [User Scope Versions](user-scope.md) article.

{% endnote %}

## Method Parameters

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
    -d '{"auth":"**put_access_token_here**"}' \
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

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.fields().response
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
                'user.fields',
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.fields", b24.Params{}, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("user.fields: %w", err)
    }

    // result is a "field code: field name" object
    var fields map[string]*string
    if err := json.Unmarshal(res.Result, &fields); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for code, title := range fields {
    	if title != nil {
    		fmt.Println(code, *title)
    	}
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "ID": "ID",
        "XML_ID": "External code",
        "ACTIVE": "Activity",
        "NAME": "First Name",
        "LAST_NAME": "Last Name",
        "SECOND_NAME": "Middle Name",
        "TITLE": "User List",
        "EMAIL": "E-Mail",
        "LAST_LOGIN": "Last Authorization",
        "DATE_REGISTER": "Registration Date",
        "TIME_ZONE": "TIME_ZONE",
        "IS_ONLINE": "IS_ONLINE",
        "TIMESTAMP_X": "TIMESTAMP_X",
        "LAST_ACTIVITY_DATE": "LAST_ACTIVITY_DATE",
        "PERSONAL_GENDER": "Gender",
        "PERSONAL_PROFESSION": "Profession",
        "PERSONAL_WWW": "Homepage",
        "PERSONAL_BIRTHDAY": "Date of Birth",
        "PERSONAL_PHOTO": "Photo",
        "PERSONAL_ICQ": "ICQ",
        "PERSONAL_PHONE": "Personal Phone",
        "PERSONAL_FAX": "Fax",
        "PERSONAL_MOBILE": "Personal Mobile",
        "PERSONAL_PAGER": "Pager",
        "PERSONAL_STREET": "Residential Street",
        "PERSONAL_CITY": "Residential City",
        "PERSONAL_STATE": "State / Region",
        "PERSONAL_ZIP": "Zip Code",
        "PERSONAL_COUNTRY": "Country",
        "PERSONAL_MAILBOX": "P.O. Box",
        "PERSONAL_NOTES": "Additional Notes",
        "WORK_PHONE": "Company Phone",
        "WORK_COMPANY": "Company",
        "WORK_POSITION": "Job Title",
        "WORK_DEPARTMENT": "Department",
        "WORK_WWW": "Company Website",
        "WORK_FAX": "WORK_FAX",
        "WORK_PAGER": "WORK_PAGER",
        "WORK_STREET": "WORK_STREET",
        "WORK_MAILBOX": "WORK_MAILBOX",
        "WORK_CITY": "Work City",
        "WORK_STATE": "WORK_STATE",
        "WORK_ZIP": "WORK_ZIP",
        "WORK_COUNTRY": "WORK_COUNTRY",
        "WORK_PROFILE": "WORK_PROFILE",
        "WORK_LOGO": "WORK_LOGO",
        "WORK_NOTES": "WORK_NOTES",
        "UF_SKYPE_LINK": "Skype Chat Link",
        "UF_ZOOM": "Zoom",
        "UF_EMPLOYMENT_DATE": "Hire Date",
        "UF_TIMEMAN": "Time Tracking",
        "UF_DEPARTMENT": "Units",
        "UF_INTERESTS": "Interests",
        "UF_SKILLS": "Skills",
        "UF_WEB_SITES": "Other Websites",
        "UF_XING": "Xing",
        "UF_LINKEDIN": "LinkedIn",
        "UF_FACEBOOK": "Facebook",
        "UF_TWITTER": "Twitter",
        "UF_SKYPE": "Microsoft Teams",
        "UF_DISTRICT": "District",
        "UF_PHONE_INNER": "Extension",
        "UF_USR_1623996826645": "Signature",
        "USER_TYPE": "USER_TYPE"
    },
    "time": {
        "start": 1793286069,
        "finish": 1793286069.582253,
        "duration": 0.5822529792785645,
        "processing": 0,
        "date_start": "2026-10-29T18:01:09+01:00",
        "date_finish": "2026-10-29T18:01:09+01:00",
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
[`object`](../data-types.md) | An object of the form `{"field code": "field name"}`; all values are strings. Call the method before using `select` in [user.get](./user-get.md) or writing data with [user.update](./user-update.md) to find out the field codes available in the current scope version.

Contains the standard profile fields allowed by the scope version. The list for each version is in the [User Scope Versions](user-scope.md) article. Types and descriptions of the standard fields are in the [user fields](user-current.md#user-fields) table.

Custom fields with the `UF_USR_` prefix are returned with the name specified when the field was created.

Names are taken from the Bitrix24 interface and may not match the purpose of the field. For example, the `TITLE` field (salutation) is named "User List". Some standard fields have no localized name, and their value matches the code, for example `"TIME_ZONE": "TIME_ZONE"` ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

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
- [{#T}](./user-current.md)
- [{#T}](./user-search.md)
- [{#T}](./userfields/index.md)