# Get a List of Workflow Tasks bizproc.task.list

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- need to check field types
- need an example of the Fields array in the PARAMETERS parameter, or describe what can be there
- the meaning of the footnote about Fields under the fields table is completely unclear
- no examples of curl, php
- need response structure
- need error descriptions
- fill in the tutorials, see also, and related methods blocks with real data, or remove them

{% endnote %}

{% endif %}

> Scope: [`bizproc`](../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns a list of workflow tasks. The method is available not only to administrators. Regular users can request tasks for themselves or their subordinates. To request their own tasks, a non-administrator should not specify a filter by `USER_ID`.

#|
|| **Parameter** | **Description** ||
|| **SELECT** | An array of fields of the records that will be returned by the method. You can specify only the fields that are necessary. See the list of available fields below. ||
|| **FILTER** | An array of the form `{"filter_field": "filter_value" [, ...]}`. The list of filterable fields is the same as for the `SELECT` parameter. If `USER_ID` is present in the filter, user subordination is checked. A manager can request a list of tasks for their subordinates. An administrator can request all tasks without restrictions. ||
|| **ORDER** | An array for sorting the result. An array of the form `{"sorting_field": 'sorting_direction' [, ...]}`. The list of fields for sorting is the same as for the `SELECT` parameter. `{'ID': 'desc'}` ||
|| **START** | The ordinal number of the list item from which to return the next items when calling the current method. Details in the article [{#T}](../../how-to-call-rest-api/list-methods-pecularities.md) ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

## Fields Available in SELECT, FILTER, and ORDER

#|
|| **Field** | **Description** ||
|| **ID**
[`integer`](../../data-types.md) | identifier of the task ||
|| **WORKFLOW_ID**
[`integer`](../../data-types.md) | identifier of the workflow ||
|| **DOCUMENT_NAME**
[`string`](../../data-types.md) | name of the document ||
|| **DESCRIPTION**
[`string`](../../data-types.md) | description of the task ||
|| **NAME**
[`string`](../../data-types.md) | name of the task ||
|| **MODIFIED**
[`datetime`](../../data-types.md) | modification date ||
|| **WORKFLOW_STARTED**
[`datetime`](../../data-types.md) | workflow start date ||
|| **WORKFLOW_STARTED_BY**
[`user`](../../data-types.md) | who started the workflow ||
|| **OVERDUE_DATE**
[`datetime`](../../data-types.md) | deadline ||
|| **WORKFLOW_TEMPLATE_ID**
[`integer`](../../data-types.md) | identifier of the workflow template ||
|| **WORKFLOW_TEMPLATE_NAME**
[`string`](../../data-types.md) | name of the workflow template ||
|| **WORKFLOW_STATE**
[`string`](../../data-types.md) | status of the workflow ||
|| **STATUS**
[`integer`](../../data-types.md) | Task status. Possible values:

- `0` - in progress;
- `1` - approved (response `Yes`);
- `2` - rejected (response `No`);
- `3` - completed (response `Ok`);
- `4` - timeout (task deadline expired). ||

|| **USER_ID**
[`user`](../../data-types.md) | identifier of the user ||
|| **USER_STATUS**
[`integer`](../../data-types.md) | User response. Possible values:

- `0` - awaiting response;
- `1` - yes (approved);
- `2` - no (rejected);
- `3` - ok (completed). ||

|| **MODULE_ID**
[`string`](../../data-types.md) | identifier of the module (by document) ||
|| **ENTITY**
[`string`](../../data-types.md) | identifier of the entity (by document) ||
|| **DOCUMENT_ID**
[`integer`](../../data-types.md) | identifier of the document ||
|| **ACTIVITY**
[`string`](../../data-types.md) | Identifier of the task type. Possible values:

- `ApproveActivity` - Document approval
- `ReviewActivity` - Document review
- `RequestInformationActivity` - Request for additional information
- `RequestInformationOptionalActivity` - Request for additional information (with rejection) ||
|| **ACTIVITY_NAME**
[`string`](../../data-types.md) | identifier of the action in the template ||
|| **PARAMETERS**
[`array`](../../data-types.md) | task parameters, array. Array keys:

- CommentLabelMessage - Name of the "Comment" field;
- CommentRequired - Comment requirement. Possible values:
  - 'N' (no),
  - 'Y' (yes),
  - 'YA' (yes upon approval),
  - 'YR' (yes upon rejection);
- `ShowComment` - Show comment. Possible values:
  - 'N' (no),
  - 'Y' (yes),
- `TaskButtonMessage` - text of the "Acknowledged" button;
- `TaskButton1Message` - text of the "Approve" button;
- `TaskButton2Message` - text of the "Reject" button;
- `Fields` - array with field descriptions ||
|#

{% include [Footnote about parameters](../../../_includes/required.md) %}

{% note info "Changes" %}

Starting from version **20.0.800** of the Business Processes module, it became possible to complete **Request for Additional Information** tasks through the REST method `bizproc.task.complete`. To understand which fields need to be filled, a new property `Fields` has been added to the PARAMETERS in the `bizproc.task.list` method.

{% endnote %}

## Examples

{% list tabs %}

- cURL

    ```http
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{ "title": "New Deal" }' \
    a proper call is needed here
    ```

- JS

    ```js
    BX24.callMethod(
        'bizproc.task.list',
        {
            select: [
                'ID',
                'WORKFLOW_ID',
                'DOCUMENT_NAME',
                'DESCRIPTION',
                'NAME',
                'MODIFIED',
                'WORKFLOW_STARTED',
                'WORKFLOW_STARTED_BY',
                'OVERDUE_DATE',
                'WORKFLOW_TEMPLATE_ID',
                'WORKFLOW_TEMPLATE_NAME',
                'WORKFLOW_STATE',
                'STATUS',
                'USER_ID',
                'USER_STATUS',
                'MODULE_ID',
                'ENTITY',
                'DOCUMENT_ID'
            ],
            order: {ID: 'DESC'},
            filter: {'USER_ID': 1}
        },
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                console.log(result.data());
        }
    );

    ```

- PHP

    ```php
    // example needed
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Successful Response

> 200 OK

```json
{
    "result": 3465,
    "time": {
        "start": 1705764932.998683,
        "finish": 1705764937.173995,
        "duration": 4.1753120422363281,
        "processing": 3.3076529502868652,
        "date_start": "2024-01-20T18:35:32+03:00",
        "date_finish": "2024-01-20T18:35:37+03:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Value** / **Type** | **Description** ||
|| **result**
`integer`| Identifier of a new deal ||
|| **time**
[`array`](../../data-types.md) | Information about the request execution time ||
|| **start**
[`double`](../../data-types.md) | Timestamp of the moment the request was initialized ||
|| **finish**
[`double`](../../data-types.md) | Timestamp of the moment the request execution was completed ||
|| **duration**
[`double`](../../data-types.md) | How long the request took in milliseconds (finish - start) ||
|| **date_start**
[`string`](../../data-types.md) | String representation of the date and time of the moment the request was initialized ||
|| **date_finish**
[`double`](../../data-types.md) | String representation of the date and time of the moment the request execution was completed ||
|| **operating_reset_at**
[`timestamp`](../../data-types.md) | Timestamp of the moment when the limit on REST API resources will be reset. Read more in the article [operation limits](../../../limits.md) ||
|| **operating**
[`double`](../../data-types.md) | After how many milliseconds will the limit on REST API resources be reset? Read more in the article [operation limits](../../../limits.md) ||
|#

## Error Response

> 40x, 50x Error

```json
{
    "error": "TITLE_EMPTY",
    "error_description": "The deal title is required"
}
```

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| TITLE_EMPTY | The required field values are not set || 
|| WRONG_REQUEST | The parameters of the request were unable to interpret || 
|#

## Tutorials

- [How to add a deal specifying the details of an existing company or contact](/tutorials/adding-deal-with-existing-company-or-contact.html)
- [How to add a deal with goods, applying discounts and taxes](/tutorials/adding-deal-with-goods.html)
- [Obtaining a funnel of a given funnel with the semantics of each stage of the deal](/tutorials/getting-funnel-stages-of-deal.html)

## See also

- [Using OAuth 2.0 tokens for executing REST API](/oauth.html)
- [Using Incoming webhooks for executing REST API](/local_webhooks.html)

{% note tip "Related methods and topics" %}

empty for now

{% endnote %}