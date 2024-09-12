# Get Information About the Current User user.current

> Scope: [`user`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `user.current` retrieves information about the [current](*current_key*) user.

{% note info "" %}

The list of fields for Bitrix24 users that will be obtained as a result of executing the method depends on the application's/webhook's scope. Details about user data access can be found in the [article](index.md).

{% endnote %}

The method does not have any parameters. However, by making a REST request using data from `$_REQUEST` to the domain `DOMAIN` and adding `AUTH_ID` to the request for access to Bitrix24, you can find out which user opened the page in the context of Bitrix24.

## Code Examples

{% include [Examples Note](../../_includes/examples.md) %}

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

- JS

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

- PHP

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

HTTP Status: **200**

```json
    {
        "result":{
            "ID":"3",
            "ACTIVE":true,
            "NAME":"John",
            "LAST_NAME":"Doe",
            "EMAIL":"test@gmail.com",
            "LAST_LOGIN":"2024-07-23T08:07:26+00:00",
            "DATE_REGISTER":"2024-07-22T00:00:00+00:00",
            "IS_ONLINE":"Y",
            "TIME_ZONE_OFFSET":"7200",
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
[`object`](../data-types.md) | The root element of the response that contains information about the user ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-add.md)
- [{#T}](./user-update.md)
- [{#T}](./user-get.md)
- [{#T}](./user-search.md)
- [{#T}](./user-fields.md)

[*current_key]: The one whose token you used when calling REST. If you are using a saved admin token, the administrator will be displayed. If you are using a token that comes in the POST request within the application frame, it will be the user who logged into the application.