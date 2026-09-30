# Add a Company to the Specified Contact crm.contact.company.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "Edit" access permission for the contact and the "Read" access permission for the company being added

The method `crm.contact.company.add` links a company to the specified contact.

The contact's other companies remain linked. To define the entire set of companies at once, use [crm.contact.company.items.set](./crm-contact-company-items-set.md). The structure of the binding object is described in the [section overview](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the contact. Must be greater than `0`.

The identifier can be retrieved using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 3`
||
|| **fields***
[`object`](../../../data-types.md) | Object with information about the company to be linked to the contact.

The list of available fields is described [below](#parameter-fields) ||
|#

### Parameter fields {#parameter-fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **COMPANY_ID***
[`crm_entity`](../../data-types.md) | Identifier of the company to be linked to the contact.

The identifier can be retrieved using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 4`.

The method does not separately check whether the company exists, so it can create a binding to a non-existent company ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md#standart-types) | Whether to make the company primary for the contact. Possible values:
- `Y` — yes
- `N` — no

If the contact does not have a primary company yet, the added company becomes primary regardless of the value passed.

The value `Y` makes the added company primary instead of the previous one: the flag of the previous company is reset to `N`, and the new company is written to the contact field `COMPANY_ID` ||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index.

If `SORT` is not passed, the method substitutes `i + 10`, where `i` is the largest positive sort index among the contact's companies. If there are none, `i` equals `0` ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of adding a contact-company link, where:
- contact identifier — `54`
- company identifier — `32`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":54,"fields":{"COMPANY_ID":32,"IS_PRIMARY":"Y","SORT":1000}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.company.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":54,"fields":{"COMPANY_ID":32,"IS_PRIMARY":"Y","SORT":1000},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.company.add
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
        method: 'crm.contact.company.add',
        params: {
          id: 54,
          fields: {
            COMPANY_ID: 32,
            IS_PRIMARY: 'Y',
            SORT: 1000,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Company added to contact:', result)
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
      async function addContactCompany() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.contact.company.add',
            params: {
              id: 54,
              fields: {
                COMPANY_ID: 32,
                IS_PRIMARY: 'Y',
                SORT: 1000,
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
          console.info('Company added to contact:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addContactCompany)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.contact.company.add(
            bitrix_id=54,
            fields={
                "COMPANY_ID": 32,
                "IS_PRIMARY": "Y",
                "SORT": 1000,
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
                'crm.contact.company.add',
                [
                    'id' => 54,
                    'fields' => [
                        'COMPANY_ID' => 32,
                        'IS_PRIMARY' => 'Y',
                        'SORT' => 1000,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result[0], true);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding contact company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.contact.company.add',
        {
            id: 54,
            fields: {
                COMPANY_ID: 32,
                IS_PRIMARY: "Y",
                SORT: 1000,
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
        'crm.contact.company.add',
        [
            'id' => 54,
            'fields' => [
                'COMPANY_ID' => 32,
                'IS_PRIMARY' => 'Y',
                'SORT' => 1000,
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
    res, err := client.Core().Call(ctx, "crm.contact.company.add", b24.Params{
    	"id": 54,
    	"fields": b24.Params{
    		"COMPANY_ID": 32,
    		"IS_PRIMARY": "Y",
    		"SORT":       1000,
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.contact.company.add: %w", err)
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
        "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2024-08-19T13:47:08+02:00",
        "date_finish": "2024-08-19T13:47:08+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Contains:
- `true` — the company is added
- `false` — the company is already linked to the contact. In this case, the method changes nothing, including `SORT` and `IS_PRIMARY`
||
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
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | The parameter 'ownerEntityID' is invalid or not defined. | The `id` parameter is not passed or is less than or equal to `0` ||
|| `400` | Empty value | The parameter 'fields' must be array. | The `fields` parameter is not passed or is passed as a string or a number ||
|| `400` | Empty value | The parameter 'fields' is not valid. | `fields` does not contain `COMPANY_ID`, or it is less than or equal to `0` ||
|| `400` | Empty value | Access denied. | The user does not have read access to CRM objects, including those in digital workspaces ||
|| `403` | `ACCESS_DENIED` | Access denied! | The user does not have permission to edit the contact ||
|| `400` | Empty value | Not found. | The contact with the provided `id` was not found ||
|| `400` | Empty value | [Company #18] You don't have permission to view this item. | The user does not have permission to read the company. The error text contains the company identifier ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-contact-company-delete.md)
- [{#T}](./crm-contact-company-fields.md)
- [{#T}](./crm-contact-company-items-get.md)
- [{#T}](./crm-contact-company-items-set.md)
- [{#T}](./crm-contact-company-items-delete.md)
