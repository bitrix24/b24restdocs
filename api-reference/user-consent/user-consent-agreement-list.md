# Get the list of agreements userconsent.agreement.list

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each AgreementItem returned in result[]
    type AgreementItem = {
      ID: string
      NAME: string
      ACTIVE: string
      LANGUAGE_ID: string | null
    }

    try {
      // userconsent.agreement.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<AgreementItem[]>({
        method: 'userconsent.agreement.list',
        params: {
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Agreements:', result.length, result)
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
      async function fetchAgreementList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // userconsent.agreement.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'userconsent.agreement.list',
            params: {
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Agreements:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', fetchAgreementList)
    </script>
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
