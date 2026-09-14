# Set a Set of Contacts Associated with the Specified Company crm.company.contact.items.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "Edit" access permission for companies

The method `crm.company.contact.items.set` sets a set of contacts associated with the specified company.

The method replaces the entire set: the contacts that are not in `items` are unlinked from the company. For every unlinked contact, the `COMPANY_ID` field switches to another of its companies with the lowest identifier, or is cleared when the contact has no other companies.

To add or remove a single contact without affecting the others, use [crm.company.contact.add](./crm-company-contact-add.md) and [crm.company.contact.delete](./crm-company-contact-delete.md). The structure of the binding object is described in the [section overview](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the company. Must be greater than `0`.

The identifier can be obtained using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 4`
||
|| **items***
[`object[]`](../../../data-types.md) | A set of objects that describe the contacts linked to the company. The structure of the binding object is detailed [below](#company_contact_binding).

An empty array unlinks all contacts from the company.

Elements without `CONTACT_ID`, or with a value less than or equal to `0`, are skipped by the method without an error, but their position is taken into account when calculating `SORT` for the other bindings ||
|#

### Parameter items {#company_contact_binding}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`crm_entity`](../../data-types.md) | Identifier of the contact to be linked to the company.

The identifier can be obtained using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 3`.

The method does not check whether the contact exists: the binding is created even with the identifier of a non-existent contact ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md#standart-types) | Indicates whether the binding is primary. Possible values:
- `Y` — yes
- `N` — no

The flag belongs to the contact and means that this company is the primary one for it.

A new binding always receives `IS_PRIMARY = Y`, regardless of the value provided. For the contact's previous primary company, the flag is reset to `N`.

For the bindings the company already had, the flag is set according to the provided set: `Y` is retained by the first binding with `IS_PRIMARY = Y`, and if `Y` is not provided in any binding, by the first binding in `items`. For the other previously existing bindings, the flag is reset to `N`.

Therefore, when `items` contains both new and already existing bindings, `IS_PRIMARY = Y` can end up on several contacts at once
||
|| **SORT**
[`integer`](../../../data-types.md) | Sort index.

If `SORT` is not provided, the method calculates it from the position of the binding in `items` as `(n + 1) * 10`, where `n` is the ordinal number of the binding in `items`, counting from zero. The first binding gets `10`, the second one `20`, and so on.

This rule also applies to bindings that already exist: their previous sort index is overwritten. To retain the index, provide `SORT` explicitly ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32,"items":[{"CONTACT_ID":8,"IS_PRIMARY":"Y","SORT":100},{"CONTACT_ID":9,"SORT":200},{"CONTACT_ID":10,"SORT":400}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.company.contact.items.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":32,"items":[{"CONTACT_ID":8,"IS_PRIMARY":"Y","SORT":100},{"CONTACT_ID":9,"SORT":200},{"CONTACT_ID":10,"SORT":400}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.company.contact.items.set
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
        method: 'crm.company.contact.items.set',
        params: {
          id: 32,
          items: [
            {
              CONTACT_ID: 8,
              IS_PRIMARY: 'Y',
              SORT: 100,
            },
            {
              CONTACT_ID: 9,
              SORT: 200,
            },
            {
              CONTACT_ID: 10,
              SORT: 400,
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
        console.info('Company contacts set:', result)
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
      async function setCompanyContactItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.company.contact.items.set',
            params: {
              id: 32,
              items: [
                {
                  CONTACT_ID: 8,
                  IS_PRIMARY: 'Y',
                  SORT: 100,
                },
                {
                  CONTACT_ID: 9,
                  SORT: 200,
                },
                {
                  CONTACT_ID: 10,
                  SORT: 400,
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
          console.info('Company contacts set:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setCompanyContactItems)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.company.contact.items.set(
            bitrix_id=32,
            items=[
                {
                    "CONTACT_ID": 8,
                    "IS_PRIMARY": "Y",
                    "SORT": 100,
                },
                {
                    "CONTACT_ID": 9,
                    "SORT": 200,
                },
                {
                    "CONTACT_ID": 10,
                    "SORT": 400,
                },
            ],
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
                'crm.company.contact.items.set',
                [
                    'id'    => 32,
                    'items' => [
                        [
                            'CONTACT_ID' => 8,
                            'IS_PRIMARY' => 'Y',
                            'SORT'       => 100,
                        ],
                        [
                            'CONTACT_ID' => 9,
                            'SORT'       => 200,
                        ],
                        [
                            'CONTACT_ID' => 10,
                            'SORT'       => 400,
                        ],
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
        echo 'Error setting contact items for company: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'crm.company.contact.items.set',
        {
            id: 32,
            items: [
                {
                    CONTACT_ID: 8,
                    IS_PRIMARY: "Y",
                    SORT: 100,
                },
                {
                    CONTACT_ID: 9,
                    SORT: 200,
                },
                {
                    CONTACT_ID: 10,
                    SORT: 400,
                }
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
        'crm.company.contact.items.set',
        [
            'id' => 32,
            'items' => [
                [
                    'CONTACT_ID' => 8,
                    'IS_PRIMARY' => 'Y',
                    'SORT' => 100,
                ],
                [
                    'CONTACT_ID' => 9,
                    'SORT' => 200,
                ],
                [
                    'CONTACT_ID' => 10,
                    'SORT' => 400,
                ]
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
    res, err := client.Core().Call(ctx, "crm.company.contact.items.set", b24.Params{
    	"id": 32,
    	"items": []b24.Params{
    		{
    			"CONTACT_ID": 8,
    			"IS_PRIMARY": "Y",
    			"SORT":       100,
    		},
    		{
    			"CONTACT_ID": 9,
    			"SORT":       200,
    		},
    		{
    			"CONTACT_ID": 10,
    			"SORT":       400,
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.company.contact.items.set: %w", err)
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
        "start": 1724139480.073569,
        "finish": 1724139481.016709,
        "duration": 0.9431400299072266,
        "processing": 0.4230809211730957,
        "date_start": "2024-08-20T09:38:00+02:00",
        "date_finish": "2024-08-20T09:38:01+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response. Contains `true` in case of success.

The method also returns `true` when the provided set matches the current one ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "",
    "error_description": "The parameter items must be array."
}
```

{% include notitle [Error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | `The parameter ownerEntityID is invalid or not defined.` | The provided `id` is less than or equal to 0 or not provided at all ||
|| Empty value | `The parameter items must be array.` | The `items` parameter is not an array ||
|| `ACCESS_DENIED` | `Access denied!` | The user does not have permission to edit companies ||
|| Empty value | `Not found.` | The company with the provided `id` was not found ||
|#

{% include [System errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-company-contact-add.md)
- [{#T}](./crm-company-contact-delete.md)
- [{#T}](./crm-company-contact-items-get.md)
- [{#T}](./crm-company-contact-items-delete.md)
- [{#T}](./crm-company-contact-fields.md)