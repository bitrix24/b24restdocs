# Remove Contact from Specified Deal crm.deal.contact.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "modify" access permission for the deal and the "read" access permission for the contact being unlinked

The method `crm.deal.contact.delete` removes a contact from the specified deal.

The method removes a single binding and does not delete the contact itself. The other contacts of the deal do not change. To unlink all contacts at once, use [crm.deal.contact.items.delete](./crm-deal-contact-items-delete.md). How the link works and how to choose a method is described in the [section overview](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the deal.

Can be obtained using the methods [crm.deal.list](../crm-deal-list.md) or [crm.deal.add](../crm-deal-add.md).
||
|| **fields***
[`object`](../../../data-types.md) | An object containing information about which contact needs to be removed from the bindings [(detailed description)](#parameter-fields) ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`integer`](../../../data-types.md) | Identifier of the contact to be removed from the bindings.

You can retrieve the identifier of a contact linked to the deal using the method [crm.deal.contact.items.get](./crm-deal-contact-items-get.md) ||
|#

{% note info "Remove the Primary Contact" %}

If you unlink the primary contact, the remaining contact with the lowest `SORT` becomes primary, and its identifier is written to the deal field `CONTACT_ID`.

If you unlink the last contact, the deal is left without a primary contact and the `CONTACT_ID` field becomes empty

{% endnote %}

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of removing a deal-contact link, where:
- deal identifier — `1875`
- contact identifier — `55`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1875,"fields":{"CONTACT_ID":55}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.contact.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1875,"fields":{"CONTACT_ID":55},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.contact.delete
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
        method: 'crm.deal.contact.delete',
        params: {
          id: 1875,
          fields: {
            CONTACT_ID: 55,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contact unlinked from deal:', result)
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
      async function deleteDealContact() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.deal.contact.delete',
            params: {
              id: 1875,
              fields: {
                CONTACT_ID: 55,
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
          console.info('Contact unlinked from deal:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteDealContact)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.deal.contact.delete(
            bitrix_id=1875,
            fields={
                "CONTACT_ID": 55,
            },
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
                'crm.deal.contact.delete',
                [
                    'id' => 1875,
                    'fields' => [
                        'CONTACT_ID' => 55,
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $e) {
        echo $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.deal.contact.delete',
        {
            id: 1875,
            fields: {
                CONTACT_ID: 55,
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
        'crm.deal.contact.delete',
        [
            'id' => 1875,
            'fields' => [
                'CONTACT_ID' => 55,
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
    res, err := client.Core().Call(ctx, "crm.deal.contact.delete", b24.Params{
    	"id": 1875,
    	"fields": b24.Params{
    		"CONTACT_ID": 55,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.deal.contact.delete: %w", err)
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
        "start": 1773234105,
        "finish": 1773234105.847951,
        "duration": 0.8479509353637695,
        "processing": 0,
        "date_start": "2026-03-11T16:01:45+01:00",
        "date_finish": "2026-03-11T16:01:45+01:00",
        "operating_reset_at": 1773234705,
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
- `true` — contact removed from the deal
- `false` — contact not linked to the deal
||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

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
|| 400 | Empty value | `The parameter 'ownerEntityID' is invalid or not defined.` | The `id` is less than 1 or not provided at all ||
|| 400 | Empty value | `The parameter 'item' must be array.` | The `fields` parameter is not provided or is not an object ||
|| 400 | Empty value | `Access denied.` | The deal with the provided `id` was not found, and the user does not have the "modify" access permission for deals ||
|| 403 | `ACCESS_DENIED` | `Access denied!` | No "modify" access permission for the deal ||
|| 400 | Empty value | `Not found.` | The deal with the provided `id` was not found ||
|| 400 | Empty value | `[Contact #55] You don't have permission to view this item.` | The user does not have the "read" access permission for the contact from `fields.CONTACT_ID`. The contact identifier is substituted in the text ||
|| 400 | Empty value | `The parameter 'fields' is not valid.` | Can occur for several reasons:
- if the required parameter `fields.CONTACT_ID` is not provided
- if the provided parameter `fields.CONTACT_ID` is less than or equal to 0 ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-deal-contact-add.md)
- [{#T}](./crm-deal-contact-items-get.md)
- [{#T}](./crm-deal-contact-items-set.md)
- [{#T}](./crm-deal-contact-items-delete.md)
- [{#T}](./crm-deal-contact-fields.md)