# Update User Data user.update

> Scope: [`user`](../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `user.update` updates user data. It can only be executed on behalf of a user with the permission to invite users.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **ID***
[`integer`](../data-types.md) | User identifier ||
|| **EMAIL**
[`string`](../data-types.md) | User e-mail ||
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
[`array`](../data-types.md) | Photograph ||
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
[`string`](../data-types.md) | State ||
|| **PERSONAL_ZIP**
[`string`](../data-types.md) | Zip code ||
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

- JS

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

- PHP

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

HTTP Status: **200**

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
[`boolean`](../data-types.md) | Success of execution ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "access_denied"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Error Message** | **Description** ||
|| `ERROR_CORE` | access_denied | Invalid `ID` provided for the user ||
|| `ERROR_CORE` | access_denied | User does not have permission to call the method ||
|| `ERROR_CORE` |  | Invalid `ID` provided for the user ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-add.md)
- [{#T}](./user-get.md)
- [{#T}](./user-current.md)
- [{#T}](./user-search.md)
- [{#T}](./user-fields.md)