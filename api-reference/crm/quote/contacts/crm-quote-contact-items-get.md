# Retrieve a Set of Contacts Associated with a Specified Estimate crm.quote.contact.items.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "read" access permission for estimates

The method `crm.quote.contact.items.get` returns a set of contacts associated with the specified estimate.

The other estimate methods do not return the full set of contacts. [crm.quote.get](../crm-quote-get.md) returns only the primary contact in the `CONTACT_ID` field, and [crm.quote.list](../crm-quote-list.md) does not return the multiple field `CONTACT_IDS` even when it is explicitly requested in `select`.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the estimate

This can be obtained using the methods [crm.quote.list](../crm-quote-list.md) or [crm.quote.add](../crm-quote-add.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of retrieving all linked contacts for an estimate with `id = 113`.

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":113}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.quote.contact.items.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":113,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.quote.contact.items.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make({
        method: 'crm.quote.contact.items.get',
        params: {
          id: 113,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Quote contacts:', result)
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
      async function getQuoteContacts() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.quote.contact.items.get',
            params: {
              id: 113,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Quote contacts:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getQuoteContacts)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.quote.contact.items.get',
                [
                    'id' => 113,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Data: ' . print_r($result, true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting quote contacts: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.quote.contact.items.get',
        {
            id: 113,
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
        'crm.quote.contact.items.get',
        [
            'id' => 113,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.quote.contact.items.get", b24.Params{
    	"id": 113,
    })
    if err != nil {
    	return fmt.Errorf("crm.quote.contact.items.get: %w", err)
    }

    var bindings []struct {
    	ContactID int    `json:"CONTACT_ID"`
    	Sort      int    `json:"SORT"`
    	RoleID    int    `json:"ROLE_ID"`
    	IsPrimary string `json:"IS_PRIMARY"`
    }
    if err := json.Unmarshal(res.Result, &bindings); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("links:", len(bindings))
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "CONTACT_ID": 2649,
            "SORT": 10,
            "ROLE_ID": 0,
            "IS_PRIMARY": "Y"
        },
        {
            "CONTACT_ID": 2647,
            "SORT": 20,
            "ROLE_ID": 0,
            "IS_PRIMARY": "N"
        }
    ],
    "time": {
        "start": 1787206096,
        "finish": 1787206096.725297,
        "duration": 0.7252969741821289,
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
[`quote_contact_binding[]`](#quote_contact_binding) | Root element of the response. Contains an array with information about the contacts linked to the estimate, sorted by `SORT` in ascending order. If the estimate has no linked contacts, an empty array is returned ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

### Parameter quote_contact_binding {#quote_contact_binding}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID**
[`integer`](../../../data-types.md) | Identifier of the contact ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index ||
|| **ROLE_ID**
[`integer`](../../../data-types.md) | Identifier of the role (a service field). It is not filled in via REST and is always equal to `0` ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md) | Indicates whether the link is primary. Possible values:
- `Y` — yes
- `N` — no ||
|#

{% note info "" %}

The method does not check whether the estimate exists. For a non-existent `id`, it returns an empty array rather than an error

{% endnote %}

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "The parameter ownerEntityID is invalid or not defined."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | `The parameter ownerEntityID is invalid or not defined.` | The provided `id` is less than 1 or not provided at all ||
|| `ACCESS_DENIED` | `Access denied!` | No permission to read the estimate. The response comes with HTTP status `403` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-quote-contact-add.md)
- [{#T}](./crm-quote-contact-delete.md)
- [{#T}](./crm-quote-contact-items-set.md)
- [{#T}](./crm-quote-contact-items-delete.md)
- [{#T}](./crm-quote-contact-fields.md)
