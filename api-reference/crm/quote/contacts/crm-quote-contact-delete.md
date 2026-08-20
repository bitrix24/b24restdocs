# Remove a Contact from a Specified Estimate crm.quote.contact.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify" access permission for estimates

The method `crm.quote.contact.delete` removes a contact from the specified estimate.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the estimate

This can be obtained using the methods [crm.quote.list](../crm-quote-list.md) or [crm.quote.add](../crm-quote-add.md)
||
|| **fields***
[`object`](../../../data-types.md) | Object with information about which contact to remove from the links [(detailed description)](#parameter-fields) ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`crm_entity`](../../data-types.md) | Identifier of the contact to be removed from the links

The identifier of a linked contact can be obtained using the method [crm.quote.contact.items.get](./crm-quote-contact-items-get.md) ||
|#

{% note info "Removing the primary link" %}

If you remove the primary link, the first available link becomes the new primary one

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of removing an estimate-contact link, where:
- estimate identifier — `113`
- contact identifier — `2647`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":113,"fields":{"CONTACT_ID":2647}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.contact.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":113,"fields":{"CONTACT_ID":2647},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.contact.delete
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'crm.quote.contact.delete',
        params: {
          id: 113,
          fields: {
            CONTACT_ID: 2647,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contact unlinked from quote:', result)
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
      async function deleteQuoteContact() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.contact.delete',
            params: {
              id: 113,
              fields: {
                CONTACT_ID: 2647,
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
          console.info('Contact unlinked from quote:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteQuoteContact)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.contact.delete',
                [
                    'id' => 113,
                    'fields' => [
                        'CONTACT_ID' => 2647,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Unlinked: ' . ($result ? 'true' : 'false');

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting quote contact: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.contact.delete',
        {
            id: 113,
            fields: {
                CONTACT_ID: 2647,
            },
        },
        (result) => {
            result.error()
                ? console.error(result.error())
                : console.info(result.data())
            ;
        },
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.quote.contact.delete',
        [
            'id' => 113,
            'fields' => [
                'CONTACT_ID' => 2647,
            ],
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.quote.contact.delete", b24.Params{
    	"id": 113,
    	"fields": b24.Params{
    		"CONTACT_ID": 2647,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.contact.delete: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1787206097,
        "finish": 1787206097.428249,
        "duration": 0.42824888229370117,
        "processing": 0,
        "date_start": "2026-08-20T08:08:17+02:00",
        "date_finish": "2026-08-20T08:08:17+02:00",
        "operating_reset_at": 1787206697,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Contains:
- `true` — the contact was removed from the estimate
- `false` — the contact is not linked to the estimate
||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | `The parameter 'ownerEntityID' is invalid or not defined.` | The provided `id` is less than 1 or not provided at all ||
|| Empty value | `The parameter 'item' must be array.` | An object was not provided in `fields` ||
|| Empty value | `The parameter 'fields' is not valid.` | Can occur for several reasons:
- if the required parameter `fields.CONTACT_ID` is not provided
- if the provided parameter `fields.CONTACT_ID` is less than or equal to 0 ||
|| Empty value | `Not found.` | The estimate with the provided `id` was not found ||
|| Empty value | `[Contact #2647] You do not have permission to view this item` | The user does not have read access permission for the contact being unlinked ||
|| `ACCESS_DENIED` | `Access denied!` | No permission to modify the estimate. The response comes with HTTP status `403` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-contact-add.md)
- [{#T}](./crm-quote-contact-items-get.md)
- [{#T}](./crm-quote-contact-items-set.md)
- [{#T}](./crm-quote-contact-items-delete.md)
- [{#T}](./crm-quote-contact-fields.md)
