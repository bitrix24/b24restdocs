# Delete Contact from Specified Company crm.company.contact.delete

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit" access permission for companies and "Read" access permission for the contact being removed from the bindings

The method `crm.company.contact.delete` removes a contact from the specified company.

Only the link between the company and the contact is removed — the contact itself stays in CRM. To unlink all contacts from a company at once, use [crm.company.contact.items.delete](./crm-company-contact-items-delete.md). The structure of the binding object is described in the [section overview](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the company. Must be greater than `0`.

The identifier can be obtained using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 4` ||
|| **fields***
[`object`](../../../data-types.md) | An object containing information about which contact needs to be removed from the bindings.

Contains a single key `CONTACT_ID`. The field is described [below](#parameter-fields) ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`crm_entity`](../../data-types.md) | Identifier of the contact to be removed from the company bindings. Must be greater than `0`.

The identifiers of the linked contacts can be obtained using the method [crm.company.contact.items.get](./crm-company-contact-items-get.md) ||
|#

{% note info "Remove Primary Binding" %}

The `IS_PRIMARY` flag belongs to the contact: it means that the company from the binding is the primary one for the contact.

If such a binding is removed, the contact's `COMPANY_ID` field switches to another of its companies with the lowest identifier, or is cleared when the contact has no other companies. The `IS_PRIMARY` flag in the contact's remaining bindings is not switched to `Y` — to make a company primary again, use the method [crm.company.contact.add](./crm-company-contact-add.md).

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32,"fields":{"CONTACT_ID":54}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.contact.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32,"fields":{"CONTACT_ID":54},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.contact.delete
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
        method: 'crm.company.contact.delete',
        params: {
          id: 32,
          fields: {
            CONTACT_ID: 54,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contact removed from company:', result)
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
      async function deleteCompanyContact() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.company.contact.delete',
            params: {
              id: 32,
              fields: {
                CONTACT_ID: 54,
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
          console.info('Contact removed from company:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteCompanyContact)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.company.contact.delete(
            bitrix_id=32,
            fields={"CONTACT_ID": 54},
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.company.contact.delete',
                [
                    'id'     => 32,
                    'fields' => [
                        'CONTACT_ID' => 54,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting contact from company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.company.contact.delete',
        {
            id: 32,
            fields: {
                CONTACT_ID: 54,
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
        'crm.company.contact.delete',
        [
            'id' => 32,
            'fields' => [
                'CONTACT_ID' => 54,
            ]
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.company.contact.delete", b24.Params{
    	"id": 32,
    	"fields": b24.Params{
    		"CONTACT_ID": 54,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.company.contact.delete: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1724072653.79827,
        "finish": 1724072717.749956,
        "duration": 63.95168590545654,
        "processing": 63.63148093223572,
        "date_start": "2024-08-19T15:04:13+02:00",
        "date_finish": "2024-08-19T15:05:17+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Contains:
- `true` — on success
- `false` — on failure, if the contact you are trying to delete is not linked to the company ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The parameter 'ownerEntityID' is invalid or not defined."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | `The parameter 'ownerEntityID' is invalid or not defined.` | The provided `id` is less than or equal to 0 or not provided at all ||
|| Empty value | `The parameter 'item' must be array.` | The `fields` parameter is not an object. In the error text, the parameter is named `item` ||
|| `ACCESS_DENIED` | `Access denied!` | The user does not have permission to edit companies or to read the contact ||
|| Empty value | `Not found.` | The company with the provided `id` was not found ||
|| Empty value | `The parameter 'fields' is not valid.` | Can occur in several cases:
- if the required parameter `fields.CONTACT_ID` is not provided
- if the provided parameter `fields.CONTACT_ID` is less than or equal to 0 ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-company-contact-add.md)
- [{#T}](./crm-company-contact-items-get.md)
- [{#T}](./crm-company-contact-items-set.md)
- [{#T}](./crm-company-contact-items-delete.md)
- [{#T}](./crm-company-contact-fields.md)