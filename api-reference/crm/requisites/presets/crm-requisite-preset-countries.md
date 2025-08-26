# Get a list of countries for the template crm.requisite.preset.countries

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a possible list of countries for [requisite templates](./index.md). Country identifiers are used as values for the `COUNTRY_ID` field of the template.

No parameters required.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.countries
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.countries
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.requisite.preset.countries',
    		{}
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
                'crm.requisite.preset.countries',
                []
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
        echo 'Error fetching countries: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.preset.countries",
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
        'crm.requisite.preset.countries',
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
    "result": [
        {
            "ID": 1,
            "CODE": "US",
            "TITLE": "United States"
        },
        {
            "ID": 4,
            "CODE": "BY",
            "TITLE": "Belarus"
        },
        {
            "ID": 6,
            "CODE": "KZ",
            "TITLE": "Kazakhstan"
        },
        {
            "ID": 14,
            "CODE": "UA",
            "TITLE": "Ukraine"
        },
        {
            "ID": 34,
            "CODE": "BR",
            "TITLE": "Brazil"
        },
        {
            "ID": 46,
            "CODE": "DE",
            "TITLE": "Germany"
        },
        {
            "ID": 77,
            "CODE": "CO",
            "TITLE": "Colombia"
        },
        {
            "ID": 110,
            "CODE": "PL",
            "TITLE": "Poland"
        },
        {
            "ID": 122,
            "CODE": "US",
            "TITLE": "United States"
        },
        {
            "ID": 132,
            "CODE": "FR",
            "TITLE": "France"
        }
    ],
    "time": {
        "start": 1716549490.84839,
        "finish": 1716549491.239788,
        "duration": 0.39139795303344727,
        "processing": 0.017835140228271484,
        "date_start": "2024-05-24T13:18:10+02:00",
        "date_finish": "2024-05-24T13:18:11+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../../data-types.md) | An array of objects describing countries ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

### Fields of the country object

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier ||
|| **CODE**
[`string`](../../../data-types.md) | Character code according to the [ISO 3166-1](https://www.iso.org/iso-3166-country-codes.html) standard ||
|| **TITLE**
[`string`](../../../data-types.md) | Name ||
|#

## Error Handling

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-add.md)
- [{#T}](./crm-requisite-preset-update.md)
- [{#T}](./crm-requisite-preset-get.md)
- [{#T}](./crm-requisite-preset-list.md)
- [{#T}](./crm-requisite-preset-delete.md)
- [{#T}](./crm-requisite-preset-fields.md)