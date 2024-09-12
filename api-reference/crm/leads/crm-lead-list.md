# Get a List of Leads crm.lead.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- mention search by phone and email with a link to a special method

{% endnote %}

{% endif %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a user with read access to leads

The method `crm.lead.list` returns a list of leads based on a filter. It is an implementation of the list method for leads.

## Method Parameters

{% include [Note on Required Parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **select**
[`array`](../../data-types.md) | An array containing the list of fields to select (see lead fields [crm-lead-fields](./crm-lead-fields.md)).

When selecting, use masks:
- "*" - to select all fields (excluding custom and multiple fields)
- "UF_*" - to select all custom fields (excluding multiple fields)

There is no mask for selecting multiple fields. To select multiple fields, specify the required ones in the selection list ("PHONE", "EMAIL", etc.).
It is not possible to add a logical OR condition to the filter if you need to select by several different fields. ||
|| **filter**
[`object`](../../data-types.md) | An object for filtering selected leads in the format `{"field_1": "value_1", ... "field_N": "value_N"}`.

Possible values for `field` correspond to lead fields [crm-lead-fields](./crm-lead-fields.md).

An additional prefix can be assigned to the key to clarify the filter's behavior. Possible prefix values:
- `>=` — greater than or equal to
- `>` — greater than
- `<=` — less than or equal to
- `<` — less than
- `@` — IN (an array is passed as a value)
- `!@` — NOT IN (an array is passed as a value)
- `%` — LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search looks for a substring in any position of the string.
- `=%` — LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - "mol%" — searching for values starting with "mol"
  - "%mol" — searching for values ending with "mol"
  - "%mol%" — searching for values where "mol" can be in any position

- `%=` — LIKE (see description above)

- `!%` — NOT LIKE, substring search. The `%` symbol in the filter value does not need to be passed. The search goes from both sides.

- `=%` — NOT LIKE, substring search. The `%` symbol needs to be passed in the value. Examples:
  - "mol%" — searching for values not starting with "mol"
  - "%mol" — searching for values not ending with "mol"
  - "%mol%" — searching for values where the substring "mol" is not present in any position

- `!%=` — NOT LIKE (see description above)

- `=` — equal, exact match (used by default)
- `!=` - not equal
- `!` — not equal
  ||
  || **order**
Possible values for `order`:
- `asc` — in ascending order
- `desc` — in descending order ||
  || **start**
  [`integer`](../data-types.md) | This parameter is used to control pagination.

The page size of results is always static: 50 records.

To select the second page of results, you need to pass the value `50`. To select the third page of results — the value `100`, and so on.

The formula for calculating the `start` parameter value:

`start = (N-1) * 50`, where `N` — the number of the desired page ||
|#

## Examples

{% list tabs %}

- cURL

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"select":["*", "UF_*"], "start":"50", "filter":{"=OPPORTUNITY": "15000.00"},"order":{"STATUS_ID": "ASC"},"auth":"**put_access_token_here**"}' \
    https://xxx.bitrix24.com/rest/crm.lead.list.json
    ```

- JS

    ```javascript 
    BX24.callMethod(
      'crm.lead.list',
      {
        select: ['*', 'UF_*'],
        start: 50,
        filter: {
            '=OPPORTUNITY': 15000,
        },
        order: {
            STATUS_ID: 'ASC',
        }, 
      },
      (result) => {
        if(result.error())
        {
          console.error(result.error());
  
          return;
        }
        
        console.info(result.data());
      }
    );
    ```

- PHP

    ```php
    $result = CRest::call('crm.lead.list', [
        'SELECT' => ['*', 'UF_*'],
        'START' => 50,
        'FILTER' => [
            '=OPPORTUNITY' => 15000,
        ],
        'ORDER' => [
            'STATUS_ID' => 'ASC',
        ],     
    ]);
    ```

- HTTPS

    ```http
    https://xxx.bitrix24.com/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.lead.list.json?select[]=*&select=UF_*&start=50&filter[=OPPORTUNITY]=15000.00&order[STATUS_ID]=ASC
    ```

{% endlist %}

## Some Practical Examples
{% list tabs %}

- Searching for unconverted leads with an amount greater than zero

  ```js
  BX24.callMethod(
      "crm.lead.list",
      {
          order: { "STATUS_ID": "ASC" },
          filter: { ">OPPORTUNITY": 0, "!STATUS_ID": "CONVERTED" },
          select: [ "ID", "TITLE", "STATUS_ID", "OPPORTUNITY", "CURRENCY_ID" ],
      },
      (result) => {
          if(result.error())
          {
              console.error(result.error());
          }
          else
          {
              console.dir(result.data());
              if (result.more())
              {
                  result.next();
              }
          }
      }
  );
  ```

- Searching for a lead by phone

  ```js
  BX24.callMethod(
      "crm.lead.list",
      {
          filter: { "PHONE": "555888" },
          select: [ "ID", "TITLE" ]
      },
      (result) => {
        if(result.error())
        {
          console.error(result.error());
        }
        else
        {
          console.dir(result.data());
          if (result.more())
          {
            result.next();
          }
        }
      }
  );
  ```

- Selecting leads for the month

  ```php
  $result = CRest::call(
      'crm.lead.list',
      [
          'filter' => [
              '>DATE_CREATE' => '2023-10-01T00:00:00',
              '<DATE_CREATE' => '2023-10-31T23:59:59',
          ],
          'select' => [
              'ID',
              'DATE_CREATE',
          ],
      ]
  );
  ```
{% endlist %}

{% include [Note on Examples](../../../_includes/examples.md) %}

## Successful Response

> 200 OK

```json
{
  "result": [
    {
      "ID": "5",
      "TITLE": "Lead 1",
      "HONORIFIC": null,
      "NAME": "Erasmus",
      "SECOND_NAME": null,
      "LAST_NAME": "Golden of Ireland",
      "COMPANY_TITLE": null,
      "COMPANY_ID": "0",
      "CONTACT_ID": "2069",
      "IS_RETURN_CUSTOMER": "N",
      "BIRTHDATE": "",
      "SOURCE_ID": "CALL",
      "SOURCE_DESCRIPTION": null,
      "STATUS_ID": "CONVERTED",
      "STATUS_DESCRIPTION": null,
      "POST": null,
      "COMMENTS": null,
      "CURRENCY_ID": "USD",
      "OPPORTUNITY": "15000.00",
      "IS_MANUAL_OPPORTUNITY": "Y",
      "HAS_PHONE": "Y",
      "HAS_EMAIL": "Y",
      "HAS_IMOL": "N",
      "ASSIGNED_BY_ID": "1",
      "CREATED_BY_ID": "1",
      "MODIFY_BY_ID": "1",
      "DATE_CREATE": "2021-05-31T15:10:16+03:00",
      "DATE_MODIFY": "2021-11-26T18:56:13+03:00",
      "DATE_CLOSED": "2021-07-16T16:43:44+03:00",
      "STATUS_SEMANTIC_ID": "S",
      "OPENED": "Y",
      "ORIGINATOR_ID": null,
      "ORIGIN_ID": null,
      "MOVED_BY_ID": "1",
      "MOVED_TIME": "2021-07-16T16:43:44+03:00",
      "ADDRESS": "7677 Hollow Ridge Alley",
      "ADDRESS_2": null,
      "ADDRESS_CITY": null,
      "ADDRESS_POSTAL_CODE": null,
      "ADDRESS_REGION": null,
      "ADDRESS_PROVINCE": null,
      "ADDRESS_COUNTRY": "Indonesia",
      "ADDRESS_COUNTRY_CODE": null,
      "ADDRESS_LOC_ADDR_ID": "1",
      "UTM_SOURCE": null,
      "UTM_MEDIUM": null,
      "UTM_CAMPAIGN": null,
      "UTM_CONTENT": null,
      "UTM_TERM": null,
      "LAST_ACTIVITY_BY": "1",
      "LAST_ACTIVITY_TIME": "2021-05-31T15:10:16+03:00",
      "UF_CRM_1704817278": null,
      "UF_CRM_1706782596092": null,
      "UF_CRM_1708952993785": false
    },
    {
      "ID": "6",
      "TITLE": "Lead 2",
      "HONORIFIC": null,
      "NAME": "Ignacius",
      "SECOND_NAME": null,
      "LAST_NAME": "Slayny",
      "COMPANY_TITLE": null,
      "COMPANY_ID": "0",
      "CONTACT_ID": "2070",
      "IS_RETURN_CUSTOMER": "N",
      "BIRTHDATE": "",
      "SOURCE_ID": "CALL",
      "SOURCE_DESCRIPTION": null,
      "STATUS_ID": "CONVERTED",
      "STATUS_DESCRIPTION": null,
      "POST": null,
      "COMMENTS": null,
      "CURRENCY_ID": "USD",
      "OPPORTUNITY": "15000.00",
      "IS_MANUAL_OPPORTUNITY": "Y",
      "HAS_PHONE": "Y",
      "HAS_EMAIL": "Y",
      "HAS_IMOL": "N",
      "ASSIGNED_BY_ID": "1",
      "CREATED_BY_ID": "1",
      "MODIFY_BY_ID": "1",
      "DATE_CREATE": "2021-05-31T15:10:16+03:00",
      "DATE_MODIFY": "2021-11-26T18:56:13+03:00",
      "DATE_CLOSED": "2021-07-16T16:43:47+03:00",
      "STATUS_SEMANTIC_ID": "S",
      "OPENED": "Y",
      "ORIGINATOR_ID": null,
      "ORIGIN_ID": null,
      "MOVED_BY_ID": "1",
      "MOVED_TIME": "2021-07-16T16:43:47+03:00",
      "ADDRESS": "35 Mosinee Street",
      "ADDRESS_2": null,
      "ADDRESS_CITY": null,
      "ADDRESS_POSTAL_CODE": null,
      "ADDRESS_REGION": null,
      "ADDRESS_PROVINCE": null,
      "ADDRESS_COUNTRY": "Japan",
      "ADDRESS_COUNTRY_CODE": null,
      "ADDRESS_LOC_ADDR_ID": "2",
      "UTM_SOURCE": null,
      "UTM_MEDIUM": null,
      "UTM_CAMPAIGN": null,
      "UTM_CONTENT": null,
      "UTM_TERM": null,
      "LAST_ACTIVITY_BY": "1",
      "LAST_ACTIVITY_TIME": "2021-05-31T15:10:16+03:00",
      "UF_CRM_1704817278": null,
      "UF_CRM_1706782596092": null,
      "UF_CRM_1708952993785": true
    },
    
      48 more leads with a similar structure
    
  ],
  "next": 50,
  "total": 654,
  "time": {
    "start": 1718292234.554781,
    "finish": 1718292234.657739,
    "duration": 0.10295796394348145,
    "processing": 0.05574321746826172,
    "date_start": "2024-06-13T18:23:54+03:00",
    "date_finish": "2024-06-13T18:23:54+03:00",
    "operating": 0
  }
}
```

### Returned Data

#|
|| **Value** / **Type** | **Description** ||
|| **result**
[`array`](../../data-types.md) | The result of the request. An array of leads. For information on the lead structure, see the method [`crm.lead.get`](./crm-lead-get.md) ||
|| **next**
[`integer`](../../data-types.md) | The value to send to get the next page of data from the list method. Shown if such elements exist. ||
|| **total**
[`integer`](../../data-types.md) | The total number of leads that match the request ||
|| **time**
[`array`](../../data-types.md) | Information about the execution time of the request ||
|| **start**
[`double`](../../data-types.md) | Timestamp of the moment the request was initialized ||
|| **finish**
[`double`](../../data-types.md) | Timestamp of the moment the request execution was completed ||
|| **duration**
[`double`](../../data-types.md) | How long in milliseconds the request took (finish - start) ||
|| **date_start**
[`string`](../../data-types.md) | String representation of the date and time of the moment the request was initialized ||
|| **date_finish**
[`double`](../../data-types.md) | String representation of the date and time of the moment the request execution was completed ||
|| **operating_reset_at**
[`timestamp`](../../data-types.md) | Timestamp of the moment when the limit on REST API resources will be reset. Read more in the article [operation limits](../../../limits.md) ||
|| **operating**
[`double`](../../data-types.md) | In how many milliseconds will the limit on REST API resources be reset? Read more in the article [operation limits](../../../limits.md) ||
|#

## Error Response

> HTTP status: 40x, 50x Error

```json
{
    "error": "",
    "error_description": "Access denied."
}
```

### Possible Errors

#|  
|| **Error Text** | **Description** ||
|| Access denied. | The user does not have permission to read leads. ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}