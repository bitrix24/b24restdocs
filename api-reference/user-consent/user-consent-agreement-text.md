# Get the text of the agreement userconsent.agreement.text

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`userconsent`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `userconsent.agreement.text` returns the text of the agreement.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../data-types.md) | Identifier of the agreement.

The identifier can be obtained using the method [userconsent.agreement.list](./user-consent-agreement-list.md) ||
|| **replace** 
[`object`](../data-types.md) | Array of replacements for text substitution. Available keys:

- **button_caption** — text of the confirmation button
- **fields** — list of form fields
  
The list of available fields is described [below](#fields)

{% note info "" %}

Substitution is performed only for standard agreements created based on templates. For custom agreements with arbitrary HTML text, the parameter is ignored 

{% endnote %} ||
|#

## Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **COMPANY_NAME**
[`text`](../data-types.md) | Company name ||
|| **COMPANY_ADDRESS**
[`text`](../data-types.md) | Company address ||
|| **PURPOSES**
[`text`](../data-types.md) | Purpose of data processing ||
|| **THIRD_PARTIES**
[`text`](../data-types.md) | Third parties to whom data is transferred ||
|| **EMAIL**
[`string`](../data-types.md) | Email address ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":19,"replace":{"button_caption":"I agree","fields":{"COMPANY_NAME":"Example Ltd.","COMPANY_ADDRESS":"New York, Example St., 1","PURPOSES":"Processing personal data to improve service","THIRD_PARTIES":"Company partners for analytics","EMAIL":"info@example.com"}}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/userconsent.agreement.text
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":19,"replace":{"button_caption":"I agree","fields":{"COMPANY_NAME":"Example Ltd.","COMPANY_ADDRESS":"New York, Example St., 1","PURPOSES":"Processing personal data to improve service","THIRD_PARTIES":"Company partners for analytics","EMAIL":"info@example.com"}},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/userconsent.agreement.text
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type AgreementTextResult = {
      LABEL: string
      TEXT: string
    }

    try {
      const response = await $b24.actions.v2.call.make<AgreementTextResult>({
        method: 'userconsent.agreement.text',
        params: {
          id: 19,
          replace: {
            button_caption: 'I agree',
            fields: {
              COMPANY_NAME: 'Example LLC',
              COMPANY_ADDRESS: '1 Example St, New York, NY',
              PURPOSES: 'Processing personal data to improve service',
              THIRD_PARTIES: 'Company partners for analytics',
              EMAIL: 'info@example.com',
            },
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Agreement label:', result.LABEL)
        console.info('Agreement text:', result.TEXT)
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
      async function getAgreementText() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'userconsent.agreement.text',
            params: {
              id: 19,
              replace: {
                button_caption: 'I agree',
                fields: {
                  COMPANY_NAME: 'Example LLC',
                  COMPANY_ADDRESS: '1 Example St, New York, NY',
                  PURPOSES: 'Processing personal data to improve service',
                  THIRD_PARTIES: 'Company partners for analytics',
                  EMAIL: 'info@example.com',
                },
              },
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Agreement label:', result.LABEL)
          console.info('Agreement text:', result.TEXT)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getAgreementText)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'userconsent.agreement.text',
                [
                    'id' => 19,
                    'replace' => [
                        'button_caption' => 'I agree',
                        'fields' => [
                            'COMPANY_NAME' => 'Example Ltd.',
                            'COMPANY_ADDRESS' => 'New York, Example St., 1',
                            'PURPOSES' => 'Processing personal data to improve service',
                            'THIRD_PARTIES' => 'Company partners for analytics',
                            'EMAIL' => 'info@example.com'
                        ]
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
    'userconsent.agreement.text',
        {
            id: 19,
            replace: {
                button_caption: "I agree",
                fields: {
                    COMPANY_NAME: "Example Ltd.",
                    COMPANY_ADDRESS: "New York, Example St., 1",
                    PURPOSES: "Processing personal data to improve service",
                    THIRD_PARTIES: "Company partners for analytics",
                    EMAIL: "info@example.com"
                }
            }
        },
        function(result) {
            if(result.error()) {
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
        'userconsent.agreement.text',
        [
            'id' => 19,
            'replace' => [
                'button_caption' => 'I agree',
                'fields' => [
                    'COMPANY_NAME' => 'Example Ltd.',
                    'COMPANY_ADDRESS' => 'New York, Example St., 1',
                    'PURPOSES' => 'Processing personal data to improve service',
                    'THIRD_PARTIES' => 'Company partners for analytics',
                    'EMAIL' => 'info@example.com'
                ]
            ]
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
"result": {
    "LABEL": "By clicking the 'I agree' button, I consent to the processing of my personal data in accordance with Federal Law No. 152-FZ of July 27, 2006 'On Personal Data', under the conditions and for the purposes defined in the Consent to the processing of personal data",
    "TEXT": "Consent to the processing of personal data\n\nHereby, in accordance with Federal Law No. 152-FZ 'On Personal Data' of July 27, 2006, I freely, of my own will and in my interest express my unconditional consent to the processing of my personal data, registered in accordance with the legislation of the USA at the address: \n (hereinafter referred to as the Operator).\n1. Consent is given for the processing of one, several, or all categories of personal data, which are not special or biometric, provided by me, which may include:\n\n- Example Ltd.;\n- New York, Example St., 1;\n- Processing personal data to improve service;\n- Company partners for analytics;\n- info@example.com.\n\n2. The Operator may perform the following actions: collection; recording; systematization; accumulation; storage; clarification (updating, modification); extraction; use; blocking; deletion; destruction. \n\n3. Methods of processing: both using automation tools and without their use.\n\n4. Purpose of processing: providing me with services/work, including sending notifications regarding the services/work provided, preparing and sending responses to my requests, sending me information about events/products/services/work of the Operator.\n\n5. Since the Operator may process my personal data using the software 'QuickBooks and other similar platforms', I give my consent to the Operator to carry out the corresponding assignment to Example Ltd., (OGRN 5077746476209), registered at the address: 109544, New York, Enthusiast Blvd., 2, floor 13, room 8-19.\n\n6. This consent is valid until revoked by sending a corresponding notification to the email address test@example.com or by sending it to the address.\n\n7. In case I revoke my consent to the processing of personal data, the Operator has the right to continue processing personal data without my consent if there are grounds provided by Federal Law No. 152-FZ 'On Personal Data' of July 27, 2006."
},
"time": {
    "start": 1760611223,
    "finish": 1760611223.240694,
    "duration": 0.2406940460205078,
    "processing": 0,
    "date_start": "2025-10-16T13:40:23+02:00",
    "date_finish": "2025-10-16T13:40:23+02:00",
    "operating_reset_at": 1760611823,
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
|| **LABEL**
[`string`](../data-types.md) | Title of the agreement ||
|| **TEXT**
[`string`](../data-types.md) | Text of the agreement ||
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
|| `400` | `ERROR_ARGUMENT` | Parameter `Agreement ID` required | Parameter `id` not provided ||
|| `400` | `ERROR_ARGUMENT` | Agreement with id `999` not found | Agreement with the specified `id` not found ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./user-consent-agreement-list.md)
- [{#T}](./user-consent-consent-add.md)
