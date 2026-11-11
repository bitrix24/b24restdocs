# Get a List of Datasets biconnector.dataset.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`biconnector`](../../scopes/permissions.md)
>
> Who can execute the method: A user who has both the "Access to BI Builder" and "Access to Analytics Hub" permissions


{% note warning "DEPRECATED" %}

Development of the method has been discontinued. Use [biconnector.table.list](../table/biconnector-table-list.md).

{% endnote %}

The `biconnector.dataset.list` method returns a list of datasets by filter.

{% note warning "" %}

The method works only in the context of an [application](../../../settings/app-installation/index.md) and returns only the datasets that the application created itself. When called via a webhook, the method returns the `ACCESS_DENIED` error

{% endnote %}

## Method Parameters

All parameters are optional: the method can be called with an empty request body.

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`string[]`](../../data-types.md) | List of fields to be filled in for the datasets in the selection. Allowed values are the field names from the schema of the [biconnector.dataset.fields](./biconnector-dataset-fields.md) method and `*`. By default all fields are taken, and an explicit `select: ["*"]` is allowed as well.

The `csvDelimiter`, `csvEncoding`, and `csvHasHeaders` fields are not part of the schema: they do arrive in the response, but in `select` they cause the `VALIDATION_FIELD_NOT_ALLOWED_IN_SELECT` error.

This is the only method of the family that returns `sourceId`: the field works in `select`. The `fields` field is not supported and is ignored ||
|| **filter**
[`object`](../../data-types.md) | Filter for selecting datasets. Example format:

```json
{
    "field_1": "value_1",
    "field_2": "value_2"
}
```

A prefix can be added to the `field_n` keys to specify the filter behavior.
Possible prefix values:

- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN, an array is passed as the value
- `!@` — NOT IN, an array is passed as the value
- `%` — LIKE, substring search. The `%` character does not need to be passed in the filter value. The search looks for a substring in any position of the string
- `=%` — LIKE, substring search. The `%` character must be passed in the value. Examples:
    - `"mol%"` — searches for values starting with "mol"
    - `"%mol"` — searches for values ending with "mol"
    - `"%mol%"` — searches for values where "mol" can be in any position
- `%=` — LIKE (similar to `=%`)
- `=` — equal, exact match (used by default)
- `!=` — not equal
- `!` — not equal

The list of available fields for filtering can be obtained using the [biconnector.dataset.fields](./biconnector-dataset-fields.md) method.

The `logic` key defines how the filter conditions are combined:

- `AND` — a dataset is included in the selection if all conditions are met. Used by default
- `OR` — one met condition is enough

Any other value of the `logic` key causes the `VALIDATION_INVALID_FILTER_LOGIC` error. Condition groups can be nested into one another.

The filter does not support the `fields` field; it will be ignored
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

where:

- `field_n` — the name of the field by which the dataset selection will be sorted
- `value_n` — a `string` value equal to:
    - `ASC` — ascending sort
    - `DESC` — descending sort

Without this parameter, no sorting is applied and the order of records in the selection is not guaranteed.

