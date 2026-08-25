# Save the User Consent userconsent.consent.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

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

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'userconsent.consent.add',
        params: {
          AGREEMENT_ID: 19,
          USER_ID: 123,
          IP: '192.168.1.100',
          URL: 'https://example.com/contact-form',
          ORIGIN_ID: 'my_contact_form',
          ORIGINATOR_ID: 'user@example.com',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created consent with ID:', result)
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
      async function addConsent() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'userconsent.consent.add',
            params: {
              AGREEMENT_ID: 19,
              USER_ID: 123,
              IP: '192.168.1.100',
              URL: 'https://example.com/contact-form',
              ORIGIN_ID: 'my_contact_form',
              ORIGINATOR_ID: 'user@example.com',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Created consent with ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addConsent)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.userconsent.consent.add(
            agreement_id=19,
            user_id=123,
            ip="192.168.1.100",
            url="https://example.com/contact-form",
            origin_id="my_contact_form",
            originator_id="user@example.com",
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "userconsent.consent.add", b24.Params{
    	"AGREEMENT_ID":  19,
    	"USER_ID":       123,
    	"IP":            "192.168.1.100",
    	"URL":           "https://example.com/contact-form",
    	"ORIGIN_ID":     "my_contact_form",
    	"ORIGINATOR_ID": "user@example.com",
    })
    if err != nil {
    	return fmt.Errorf("userconsent.consent.add: %w", err)
    }

    var newID b24.ID
    if err := json.Unmarshal(res.Result, &newID); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("id:", newID)
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
