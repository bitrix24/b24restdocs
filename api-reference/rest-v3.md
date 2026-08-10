# Overview of REST API 3.0

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

REST 3.0 is a new version of the API in Bitrix24 that makes working with integrations more predictable and structured. Key improvements include:
- a unified response format,
- retrieving related data in a single request,
- built-in OpenAPI documentation.

> [Comparison table of versions of REST](#table)

## How to call the new version

Call address: `https://{installation_address}/rest/api/{user_id}/{webhook_code}/{method}`.

- `{installation_address}` — the address of Bitrix24.

- `/rest/api/` — indicates the version of REST. After `rest/`, specify `/api/` — this is the main difference in calling the new version from the old one. If `/api/` is not specified, Bitrix24 will execute the method of the old version of REST if it exists, or return the error `Method not found` for methods available only in REST 3.0.

- `/{user_id}/{webhook_code}/` — webhook authorization data. For applications, pass the authorization token in the `auth` field in the request body.

- `{method}` — the method being called.

**Example of calling a new method with webhook authorization**

```json
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"fields":{"taskId":51,"text":"Message from external system"}}' \
https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.chat.message.send
```

**Example of calling a new method with application authorization**

```json
curl -X POST \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d '{"fields":{"taskId":51,"text":"Message from external system"},"auth":"**put_access_token_here**"}' \
https://**put_your_bitrix24_address**/rest/api/tasks.task.chat.message.send
```

### Technical requirements

- Pass the request body in JSON format with the header `Content-Type: application/json`.

- All requests with parameters should be sent only in POST format, for example, the method [tasks.task.chat.message.send](./tasks/tasks-task-chat-message-send.md) with parameters `taskId` and `text`.

 ```json
 curl -X POST \
 -H "Content-Type: application/json" \
 -H "Accept: application/json" \
 -d '{"fields":{"taskId":51,"text":"Message from external system"}}' \
 https://**put_your_bitrix24_address**/rest/api/**put_your_user_id_here**/**put_your_webhook_here**/tasks.task.chat.message.send
 ```

- Requests without parameters can be sent as GET or POST, for example, the method without parameters `rest.documentation.openapi`.

 ```json
 curl -X GET "https://{account}/rest/api/{user_id}/{webhook}/documentation"
 ```

{% note info "" %}

The Go SDK [b24gosdk](../sdk/b24gosdk/index.md) calls REST 3.0 methods: pass a base address that contains the `/rest/api/` segment, and nothing else needs to be configured. List traversal and `batch` in it work only with the previous version — on REST 3.0, call them through a regular method call.

Other SDKs do not yet support the `/rest/api/` address in calls. Use direct HTTP requests, for example, via `curl` or `fetch`.

{% endnote %}

## OpenAPI documentation {#openapi}

In REST 3.0, automatically generated documentation in OpenAPI standard is available.

To obtain the documentation, call the method `rest.documentation.openapi` or `documentation` without parameters.

```bash
curl -X POST "https://{account}/rest/api/{user_id}/{webhook}/documentation"
```

The result will be JSON in OpenAPI format.

The method can be called in Swagger, Postman, or another program that works with OpenAPI.

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

### What to consider

- The header works only in REST 3.0, that is, when calling the `/rest/api/` address. In the previous version of REST it is ignored.

- The header is taken into account in methods that create, modify, and delete data. In methods that retrieve data it is not needed: a repeated call does not change the state of Bitrix24.

- The key is unique within a single application or webhook, a single user, and a single method. The same key in two different methods means two independent calls.

- The response is stored only when the call succeeds. If the method returns an error, a repeated call with the same key runs the method again.

- The key value is 1 to 255 printable ASCII characters.

- Send a repeated call after you receive a response or the timeout expires. Two requests with the same key sent at the same time may both run.

### Errors

#|
|| **Code** | **Reason** | **What to do** ||
|| `BITRIX_REST_V3_EXCEPTION_IDEMPOTENCYKEYREUSEDEXCEPTION` | The key was already used with a different request body | Take a new key. Reuse the previous key only when you repeat the same call ||
|| `BITRIX_REST_V3_EXCEPTION_INVALIDIDEMPOTENCYKEYEXCEPTION` | The key value does not fit the length limit or contains invalid characters | Pass a string of 1 to 255 printable ASCII characters ||
|#

## Unified response format

In the previous version of REST, different modules returned results in various formats. For example, the identifier of a created item could be a nested object `"result": { "id": 1823 }` or returned directly in the result as `"result": 1823`. Different methods required writing custom logic for response handling.

The new version of REST returns responses in a unified format that applies to all methods, regardless of the module.

### Structure of a successful response

If the method returns data, such as a list of found items or the identifier of a created item, they will be returned as a nested object within `"result": {...}`.

```json
{
  "result": {
    "item": {
      "id": 42,
      "title": "Prepare report"
    }
  }
}
```

If the method returns the result of an operation as true or false, for example, when deleting an item, it will return as a nested object within `"result": {...}`.

```json
{
  "result": {
    "result": true
  }
} 
```

### Structure of an unsuccessful response

Any method can return an error, for example, due to access denial or incorrect request parameters. In the new format, the error contains:

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

## Connections between objects {#connection}

REST 3.0 allows retrieving data from related objects in a single response. For example, a task has a `responsible` field — this field contains the identifier of another object, a user. In the old version of REST, you first had to get the identifier from the `responsible` field using the old method [tasks.task.get](./tasks/tasks-task-get.md), and then separately call the method [user.get](./user/user-get.md) to get data by the user identifier.

In the new version of REST, you can specify the fields of related objects directly in the request [tasks.task.get](./tasks/tasks-task-get-rest-v3.md) using `select`: `"select": ["responsible.name", "responsible.email"]`.

```json
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
            "...": "", // other fields
            "responsible": {
                "name": "Name",
                "email": "mail@bitrix.com"
            },
        }
    },
}
```

If incorrect or unavailable fields are specified in `select`, the request will return an error.

```json
{
    "error": {
        "code": "BITRIX_REST_V3_EXCEPTION_UNKNOWNDTOPROPERTYEXCEPTION",
        "message": "Unknown field `stageId` for entity `UserDto`"
    }
}
```

To find out which connections and fields are supported, use the [OpenAPI documentation](#openapi) or the article [Task fields in REST 3.0](./tasks/fields-rest-v3.md).

## Filtering {#filter}

In REST 3.0, data filtering is based on logical expressions that can be combined. All methods that support the `filter` parameter operate according to this scheme.

### Filtering principles

- Conditions within a single level are combined using the logic of **AND** — all must be satisfied simultaneously.

- Groups of conditions can be combined using the logic of **OR** with a special object with the key `"logic": "or"`.

**Example of a simple filter.** Find all records that satisfy two conditions simultaneously:

1. Status = "NEW".

2. ID equals 3, 4, or 5.

```json
{
    "filter": [
        ["status", "=", "NEW"],
        ["id", "in", [3,4,5]]
    ]
}
```

All elements in the `filter` array are combined using AND logic -> Status = NEW **AND** (ID = 3, 4, or 5).

**Example of a complex filter with logic.** Find all records that satisfy two conditions simultaneously:

1. Status = "NEW".

2. ID equals 1 or 2, **OR** ID equals 3, 4, or 5.

```json
{
    "filter": [
        ["status", "=", "NEW"],
        {
            "logic": "or", 
            "conditions": [
                ["id", [1,2]], // id in (1,2)
                ["id", "in", [3,4,5]]
            ]
        }
    ]
}
```

1. `["status", "=", "NEW"]` -> This is a simple condition: find all elements where the `status` field **equals** the value `NEW`.
2. `{ "logic": "or", "conditions": [...] }` -> This is a group of conditions combined using OR logic. Inside the group are two conditions:
   - `["id", [1,2]]` — this is a shorthand for `["id", "in", [1,2]]` -> "ID must be one of these numbers: 1 or 2",
   - `["id", "in", [3,4,5]]` -> "ID must be 3, 4, or 5". `"logic": "or"` indicates that an element will match if at least one of these two conditions is satisfied.
3. All elements in the `filter` array are combined using AND logic -> Status = NEW **AND** (ID = 1 or 2 **OR** ID = 3 or 4 or 5).

### Supported operators

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

## Comparison of REST 3.0 with the old version of the API {#table}

#|
|| **What changes** | **REST v2** | **REST v3** ||
|| Call path | `/rest/{id}/{webhook}/{method}` | `/rest/api/{id}/{webhook}/{method}` ||
|| Body format | JSON or form-data | Only JSON ||
|| Response structure | Different response formats in modules | Unified format for all methods ||
|| Fields and connections | Only fields of the current object of the call are available | Fields of related objects are available, RelationToOne/RelationToMany connections ||
|| Documentation | apidocs.bitrix24.com and github | OpenAPI, apidocs.bitrix24.com and github ||
|| Number of requests | More, all related objects are separate calls | Fewer, nested selections reduce the number of separate requests and lower overall server load ||
|| batch | URL-encoding of nested requests, limited ability to pass data to subsequent steps | JSON array of objects, data from previous steps is structurally substituted, arrays from the results of the previous method can be used ||
|| Filter | Different capabilities for methods, most do not support complex logic | All methods support complex logic ||
|| Errors | Format depends on the method | Unified code and description ||
|#
