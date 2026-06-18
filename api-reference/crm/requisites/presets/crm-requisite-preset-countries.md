# Get a list of countries for the template crm.requisite.preset.countries

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each country item returned in result[]
    type CountryItem = {
      ID: number,
      CODE: string,
      TITLE: string,
    }

    try {
      const response = await $b24.actions.v2.call.make<CountryItem[]>({
        method: 'crm.requisite.preset.countries',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Countries count:', result.length, 'First country:', result[0])
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
      async function fetchCountries() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.requisite.preset.countries',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Countries count:', result.length, 'First country:', result[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchCountries)
    </script>
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