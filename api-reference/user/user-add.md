# Invite User user.add

> Scope: [`user`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `user.add` invites a user. This can only be done on behalf of a user with the rights to invite users, typically an administrator. Upon success, a standard invitation will be sent to the account. The `result` returns the identifier of the new user.

If you need to add an extranet user, you must provide the fields: `EXTRANET: Y` and `SONET_GROUP_ID: [...]`. If you need to add an intranet user, it is **mandatory** to provide: `UF_DEPARTMENT: [...]`.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **EMAIL***
[`string`](../data-types.md) | User's e-mail ||
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
[`string`](../data-types.md) | Personal webpage ||
|| **PERSONAL_BIRTHDAY**
[`string`](../data-types.md) | Date of birth ||
|| **PERSONAL_PHOTO**
[`array`](../data-types.md) | Photo ||
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
[`string`](../data-types.md) | Street address ||
|| **PERSONAL_CITY**
[`string`](../data-types.md) | City of residence ||
|| **PERSONAL_STATE**
[`string`](../data-types.md) | State/Region ||
|| **PERSONAL_ZIP**
[`string`](../data-types.md) | Postal code ||
|| **PERSONAL_COUNTRY**
[`string`](../data-types.md) | Country ||
|| **PERSONAL_MAILBOX**
[`string`](../data-types.md) | Mailbox ||
|| **PERSONAL_NOTES**
[`string`](../data-types.md) | Additional notes ||
|| **WORK_PHONE**
[`string`](../data-types.md) | Company phone ||
|| **WORK_COMPANY**
[`string`](../data-types.md) | Company ||
|| **WORK_POSITION**
[`string`](../data-types.md) | Position ||
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
[`string`](../data-types.md) | City of work ||
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
[`string`](../data-types.md) | Departments ||
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
[`string`](../data-types.md) | Skype ||
|| **UF_DISTRICT**
[`string`](../data-types.md) | District ||
|| **UF_PHONE_INNER**
[`string`](../data-types.md) | Internal phone ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

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

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'user.add',
    		{
    			'EMAIL': 'newuser1@example.com',
    			'UF_DEPARTMENT': [1]
    		}
    	);
    	
    	const result = response.getData().result;
    	console.dir(result);
    }
    catch( error )
    {
    	console.error('Error:', error);
    }
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
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding user: ' . $e->getMessage();
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

{% endlist %}

## Response Handling

HTTP Status: **200**

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
[`integer`](../data-types.md) | Identifier of the new user ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "wrong_email",
    "argument": ""
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ERROR_ARGUMENT` | wrong_email | The `EMAIL` parameter is missing or an incorrect e-mail is provided ||
|| `ERROR_ARGUMENT` | User with this email already exists | Attempt to register a user with an email that is already taken ||
|| `ERROR_CORE` | access_denied | The user does not have permission to call the method ||
|| `ERROR_ARGUMENT` | user_count_exceeded | The number of users has been exceeded ||
|| `ERROR_GROUPID` | Group code not specified | Group code not specified when adding a user to the extranet ||
|| `ERROR_NO_GROUP` | Group specified incorrectly | Incorrect group specified when adding a user ||
|| `ERROR_ARGUMENT` | no_extranet_field | The method call did not specify which group the user should belong to ||
|| `ERROR_CORE` |  | Error updating user fields ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-current.md)
- [{#T}](./user-search.md)
- [{#T}](./user-fields.md)