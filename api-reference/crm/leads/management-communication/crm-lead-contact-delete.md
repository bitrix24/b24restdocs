# Remove contact binding from lead crm.lead.contact.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit" access permission for the lead and the "Read" access permission for the contact being removed

The method `crm.lead.contact.delete` removes a contact from the specified lead.

Only the link between the lead and the contact is removed — the contact itself remains in CRM. To unlink all contacts from the lead at once, use [crm.lead.contact.items.delete](./crm-lead-contact-items-delete.md). The structure of the binding object is described in the [section overview](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the lead. Must be greater than `0`.

The identifier can be retrieved using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 1` ||
|| **fields***
[`object`](../../../data-types.md) | An object with information about which contact to remove from the bindings.

Contains a single key `CONTACT_ID`. The field is described [below](#parameter-fields) ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`crm_entity`](../../data-types.md) | Identifier of the contact to remove from the lead's bindings. Must be greater than `0`.

The identifiers of the linked contacts can be retrieved using the method [crm.lead.contact.items.get](./crm-lead-contact-items-get.md) ||
|#

{% note info "Remove the Primary Contact" %}

If you remove the lead's primary contact, the first of the remaining contacts in ascending `SORT` order becomes primary: Bitrix24 writes it to the lead's `CONTACT_ID` field. If the lead has no other contacts, the field is cleared.

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"CONTACT_ID":1010}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.contact.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"CONTACT_ID":1010},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.lead.contact.delete
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
        method: 'crm.lead.contact.delete',
        params: {
          id: 1,
          fields: {
            CONTACT_ID: 1010,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contact unlinked from lead:', result)
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
      async function deleteLeadContact() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.lead.contact.delete',
            params: {
              id: 1,
              fields: {
                CONTACT_ID: 1010,
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
          console.info('Contact unlinked from lead:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteLeadContact)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.lead.contact.delete(
            bitrix_id=1,
            fields={"CONTACT_ID": 1010},
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
                'crm.lead.contact.delete',
                [
                    'id'     => 1,
                    'fields' => [
                        'CONTACT_ID' => 1010,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result[0], true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error deleting lead contact: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
            "crm.lead.contact.delete",
            {
                id: 1,
                fields:
                {
                    "CONTACT_ID": 1010,
                }
            },
            result => {
                if (result.error())
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
        'crm.lead.contact.delete',
        [
            'id' => 1,
            'fields' =>
            [
                'CONTACT_ID' => 1010,
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
    res, err := client.Core().Call(ctx, "crm.lead.contact.delete", b24.Params{
    	"id": 1,
    	"fields": b24.Params{
    		"CONTACT_ID": 1010,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.lead.contact.delete: %w", err)
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
        "start": 1715091541.642592,
        "finish": 1715091541.730599,
        "duration": 0.08800697326660156,
        "date_start": "2024-05-07T17:19:01+02:00",
        "date_finish": "2024-05-07T17:19:01+02:00",
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
- `true` — the contact is removed from the bindings
- `false` — the contact is not linked to the lead
||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | The parameter 'ownerEntityID' is invalid or not defined. | The `id` parameter is not passed or is less than or equal to `0` ||
|| `400` | Empty value | The parameter 'item' must be array. | The `fields` parameter is not passed or is passed as a string or a number. In the error text, the parameter is named `item` ||
|| `400` | Empty value | The parameter 'fields' is not valid. | `fields` does not contain `CONTACT_ID`, or its value is less than or equal to `0` ||
|| `400` | Empty value | Access denied. | The user does not have read access to CRM objects, including those in digital workspaces ||
|| `403` | `ACCESS_DENIED` | Access denied! | The user does not have permission to edit the lead ||
|| `400` | Empty value | Not found. | The lead with the passed `id` was not found ||
|| `400` | Empty value | [Contact #29] You don't have permission to view this item. | The user does not have permission to read the contact. The error text contains the contact identifier ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-lead-contact-add.md)
- [{#T}](./crm-lead-contact-items-get.md)
- [{#T}](./crm-lead-contact-items-set.md)
- [{#T}](./crm-lead-contact-items-delete.md)
- [{#T}](./crm-lead-contact-fields.md)