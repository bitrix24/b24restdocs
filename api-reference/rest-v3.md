# Overview of REST 3.0

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

REST 3.0 is a new version of the API in Bitrix24 that makes working with integrations more predictable and structured. Key improvements include:

- a unified response format for all methods,
- retrieving related data in a single request,
- repeated calls without duplicates using the `Idempotency-Key` header,
- built-in OpenAPI documentation.

Both versions work simultaneously and do not replace each other. The old methods keep working at the previous address, while only the methods already migrated to the new version are available at the REST 3.0 address — see the list in the [Where REST 3.0 is available](#where) section.

> Quick navigation: [comparison table of versions](#table)

## How to call the new version {#call}

Call address: `https://{installation_address}/rest/api/{user_id}/{webhook_code}/{method}`.

- `{installation_address}` — the address of Bitrix24.

- `/rest/api/` — indicates the version of REST. After `rest/`, specify `/api/` — this is the main difference in calling the new version from the old one. If `/api/` is not specified, Bitrix24 will execute the method of the old version of REST if it exists. For methods available only in REST 3.0, the error `ERROR_METHOD_NOT_FOUND` with status 404 is returned.

- `/{user_id}/{webhook_code}/` — webhook authorization data. For applications, pass the authorization token in the `auth` field in the request body.

- `{method}` — the method being called.

**Example of calling a new method with webhook authorization**

```bash
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"fields":{"taskId":51,"text":"Message from external system"}}' \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.chat.message.send
```

**Example of calling a new method with application authorization**

```bash
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"fields":{"taskId":51,"text":"Message from external system"},"auth":"**put_access_token_here**"}' \
https://**put_your_bitrix24_address**/rest/api/tasks.task.chat.message.send
```

### Technical requirements

- Pass the request body in JSON format with the header `Content-Type: application/json`. A body in a different format, such as `form-data`, returns the error `BITRIX_REST_V3_EXCEPTION_INVALIDJSONEXCEPTION`.

- All requests with parameters should be sent only in POST format — as in the examples above. REST 3.0 does not read parameters from the query string. The method [tasks.task.get](./tasks/tasks-task-get-rest-v3.md) called as `GET .../tasks.task.get?id=51` returns the validation error `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION`: the required `id` field is not specified.

- Requests without parameters can be sent as GET or POST, for example, the method without parameters `rest.documentation.openapi`.

 ```bash
 curl -X GET \
 https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/rest.documentation.openapi
 ```

### Calling through an SDK {#sdk}

#|
|| **SDK** | **REST 3.0 support** ||
|| [b24gosdk](../sdk/b24gosdk/index.md), Go | Yes. Pass a base address that contains the `/rest/api/` segment, nothing else needs to be configured ||
|| [b24pysdk](../sdk/b24pysdk/index.md), Python | Yes. Pass the `prefer_version=3` parameter when creating the client: `Client(..., prefer_version=3)`.

Only the methods that the SDK recognizes as available in the new version are sent to REST 3.0. The client sends the remaining calls to the previous version of REST, and no error is raised ||
|| [b24jssdk](../sdk/b24jssdk/index.md), JS and TS | Yes. Call the methods through the `$b24.actions.v3` namespace instead of `$b24.actions.v2` ||
|| b24phpsdk, BX24.js, PHP CRest | No. Use direct HTTP requests, for example, via `curl` or `fetch` ||
|#

{% note warning "" %}

List traversal and `batch` in b24gosdk and b24pysdk work only with the previous version of REST. On REST 3.0, call these methods through a regular method call.

{% endnote %}

## Where REST 3.0 is available {#where}

Not all Bitrix24 methods are migrated to REST 3.0. The current list of the new version methods is returned by the [OpenAPI documentation](#openapi).

The REST 3.0 methods are described in the following sections:

#|
|| **Section** | **What the methods do** ||
|| [Tasks](./tasks/index.md) | Create and modify tasks, work with the task chat, results, files, and access permissions ||
|| [Knowledge Base 2.0](./note/index.md) | Maintain knowledge bases, documents, and attached files ||
|| [E-mail](./mail/index.md) | Work with mailboxes, messages, and recipients ||
|| [Company Structure](./departments/index.md) | Work with departments, teams, members, and roles — the `humanresources.*` group of methods. The `department.*` methods of the same section belong to the old version ||
|| [Event Log](./event-log/index.md) | Retrieve the entries of the Bitrix24 event log ||
|| [Time Tracking Records](./timeman/record/index.md) | Retrieve and modify time tracking records ||
|| [Call Follow-up](./telephony/follow-up/index.md) | Retrieve the materials that the AI assistant generates after a video call ||
|#

A method of the same section can exist in both versions with different parameters and a different response. For example, `tasks.task.get` exists both in the [old version](./tasks/tasks-task-get.md) and in [REST 3.0](./tasks/tasks-task-get-rest-v3.md). Rely on the REST 3.0 note at the beginning of the method page.

## OpenAPI documentation {#openapi}

In REST 3.0, automatically generated documentation in OpenAPI standard is available. The documentation describes the methods, parameters, and response schemas of your Bitrix24, so it shows which methods of the new version are available right now.

To obtain the documentation, call the method `rest.documentation.openapi` without parameters. It has a short synonym `documentation` — both names return the same result. The method has no parameters, so both GET and POST work.

```bash
curl -X POST \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/rest.documentation.openapi
```

The result will be JSON in OpenAPI format. The list of available methods is in the `paths` object, and the object schemas are in `components.schemas`.

```json
{
    "openapi": "3.0.0",
    "info": {
        "title": "Bitrix24 REST V3 API",
        "version": "1.0.0"
    },
    "tags": [
        {
            "name": "tasks",
            "description": "tasks module methods"
        }
    ],
    "paths": {
        "/tasks.task.get": {}
    }
}
```

The method can be called in Swagger, Postman, or another program that works with OpenAPI.

## Permissions and scopes {#permissions}

Permissions in REST 3.0 are checked the same way as in the previous version and depend on two conditions.

- The scope of the webhook or application. If the required scope is missing, the method returns the error `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION` with status 403. The scope is specified at the beginning of each method page, and the full list is in the article [Available Scopes in Bitrix24](./scopes/permissions.md).

- The permissions of the user on whose behalf the call is made. If the user has no access to the object, the method returns the error `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` with status 403.

The `rest.scope.list` method returns the method-to-scope mapping for all modules. It exists only in REST 3.0 and helps to determine the scope when the method page does not specify it.

```bash
curl -X POST \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/rest.scope.list
```

## Repeated calls without duplicates {#idempotency}

If a request does not reach Bitrix24 or the response is lost, an integration usually repeats the call. A method that creates or modifies data runs a second time, and a duplicate appears in Bitrix24.

To prevent this, pass the `Idempotency-Key` header in the request — an arbitrary string that identifies a specific call. For example, a UUID.

```bash
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-H "Idempotency-Key: 9f1c1a7e-5f1b-4a1e-9a2c-2f5f2b6c7d80" \
-d '{"fields":{"title":"Prepare a report","responsibleId":1,"creatorId":1}}' \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.add
```

The first call runs as usual, and its response is stored for 24 hours. When the call is repeated with the same key and the same request body, the method does not run: Bitrix24 returns the stored response and adds the `Idempotent-Replayed: true` header to it.

Bitrix24 returns the `Idempotency-Key` header with the value you passed in both cases, while `Idempotent-Replayed` is returned only on a repeated call. This is how an integration tells a repeat from the first call.

### What to consider

- The header works only in REST 3.0, that is, when calling the `/rest/api/` address. In the previous version of REST it is ignored.

- The header is taken into account in methods that create, modify, and delete data. In methods that retrieve data it is not needed: a repeated call does not change the state of Bitrix24.

- The key is unique within a single application or webhook, a single user, and a single method. The same key in two different methods means two independent calls.

- The response is stored only when the call succeeds. If the method returns an error, a repeated call with the same key runs the method again.

- The key value is 1 to 255 printable ASCII characters.

- Send a repeated call after you receive a response or the timeout expires. Two requests with the same key sent at the same time may both run.

### Errors

#|
|| **Code** | **HTTP status** | **Reason** | **What to do** ||
|| `BITRIX_REST_V3_EXCEPTION_IDEMPOTENCYKEYREUSEDEXCEPTION` | 422 | The key was already used with a different request body | Take a new key. Reuse the previous key only when you repeat the same call ||
|| `BITRIX_REST_V3_EXCEPTION_INVALIDIDEMPOTENCYKEYEXCEPTION` | 400 | The key value does not fit the length limit or contains invalid characters | Pass a string of 1 to 255 printable ASCII characters ||
|#

## Unified response format {#response}

In the previous version of REST, different modules returned results in various formats. For example, the identifier of a created item could be a nested object `"result": { "id": 1823 }` or returned directly in the result as `"result": 1823`. Different methods required writing custom logic for response handling.

The new version of REST returns responses in a unified format that applies to all methods, regardless of the module.

### Structure of a successful response

A successful response comes with status 200 and contains two top-level objects: `result` with the method data and [`time`](./data-types.md#time) with information about the request execution time.

If the method returns data, such as a list of found items or the identifier of a created item, they will be returned as a nested object within `result`.

```json
{
    "result": {
        "item": {
            "id": 42,
            "title": "Prepare report"
        }
    },
    "time": {
        "start": 1787219542,
        "finish": 1787219542.888997,
        "duration": 0.8889970779418945,
        "processing": 0,
        "date_start": "2026-08-20T11:52:22+02:00",
        "date_finish": "2026-08-20T11:52:22+02:00",
        "operating_reset_at": 1787220142,
        "operating": 0.1199338436126709
    }
}
```

If the method returns the result of an operation as true or false, for example, when deleting an item, it will return as a nested object within `result`.

```json
{
    "result": {
        "result": true
    }
}
```

Lists are returned as an array within the `items` key.

```json
{
    "result": {
        "items": [
            {
                "id": 42
            },
            {
                "id": 43
            }
        ]
    }
}
```

### Structure of an unsuccessful response

Any method can return an error, for example, due to access denial or incorrect request parameters. An error response comes with a 4xx status and does not contain the `result` object. In the new format, the error contains:

- `code` — the error code, always returned,

- `message` — the error message in the language of your Bitrix24, always returned,

- `validation` — details about the error, returned if the error is related to request parameters.

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION",
        "message": "Error validating request object",
        "validation": [
            {
                "message": "Required field `id` is not specified",
                "field": "id"
            }
        ]
    }
}
```

Error codes start with the `BITRIX_REST_V3_EXCEPTION_` prefix. Typical errors and statuses common to all methods:

#|
|| **Code** | **HTTP status** | **Reason** ||
|| `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` | 400 | A required field is not filled in or the value does not fit the type ||
|| `BITRIX_REST_V3_EXCEPTION_INVALIDJSONEXCEPTION` | 400 | The request body is not valid JSON ||
|| `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` | 400 | A field that the object does not have is specified in `filter` or in a related field of `select` ||
|| `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION` | 400 | A non-existent operator is specified in a filter condition ||
|| `BITRIX_REST_V3_EXCEPTION_ENTITYNOTFOUNDEXCEPTION` | 400 | An object with the passed identifier is not found ||
|| `BITRIX_REST_V3_EXCEPTION_ACCESSDENIEDEXCEPTION` | 403 | The user has no permissions for the object ||
|| `BITRIX_REST_V3_EXCEPTION_INSUFFICIENTSCOPEEXCEPTION` | 403 | The webhook or application does not have the required scope ||
|#

## Connections between objects {#connection}

REST 3.0 allows retrieving data from related objects in a single response. For example, a task has a `responsible` field — this field contains the identifier of another object, a user. In the old version of REST, you first had to get the identifier from the `responsible` field using the old method [tasks.task.get](./tasks/tasks-task-get.md), and then separately call the method [user.get](./user/user-get.md) to get data by the user identifier.

In the new version of REST, you can specify the fields of related objects directly in the request [tasks.task.get](./tasks/tasks-task-get-rest-v3.md) using `select`: `"select": ["responsible.name", "responsible.email"]`.

```bash
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"id":3835,"select":["responsible.name","responsible.email"]}' \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.get
```

Related fields are specified in the request using a dot: `responsible.name`, `group.title`, `company.phone`. In the response, related fields are transformed into an object with a structure corresponding to the selected fields.

```json
{
    "result": {
        "item": {
            "id": 3835,
            "title": "task",
            "responsible": {
                "name": "Name",
                "email": "mail@bitrix.com"
            }
        }
    }
}
```

If an unknown field of a related object is specified in `select`, the request returns the error `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` with status 400. The error message states which field is missing and which object it is missing from.

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field `stageId` for entity `UserDto`"
    }
}
```

{% note warning "" %}

An unknown field of the object itself, as opposed to a related object, is not treated as an error: the request succeeds with status 200, and the field is omitted from the response. For example, `"select": ["id", "stageId"]` for the `tasks.task.get` method returns only `id`. Check the contents of the response if an expected field is missing from it.

{% endnote %}

To find out which connections and fields are supported, use the [OpenAPI documentation](#openapi), the `*.field.list` and `*.field.get` group of methods of the corresponding section, or the article [Task Fields in REST 3.0](./tasks/fields-rest-v3.md).

## Filtering {#filter}

In REST 3.0, data filtering is based on logical expressions that can be combined. This scheme works in all methods that support the `filter` parameter.

### Filtering principles

- Conditions within a single level are combined using the logic of **AND** — all must be satisfied simultaneously.

- Groups of conditions can be combined using the logic of **OR** with a special object with the key `"logic": "or"`.

**Example of a simple filter.** Find all records that satisfy two conditions simultaneously:

1. The `status` field equals `NEW`
2. The `id` field equals 3, 4, or 5

```json
{
    "filter": [
        ["status", "=", "NEW"],
        ["id", "in", [3,4,5]]
    ]
}
```

All elements in the `filter` array are combined using AND logic: `status` equals `NEW` **AND** `id` equals 3, 4, or 5.

**Example of a complex filter with logic.** Find all records that satisfy two conditions simultaneously:

1. The `status` field equals `NEW`
2. The `id` field equals 1 or 2, **OR** the `id` field equals 3, 4, or 5

```json
{
    "filter": [
        ["status", "=", "NEW"],
        {
            "logic": "or",
            "conditions": [
                ["id", [1,2]],
                ["id", "in", [3,4,5]]
            ]
        }
    ]
}
```

How to read this filter:

1. `["status", "=", "NEW"]` — a simple condition: the `status` field **equals** the value `NEW`
2. `{"logic": "or", "conditions": [...]}` — a group of two conditions combined using OR logic. An element matches if at least one of them is satisfied
3. `["id", [1,2]]` inside the group — a shorthand for `["id", "in", [1,2]]`: the `id` field equals 1 or 2
4. `["id", "in", [3,4,5]]` inside the group — the `id` field equals 3, 4, or 5
5. The elements of the `filter` array are combined using AND logic: `status` equals `NEW` **AND** `id` equals 1, 2, 3, 4, or 5

### Supported operators {#operators}

#|
|| **Operator** | **Value** | **Example** ||
|| `=` | equals | `["status", "=", "NEW"]` → status exactly NEW ||
|| `!=` | not equals | `["status", "!=", "CLOSED"]` → not closed ||
|| `>` | greater than | `["date", ">", "2025-01-01"]` → after January 1, 2025 ||
|| `>=` | greater than or equal to | `["price", ">=", 1000]` → price from 1000 and above ||
|| `<` | less than | `["date", "<", "2025-01-01"]` → before January 1, 2025 ||
|| `<=` | less than or equal to | `["price", "<=", 1000]` → price up to 1000 inclusive ||
|| `in` | one of the values in the list | `["id", "in", [1,2,3]]` → id = 1 or 2 or 3 ||
|| `between` | in the range | `["date", "between", ["2025-01-01", "2025-12-31"]]` → in 2025 ||
|#

### Which fields are available in the filter {#filterable}

Filtering is not available for every field of an object. The list of fields is returned by the `*.field.list` method of the corresponding section: a field available in the filter has the `filterable` attribute set to `true`. The `sortable` attribute shows the same for sorting.

```bash
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"select":["name","type","filterable","sortable"]}' \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.field.list
```

### Filter errors {#filter-errors}

#|
|| **Code** | **HTTP status** | **Reason** ||
|| `BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION` | 400 | A field that the object does not have is specified in a condition ||
|| `BITRIX_REST_V3_EXCEPTION_UNKNOWNFILTEROPERATOREXCEPTION` | 400 | An operator that is not in the table above is specified in a condition ||
|| `BITRIX_REST_V3_EXCEPTION_VALIDATION_REQUESTVALIDATIONEXCEPTION` | 400 | The value in a condition does not fit the type, for example, a string instead of a number ||
|#

## Batch call {#batch}

The `batch` method combines several calls into a single request. In REST 3.0, its format is different: the request body is a JSON array at the root, without the `cmd` wrapper. Each element of the array describes one call and contains two fields:

- `method` — the method name,
- `query` — an object with the method parameters in the same form in which they are passed in a regular call.

```bash
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '[{"method":"tasks.task.get","query":{"id":289,"select":["id","title"]}},{"method":"tasks.task.get","query":{"id":429,"select":["id","title"]}}]' \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/batch
```

In the response, `result` is an array of results in the same order in which the calls were passed.

```json
{
    "result": [
        {
            "item": {
                "id": 289,
                "title": "Test time 1"
            }
        },
        {
            "item": {
                "id": 429,
                "title": "Deadline automation"
            }
        }
    ]
}
```

{% note warning "" %}

The format of the previous version with the `cmd` object and strings like `"method?param=value"` does not work in REST 3.0: the request returns the error `BITRIX_REST_V3_EXCEPTION_INVALIDSELECTEXCEPTION`.

{% endnote %}

## Comparison of REST 3.0 with the old version of the API {#table}

#|
|| **What changes** | **Old version of REST** | **REST 3.0** ||
|| Call path | `/rest/{id}/{webhook}/{method}` | `/rest/api/{id}/{webhook}/{method}` ||
|| Body format | JSON or form-data | Only JSON ||
|| Response structure | Different response formats in modules | Unified format for all methods ||
|| Fields and connections | Only fields of the current object of the call are available | Fields of related objects are available, they are specified in `select` with a dot ||
|| Documentation | apidocs.bitrix24.com and github | OpenAPI, apidocs.bitrix24.com and github ||
|| Number of requests | More, all related objects are separate calls | Fewer, nested selections reduce the number of separate requests and lower overall server load ||
|| batch | URL-encoding of nested requests, limited ability to pass data to subsequent steps | JSON array of objects, data from previous steps is structurally substituted, arrays from the results of the previous method can be used ||
|| Filter | Different capabilities for methods, most do not support complex logic | A common filter scheme in all methods that support the `filter` parameter ||
|| Errors | Format depends on the method | Unified code, description, and HTTP status ||
|| Repeated call | No protection against duplicates | The `Idempotency-Key` header returns the stored response instead of running the method again ||
|| Scopes | A flat list of modules, the `scope` method | Method-to-scope mapping, the `rest.scope.list` method ||
|#

## Continue Learning

- [{#T}](./scopes/permissions.md)
- [{#T}](./data-types.md)
- [{#T}](./tasks/fields-rest-v3.md)
- [{#T}](./tasks/index.md)
- [{#T}](./note/index.md)
