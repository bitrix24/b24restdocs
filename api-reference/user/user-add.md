# Invite a user user.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`user`](../scopes/permissions.md)
>
> Who can execute the method: administrator, and in cloud Bitrix24 also a user with permission to invite employees

The `user.add` method invites a user to Bitrix24 and sends an invitation to `EMAIL`. For an employee, pass the department in `UF_DEPARTMENT`; for an external extranet user, pass `EXTRANET` and `SONET_GROUP_ID`.

{% note info "" %}

The method works only in the `user` scope version. In the `user_brief` and `user_basic` versions, the call returns the `insufficient_scope` error. For details, see the [User Scope Versions](user-scope.md) article. The [onUserAdd](../common/events/on-user-add.md) event does not fire immediately, but when the invited user logs in to Bitrix24 for the first time and completes registration.

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **EMAIL***
[`string`](../data-types.md) | User e-mail. The invitation is sent to it. The address must pass the Bitrix24.Network check ||
|| **NAME**
[`string`](../data-types.md) | First name ||
|| **LAST_NAME**
[`string`](../data-types.md) | Last name ||
|| **SECOND_NAME**
[`string`](../data-types.md) | Middle name ||
|| **PERSONAL_GENDER**
[`string`](../data-types.md) | Gender:
- `M` — male
- `F` — female ||
|| **PERSONAL_PROFESSION**
[`string`](../data-types.md) | Profession ||
|| **PERSONAL_WWW**
[`string`](../data-types.md) | Personal website ||
|| **PERSONAL_BIRTHDAY**
[`date`](../data-types.md) | Date of birth in ISO 8601 format, for example `1990-05-20` ||
|| **PERSONAL_PHOTO**
[`array`](../data-types.md) | Photo, an array of two strings: the file name and the content in [Base64](../files/how-to-upload-files.md), for example `["photo.png", "iVBORw0KGgo..."]` ||
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
[`string`](../data-types.md) | City of residence ||
|| **PERSONAL_STATE**
[`string`](../data-types.md) | Residential region or territory ||
|| **PERSONAL_ZIP**
[`string`](../data-types.md) | Postal code ||
|| **PERSONAL_COUNTRY**
[`string`](../data-types.md) | Numeric code of the country of residence from the Bitrix24 country directory. The code of an already specified country is returned in the `PERSONAL_COUNTRY` field of the [user.get](./user-get.md) response ||
|| **PERSONAL_MAILBOX**
[`string`](../data-types.md) | P.O. box ||
|| **PERSONAL_NOTES**
[`string`](../data-types.md) | Additional notes ||
|| **WORK_PHONE**
[`string`](../data-types.md) | Work phone ||
|| **WORK_COMPANY**
[`string`](../data-types.md) | company ||
|| **WORK_POSITION**
[`string`](../data-types.md) | Job title ||
|| **WORK_DEPARTMENT**
[`string`](../data-types.md) | Department, a text field. Pass the department from the company structure in `UF_DEPARTMENT` ||
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
[`string`](../data-types.md) | Numeric code of the company address country from the Bitrix24 country directory. The code of an already specified country is returned in the `WORK_COUNTRY` field of the [user.get](./user-get.md) response ||
|| **WORK_PROFILE**
[`string`](../data-types.md) | Company's areas of business ||
|| **WORK_LOGO**
[`array`](../data-types.md) | Company logo. Pass an array of the file name and a [Base64](../files/how-to-upload-files.md) string, as for `PERSONAL_PHOTO` ||
|| **WORK_NOTES**
[`string`](../data-types.md) | Notes about the workplace ||
|| **UF_SKYPE_LINK**
[`string`](../data-types.md) | Skype chat link ||
|| **UF_ZOOM**
[`string`](../data-types.md) | Zoom ||
|| **UF_DEPARTMENT**
[`integer[]`](../data-types.md) | IDs of departments from the company structure. Retrieve departments using [department.get](../departments/department-get.md).

Required for an employee if `EXTRANET` = `Y` is not passed. Without it, the method returns the `no_extranet_field` error ||
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
[`string`](../data-types.md) | Extension ||
|| **EXTRANET**
[`string`](../data-types.md) | Invite an external extranet user:
- `Y` — invite to the extranet

Any other value or no parameter invites an employee, which requires `UF_DEPARTMENT`. Together with `EXTRANET` = `Y`, you must pass `SONET_GROUP_ID`. In this case, the `UF_DEPARTMENT` value is ignored. Works if the extranet module is installed in Bitrix24 ||
|| **SONET_GROUP_ID**
[`integer[]`](../data-types.md) | IDs of the workgroups to which the extranet user is invited. You can pass a single number. Retrieve groups using [sonet_group.get](../sonet-group/sonet-group-get.md).

Required if `EXTRANET` = `Y` is passed. Without it, the method returns the `no_sonet_group_for_extranet` error ||
|| **MESSAGE_TEXT**
[`string`](../data-types.md) | Invitation text. If not passed, the standard text is used ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