The direction value is case-insensitive, but the field accepts no other values. An empty string, a number, or any word other than `ASC` and `DESC` breaks off at the ORM level: the response arrives with HTTP status **400** and the `ERROR_ARGUMENT` error in the root, not [inside `result`](../index.md#errors) like the other errors of the section
||
|| **page**
[`integer`](../../data-types.md) | Number of the selection page, numbering starts at one, and the default value is 1. The page size is fixed at 50 records. A non-numeric, zero, or negative value causes no error: the method silently returns the first page. The method returns neither the total number of datasets nor a link to the next page, so there is only one sign that the selection has ended: the page returned fewer than 50 elements, [more about navigation](../index.md#pagination) ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

Retrieve the list of datasets where:

- the name starts with `sales`
- the description is not empty
- the source identifier equals `2` or `4`

Return only the required fields:

- identifier `id`
- name `name`
- description `description`

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{
        "select": ["id", "name", "description"],
        "filter": {
            "%=name": "sales%",
            "!description": "",
            "@sourceId": [2, 4]
        },
        "order": {
            "dateCreate": "DESC"
        },
        "page": 1,
        "auth": "**put_access_token_here**"
    }' \
    https://**put_your_bitrix24_address**/rest/biconnector.dataset.list
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

    // Shape of each DatasetItem returned in result[]
    type DatasetItem = {
      id: number
      name: string
      description: string
    }

    try {
      // biconnector.dataset.list returns a single page (max 50 records). The list helpers
      // ($b24.actions.v2.callList.make, fetchList.make) do not work here: this method uses
      // its own `page` navigation and returns neither `total` nor `next`. Walk the pages
      // yourself, increasing `page` until a response comes back with fewer than 50 records.
      const response = await $b24.actions.v2.call.make<DatasetItem[] | BiconnectorError>({
        method: 'biconnector.dataset.list',
        params: {
          select: ['id', 'name', 'description'],
          filter: {
            '%=name': 'sales%',
            '!description': '',
            '@sourceId': [2, 4],
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
          console.info('Datasets page:', result.length, result)
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
      async function listDatasets() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // biconnector.dataset.list returns a single page (max 50 records). The list helpers
          // ($b24.actions.v2.callList.make, fetchList.make) do not work here: this method uses
          // its own `page` navigation and returns neither `total` nor `next`. Walk the pages
          // yourself, increasing `page` until a response comes back with fewer than 50 records.
          const response = await $b24.actions.v2.call.make({
            method: 'biconnector.dataset.list',
            params: {
              select: ['id', 'name', 'description'],
              filter: {
                '%=name': 'sales%',
                '!description': '',
                '@sourceId': [2, 4],
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

          console.info('Datasets page:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', listDatasets)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.biconnector.dataset.list(
            select=[
                "id",
                "name",
                "description",
            ],
            filter={
                "%=name": "sales%",
                "!description": "",
                "@sourceId": [
                    2,
                    4,
                ],
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
                'biconnector.dataset.list',
                [
                    'select' => ["id", "name", "description"],
                    'filter' => [
                        '%=name'      => "sales%",
                        '!description' => "",
                        "@sourceId"   => [2, 4]
                    ],
                    'order'  => [
                        'dateCreate' => "DESC"
                    ],
                    'page'   => 1
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
        echo 'Error calling biconnector.dataset.list: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'biconnector.dataset.list',
        {
            select: ["id", "name", "description"],
            filter: {
                '%=name': "sales%",
                '!description': "",
                "@sourceId": [2, 4]
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
        'biconnector.dataset.list',
        [
            'select' => ["id", "name", "description"],
            'filter' => ['%=name' => "sales%", '!description' => "", '@sourceId' => [2, 4]],
            'order' => ['dateCreate' => "DESC"],
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
    res, err := client.Core().Call(ctx, "biconnector.dataset.list", b24.Params{
    	"select": []string{"id", "name", "description"},
    	"filter": b24.Params{
    		"%=name":       "sales%",
    		"!description": "",
    		"@sourceId":    []int{2, 4},
    	},
    	"order": b24.Params{
    		"dateCreate": "DESC",
    	},
    	"page": 1,
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("biconnector.dataset.list: %w", err)
    }

    // Methods of this section put errors inside result and answer with HTTP 200.
    var apiErr struct {
    	Error *struct {
    		Error       string `json:"error"`
    		Description string `json:"error_description"`
    	} `json:"error"`
    }
    if err := json.Unmarshal(res.Result, &apiErr); err == nil && apiErr.Error != nil {
    	return fmt.Errorf("biconnector.dataset.list: %s: %s", apiErr.Error.Error, apiErr.Error.Description)
    }

    var items []struct {
    	ID          b24.ID `json:"id"`
    	Name        string `json:"name"`
    	Description string `json:"description"`
    }
    if err := json.Unmarshal(res.Result, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.ID, it.Name)
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
            "id": 9,
            "name": "sales_data_main",
            "description": "Monthly sales report"
        },
        {
            "id": 6,
            "name": "sales_data_first_filial",
            "description": "Monthly sales report for first filial"
        },
        {
            "id": 5,
            "name": "sales_data_second_filial",
            "description": "Monthly sales report for second filial"
        }
    ],
    "time": {
        "start": 1743061675.963969,
        "finish": 1743061676.064591,
        "duration": 0.10062193870544434,
        "processing": 0.011152029037475586,
        "date_start": "2025-03-27T07:47:55+00:00",
        "date_finish": "2025-03-27T07:47:56+00:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Root element of the response. A flat array of datasets without an additional wrapper [(detailed description)](#dataset) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

#### Element of the result array {#dataset}

An element of the selection is the same dataset that [biconnector.dataset.get](./biconnector-dataset-get.md) returns, but without the `fields` array and with an additional `sourceId` key.

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Unique identifier of the dataset ||
|| **sourceId**
[`integer`](../../data-types.md) | Identifier of the source the dataset is linked to. Of all the methods of the family, only `biconnector.dataset.list` returns this field ||
|| **type**
[`string`](../../data-types.md) | Dataset type. For datasets created via the REST API, the value is always `rest` ||
|| **name**
[`string`](../../data-types.md) | Dataset name ||
|| **verboseName**
[`string`](../../data-types.md) | User-friendly dataset name. If it is not set, an empty string is returned ||
|| **description**
[`string`](../../data-types.md) | Dataset description ||
|| **externalCode**
[`string`](../../data-types.md) | External code of the dataset — the name the application knows it by ||
|| **externalName**
[`string`](../../data-types.md) | External name of the dataset ||
|| **dateCreate**
[`datetime`](../../data-types.md) | Date the dataset was created, in the `Y-m-d H:i:s` format ||
|| **dateUpdate**
[`datetime`](../../data-types.md) | Date the dataset was updated, in the `Y-m-d H:i:s` format. For a dataset that has never been updated, the value is `null` ||
|| **createdById**
[`integer`](../../data-types.md) | Identifier of the user who created the dataset ||
|| **updatedById**
[`integer`](../../data-types.md) | Identifier of the user who updated the dataset. For a dataset that has never been updated, the value is `0` ||
|| **externalId**
[`integer`](../../data-types.md) | Identifier of the BI Builder dataset created together with the object by the [biconnector.dataset.add](./biconnector-dataset-add.md) method. For objects created with the [biconnector.table.add](../table/biconnector-table-add.md) method, the value is always `0` ||
|| **csvDelimiter**
[`string`](../../data-types.md) | Column delimiter in a CSV file. For datasets of a REST source it arrives as an empty string ||
|| **csvEncoding**
[`string`](../../data-types.md) | Encoding of a CSV file. For datasets of a REST source it arrives as an empty string ||
|| **csvHasHeaders**
[`boolean`](../../data-types.md) | Indicates that the first row of a CSV file holds the column headers. For datasets of a REST source it arrives as `false` ||
|#

This is what an element looks like when the method is called without `select`:

```json
{
    "id": 27,
    "type": "rest",
    "name": "sales_orders",
    "verboseName": "Orders",
    "description": "Orders from an external service",
    "externalCode": "sales_orders",
    "externalName": "Sales orders",
    "dateCreate": "2026-09-16 12:00:46",
    "dateUpdate": null,
    "createdById": 1,
    "updatedById": 0,
    "externalId": 31,
    "csvDelimiter": "",
    "csvEncoding": "",
    "csvHasHeaders": false,
    "sourceId": 3
}
```

{% note warning "" %}

Do not list the date fields — `dateCreate` and `dateUpdate` — in `select`. With an explicit selection, the date is not formatted and an empty `{}` object arrives instead of a string. To retrieve the dates, call the method without `select` or with `select: ["*"]`

{% endnote %}

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
|| `ACCESS_DENIED` | Access denied. | One of the two permissions is missing, BI Builder external data sources are not available on this Bitrix24, or the method was called via a webhook or outside the application context ||
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
- [{#T}](./biconnector-dataset-add.md)
- [{#T}](./biconnector-dataset-update.md)
- [{#T}](./biconnector-dataset-get.md)
- [{#T}](./biconnector-dataset-delete.md)
- [{#T}](./biconnector-dataset-fields-update.md)
- [{#T}](./biconnector-dataset-fields.md)
