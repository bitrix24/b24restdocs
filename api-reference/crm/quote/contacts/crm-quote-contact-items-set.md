# Set the Contacts Associated with a Specified Estimate crm.quote.contact.items.set

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "modify" access permission for estimates

The method `crm.quote.contact.items.set` sets the collection of contacts associated with the specified estimate. Contacts that are not in the list you pass are unlinked.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the estimate

This can be obtained using the methods [crm.quote.list](../crm-quote-list.md) or [crm.quote.add](../crm-quote-add.md)
||
|| **items***
[`object[]`](../../../data-types.md) | Set of objects that describe the contacts linked to the estimate [(detailed description)](#quote_contact_binding)

If you pass an empty array, all contact links are removed ||
|#

### Structure of the Link Object {#quote_contact_binding}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`crm_entity`](../../data-types.md) | Identifier of the contact to be linked to the estimate

This can be obtained using the methods [crm.contact.list](../../contacts/crm-contact-list.md) or [crm.contact.add](../../contacts/crm-contact-add.md) ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md) | Indicates whether the link is primary. Possible values:
- `Y` — yes
- `N` — no

If there is no link with `IS_PRIMARY = Y`, it is set for the first link in `items`.

If several links with `IS_PRIMARY = Y` are passed, the first link with `IS_PRIMARY = Y` is considered the primary one ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index

By default `(i + 1) * 10`, where `i` is the index of the element in the `items` array, starting from 0 ||
|#

The identifier of the primary contact goes into the estimate field `CONTACT_ID` and is returned by the method [crm.quote.get](../crm-quote-get.md).

{% note warning "" %}

The method does not check whether the contacts exist. If you pass the identifier of a non-existent contact, the link is created, and the identifier itself goes into the estimate field `CONTACT_ID`

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Set the following linked contacts for the estimate with `id = 113`:
- the contact with `id = 2649`, make it primary and set `SORT = 10`
- the contact with `id = 2647`, set `SORT = 20`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":113,"items":[{"CONTACT_ID":2649,"SORT":10,"IS_PRIMARY":"Y"},{"CONTACT_ID":2647,"SORT":20}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.contact.items.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":113,"items":[{"CONTACT_ID":2649,"SORT":10,"IS_PRIMARY":"Y"},{"CONTACT_ID":2647,"SORT":20}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.contact.items.set
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
        method: 'crm.quote.contact.items.set',
        params: {
          id: 113,
          items: [
            {
              CONTACT_ID: 2649,
              SORT: 10,
              IS_PRIMARY: 'Y',
            },
            {
              CONTACT_ID: 2647,
              SORT: 20,
            },
          ],
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Quote contacts saved:', result)
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
      async function setQuoteContacts() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.contact.items.set',
            params: {
              id: 113,
              items: [
                {
                  CONTACT_ID: 2649,
                  SORT: 10,
                  IS_PRIMARY: 'Y',
                },
                {
                  CONTACT_ID: 2647,
                  SORT: 20,
                },
              ],
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Quote contacts saved:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setQuoteContacts)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.contact.items.set',
                [
                    'id' => 113,
                    'items' => [
                        [
                            'CONTACT_ID' => 2649,
                            'SORT' => 10,
                            'IS_PRIMARY' => 'Y',
                        ],
                        [
                            'CONTACT_ID' => 2647,
                            'SORT' => 20,
                        ],
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Updated: ' . ($result ? 'true' : 'false');

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error setting quote contacts: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.contact.items.set',
        {
            id: 113,
            items: [
                {
                    CONTACT_ID: 2649,
                    SORT: 10,
                    IS_PRIMARY: 'Y',
                },
                {
                    CONTACT_ID: 2647,
                    SORT: 20,
                },
            ],
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
        'crm.quote.contact.items.set',
        [
            'id' => 113,
            'items' => [
                [
                    'CONTACT_ID' => 2649,
                    'SORT' => 10,
                    'IS_PRIMARY' => 'Y',
                ],
                [
                    'CONTACT_ID' => 2647,
                    'SORT' => 20,
                ],
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
    res, err := client.Core().Call(ctx, "crm.quote.contact.items.set", b24.Params{
    	"id": 113,
    	"items": []b24.Params{
    		{
    			"CONTACT_ID": 2649,
    			"SORT":       10,
    			"IS_PRIMARY": "Y",
    		},
    		{
    			"CONTACT_ID": 2647,
    			"SORT":       20,
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.contact.items.set: %w", err)
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
        "start": 1787206096,
        "finish": 1787206096.073034,
        "duration": 0.07303404808044434,
        "processing": 0,
        "date_start": "2026-08-20T08:08:16+02:00",
        "date_finish": "2026-08-20T08:08:16+02:00",
        "operating_reset_at": 1787206696,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Contains `true` if the operation succeeds ||
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
|| Empty value | `The parameter ownerEntityID is invalid or not defined.` | The provided `id` is less than 1 or not provided at all ||
|| Empty value | `The parameter items must be array.` | An array was not provided in `items` ||
|| Empty value | `Not found.` | The estimate with the provided `id` was not found ||
|| Empty value | `[Contact #2647] You do not have permission to view this item` | The user does not have read access permission for the contact whose link is being added or removed ||
|| `ACCESS_DENIED` | `Access denied!` | No permission to modify the estimate. The response comes with HTTP status `403` ||
|| `ERROR_CORE` | Text of the internal error | A failure while normalizing the set of links, the response comes with HTTP status `500` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-contact-add.md)
- [{#T}](./crm-quote-contact-delete.md)
- [{#T}](./crm-quote-contact-items-get.md)
- [{#T}](./crm-quote-contact-items-delete.md)
- [{#T}](./crm-quote-contact-fields.md)
