# Save the User Consent userconsent.consent.add

> Scope: [`userconsent`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `userconsent.consent.add` saves the user's consent.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **AGREEMENT_ID***
[`integer`](../data-types.md) | Agreement identifier.

The identifier can be obtained using the method [userconsent.agreement.list](./user-consent-agreement-list.md) ||
|| **IP***
[`string`](../data-types.md) | User's IP address ||
|| **USER_ID**
[`integer`](../data-types.md) | User identifier.

The identifier can be obtained using the methods [user.get](../user/user-get.md) and [user.search](../user/user-search.md) ||
|| **URL**
[`string`](../data-types.md) | URL of the page where consent was obtained ||
|| **ORIGIN_ID**
[`string`](../data-types.md) | Identifier of the source, for example, `my_contact_form` ||
|| **ORIGINATOR_ID**
[`string`](../data-types.md) | Identifier of the element in the source, for example, e-mail ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"AGREEMENT_ID":19,"USER_ID":123,"IP":"192.168.1.100","URL":"https://example.com/contact-form","ORIGIN_ID":"my_contact_form","ORIGINATOR_ID":"user@example.com"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userconsent.consent.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"AGREEMENT_ID":19,"USER_ID":123,"IP":"192.168.1.100","URL":"https://example.com/contact-form","ORIGIN_ID":"my_contact_form","ORIGINATOR_ID":"user@example.com","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userconsent.consent.add
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'userconsent.consent.add',
            {
                AGREEMENT_ID: 19,
                USER_ID: 123,
                IP: '192.168.1.100',
                URL: 'https://example.com/contact-form',
                ORIGIN_ID: 'my_contact_form',
                ORIGINATOR_ID: 'user@example.com'
            }
        );
        
        const result = response.getData().result;
        console.log('Created element with ID:', result);
        
        processResult(result);
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
                'userconsent.consent.add',
                [
                    'AGREEMENT_ID' => 19,
                    'USER_ID' => 123,
                    'IP' => '192.168.1.100',
                    'URL' => 'https://example.com/contact-form',
                    'ORIGIN_ID' => 'my_contact_form',
                    'ORIGINATOR_ID' => 'user@example.com'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding consent: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'userconsent.consent.add',
    {
        AGREEMENT_ID: 19,
        USER_ID: 123,
        IP: "192.168.1.100",
        URL: "https://example.com/contact-form",
        ORIGIN_ID: "my_contact_form",
        ORIGINATOR_ID: "user@example.com"
    },
    function(result) {
        if (result.error()) {
        console.error(result.error());
        } else {
        console.log(result.data());
        }
    }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'userconsent.consent.add',
        [
            'AGREEMENT_ID' => 19,
            'USER_ID' => 123,
            'IP' => '192.168.1.100',
            'URL' => 'https://example.com/contact-form',
            'ORIGIN_ID' => 'my_contact_form',
            'ORIGINATOR_ID' => 'user@example.com'
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
"result": 525,
"time": {
    "start": 1760459630,
    "finish": 1760459630.700988,
    "duration": 0.7009880542755127,
    "processing": 0,
    "date_start": "2025-10-14T19:33:50+02:00",
    "date_finish": "2025-10-14T19:33:50+02:00",
    "operating_reset_at": 1760460230,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../data-types.md) | Identifier of the added consent ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"400",
    "error_description":"Parameter `Agreement ID` required."
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `ERROR_ARGUMENT` | Parameter `Agreement ID` required | Parameter `AGREEMENT_ID` not provided ||
|| `400` | `ERROR_ARGUMENT` | Agreement with id `999` not found | Agreement with the specified `ID` not found ||
|| `400` | `ERROR_ARGUMENT` | — | Invalid IP address format ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-consent-agreement-list.md)
- [{#T}](./user-consent-agreement-text.md)