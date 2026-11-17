# Get a List of Contacts crm.contact.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: any user with "read" access permission for contacts

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use the REST 3.0 method [crm.contact.list](../rest-v3/items/crm-entity-list-rest-v3.md).

{% endnote %}

The method `crm.contact.list` returns a list of contacts based on a filter.

The method selects only contacts from the general category. A filter by `CATEGORY_ID` does not change this behavior. Retrieve contacts from other categories, such as "Vendor Contacts", using [crm.item.list](../universal/crm-item-list.md) with `entityTypeId=3` and `categoryId`.

To retrieve a list of companies associated with a contact, use the method [`crm.contact.company.items.get`](company/crm-contact-company-items-get.md).

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | A list of contact fields to return in the response. The `ID` field is returned even if it is not included in the list.

Masks:
- `'*'` — all fields except custom and multiple fields
- `'UF_*'` — all custom fields, including multiple fields

To select multiple contact details, specify the required fields in the selection list (`PHONE`, `EMAIL`, etc.).

You can find the list of available fields for selection using the method [`crm.contact.fields`](crm-contact-fields.md).

By default, standard fields are returned. To retrieve custom fields, specify them or the `UF_*` mask
||
|| **filter**
[`object`](../../data-types.md) | An object in the format:

```text
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

where:
- `field_n` — the name of the field by which contacts are filtered
- `value_n` — the filter value

You can add a prefix to the keys `field_n` to specify the filter operation.
Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for a substring in any position of the string
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

A filter by the multiple fields `PHONE`, `EMAIL`, `WEB`, and `IM` without a prefix searches for values that start with the provided string. A filter by `LINK` is ignored.

Additionally, the `LIKE` filter does not work with fields of type `crm_status`, `crm_contact`, `crm_company` — for example, Contact Type (`TYPE_ID`), Salutation (`HONORIFIC`), etc.

You can find the list of available fields for filtering using the method [`crm.contact.fields`](crm-contact-fields.md).

The `logic` key in the filter is not supported. To use complex logic in the filter, use the method [crm.item.list](../universal/crm-item-list.md).
||
|| **order**
[`object`](../../data-types.md) | An object in the format:

```text
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```
where:
- `field_n` — the name of the field by which contacts are sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending order
    - `DESC` — descending order

Multiple contact details, such as `EMAIL` and `PHONE`, are not supported for sorting.

You can find the list of available fields for sorting using the method [`crm.contact.fields`](crm-contact-fields.md)
||
|| **start**
[`integer`](../../data-types.md) | A parameter for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the desired page number
||
|#

Also, see the description of [list methods](../../../settings/how-to-call-rest-api/list-methods-pecularities.md).

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

Get a list of contacts where:
1. the source is a CRM form
2. first name and last name are not empty
3. first name and last name start with "K"
4. the export flag is `Y`
5. e-mail starts with `special-for@example.com`
6. the responsible user is user 1 or 6
7. the creation date is no earlier than six months ago

Sort by last name and first name in ascending order.

For clarity, select only the necessary fields:
- contact ID
- first name
- last name
- e-mail
- whether the contact participates in export
- responsible user
- creation date

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FILTER":{"SOURCE_ID":"WEBFORM","!=NAME":"","!=LAST_NAME":"","=%NAME":"K%","=%LAST_NAME":"K%","EMAIL":"special-for@example.com","@ASSIGNED_BY_ID":[1,6],"EXPORT":"Y",">=DATE_CREATE":"**put_six_month_ago_date_here**"},"ORDER":{"LAST_NAME":"ASC","NAME":"ASC"},"SELECT":["ID","NAME","LAST_NAME","EMAIL","EXPORT","ASSIGNED_BY_ID","DATE_CREATE"]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.contact.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FILTER":{"SOURCE_ID":"WEBFORM","!=NAME":"","!=LAST_NAME":"","=%NAME":"K%","=%LAST_NAME":"K%","EMAIL":"special-for@example.com","@ASSIGNED_BY_ID":[1,6],"EXPORT":"Y",">=DATE_CREATE":"**put_six_month_ago_date_here**"},"ORDER":{"LAST_NAME":"ASC","NAME":"ASC"},"SELECT":["ID","NAME","LAST_NAME","EMAIL","EXPORT","ASSIGNED_BY_ID","DATE_CREATE"],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.contact.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame, ISODate } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each contact object returned in result[]
    type CrmContactListItem = {
      ID: string
      NAME: string
      LAST_NAME: string
      EXPORT: string
      ASSIGNED_BY_ID: string
      DATE_CREATE: ISODate | null
      EMAIL?: CrmContactEmail[]
    }

    type CrmContactEmail = {
      ID: string
      VALUE_TYPE: string
      VALUE: string
      TYPE_ID: string
    }

    const sixMonthAgo = new Date()
    sixMonthAgo.setMonth(new Date().getMonth() - 6)

    try {
      // crm.contact.list returns a single page (max 50 records). For the whole result set
      // use a list helper: $b24.actions.v2.callList.make() returns every record as one
      // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
      // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
      // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
      const response = await $b24.actions.v2.call.make<CrmContactListItem[]>({
        method: 'crm.contact.list',
        params: {
          filter: {
            SOURCE_ID: 'WEBFORM',
            '!=NAME': '',
            '!=LAST_NAME': '',
            '=%NAME': 'K%',
            '=%LAST_NAME': 'K%',
            EMAIL: 'special-for@example.com',
            '@ASSIGNED_BY_ID': [1, 6],
            EXPORT: 'Y',
            '>=DATE_CREATE': sixMonthAgo.toISOString(),
          },
          order: {
            LAST_NAME: 'ASC',
            NAME: 'ASC',
          },
          select: [
            'ID',
            'NAME',
            'LAST_NAME',
            'EMAIL',
            'EXPORT',
            'ASSIGNED_BY_ID',
            'DATE_CREATE',
          ],
          start: 0,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Contacts on this page:', result.length, result)
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
      async function listContacts() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const sixMonthAgo = new Date()
          sixMonthAgo.setMonth(new Date().getMonth() - 6)

          // crm.contact.list returns a single page (max 50 records). For the whole result set
          // use a list helper: $b24.actions.v2.callList.make() returns every record as one
          // array, $b24.actions.v2.fetchList.make() yields them in chunks (async generator).
          // NOTE: the list helpers do not accept `order` (it is excluded from their params, so
          // passing it is a TS error) — keep this call.make + `start` variant when sort matters.
          const response = await $b24.actions.v2.call.make({
            method: 'crm.contact.list',
            params: {
              filter: {
                SOURCE_ID: 'WEBFORM',
                '!=NAME': '',
                '!=LAST_NAME': '',
                '=%NAME': 'K%',
                '=%LAST_NAME': 'K%',
                EMAIL: 'special-for@example.com',
                '@ASSIGNED_BY_ID': [1, 6],
                EXPORT: 'Y',
                '>=DATE_CREATE': sixMonthAgo.toISOString(),
              },
              order: {
                LAST_NAME: 'ASC',
                NAME: 'ASC',
              },
              select: [
                'ID',
                'NAME',
                'LAST_NAME',
                'EMAIL',
                'EXPORT',
                'ASSIGNED_BY_ID',
                'DATE_CREATE',
              ],
              start: 0,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Contacts on this page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listContacts)
    </script>
    ```

