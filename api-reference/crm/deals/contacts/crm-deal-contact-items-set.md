# Set a set of contacts associated with the specified deal crm.deal.contact.items.set

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the "modify" access permission for the deal

The method `crm.deal.contact.items.set` sets a set of contacts associated with the specified deal. Contacts not included in the provided list will be unlinked.

To add or remove a single contact without affecting the others, use [crm.deal.contact.add](./crm-deal-contact-add.md) or [crm.deal.contact.delete](./crm-deal-contact-delete.md). How the link works and how to choose a method is described in the [section overview](./index.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the deal

Can be obtained using the methods [crm.deal.list](../crm-deal-list.md) or [crm.deal.add](../crm-deal-add.md)
||
|| **items***
[`object[]`](../../../data-types.md) | A set of objects describing the contacts linked to the deal [(detailed description)](#deal_contact_binding).

If you pass an empty array `[]`, the method unlinks all contacts from the deal.

The method silently skips elements without a valid `CONTACT_ID`. If the same contact is passed several times, the last occurrence is retained together with its `IS_PRIMARY` and `SORT`, so pass each contact only once ||
|#

### Object deal_contact_binding {#deal_contact_binding}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CONTACT_ID***
[`integer`](../../../data-types.md) | Identifier of the contact to be linked to the deal

You can retrieve the identifier using the method [crm.item.list](../../universal/crm-item-list.md) with `entityTypeId = 3`.

The method does not check that the contact exists: a binding to a nonexistent identifier is created as well ||
|| **IS_PRIMARY**
[`char`](../../../data-types.md) | Indicates whether the contact is the primary contact of the deal. Possible values:
- `Y` — yes
- `N` — no

If no binding has `IS_PRIMARY = Y`, the first contact in `items` with a valid `CONTACT_ID` becomes primary.

If `IS_PRIMARY = Y` is passed for several bindings, the first of these contacts becomes primary.

The identifier of the primary contact is written to the deal field `CONTACT_ID` ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting index

If `SORT` is not provided or is not greater than 0, `(i + 1) * 10` is used, where `i` is the index of the element in the `items` array, starting from 0. Skipped elements without a valid `CONTACT_ID` are also counted in the index ||
|#

## Code Examples

{% include [Example Note](../../../../_includes/examples.md) %}

Set the following linked contacts for the deal with `id = 1875`:
- contact with `id = 55`, make it primary and set `SORT = 100`
- contact with `id = 54`, set `SORT = 200`
- contact with `id = 56`, set `SORT = 400`

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1875,"items":[{"CONTACT_ID":55,"IS_PRIMARY":"Y","SORT":100},{"CONTACT_ID":54,"SORT":200},{"CONTACT_ID":56,"SORT":400}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.deal.contact.items.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1875,"items":[{"CONTACT_ID":55,"IS_PRIMARY":"Y","SORT":100},{"CONTACT_ID":54,"SORT":200},{"CONTACT_ID":56,"SORT":400}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.deal.contact.items.set
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
        method: 'crm.deal.contact.items.set',
        params: {
          id: 1875,
          items: [
            {
              CONTACT_ID: 55,
              IS_PRIMARY: 'Y',
              SORT: 100,
            },
            {
              CONTACT_ID: 54,
              SORT: 200,
            },
            {
              CONTACT_ID: 56,
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
        console.info('Deal contacts set:', result)
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
      async function setDealContactItems() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.deal.contact.items.set',
            params: {
              id: 1875,
              items: [
                {
                  CONTACT_ID: 55,
                  IS_PRIMARY: 'Y',
                  SORT: 100,
                },
                {
                  CONTACT_ID: 54,
                  SORT: 200,
                },
                {
                  CONTACT_ID: 56,
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
          console.info('Deal contacts set:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', setDealContactItems)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.deal.contact.items.set(
            bitrix_id=1875,
            items=[
                {
                    "CONTACT_ID": 55,
                    "IS_PRIMARY": "Y",
                    "SORT": 100,
                },
                {
                    "CONTACT_ID": 54,
                    "SORT": 200,
                },
                {
                    "CONTACT_ID": 56,
                    "SORT": 400,
                },
            ],
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
                'crm.deal.contact.items.set',
                [
                    'id'    => 1875,
                    'items' => [
                        [
                            'CONTACT_ID' => 55,
                            'IS_PRIMARY' => 'Y',
                            'SORT'       => 100,
                        ],
                        [
                            'CONTACT_ID' => 54,
                            'SORT'       => 200,
                        ],
                        [
                            'CONTACT_ID' => 56,
                            'SORT'       => 400,
                        ],
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
        'crm.deal.contact.items.set',
        {
            id: 1875,
            items: [
                {
                    CONTACT_ID: 55,
                    IS_PRIMARY: "Y",
                    SORT: 100,
                },
                {
                    CONTACT_ID: 54,
                    SORT: 200,
                },
                {
                    CONTACT_ID: 56,
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
        'crm.deal.contact.items.set',
        [
            'id' => 1875,
            'items' => [
                [
                    'CONTACT_ID' => 55,
                    'IS_PRIMARY' => 'Y',
                    'SORT' => 100,
                ],
                [
                    'CONTACT_ID' => 54,
                    'SORT' => 200,
                ],
                [
                    'CONTACT_ID' => 56,
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
    res, err := client.Core().Call(ctx, "crm.deal.contact.items.set", b24.Params{
    	"id": 1875,
    	"items": []b24.Params{
    		{
    			"CONTACT_ID": 55,
    			"IS_PRIMARY": "Y",
    			"SORT":       100,
    		},
    		{
    			"CONTACT_ID": 54,
    			"SORT":       200,
    		},
    		{
    			"CONTACT_ID": 56,
    			"SORT":       400,
    		},
    	},
    })
    if err != nil {
    	return fmt.Errorf("crm.deal.contact.items.set: %w", err)
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
        "start": 1773225418,
        "finish": 1773225418.981286,
        "duration": 0.9812860488891602,
        "processing": 0,
        "date_start": "2026-03-11T13:36:58+01:00",
        "date_finish": "2026-03-11T13:36:58+01:00",
        "operating_reset_at": 1773226018,
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

The method also returns `true` when the passed set matches the current one ||
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
|| 400 | Empty value | `The parameter ownerEntityID is invalid or not defined.` | The `id` is less than 1 or not provided at all ||
|| 400 | Empty value | `The parameter items must be array.` | The `items` parameter is not provided or is not an array ||
|| 400 | Empty value | `Access denied.` | The deal with the provided `id` was not found, and the user does not have the "modify" access permission for deals ||
|| 403 | `ACCESS_DENIED` | `Access denied!` | No "modify" access permission for the deal ||
|| 400 | Empty value | `Not found.` | The deal with the provided `id` was not found ||
|| 400 | Empty value | `[Contact #1] You don't have permission to view this item.` | The set changes, and the user does not have the "read" access permission for the contact with the identifier `1`. Due to an implementation detail, the method checks the permission for this contact rather than for the contacts from `items` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-deal-contact-add.md)
- [{#T}](./crm-deal-contact-delete.md)
- [{#T}](./crm-deal-contact-items-get.md)
- [{#T}](./crm-deal-contact-items-delete.md)
- [{#T}](./crm-deal-contact-fields.md)