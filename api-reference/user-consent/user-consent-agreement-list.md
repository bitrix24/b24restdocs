# Get the list of agreements userconsent.agreement.list

> Scope: [`userconsent`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `userconsent.agreement.list` returns a list of agreements.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userconsent.agreement.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userconsent.agreement.list
    ```

- JS

    ```js
    // callListMethod: Retrieves all data at once.
    // Use only for small selections (< 1000 items) due to high
    // memory load.

    try {
    const response = await $b24.callListMethod(
        'userconsent.agreement.list',
        {},
        (progress: number) => { console.log('Progress:', progress) }
    );
    const items = response.getData() || [];
    for (const entity of items) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // fetchListMethod: Retrieves data in parts using an iterator.
    // Use for large volumes of data for efficient memory consumption.

    try {
    const generator = $b24.fetchListMethod('userconsent.agreement.list', {}, 'ID');
    for await (const page of generator) {
        for (const entity of page) { console.log('Entity:', entity) }
    }
    } catch (error: any) {
    console.error('Request failed', error)
    }

    // callMethod: Manual control of pagination through the start parameter.
    // Use for precise control over request batches.
    // Less efficient for large data than fetchListMethod.

    try {
    const response = await $b24.callMethod('userconsent.agreement.list', {}, 0);
    const result = response.getData().result || [];
    for (const entity of result) { console.log('Entity:', entity) }
    } catch (error: any) {
    console.error('Request failed', error)
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'userconsent.agreement.list',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching agreement list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'userconsent.agreement.list',
    {},
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
        'userconsent.agreement.list',
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
"result": [
    {
     "ID": "35",
     "NAME": "Consent to receive newsletters",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "de"
    },
    {
     "ID": "33",
     "NAME": "Conditions of use for Bitrix24 Sites",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "fr"
    },
    {
     "ID": "31",
     "NAME": "Terms of use for the notification service",
     "ACTIVE": "Y",
     "LANGUAGE_ID": null
    },
    {
     "ID": "29",
     "NAME": "ALEX",
     "ACTIVE": "Y",
     "LANGUAGE_ID": ""
    },
    {
     "ID": "27",
     "NAME": "Terms of use for the B24 Notification Center",
     "ACTIVE": "Y",
     "LANGUAGE_ID": null
    },
    {
     "ID": "25",
     "NAME": "Termos de Uso do Bitrix24 Sites",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "br"
    },
    {
     "ID": "23",
     "NAME": "Second agreement",
     "ACTIVE": "Y",
     "LANGUAGE_ID": ""
    },
    {
     "ID": "21",
     "NAME": "Bitrix24 Sites: Terms of Use",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "de"
    },
    {
     "ID": "19",
     "NAME": "Consent with Cookies",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "de"
    },
    {
     "ID": "17",
     "NAME": "Cookie consent",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "en"
    },
    {
     "ID": "15",
     "NAME": "Rules for using Bitrix24.Sites",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "ua"
    },
    {
     "ID": "13",
     "NAME": "Test newwef",
     "ACTIVE": "Y",
     "LANGUAGE_ID": ""
    },
    {
     "ID": "11",
     "NAME": "Bitrix24 Sites Terms of Use",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "en"
    },
    {
     "ID": "9",
     "NAME": "Rules for using Bitrix24.Sites",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "es"
    },
    {
     "ID": "7",
     "NAME": "Rules for using Bitrix24.Sites",
     "ACTIVE": "Y",
     "LANGUAGE_ID": "de"
    },
    {
     "ID": "1",
     "NAME": "Example of consent for processing personal data",
     "ACTIVE": "Y",
     "LANGUAGE_ID": ""
    }
],
"time": {
    "start": 1760352862,
    "finish": 1760352862.776508,
    "duration": 0.776508092880249,
    "processing": 0,
    "date_start": "2025-10-13T13:54:22+02:00",
    "date_finish": "2025-10-13T13:54:22+02:00",
    "operating_reset_at": 1760353462,
    "operating": 0
}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Root element of the response ||
|| **ID**
[`integer`](../data-types.md) | Identifier of the agreement ||
|| **NAME**
[`string`](../data-types.md) | Name of the agreement ||
|| **ACTIVE**
[`string`](../data-types.md) | Activity status. Possible values:
- `Y` — yes
- `N` — no ||
|| **LANGUAGE_ID**
[`string`](../data-types.md) | Language identifier of the agreement ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-consent-agreement-text.md)
- [{#T}](./user-consent-consent-add.md)