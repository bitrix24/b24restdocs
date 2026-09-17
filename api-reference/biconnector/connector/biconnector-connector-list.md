# Get a List of Connectors biconnector.connector.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions

The `biconnector.connector.list` method returns a list of connectors by filter.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and returns only the connectors that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

The result page size is 50 records. The method returns neither the total number of connectors nor a link to the next page, so [list traversal](../index.md#pagination) is built on the page number: the selection has ended when fewer than 50 records arrive in the response.

## Method Parameters

All parameters are optional: the method can be called with an empty request body.

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | List of fields to be filled in for the connectors in the selection. Allowed values are the field names from the [selection element](#connector) table and `*`. The `["*"]` value returns all fields and is also used by default ||
|| **filter**
[`object`](../../data-types.md) | Filter for selecting connectors. Example format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2"
}
```

A prefix that refines how the filter works can be added to the `field_n` keys. Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The symbol `%` does not need to be passed in the filter value. The search looks for a substring in any position of the string
- `=%` — LIKE, substring search. The symbol `%` must be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The list of fields available for filtering can be retrieved with the [biconnector.connector.fields](./biconnector-connector-fields.md) method.

The `logic` key defines how the filter conditions are combined:

- `AND` — a connector is included in the selection if all conditions are met. Used by default
- `OR` — one met condition is enough

Any other value of the `logic` key causes the `VALIDATION_INVALID_FILTER_LOGIC` error. Condition groups can be nested into one another.

```json
{
    "logic": "AND",
    "!description": "",
    "0": {
        "logic": "OR",
        "%=title": "MyConnector%",
        "@id": [9, 11]
    }
}
```
||
|| **order**
[`object`](../../data-types.md) | Sorting parameters. Example format:

```
{
    field_1: value_1,
    field_2: value_2,
    ...,
    field_n: value_n,
}
```

- `field_n` — the name of the field by which the selection of connectors will be sorted
- `value_n` — a `string` type value, equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

Without this parameter, no sorting is applied and the order of records in the selection is not guaranteed.

The direction value is case-insensitive, but the field accepts no other values. An empty string, a number, or any word other than `ASC` and `DESC` breaks off at the ORM level: the response arrives with HTTP status **400** and the `ERROR_ARGUMENT` error in the root, not [inside `result`](../index.md#errors) like the other errors of the section
||
|| **page**
[`integer`](../../data-types.md) | Number of the result page. Numbering starts at one, and the default value is 1. A non-numeric, zero, or negative value causes no error: the method silently returns the first page. The `start` parameter, common to most list methods of the REST API, does not work here ||
|#

The `settings` field is retained as a JSON string, so connectors cannot be selected or sorted by individual settings parameters: the filter and the sorting work with this string as a whole.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Get a list of connectors where:

- the name starts with `MyConnector`
- the description is not empty

Return only the required fields:

- identifier `id`
- name `title`
- endpoint for checking the source availability `urlCheck`
- Create date `dateCreate`

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{
             "select": [
                 "id",
                 "title",
                 "urlCheck",
                 "dateCreate"
             ],
             "filter": {
                 "%=title": "MyConnector%",
                 "!description": ""
             },
             "order": {
                 "dateCreate": "DESC"
             },
             "page": 1,
             "auth": "**put_access_token_here**"
             }' \
         https://**put_your_bitrix24_address**/rest/biconnector.connector.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Methods of this section put errors inside result and answer with HTTP 200
    type BiconnectorError = {
      error: {
        error: string
        error_description: string
      }
    }

    // Shape of each connector returned in result[]
    type ConnectorItem = {
      id: number
      title: string
      urlCheck: string
      dateCreate: string | null // Y-m-d H:i:s, not ISO 8601
    }

    try {
      // biconnector.connector.list returns a single page (max 50 records). The list helpers
      // ($b24.actions.v2.callList.make, fetchList.make) do not work here: this method uses
      // its own `page` navigation and returns neither `total` nor `next`. Walk the pages
      // yourself, increasing `page` until a response comes back with fewer than 50 records.
      const response = await $b24.actions.v2.call.make<ConnectorItem[] | BiconnectorError>({
        method: 'biconnector.connector.list',
        params: {
          select: [
            'id',
            'title',
            'urlCheck',
            'dateCreate',
          ],
          filter: {
            '%=title': 'MyConnector%',
            '!description': '',
          },
          order: {
            dateCreate: 'DESC',
          },
          page: 1,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result

        // The SDK sees HTTP 200 as success, so check the error inside result yourself
        if (!Array.isArray(result)) {
          console.error(result.error.error, result.error.error_description)
        } else {
          console.info('Connectors:', result.length, result)
        }
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
      async function loadConnectorList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // biconnector.connector.list returns a single page (max 50 records). The list helpers
          // ($b24.actions.v2.callList.make, fetchList.make) do not work here: this method uses
          // its own `page` navigation and returns neither `total` nor `next`. Walk the pages
          // yourself, increasing `page` until a response comes back with fewer than 50 records.
          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.connector.list',
            params: {
              select: [
                'id',
                'title',
                'urlCheck',
                'dateCreate',
              ],
              filter: {
                '%=title': 'MyConnector%',
                '!description': '',
              },
              order: {
                dateCreate: 'DESC',
              },
              page: 1,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result

          // The SDK sees HTTP 200 as success, so check the error inside result yourself
          if (result && result.error) {
            console.error(result.error.error, result.error.error_description)
            return
          }

          console.info('Connectors:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', loadConnectorList)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.biconnector.connector.list(
            select=[
                "id",
                "title",
                "urlCheck",
                "dateCreate",
            ],
            filter={
                "%=title": "MyConnector%",
                "!description": "",
            },
            order={
                "dateCreate": "DESC",
            },
            page=1,
        ).response
        result = bitrix_response.result

        # Methods of this section put errors inside result and answer with HTTP 200
        if isinstance(result, dict) and "error" in result:
            print(
                "BIconnector error",
                f"error: {result['error']['error']}",
                f"error_description: {result['error']['error_description']}",
                sep="\n",
            )
        else:
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
                'biconnector.connector.list',
                [
                    'select' => [
                        "id",
                        "title",
                        "urlCheck",
                        "dateCreate"
                    ],
                    'filter' => [
                        '%=title'      => "MyConnector%",
                        '!description' => ''
                    ],
                    'order' => [
                        'dateCreate' => "DESC"
                    ],
                    'page' => 1
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            $data = $result->data();

            // Methods of this section put errors inside result and answer with HTTP 200
            if (isset($data['error'])) {
                echo 'BIconnector error: ' . $data['error']['error'] . ': ' . $data['error']['error_description'];
            } else {
                echo 'Data: ' . print_r($data, true);
            }
        }

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error calling biconnector.connector.list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.connector.list',
        {
            select: [
                "id",
                "title",
                "urlCheck",
                "dateCreate"
            ],
            filter: {
                '%=title': "MyConnector%",
                '!description': ''
            },
            order: {
                dateCreate: "DESC"
            },
            page: 1
        },
        (result) => {
            if (result.error()) {
                console.error(result.error());
                return;
            }

            const data = result.data();

            // Methods of this section put errors inside result and answer with HTTP 200
            if (data && data.error) {
                console.error(data.error.error, data.error.error_description);
                return;
            }

            console.info(data);
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'biconnector.connector.list',
        [
            'select' => [
                "id",
                "title",
                "urlCheck",
                "dateCreate"
            ],
            'filter' => [
                '%=title' => "MyConnector%",
                '!description' => ''
            ],
            'order' => [
                'dateCreate' => "DESC"
            ],
            'page' => 1
        ]
    );

    // Methods of this section put errors inside result and answer with HTTP 200
    if (isset($result['result']['error'])) {
        echo 'BIconnector error: ' . $result['result']['error']['error']
            . ': ' . $result['result']['error']['error_description'];
    } else {
        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "biconnector.connector.list", b24.Params{
    	"select": []string{"id", "title", "urlCheck", "dateCreate"},
    	"filter": b24.Params{
    		"%=title":      "MyConnector%",
    		"!description": "",
    	},
    	"order": b24.Params{
    		"dateCreate": "DESC",
    	},
    	"page": 1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("biconnector.connector.list: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.connector.list: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
    }

    var items []struct {
    	ID         b24.ID `json:"id"`
    	Title      string `json:"title"`
    	UrlCheck   string `json:"urlCheck"`
    	DateCreate string `json:"dateCreate"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Title)
    }

    // This method returns neither Total nor Next: pagination is built
    // on the page parameter, while 50 records keep arriving in the response.
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": [
        {
            "id": 11,
            "title": "MyConnector_2",
            "urlCheck": "https://new_example.com/check",
            "dateCreate": "2025-03-24 07:25:59"
        },
        {
            "id": 9,
            "title": "MyConnector",
            "urlCheck": "https://example.com/check",
            "dateCreate": "2025-03-21 12:22:32"
        }
    ],
    "time": {
        "start": 1742804947.923552,
        "finish": 1742804947.995446,
        "duration": 0.07189393043518066,
        "processing": 0.0017020702362060547,
        "date_start": "2025-03-24T08:29:07+00:00",
        "date_finish": "2025-03-24T08:29:07+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Root element of the response. A flat array of connectors without an additional wrapper [(detailed description)](#connector) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Element of the result array {#connector}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the connector ||
|| **title**
[`string`](../../data-types.md) | Connector name ||
|| **logo**
[`string`](../../data-types.md) | Logo URL or a base64 string ||
|| **description**
[`string`](../../data-types.md) | Connector description ||
|| **sort**
[`integer`](../../data-types.md) | Sorting order ||
|| **urlCheck**
[`string`](../../data-types.md) | [URL for checking the connection](./index.md#urlCheck) ||
|| **urlData**
[`string`](../../data-types.md) | [URL for retrieving data](./index.md#urlData) ||
|| **urlTableList**
[`string`](../../data-types.md) | [URL for the table list](./index.md#urlTableList) ||
|| **urlTableDescription**
[`string`](../../data-types.md) | [URL for the table description](./index.md#urlTableDescription) ||
|| **settings**
[`array`](../../data-types.md) | Authorization parameters of the connector. The structure of an element is described in the [Settings Field](./index.md#settings) section. The parameter values are retained by the source, while the connector returns only their description ||
|| **supportMapping**
[`boolean`](../../data-types.md) | Support for mapping the table fields to the fields of the external system ||
|| **sourceCode**
[`string`](../../data-types.md) | String code of the external system ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date the connector was created, in the `Y-m-d H:i:s` format ||
|#

If the `select` parameter is specified, only the listed fields remain in the elements.

## Error Handling

HTTP status: **200**

```json
{
    "result": {
        "error": {
            "error": "VALIDATION_SELECT_TYPE",
            "error_description": "Parameter \"select\" must be array."
        }
    }
}
```

{% note warning "" %}

The method returns an error [inside the `result` field](../index.md#errors) and with HTTP status 200. Check `result.error`: the SDK wrappers parse only the top level of the response and treat such an error as a success

{% endnote %}

One error of the method arrives differently. If the `order` parameter receives a sorting direction other than `ASC` and `DESC`, the request breaks off at the ORM level: the response gets HTTP status **400**, and the error code is in the root rather than inside `result`.

```json
{
    "error": "ERROR_ARGUMENT",
    "error_description": "Invalid order \"UPWARDS\""
}
```

{% include notitle [Error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ACCESS_DENIED` | Access denied. | One of the two permissions is missing, or the method was called via a webhook or outside the application context ||
|| `VALIDATION_SELECT_TYPE` | Parameter "select" must be array. | The `select` parameter must be an array ||
|| `VALIDATION_FILTER_TYPE` | Parameter "filter" must be array. | The `filter` parameter must be an array ||
|| `VALIDATION_ORDER_TYPE` | Parameter "order" must be array. | The `order` parameter must be an array ||
|| `VALIDATION_FIELD_NOT_ALLOWED_IN_SELECT` | Field "#TITLE#" is not allowed in the "select". | These fields are not allowed in the selection ||
|| `VALIDATION_FIELD_NOT_ALLOWED_IN_FILTER` | Field "#TITLE#" is not allowed in the "filter". | These fields are not allowed in the filter ||
|| `VALIDATION_FIELD_NOT_ALLOWED_IN_ORDER` | Field "#TITLE#" is not allowed in the "order". | These fields are not allowed for sorting ||
|| `VALIDATION_INVALID_FILTER_LOGIC` | Field "logic" must be either "AND" or "OR". | The `logic` field can only have the value "AND" or "OR" ||
|| `ERROR_ARGUMENT` | Invalid order "#VALUE#". | The sorting direction in `order` differs from `ASC` and `DESC`. `#VALUE#` is replaced with the passed value in uppercase. This is the only error of the method with HTTP status 400: the code arrives in the root of the response, not inside `result` ||

|#

{% include [System errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./biconnector-connector-add.md)
- [{#T}](./biconnector-connector-update.md)
- [{#T}](./biconnector-connector-get.md)
- [{#T}](./biconnector-connector-delete.md)
- [{#T}](./biconnector-connector-fields.md)
