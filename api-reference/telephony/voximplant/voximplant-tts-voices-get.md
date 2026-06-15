# Get a List of Available Voices for Text-to-Speech voximplant.tts.voices.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `voximplant.tts.voices.get` returns a list of available voices for text-to-speech synthesis.

## Method Parameters

No parameters.

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.tts.voices.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.tts.voices.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type VoicesResult = Record<string, string>

    try {
      const response = await $b24.actions.v2.call.make<VoicesResult>({
        method: 'voximplant.tts.voices.get',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Available TTS voices:', result)
        console.info('Total voices:', Object.keys(result).length)
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
      async function getVoices() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'voximplant.tts.voices.get',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Available TTS voices:', result)
          console.info('Total voices:', Object.keys(result).length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getVoices)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'voximplant.tts.voices.get',
                []
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
        'voximplant.tts.voices.get',
        {},
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'voximplant.tts.voices.get',
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
    "result": {
        "auenglishfemale": "Australian English (Female) (Amazon)",
        "brportuguesefemale": "Brazilian Portuguese (Female) (Amazon)",
        "caenglishfemale": "Canadian English (Female) (Default)",
        "cafrenchfemale": "Canadian French (Female) (Amazon)",
        "cafrenchmale": "Canadian French (Male) (Amazon)",
        "chchinesefemale": "Chinese (Female) (Default)",
        "chchinesemale": "Chinese (Male) (Default)",
        "eurcatalanfemale": "Catalan (Female) (Default)",
        "eurczechfemale": "Czech (Female) (Default)",
        "eurdanishfemale": "Danish (Female) (Default)",
        "eurdutchfemale": "Dutch (Female) (Default)",
        "eurfinnishfemale": "Finnish (Female) (Default)",
        "eurfrenchfemale": "French (Female) (Amazon)",
        "eurfrenchmale": "French (Male) (Amazon)",
        "eurgermanfemale": "German (Female) (Default)",
        "eurgermanmale": "German (Male) (Default)",
        "euritalianfemale": "Italian (Female) (Default)",
        "euritalianmale": "Italian (Male) (Default)",
        "eurnorwegianfemale": "Norwegian (Female) (Default)",
        "eurpolishfemale": "Polish (Female) (Default)",
        "eurportuguesefemale": "Portuguese (Female) (Default)",
        "eurportuguesemale": "Portuguese (Male) (Default)",
        "eurspanishfemale": "Spanish (Female) (Default)",
        "eurspanishmale": "Spanish (Male) (Default)",
        "eurturkishfemale": "Turkish (Female) (Default)",
        "eurturkishmale": "Turkish (Male) (Default)",
        "hkchinesefemale": "Hong Kong Cantonese (Female) (Default)",
        "huhungarianfemale": "Hungarian (Female) (Default)",
        "jpjapanesefemale": "Japanese (Female) (Default)",
        "jpjapanesemale": "Japanese (Male) (Default)",
        "krkoreanfemale": "Korean (Female) (Default)",
        "krkoreanmale": "Korean (Male) (Default)",
        "usinternalfemale": "English (Female) (Default)",
        "usinternalmale": "English (Male) (Default)",
        "swswedishfemale": "Swedish (Female) (Default)",
        "twchinesefemale": "Taiwanese Chinese (Female) (Default)",
        "ukenglishfemale": "English (Female) (Amazon)",
        "ukenglishmale": "English (Male) (Amazon)",
        "usenglishfemale": "American English (Female) (Default)",
        "usenglishmale": "American English (Male) (Default)",
        "usspanishfemale": "American Spanish (Female) (Amazon)",
        "usspanishmale": "American Spanish (Male) (Amazon)"
    },
    "time": {
        "start": 1773323829,
        "finish": 1773323829.353531,
        "duration": 0.3535308837890625,
        "processing": 0,
        "date_start": "2026-03-12T16:57:09+01:00",
        "date_finish": "2026-03-12T16:57:09+01:00",
        "operating_reset_at": 1773324429,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object containing a list of available voices for text-to-speech synthesis, where the key is the voice identifier and the value is the voice name ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-callback-start.md)
- [{#T}](./voximplant-infocall-start-with-sound.md)
- [{#T}](./voximplant-infocall-start-with-text.md)
- [{#T}](./voximplant-url-get.md)