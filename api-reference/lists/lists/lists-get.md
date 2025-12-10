# Get Data from Universal List or Array of Lists lists.get

> Scope: [`lists`](../../scopes/permissions.md)
>
> Who can execute the method: user with "Read" access permission for lists

The `lists.get` method returns a universal list or an array of lists.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **IBLOCK_TYPE_ID***
[`string`](../../data-types.md) | Identifier of the information block type. Possible values: 
- `lists` — list information block type 
- `bitrix_processes` — processes information block type 
- `lists_socnet` — group lists information block type

The identifier can be obtained using the [lists.get-iblock-type-id](./lists-get-iblock-type-id.md) method ||
|| **IBLOCK_ID**
[`integer`](../../data-types.md) | Identifier of the information block ||
|| **IBLOCK_CODE** 
[`string`](../../data-types.md) | Symbolic code of the information block

{% note info "" %}

When requesting without `IBLOCK_ID` or `IBLOCK_CODE`, all lists of the specified type available to the user are returned.

{% endnote %} ||
|| **SOCNET_GROUP_ID**
[`integer`](../../data-types.md) | Group identifier. Required for group lists; otherwise, an access error will occur.

The identifier can be obtained using the [socialnetwork.api.workgroup.list](../../sonet-group/socialnetwork-api-workgroup-list.md), [sonet_group.get](../../sonet-group/sonet-group-get.md), and [sonet_group.user.groups](../../sonet-group/sonet-group-user-groups.md) methods
||
|| **IBLOCK_ORDER**
[`object`](../../data-types.md) | Object for sorting list fields in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

The sorting direction can take the following values:
- `asc` — ascending
- `desc` — descending 
  
Allowed fields:
- `ID` - list identifier
- `IBLOCK_TYPE` - information block type
- `NAME` - list name
- `CODE` - symbolic code of the list
- `SORT` - sorting
- `TIMESTAMP_X` - last modification time

Default value — `decs`
||
|| **start**
[`integer`](../../data-types.md) | Parameter used for managing pagination.

The page size of results is always static — 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N - 1) * 50`, where `N` — the number of the desired page ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists_socnet","SOCNET_GROUP_ID":33,"IBLOCK_ORDER":{"SORT":"asc","NAME":"asc"},"start":0}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/lists.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"IBLOCK_TYPE_ID":"lists_socnet","SOCNET_GROUP_ID":33,"IBLOCK_ORDER":{"SORT":"asc","NAME":"asc"},"start":0,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/lists.get
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod(
          'lists.get',
          {
              IBLOCK_TYPE_ID: 'lists_socnet',
              SOCNET_GROUP_ID: 33,
              IBLOCK_ORDER: {
                SORT: 'asc',
                NAME: 'asc'
              },
              start: 0
          }
      );

      const result = response.getData().result;
      console.log('Fetched lists:', result);
      processResult(result);
  } catch (error) {
      console.error('Error:', error);
  }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'lists.get',
                [
                    'IBLOCK_TYPE_ID' => 'lists_socnet',
                    'SOCNET_GROUP_ID' => 33,
                    'IBLOCK_ORDER' => [
                        'SORT' => 'asc',
                        'NAME' => 'asc'
                    ],
                    'start' => 0
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error fetching lists: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
      'lists.get',
      {
         IBLOCK_TYPE_ID: 'lists_socnet',
         SOCNET_GROUP_ID: 33,
         IBLOCK_ORDER: {
           SORT: 'asc',
           NAME: 'asc'
         },
         start: 0
      },
         function(result) {
             if (result.error()) {
                console.error(result.error());
             } else {
                console.log(result.data());
             }
         }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'lists.get',
        [
            'IBLOCK_TYPE_ID' => 'lists_socnet',
            'SOCNET_GROUP_ID' => 33,
            'IBLOCK_ORDER' => [
                'SORT' => 'asc',
                'NAME' => 'asc'
            ],
            'start' => 0
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
        "ID": "89",
        "TIMESTAMP_X": "05/17/2023 04:09:23 pm",
        "IBLOCK_TYPE_ID": "lists_socnet",
        "LID": "s1",
        "CODE": null,
        "API_CODE": null,
        "NAME": "Group List",
        "ACTIVE": "Y",
        "SORT": "500",
        "LIST_PAGE_URL": null,
        "DETAIL_PAGE_URL": null,
        "SECTION_PAGE_URL": null,
        "CANONICAL_PAGE_URL": null,
        "PICTURE": null,
        "DESCRIPTION": "",
        "DESCRIPTION_TYPE": "text",
        "RSS_TTL": "24",
        "RSS_ACTIVE": "Y",
        "RSS_FILE_ACTIVE": "N",
        "RSS_FILE_LIMIT": null,
        "RSS_FILE_DAYS": null,
        "XML_ID": null,
        "TMP_ID": null,
        "INDEX_ELEMENT": "Y",
        "INDEX_SECTION": "N",
        "WORKFLOW": "N",
        "BIZPROC": "Y",
        "SECTION_CHOOSER": "L",
        "LIST_MODE": null,
        "RIGHTS_MODE": "E",
        "SECTION_PROPERTY": null,
        "PROPERTY_INDEX": null,
        "VERSION": "1",
        "LAST_CONV_ELEMENT": "0",
        "SOCNET_GROUP_ID": "33",
        "EDIT_FILE_BEFORE": null,
        "EDIT_FILE_AFTER": null,
        "SECTIONS_NAME": "Sections",
        "SECTION_NAME": "Section",
        "ELEMENTS_NAME": "Elements",
        "ELEMENT_NAME": "Element",
        "REST_ON": "N",
        "FULLTEXT_INDEX": "N",
        "EXTERNAL_ID": null,
        "LANG_DIR": "/",
        "SERVER_NAME": null
        }
    ],
    "total": 1,
    "time": {
        "start": 1764694297,
        "finish": 1764694298.018582,
        "duration": 1.0185821056365967,
        "processing": 1,
        "date_start": "2025-12-02T15:51:37+01:00",
        "date_finish": "2025-12-02T15:51:38+01:00",
        "operating_reset_at": 1764694897,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../../data-types.md) | Data of the list or array of lists.

An empty array means that no lists were found ||
|| **total**
[`integer`](../../data-types.md) | Total number of lists ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error":"ERROR_REQUIRED_PARAMETERS_MISSING",
    "error_description":"Required parameter `X` is missing"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_REQUIRED_PARAMETERS_MISSING` | Required parameter `X` is missing | Required parameter not provided ||
|| `ACCESS_DENIED` | Access denied | Insufficient rights to read the list ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./lists-add.md)
- [{#T}](./lists-update.md)
- [{#T}](./lists-delete.md)
- [{#T}](./lists-get-iblock-type-id.md)