### Invite an Employee

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "EMAIL": "newuser1@example.com",
        "UF_DEPARTMENT": [1]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.add
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "EMAIL": "newuser1@example.com",
        "UF_DEPARTMENT": [1],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'user.add',
        params: {
          EMAIL: 'newuser1@example.com',
          UF_DEPARTMENT: [1]
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('New user ID:', result)
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
      async function addUser() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'user.add',
            params: {
              EMAIL: 'newuser1@example.com',
              UF_DEPARTMENT: [1]
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('New user ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addUser)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.add(
            fields={
                "EMAIL": "newuser1@example.com",
                "UF_DEPARTMENT": [
                    1,
                ],
            },
        ).response
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
                'user.add',
                [
                    'EMAIL'        => 'newuser1@example.com',
                    'UF_DEPARTMENT' => [1],
                ]
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
        "user.add",
        {
            "EMAIL": "newuser1@example.com",
            "UF_DEPARTMENT": [1]
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
        'user.add',
        [
            'EMAIL' => 'newuser1@example.com',
            'UF_DEPARTMENT' => [1]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.add", b24.Params{
    	"EMAIL":         "newuser1@example.com",
    	"UF_DEPARTMENT": []int{1},
    })
    if err != nil {
    	return fmt.Errorf("user.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

### Invite an Extranet User

The user receives an invitation to workgroup `7`. The `UF_DEPARTMENT` department is not passed for this user.

{% list tabs %}

- cURL (Webhook)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "EMAIL": "partner@example.com",
        "EXTRANET": "Y",
        "SONET_GROUP_ID": [7]
    }' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/user.add
    ```

- cURL (OAuth)

    ```curl
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "EMAIL": "partner@example.com",
        "EXTRANET": "Y",
        "SONET_GROUP_ID": [7],
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/user.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'user.add',
        params: {
          EMAIL: 'partner@example.com',
          EXTRANET: 'Y',
          SONET_GROUP_ID: [7]
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('New user ID:', result)
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
      async function addUser() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'user.add',
            params: {
              EMAIL: 'partner@example.com',
              EXTRANET: 'Y',
              SONET_GROUP_ID: [7]
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('New user ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addUser)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.user.add(
            fields={
                "EMAIL": "partner@example.com",
                "EXTRANET": "Y",
                "SONET_GROUP_ID": [
                    7,
                ],
            },
        ).response
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
                'user.add',
                [
                    'EMAIL'        => 'partner@example.com',
                    'EXTRANET' => 'Y',
                    'SONET_GROUP_ID' => [7],
                ]
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
        "user.add",
        {
            "EMAIL": "partner@example.com",
            "EXTRANET": "Y",
            "SONET_GROUP_ID": [7]
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
        'user.add',
        [
            'EMAIL' => 'partner@example.com',
            'EXTRANET' => 'Y',
            'SONET_GROUP_ID' => [7]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "user.add", b24.Params{
    	"EMAIL":         "partner@example.com",
    	"EXTRANET":      "Y",
    	"SONET_GROUP_ID": []int{7},
    })
    if err != nil {
    	return fmt.Errorf("user.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
    {
        "result":12,
        "time":{
            "start":1721733827.713938,
            "finish":1721733828.286292,
            "duration":0.5723540782928467,
            "processing":0.5508849620819092,
            "date_start":"2024-07-23T11:23:47+00:00",
            "date_finish":"2024-07-23T11:23:48+00:00",
            "operating":0.5508630275726318
        }
    }
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | New user identifier ||
|| **time**
[`time`](../data-types.md) | Request execution time information ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "no_extranet_field",
    "argument": ""
}
```

{% include notitle [Error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_ARGUMENT` | wrong_email | `EMAIL` is not passed or an invalid e-mail is passed ||
|| `400` | `ERROR_ARGUMENT` | no_extranet_field | `UF_DEPARTMENT` is not passed for an employee and `EXTRANET` = `Y` is not passed ||
|| `400` | `ERROR_ARGUMENT` | no_sonet_group_for_extranet | `EXTRANET` = `Y` is passed, but `SONET_GROUP_ID` is not passed ||
|| `400` | `ERROR_ARGUMENT` | invalid_type_field | An array or object is passed in a text field. The field code is returned in the `argument` key of the response ||
|| `400` | `ERROR_ARGUMENT` | Could not verify Bitrix24.Network. Try again later or contact support. | The e-mail did not pass the Bitrix24.Network check ||
|| `400` | `ERROR_ARGUMENT` | A user with this email already exists | A user with this e-mail already exists in Bitrix24 ||
|| `400` | `ERROR_ARGUMENT` | user_count_exceeded | The user limit of the plan is exceeded ||
|| `400` | `ERROR_CORE` | access_denied | The user does not have permission to invite employees ||
|| `400` | `ERROR_CORE` | Error text depends on the cause | Could not send the invitation via Bitrix24.Network or add the user to the group from `SONET_GROUP_ID` ||
|| `401` | `insufficient_scope` | The request requires higher privileges than provided by the access token | The application or webhook has the `user_brief` or `user_basic` scope version. The method works only in the `user` version ||
|#

{% include [System errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-current.md)
- [{#T}](./user-search.md)
- [{#T}](./user-fields.md)