- PHP

    ```php
    try {
        $sixMonthAgo = new DateTime();
        $sixMonthAgo->modify('-6 months');
    
        $response = $b24Service
            ->core
            ->call(
                'crm.contact.list',
                [
                    'filter' => [
                        'SOURCE_ID'      => 'WEBFORM',
                        '!=NAME'         => '',
                        '!=LAST_NAME'    => '',
                        '=%NAME'         => 'K%',
                        '=%LAST_NAME'    => 'K%',
                        'EMAIL'          => 'special-for@example.com',
                        '@ASSIGNED_BY_ID' => [1, 6],
                        'EXPORT'         => 'Y',
                        '>=DATE_CREATE'  => $sixMonthAgo->format('Y-m-d\TH:i:s'),
                    ],
                    'order'  => [
                        'LAST_NAME' => 'ASC',
                        'NAME'      => 'ASC',
                    ],
                    'select' => [
                        'ID',
                        'NAME',
                        'LAST_NAME',
                        'EMAIL',
                        'EXPORT',
                        'ASSIGNED_BY_ID',
                        'DATE_CREATE',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching contact list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    const sixMonthAgo = new Date();
    sixMonthAgo.setMonth((new Date()).getMonth() - 6);

    BX24.callMethod(
        'crm.contact.list',
        {
            filter: {
                "SOURCE_ID": "WEBFORM",
                "!=NAME": "",
                "!=LAST_NAME": "",
                "=%NAME": "K%",
                "=%LAST_NAME": "K%",
                "EMAIL": "special-for@example.com",
                "@ASSIGNED_BY_ID": [1, 6],
                "EXPORT": "Y",
                ">=DATE_CREATE": sixMonthAgo.toISOString(),
            },
            order: {
                LAST_NAME: "ASC",
                NAME: "ASC",
            },
            select: [
                "ID",
                "NAME",
                "LAST_NAME",
                "EMAIL",
                "EXPORT",
                "ASSIGNED_BY_ID",
                "DATE_CREATE",
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

    $sixMonthAgo = new DateTime();
    $sixMonthAgo->modify('-6 months');

    $result = CRest::call(
        'crm.contact.list',
        [
            'FILTER' => [
                'SOURCE_ID' => 'WEBFORM',
                '!=NAME' => '',
                '!=LAST_NAME' => '',
                '=%NAME' => 'K%',
                '=%LAST_NAME' => 'K%',
                'EMAIL' => 'special-for@example.com',
                '@ASSIGNED_BY_ID' => [1, 6],
                'EXPORT' => 'Y',
                '>=DATE_CREATE' => $sixMonthAgo->format(DateTime::ATOM),
            ],
            'ORDER' => [
                'LAST_NAME' => 'ASC',
                'NAME' => 'ASC',
            ],
            'SELECT' => [
                'ID',
                'NAME',
                'LAST_NAME',
                'EMAIL',
                'EXPORT',
                'ASSIGNED_BY_ID',
                'DATE_CREATE',
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
    res, err := client.Core().Call(ctx, "crm.contact.list", b24.Params{
    	"filter": b24.Params{
		"SOURCE_ID":       "WEBFORM",
		"!=NAME":          "",
		"!=LAST_NAME":     "",
		"=%NAME":          "K%",
		"=%LAST_NAME":     "K%",
		"EMAIL":           "special-for@example.com",
		"@ASSIGNED_BY_ID": []int{1, 6},
		"EXPORT":          "Y",
    		">=DATE_CREATE":   "**put_six_month_ago_date_here**",
    	},
    	"order": b24.Params{
    		"LAST_NAME": "ASC",
    		"NAME":      "ASC",
    	},
    	"select": []string{"ID", "NAME", "LAST_NAME", "EMAIL", "EXPORT", "ASSIGNED_BY_ID", "DATE_CREATE"},
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.contact.list: %w", err)
    }

    var items []struct {
    	ID           b24.ID `json:"ID"`
    	Name         string `json:"NAME"`
    	LastName     string `json:"LAST_NAME"`
    	Export       string `json:"EXPORT"`
    	AssignedByID b24.ID `json:"ASSIGNED_BY_ID"`
    	DateCreate   string `json:"DATE_CREATE"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Name)
    }

    // Total and Next are filled in by list methods; for a full
    // list traversal, use client.Core().Pages and Scan.
    if res.Total != nil {
    	fmt.Println("total:", *res.Total)
    }
    ```

{% endlist %}

## Response Handling

Example response to a request made on August 16, 2024: the lower bound of `DATE_CREATE` is February 16, 2024.

HTTP Status: **200**

```json
{
    "result": [
        {
            "ID": "78",
            "NAME": "Klaus",
            "LAST_NAME": "Keller",
            "EXPORT": "Y",
            "ASSIGNED_BY_ID": "1",
            "DATE_CREATE": "2024-08-15T00:00:00+02:00",
            "EMAIL": [
                {
                    "ID": "218",
                    "VALUE_TYPE": "WORK",
                    "VALUE": "special-for@example.com",
                    "TYPE_ID": "EMAIL"
                }
            ]
        }
    ],
    "total": 1,
	"time": {
		"start": 1723807142.916445,
		"finish": 1723807143.44846,
		"duration": 0.5320150852203369,
		"processing": 0.1967020034790039,
		"date_start": "2024-08-16T13:19:02+02:00",
		"date_finish": "2024-08-16T13:19:03+02:00"
	}
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`contact[]`](./crm-contact-get.md#contact) | The root element of the response. An array containing information about the found contacts.

The `select` parameter defines the contact fields ||
|| **total**
[`integer`](../../data-types.md) | The total number of contacts found based on the specified conditions ||
|| **next**
[`integer`](../../data-types.md) | Value for `start` in the next request to retrieve the next page. The `next` field is returned if there are more records after the current page ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | Empty value | `Access denied.` | The user does not have permission for "Read" contacts ||
|| `400` | Empty value | `Parameter 'order' must be array.` | A non-array value was passed to the `order` parameter ||
|| `400` | Empty value | `Parameter 'filter' must be array.` | A non-array value was passed to the `filter` parameter ||
|| `400` | Empty value | `Failed to get list. General error.` | An unknown error occurred ||
|#

{% include [system errors](./../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-contact-add.md)
- [{#T}](./crm-contact-update.md)
- [{#T}](./crm-contact-get.md)
- [{#T}](./crm-contact-delete.md)
- [{#T}](./crm-contact-fields.md)
- [{#T}](../../../tutorials/crm/how-to-get-lists/search-by-phone-and-email.md)
