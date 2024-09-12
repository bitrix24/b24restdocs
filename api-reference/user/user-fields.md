# Get User Fields user.fields

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The `user.fields` method allows you to retrieve a list of user field names. The method returns a standard list of fields, and the use of custom fields is not supported.

{% note info "" %}

The list of user fields in Bitrix24 that will be obtained as a result of executing the method depends on the application's/webhook's scope. Details about accessing user data can be found in the [article](index.md).

{% endnote %}

No parameters.

## Code Examples

{% include [Example Note](../../_includes/examples.md) %}

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

- JS

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

- PHP

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

HTTP Status: **200**

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
            "LAST_LOGIN":"Last Login",
            "DATE_REGISTER":"Registration Date",
            "TIME_ZONE":"TIME_ZONE",
            "IS_ONLINE":"IS_ONLINE",
            "TIME_ZONE_OFFSET":"TIME_ZONE_OFFSET",
            "TIMESTAMP_X":"TIMESTAMP_X",
            "LAST_ACTIVITY_DATE":"LAST_ACTIVITY_DATE",
            "PERSONAL_GENDER":"Gender",
            "PERSONAL_PROFESSION":"Profession",
            "PERSONAL_WWW":"Homepage",
            "PERSONAL_BIRTHDAY":"Birthday",
            "PERSONAL_PHOTO":"Photo",
            "PERSONAL_ICQ":"ICQ",
            "PERSONAL_PHONE":"Personal Phone",
            "PERSONAL_FAX":"Fax",
            "PERSONAL_MOBILE":"Personal Mobile",
            "PERSONAL_PAGER":"Pager",
            "PERSONAL_STREET":"Street Address",
            "PERSONAL_CITY":"City",
            "PERSONAL_STATE":"Region / Territory",
            "PERSONAL_ZIP":"Postal Code",
            "PERSONAL_COUNTRY":"Country",
            "PERSONAL_MAILBOX":"Mailbox",
            "PERSONAL_NOTES":"Additional Notes",
            "WORK_PHONE":"Company Phone",
            "WORK_COMPANY":"Company",
            "WORK_POSITION":"Position",
            "WORK_DEPARTMENT":"Department",
            "WORK_WWW":"Company Website",
            "WORK_FAX":"WORK_FAX",
            "WORK_PAGER":"WORK_PAGER",
            "WORK_STREET":"WORK_STREET",
            "WORK_MAILBOX":"WORK_MAILBOX",
            "WORK_CITY":"City of Work",
            "WORK_STATE":"WORK_STATE",
            "WORK_ZIP":"WORK_ZIP",
            "WORK_COUNTRY":"WORK_COUNTRY",
            "WORK_PROFILE":"WORK_PROFILE",
            "WORK_LOGO":"WORK_LOGO",
            "WORK_NOTES":"WORK_NOTES",
            "UF_SKYPE_LINK":"Skype Chat Link",
            "UF_ZOOM":"Zoom",
            "UF_EMPLOYMENT_DATE":"Employment Date",
            "UF_TIMEMAN":"Time Management",
            "UF_DEPARTMENT":"Departments",
            "UF_INTERESTS":"Interests",
            "UF_SKILLS":"Skills",
            "UF_WEB_SITES":"Other Websites",
            "UF_XING":"Xing",
            "UF_LINKEDIN":"LinkedIn",
            "UF_FACEBOOK":"Facebook",
            "UF_TWITTER":"Twitter",
            "UF_SKYPE":"Skype",
            "UF_DISTRICT":"District",
            "UF_PHONE_INNER":"Internal Phone",
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
[`object`](../data-types.md) | Root element of the response containing user fields ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-add.md)
- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-current.md)
- [{#T}](./user-search.md)