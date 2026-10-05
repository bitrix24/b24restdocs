# Installation and Usage of B24PySDK

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

B24PySDK is the official Python SDK for the Bitrix24 REST API. It provides a convenient Python interface to API methods, supports authorization via inbound webhooks and the OAuth protocol, validates request types and parameters before sending, returns data in standard Python structures, and unifies REST API error handling.

The SDK includes integrations for Django, FastAPI, and Flask: these help validate data sent by Bitrix24 when opening an application, calling event handlers, or working with workflows.

Use B24PySDK if:

- You are developing an application, integration, or automation in Python;
- You need to work with the Bitrix24 REST API without manually constructing HTTP requests;
- IDE autocompletion, request parameter validation, and predictable response structures are important;
- You are planning a backend application that needs to reliably handle authorization, events, and API errors.

B24PySDK supports:

1. Authorization via [inbound webhooks](../../local-integrations/local-webhooks.md) and the [OAuth protocol](../../settings/oauth/index.md);
2. Type hints and IDE autocompletion for available methods and parameters;
3. Argument type validation before sending a request;
4. Typed response adapters through `.value` and `.values` for methods with defined Python result schemas;
5. Pagination for list methods via `.as_list()` and `.as_list_fast()`;
6. Batch requests and unified REST API error handling;
7. The `b24pysdk.objects` object layer: typed Python objects over the REST API, lazy loading, object managers, `filter`, `order`, `select`, batch writes, links, and `select_related()`.

## Contents {#contents}

- [Core SDK Modules](#modules)
- [Installation](#install)
- [Using with Inbound Webhooks](#webhook)
- [Retrieving bitrix_token in Marketplace Applications](#market-token)
- [Retrieving bitrix_token in Local Applications](#local-token)
- [OAuth Token Lifecycle](#oauth-token-lifecycle)
- [REST API Versions](#rest-versions)
- [Responses and Requests](#responses-and-requests)
- [Supported SDK Methods](#supported-methods)
- [Method Parameters and Type Checking](#method-params)
- [List Methods and Large Datasets](#list-methods)
- [Direct Calls Without a Wrapper](#direct-calls)
- [Batch Requests](#batch)
- [Error Handling](#errors)
- [Validation of Inbound Data From Bitrix24](#incoming-data)
- [Object Layer](#objects)
- [Django Integration](#django)
- [FastAPI Integration](#fastapi)
- [Flask Integration](#flask)
- [Token Events](#token-events)
- [Configuring Timeouts, Retries, and Logging](#config)
- [Constants](#constants)

## Core SDK Modules {#modules}

B24PySDK consists of several groups of modules that cover different aspects of working with the REST API:

- `b24pysdk.Client` - the entry point for calling REST API methods. Through the client, scopes such as `crm`, `user`, `department`, `disk`, `bizproc`, `tasks`, and others are available.
- `b24pysdk.credentials` - authorization classes: `BitrixWebhook`, `BitrixToken`, `BitrixTokenLocal`, `BitrixApp`, `BitrixAppLocal`, as well as OAuth data models received from Bitrix24.
- `b24pysdk.api.requests` - request objects returned by method wrappers: standard requests, list requests, and batch requests.
- `b24pysdk.api.responses` - response objects that provide access to `result`, `value`, `values`, `time`, `total`, `next`, and other response data.
- `b24pysdk.schemas` - typed schemas for convenient Python representations of Bitrix24 data: user profiles, application information, access permission names, CRM fields, CRM enumerations, VAT, entity merge results, and API error data.
- `b24pysdk.objects` - a typed object layer over supported REST entities: objects, field descriptors, object managers, links, `BitrixObjectList`, and batch operation results.
- `b24pysdk.errors` - a unified exception hierarchy for network errors, HTTP responses, JSON, REST API, OAuth, and parameter validation errors.
- `b24pysdk.integrations` - integrations for Django, FastAPI, and Flask: decorators, dependencies, and helper functions for processing inbound requests from Bitrix24.
- `b24pysdk.constants` - constants for CRM, users, tasks, telephony, and other API sections.
- `b24pysdk.events` and `b24pysdk.signals` - SDK events, such as OAuth token updates or automatic account domain changes after a redirect. To subscribe to signals directly, install the optional `signals` dependencies.
- `b24pysdk.log` and `b24pysdk.Config` - configuration for timeouts, retries, time zone, and logging.

The basic import for most scenarios looks like this:

```python
from b24pysdk import BitrixWebhook, Client
from b24pysdk.errors import BitrixAPIError, BitrixSDKException

bitrix_token = BitrixWebhook(
    domain="example.bitrix24.com",
    webhook_token="1/webhook_key",
)

client = Client(bitrix_token)
```

The client returns a `BaseClient` object, whose methods are grouped by REST scopes:

```python
deal = client.crm.deal.get(bitrix_id=1).result
user = client.user.current().result
fields = client.crm.company.fields().result
```

## Installation {#install}

B24PySDK requires Python 3.9 or higher. To install, use `pip`:

```bash
pip install b24pysdk
```

If you are using integrations with web frameworks, install the SDK with the required additional set of dependencies:

```bash
pip install "b24pysdk[django]"
pip install "b24pysdk[fastapi]"
pip install "b24pysdk[flask]"
```

If your application subscribes to internal SDK events, install the optional `signals` dependencies:

```bash
pip install "b24pysdk[signals]"
```

Regular REST calls and automatic OAuth token refresh work without these dependencies. They are required only for directly importing `b24pysdk.signals` and subscribing to SDK signals.

The SDK repository is available on GitHub: [bitrix24/b24pysdk](https://github.com/bitrix24/b24pysdk).

## Using with Inbound Webhooks {#webhook}

To connect the SDK to an inbound webhook, provide the account domain and the webhook code in the `user_id/webhook_key` format. For example, for a webhook `https://example.bitrix24.com/rest/1/abcdef/` the domain will be `example.bitrix24.com`, and the webhook code is `1/abcdef`.

```python
from b24pysdk import BitrixWebhook, Client

bitrix_token = BitrixWebhook(
    domain="example.bitrix24.com",
    webhook_token="1/webhook_key",
)

client = Client(bitrix_token)
```

Once the client is initialized, it can be used to call various Bitrix24 REST API methods. In the example below, the variable `result` will receive the deal ID as a result of its creation:

```python
result = client.crm.deal.add(
    fields={
        "TITLE": "New deal",
        "TYPE_ID": "SALE",
        "STAGE_ID": "NEW",
    }
).result
```

If there is no ready-made wrapper in the SDK for the method you need, call the REST method directly via `bitrix_token.call_method()`. This allows you to call any REST API method:

```python
response = bitrix_token.call_method(
    api_method="crm.deal.add",
    params={
        "fields": {
            "TITLE": "New deal",
            "TYPE_ID": "SALE",
            "STAGE_ID": "NEW",
        },
    },
)

result = response["result"]
```

In this case, you specify the REST method name and request parameters yourself. The SDK will perform the authorized HTTP call, handle network errors, and return the Bitrix24 REST API JSON response. IDE autocompletion for a specific method wrapper will not work.

## Retrieving bitrix_token in Marketplace Applications {#market-token}

For a marketplace application, use `BitrixApp` and the OAuth tokens that Bitrix24 passed to the application. The account domain is passed explicitly:

```python
from b24pysdk import BitrixApp, BitrixToken, Client

bitrix_app = BitrixApp(
    client_id="put-your-client-id-here",
    client_secret="put-your-client-secret-here",
)

bitrix_token = BitrixToken(
    domain="example.bitrix24.com",
    auth_token="put-access-token-here",
    refresh_token="put-refresh-token-here",
    bitrix_app=bitrix_app,
)

client = Client(bitrix_token)
```

The values for `client_id` and `client_secret` are taken from the application settings. `auth_token` is the current access token, and `refresh_token` is required by the SDK to automatically refresh the OAuth token.

## Retrieving bitrix_token in Local Applications {#local-token}

For a local application, use `BitrixAppLocal` and `BitrixTokenLocal`. Unlike a marketplace app, `BitrixAppLocal` is tied to a specific account:

```python
from b24pysdk import BitrixAppLocal, BitrixTokenLocal, Client

bitrix_app = BitrixAppLocal(
    domain="example.bitrix24.com",
    client_id="put-your-client-id-here",
    client_secret="put-your-client-secret-here",
)

bitrix_token = BitrixTokenLocal(
    auth_token="put-access-token-here",
    refresh_token="put-refresh-token-here",
    bitrix_app=bitrix_app,
)

client = Client(bitrix_token)
```

In the case of a local application, the values for `domain`, `client_id`, and `client_secret` are taken from the local application settings in your Bitrix24. The list of permissions is defined there and determines which REST methods will be available to the retrieved token.

After initializing the `client` object, you can use it to call various Bitrix24 REST API methods:

```python
result = client.crm.deal.add(
    fields={
        "TITLE": "New deal",
        "TYPE_ID": "SALE",
        "STAGE_ID": "NEW",
    }
).result
```

## OAuth Token Lifecycle {#oauth-token-lifecycle}

For OAuth applications, the SDK provides public methods to retrieve, refresh, and validate tokens.

Through the application object:

```python
renewed_oauth = bitrix_app.get_oauth_token(code)
oauth_token = renewed_oauth.oauth_token

renewed_oauth = bitrix_app.refresh_oauth_token(refresh_token)

app_info_response = bitrix_app.get_app_info(auth_token)
app_info = app_info_response.result
```

Through the token object:

```python
renewed_oauth = bitrix_token.refresh_oauth_token()
bitrix_token.refresh_and_set_oauth_token()

app_info_response = bitrix_token.get_app_info()
app_info = app_info_response.result
```

`refresh_oauth_token()` retrieves new OAuth data and returns it without modifying the current token object. `refresh_and_set_oauth_token()` retrieves new OAuth data and stores it in the current `bitrix_token`.

Useful token properties:

```python
oauth_token = bitrix_token.oauth_token

print(bitrix_token.has_expired)
print(bitrix_token.is_one_off)
print(bitrix_token.is_webhook)
```

To create a client from an existing token, use `get_client()`:

```python
client = bitrix_token.get_client()
client_v3 = bitrix_token.get_client(prefer_version=3)
```

## REST API Versions {#rest-versions}

`Client` can work with different REST API versions. If no version is specified, the SDK uses the default version. If you need to explicitly prefer API v3, pass `prefer_version=3`:

```python
client = Client(bitrix_token, prefer_version=3)
```

The default version is v2. The SDK supports `prefer_version=1`, `prefer_version=2`, and `prefer_version=3`. With `prefer_version=3`, a call uses REST 3.0 only if the SDK knows that the method supports the new API version. Other compatible calls use the older REST version.

The list of methods that the SDK considers available in REST 3.0 is stored in `Config().api_v3_methods`. You can override it globally:

```python
from b24pysdk import Config

Config().configure(
    api_v3_methods={
        "tasks.task.get",
        "tasks.task.list",
    },
)
```

The v3 client has separate contexts for v3 methods:

- `call`;
- `documentation`;
- `humanresources`;
- `mail`;
- `main`;
- `note`;
- `rest`;
- `tasks`;
- `timeman`.

## Responses and Requests {#responses-and-requests}

SDK methods do not send a request at the moment the object is created. They return a request object. The call executes when you first access `.response`, `.result`, `.time`, `.value`, or `.values`.

```python
request = client.crm.deal.get(bitrix_id=1)

deal = request.result
duration = request.time.duration
```

The response is cached after the first execution. Subsequent property access reuses the response. To send a new REST request and replace the cached response, call `.call()`:

```python
request = client.crm.deal.get(bitrix_id=1)

first_response = request.response
fresh_response = request.call()
```

The request object exposes the following shortcut properties:

- `request.response` - the entire response object;
- `request.result` - the raw Bitrix24 result;
- `request.value` - a single typed Python value;
- `request.values` - a list or generator of adapted values;
- `request.time` - execution timing information.

`request.response` exposes the same data at the response-object level:

- `response.result` - the data returned by the REST method;
- `response.value` - a single adapted Python object for methods with a defined schema for a single result;
- `response.values` - a list or generator of adapted Python objects for methods that return multiple values;
- `response.time` - execution timing information;
- `response.total` and `response.next` - available in list responses if the API returned pagination data.

```python
request = client.crm.deal.list(
    select=["ID", "TITLE", "STAGE_ID"],
    order={"ID": "ASC"},
)

response = request.response
print(response.result)
print(response.total)
print(response.next)
```

For methods with typed adapters, you can use `.value` or `.values`. They convert the Bitrix24 result into objects from `b24pysdk.schemas` but do not replace `.result`: continue using `.result` if you need the raw API response.

```python
request = client.profile()

raw_profile = request.result
profile = request.value

print(raw_profile["ID"])
print(profile.bitrix_id)
```

For methods that return multiple values of the same type, use `.values`:

```python
request = client.crm.enum.ownertype()

raw_owner_types = request.result
owner_types = request.values

print(raw_owner_types[0]["ID"])
print(owner_types[0].bitrix_id)
```

## Supported SDK Methods {#supported-methods}

The SDK's REST API coverage is expanding. To check which method wrappers are available in the installed SDK version, use the client methods:

```python
all_methods = client.get_supported_api_methods()
deal_methods = client.get_supported_api_methods("crm.deal")

client.print_supported_api_methods("crm")
```

You can pass a string prefix or a client context to `get_supported_api_methods()`. The method returns a list of REST methods for which the SDK provides wrappers.

## Method Parameters and Type Checking {#method-params}

In SDK wrappers, parameters are named as Python arguments. For example, the REST parameter `ID` in entity retrieval methods is typically passed as `bitrix_id`, and the `fields` object remains a dictionary:

```python
result = client.crm.deal.update(
    bitrix_id=1,
    fields={
        "TITLE": "New deal name",
    },
).result
```

The SDK checks argument types before sending a request. This helps catch errors on the application side rather than after receiving a response from the Bitrix24 REST API. Pass values of the type specified in the IDE hints and the method signature.

## List Methods and Large Datasets {#list-methods}

A standard call to a list method returns a single page of data. For the Bitrix24 REST API, this is usually up to 50 items:

```python
deals = client.crm.deal.list(
    select=["ID", "TITLE"],
    order={"ID": "ASC"},
).result
```

If you need to retrieve all pages, use `.as_list()`:

```python
deals = client.crm.deal.list(
    select=["ID", "TITLE"],
    order={"ID": "ASC"},
).as_list().result
```

For large tables, use `.as_list_fast()` when the method supports fast retrieval by `ID`:

```python
deals = client.crm.deal.list(
    select=["ID", "TITLE"],
).as_list_fast().result

for deal in deals:
    print(deal["ID"], deal["TITLE"])
```

`.as_list_fast()` returns a generator and makes requests lazily during iteration. This is convenient for large volumes of data when you do not need to keep the entire result set in memory.

Do not pass `order`, `sort`, or `start` to the original `.list()` call when using `.as_list_fast()`: the SDK manages sorting and ID-based pagination itself and raises an error if these parameters are already present in the request. Set the iteration direction with `descending=True`:

```python
deals = client.crm.deal.list(
    select=["ID", "TITLE"],
).as_list_fast(descending=True).result
```

`call_list_fast()` and `.as_list_fast()` do not yet support REST 3.0. Use a regular method call for v3 methods.

## Direct Calls Without a Wrapper {#direct-calls}

If the SDK does not yet provide a wrapper for a REST method, you can call the API directly through `bitrix_token.call_*()` methods. These methods execute immediately and return the raw response in REST API format:

```python
result = bitrix_token.call_method(
    "crm.deal.get",
    {
        "ID": 1,
    },
)

deals = bitrix_token.call_list(
    "crm.deal.list",
    {
        "select": ["ID", "TITLE"],
    },
)

fast_deals = bitrix_token.call_list_fast(
    "crm.deal.list",
    {
        "select": ["ID", "TITLE"],
    },
)

batch_result = bitrix_token.call_batch({
    "deal": ("crm.deal.get", {"ID": 1}),
    "fields": ("crm.deal.fields", {}),
})

batches_result = bitrix_token.call_batches([
    ("crm.deal.get", {"ID": 1}),
    ("crm.deal.get", {"ID": 2}),
])
```

`Client` methods first create a request object, and the REST call executes when the result is accessed. The `bitrix_token.call_*()` methods send the request immediately.

## Batch Requests {#batch}

If you need to perform several REST calls in a single request, use `client.call_batch()`. It is suitable for a set of up to 50 commands.

Commands can be passed as a dictionary. In this case, the command keys are preserved in the result:

```python
requests_data = {
    "deal": client.crm.deal.get(bitrix_id=1),
    "fields": client.crm.deal.fields(),
}

batch_request = client.call_batch(requests_data)

print(batch_request.result.result["deal"])
print(batch_request.result.result["fields"])
```

Commands can also be passed as a list. In this case, the result will be a list in the same order:

```python
requests_data = [
    client.crm.deal.get(bitrix_id=1),
    client.crm.deal.fields(),
]

batch_request = client.call_batch(requests_data)

for result in batch_request.result.result:
    print(result)
```

If there are more than 50 commands, use `client.call_batches()`. The SDK will split requests into several batch calls. `call_batches()` also accepts a dictionary or a list:

```python
requests = [
    client.crm.deal.get(bitrix_id=1),
    client.crm.deal.get(bitrix_id=2),
    client.crm.deal.get(bitrix_id=3),
]

batches_request = client.call_batches(requests)

for deal in batches_request.result.result:
    print(deal)
```

Batch requests accept the same lazy request objects that standard SDK methods return.

`client.call_batch()` accepts `halt` and `ignore_size_limit`. `halt` controls whether the batch stops when a command fails, while `ignore_size_limit=True` disables the SDK's request size limit check.

The batch response exposes:

- `result` - successful command results;
- `result_error` - individual command errors;
- `result_total` - `total` values for commands;
- `result_next` - `next` values for commands;
- `result_time` - command execution timing information.

If commands are passed as a dictionary, the SDK preserves the keys in `result`, `result_error`, `result_total`, `result_next`, and `result_time`.

`client.call_batches()` automatically splits commands into multiple batch requests and combines their results into a single response.

## Error Handling {#errors}

All SDK exceptions inherit from `BitrixSDKException`. The main error types differ in the layer where the failure occurred and the fields they expose:

| Error Type | When It Occurs | Main Fields |
| --- | --- | --- |
| `BitrixRequestError`, `BitrixRequestTimeout` | The request did not reach Bitrix24 due to a network issue or timeout | `message`; for timeouts, also `timeout` |
| `BitrixResponseError` | Bitrix24 responded, but the response cannot be processed as successful | HTTP response data and `message` |
| `BitrixResponseJSONDecodeError` | The response cannot be parsed as the expected JSON | HTTP response data and `message` |
| `BitrixAPIError` from `b24pysdk.errors` | REST API v1/v2 returned an error | `error`, `error_description` |
| `BitrixAPIError` from `b24pysdk.errors.v3` | REST API v3 returned an error | `code`, `error.message`, `validation`, `has_validation` |

For REST API v1/v2, Bitrix24 returns errors in the older format: a string `error` code and an `error_description`. More specific classes inherit from `BitrixAPIError`: `BitrixAPIExpiredToken` for an expired OAuth token, `BitrixAPIInsufficientScope` for insufficient permissions, and `BitrixAPITooManyRequests`, `BitrixAPIQueryLimitExceeded`, and `BitrixAPIOverloadLimit` for limits and overload conditions.

REST API v3 uses a different structure: the response contains an `error` object with `code`, `message`, and, for request parameter errors, a `validation` list. Therefore, v3 errors are imported from `b24pysdk.errors.v3`.

API errors inherit through the response error hierarchy. The SDK provides separate classes for HTTP/API statuses, such as `BitrixAPIBadRequest`, `BitrixAPIUnauthorized`, `BitrixAPIForbidden`, `BitrixAPINotFound`, and `BitrixAPIServiceUnavailable`. Some HTTP statuses also have separate JSON parsing error classes.

Example of general error handling:

```python
from b24pysdk.errors import (
    BitrixAPIError,
    BitrixAPIInsufficientScope,
    BitrixAPIQueryLimitExceeded,
    BitrixAPITooManyRequests,
    BitrixRequestTimeout,
    BitrixSDKException,
)

try:
    deal = client.crm.deal.get(bitrix_id=1).result
except BitrixRequestTimeout as error:
    print(f"Request timed out: {error.timeout}")
except BitrixAPIInsufficientScope as error:
    print(f"Insufficient permissions: {error.error_description}")
except (BitrixAPITooManyRequests, BitrixAPIQueryLimitExceeded) as error:
    print(f"Rate limit exceeded: {error.error_description}")
except BitrixAPIError as error:
    print(
        "REST API error",
        f"error: {error.error}",
        f"error_description: {error.error_description}",
        sep="\n",
    )
except BitrixSDKException as error:
    print(f"SDK error: {error.message}")
else:
    print(deal)
```

If you need to log all unforeseen application errors, add a standard `Exception` after handling SDK errors:

```python
try:
    result = client.crm.deal.fields().result
except BitrixSDKException as error:
    print(f"SDK error: {error.message}")
except Exception as error:
    print(f"Unexpected application error: {error}")
else:
    print(result)
```

Example of REST API v3 error handling:

```python
from b24pysdk import Client
from b24pysdk.errors.v3 import BitrixAPIError

client_v3 = Client(bitrix_token, prefer_version=3)

try:
    result = client_v3.tasks.task.get(bitrix_id=51).result
except BitrixAPIError as error:
    print(error.code)
    print(error.error.message)

    if error.has_validation:
        for issue in error.validation or []:
            print(issue.field, issue.message)
```

## Validation of Inbound Data From Bitrix24 {#incoming-data}

Bitrix24 passes data to the application when an application is opened within the interface, when a widget is opened, when event handlers are called, and during the execution of workflows. The ready-to-use SDK integrations collect inbound request parameters, validate the inbound data, and return typed data:

- `OAuthPlacementData` - data from opening an application or a widget;
- `OAuthEventData` - event handler data;
- `OAuthWorkflowData` - workflow automation rule data;
- `OAuth`, `EventOAuth`, `WorkflowOAuth`, `RenewedOAuth` - OAuth data within inbound data.

If you pass `bitrix_app` to Django, FastAPI, or Flask integrations, the SDK can additionally verify inbound OAuth data via `app.info`.

You can use the typed inbound data models without a web framework integration. Pass a dictionary of inbound parameters to `from_dict()` and, if needed, validate the application data through `validate_against_app_info()`:

```python
from b24pysdk import BitrixApp
from b24pysdk.credentials import OAuthEventData, OAuthPlacementData, OAuthWorkflowData

bitrix_app = BitrixApp(
    client_id="put-your-client-id-here",
    client_secret="put-your-client-secret-here",
)

placement_data = OAuthPlacementData.from_dict(placement_payload)
event_data = OAuthEventData.from_dict(event_payload)
workflow_data = OAuthWorkflowData.from_dict(workflow_payload)

payload = placement_data.to_dict()

app_info = placement_data.get_app_info(bitrix_app)
placement_data.validate_against_app_info(app_info)
```

This lets you manually validate application or widget placement data, event handler data, and workflow automation rule data when your application does not use Django, FastAPI, or Flask.

## Object Layer {#objects}

`b24pysdk.objects` is a high-level typed layer over supported Bitrix24 REST entities.

It does not replace regular calls through `Client`. Scope wrappers remain the main direct interface to the REST API, while the object layer adds ORM-like entity access:

- Python objects with typed fields;
- lazy loading;
- local changes and `save()`;
- typed object managers;
- filtering, sorting, field selection, and pagination;
- links through `ObjectField`;
- bulk preloading of linked objects through `select_related()`;
- field metadata and display values for list fields;
- batch create, update, and delete operations;
- `BitrixObjectList`;
- custom object subclasses and managers that preserve typing.

Each manager exposes only the operations correctly supported by the REST API for its entity.

### Supported Object Families

The object layer includes models for:

- users and user custom fields;
- departments;
- workgroups;
- social network groups/projects;
- events and offline events;
- placements;
- universal lists;
- workflow lists;
- workgroup lists.

Example imports:

```python
from b24pysdk.objects.user import User
from b24pysdk.objects.department import Department
from b24pysdk.objects.list.element import ListElement
```

### Connecting a Client

You can pass a client explicitly:

```python
user = User(1, client=client)

users = User.objects.using(client=client).filter(active=True)
```

For an application that works with a single account, you can configure a default client factory:

```python
from b24pysdk import Config

Config().configure(
    default_client_factory=lambda: client,
)
```

Then:

```python
user = User(1)
users = User.objects.filter(active=True)
```

For an application that works with multiple accounts, pass the appropriate `client` or `client_factory` explicitly.

### Registering Object Classes

Object classes are registered when the Python class definition is executed, after the module defining the class is imported.

The object module must therefore be imported at least once before the class is first used dynamically.

This is especially important for custom overrides:

```python
import myapp.bitrix_objects
```

Import the module at application startup, before the first object-layer queries, `.value` / `.values` adaptation, or link resolution.

### Lazy Loading a Single Object

Creating an object by its primary key does not itself send a REST request:

```python
user = User(1, client=client)
```

The request is sent when a field that has not been loaded is first read:

```python
print(user.name)
```

The primary key is available without loading:

```python
print(user.bitrix_pk)
print(user.bitrix_id)
```

You can also create an object from existing Bitrix24 data:

```python
user = User(
    bitrix_data={
        "ID": "1",
        "NAME": "John",
        "ACTIVE": "Y",
    },
    client=client,
)
```

If the application passes `bitrix_data` directly to the constructor, the SDK makes a `deepcopy` so that subsequent changes to the original dictionary do not affect the object's state.

Internal SDK adapters work differently: a fresh raw REST result can be passed to an object without an additional `deepcopy`. As a result, `.result` and the object from `.value` / `.values` may share references to nested mutable values. Treat the raw `.result` as read-only after adaptation.

However:

```python
obj.bitrix_data
obj.local_data
```

return independent data snapshots.

### Object Fields

Object attributes are represented by typed descriptors.

For example:

- `IntField`;
- `FloatField`;
- `TextField`;
- `HTMLField`;
- `BoolField`;
- `DateField`;
- `DateTimeField`;
- `TimeField`;
- `ListField`;
- `EnumField`;
- `DictField`;
- `FileField`;
- `ObjectField`;
- other specialized fields.

Assigning a field changes the object's local state and does not itself send a REST request:

```python
user.name = "John"

print(user.has_changes)
print(user.local_data)
```

### `update()` and `save()`

`update()` sends changes immediately:

```python
user.update(
    name="John",
    last_name="Smith",
)
```

You can accumulate local changes through descriptors and then call:

```python
user.name = "John"
user.last_name = "Smith"

user.save()
```

By default, `save()` sends only fields marked as changed.

```python
user.name = "John"
user.save()
```

To explicitly send the current values of specific fields, use `update_fields`:

```python
user.save(
    update_fields=[
        "name",
        "last_name",
    ],
)
```

Fields in `update_fields` do not have to be assigned through a setter before `save()`.

This is useful for mutable field values that were modified in place.

For example:

```python
settings = userfield.settings
settings["DEFAULT_VALUE"] = "new value"

print(userfield.has_changes)  # False

userfield.save(
    update_fields=["settings"],
)
```

### Accessing a Field Through `.field(attr_name)`

In addition to regular access:

```python
item.status
```

you can retrieve a bound field accessor:

```python
status = item.field("status")
```

`field()` accepts the Python attribute name, not the original Bitrix24 field code.

The field accessor exposes:

```python
status.value
status.raw_value
status.meta
status.title
```

For `ListField`, the following are also available:

```python
status.items
status.display_value
```

Example:

```python
for choice in item.field("status").items:
    print(choice.bitrix_id, choice.value)

print(item.field("status").display_value)
```

`display_value` converts the stored list item ID to its display value.

You can also perform the reverse conversion:

```python
item.field("status").display_value = "In progress"
item.save()
```

The SDK finds the ID of the matching list item and assigns it as the regular field value.

For a multiple `ListField`, you can pass a list of display values.

### Caching Field Metadata

Field metadata is not requested separately for each object instance.

The cache is stored on `Client`.

The cache key for object-layer metadata includes the object family and discriminator.

For elements of a specific universal list, field metadata is associated with:

```text
"list.field" + (IBLOCK_TYPE_ID, IBLOCK_ID)
```

For example:

```python
projects = (
    ProjectElement.objects
    .using(client=client)
    .all()
)

for project in projects:
    print(project.field("status").display_value)
```

The SDK loads the list's field definitions the first time an operation requires metadata.

Subsequent `ProjectElement` objects with the same `client` and `IBLOCK_ID` reuse the loaded metadata collection.

Accessing `.meta`, `.title`, `.items`, and `.display_value` during iteration therefore does not send a separate `lists.field.get` request for each element.

A different list or `Client` uses a separate metadata cache.

### Object Managers

An object manager is a typed query builder at the class level.

For a filtered query:

```python
users = (
    User.objects
    .using(client=client)
    .filter(active=True)
)
```

For an unfiltered query:

```python
users = (
    User.objects
    .using(client=client)
    .all()
)
```

You can then iterate over the manager lazily:

```python
for user in users:
    print(user.name)
```

The manager itself is a query builder.

A chain containing only modifiers, such as:

```python
User.objects.select("name").order("name")
```

must end with `.all()` before iteration:

```python
users = (
    User.objects
    .using(client=client)
    .select("name")
    .order("name")
    .all()
)
```

If `filter()` has already been used, a final `.all()` is not required.

### Core Manager Operations

Depending on the entity, a manager may support:

```python
.using(...)
.filter(...)
.all()
.from_pks(...)
.order(...)
.reverse()
.select(...)
.select_all()
.select_related(...)
.start(...)
.limit(...)
.as_fast(...)
.to_list()
.count()
.exists()
.first()
.last()
.add(...)
.add_many(...)
.update(...)
.delete(...)
```

Some managers expose operations specific to their entity.

For example:

```python
User.objects.current()
User.objects.search(...)
```

### Filtering

Filters use Python field names:

```python
users = User.objects.filter(
    active=True,
    name__contains="John",
)
```

Lookup suffixes are supported when both the field descriptor and the corresponding API method support them:

```text
__ne
__gt
__gte
__lt
__lte
__in
__not_in
__contains
__like
__not_contains
__not_like
```

You can filter by the synthetic primary key field:

```python
User.objects.filter(bitrix_pk=1)

User.objects.filter(
    bitrix_pk__in=[1, 2, 3],
)
```

Bulk query by primary keys:

```python
users = User.objects.from_pks([1, 2, 3])
```

### `select()`

If the API method supports field selection:

```python
users = (
    User.objects
    .select(
        "bitrix_id",
        "name",
        "email",
    )
    .all()
)
```

The manager uses Python field names.

Required primary key fields are added automatically.

You can select all registered fields:

```python
users = User.objects.select_all().all()
```

Partially loaded objects remain safe to use: accessing a registered field that was not loaded may trigger a full lazy reload by the object layer.

### `count()`, `exists()`, `first()`, `last()`

```python
query = User.objects.filter(active=True)

count = query.count()
exists = query.exists()
first = query.first()
last = query.last()
```

`count()` respects `limit` but ignores `start`.

For an already materialized `BitrixObjectList`, the length is read from memory.

### Fast Loading

```python
users = (
    User.objects
    .filter(active=True)
    .as_fast()
)

for user in users:
    ...
```

A fast manager may use a single-pass generator.

If you need to reuse the result:

```python
users = users.to_list()
```

`start()` is not used in fast mode.

Explicit sorting in fast mode must match the complete primary key and use a single direction.

### Links and `ObjectField`

`ObjectField` represents a link as another SDK object.

For example, `Department` has:

```python
department.uf_head_id
department.uf_head
```

A regular `ObjectField` is loaded lazily.

After the department is loaded:

```python
department.uf_head
```

creates a lightweight linked `User` object by its primary key without a separate request.

A request may be sent later:

```python
department.uf_head.name
```

if the linked object has not yet been loaded.

### `select_related()` and Eliminating N+1 Queries

Reading a link for many parent objects through regular lazy access can lead to N+1 queries.

Use `select_related()` to preload linked objects in bulk:

```python
departments = (
    Department.objects
    .using(client=client)
    .select_related(
        "parent",
        "uf_head",
    )
    .all()
)
```

#### Link Paths

A path must start with an `ObjectField`.

You can load the entire linked object:

```python
select_related("uf_head")
```

or specific fields of the linked object if its manager supports `select()`:

```python
select_related(
    "uf_head.name",
    "uf_head.email",
)
```

You can use nested links:

```python
select_related(
    "parent.uf_head.name",
)
```

All intermediate path components must be `ObjectField` instances.

#### How `select_related()` Executes

When executing a query, the manager:

1. loads the parent objects;
2. ensures that each parent object's response contains the underlying link field, such as the linked object's ID;
3. automatically adds the required underlying link field if an explicit `select()` is used;
4. collects linked object primary keys from all parent objects;
5. removes duplicate PKs;
6. creates a single bulk query through the linked manager's `from_pks(unique_pks)`;
7. passes the same `Client` to the linked manager;
8. passes a timeout if required;
9. combines terminal fields into a single `select()` for the linked query if the path specifies them;
10. recursively processes nested `select_related` paths;
11. indexes the loaded linked objects by primary key;
12. replaces lightweight link placeholders with the loaded objects.

Therefore:

```python
Department.objects.select_related("uf_head").all()
```

logically executes:

```text
1 query for parent objects
+ 1 bulk query for linked objects
```

instead of:

```text
1 query for parent objects
+ N user queries
```

For:

```python
.select_related(
    "uf_head.name",
    "uf_head.email",
    "uf_head.work_position",
)
```

`uf_head` is loaded once with the combined set of terminal fields.

For two different links:

```python
.select_related(
    "parent",
    "uf_head",
)
```

a separate bulk query is executed for each link at the current level.

#### Multiple Links

For a multiple `ObjectField`, the SDK:

- collects unique IDs from all parent objects;
- loads the linked primary keys in bulk;
- preserves the original order of each parent object's list of links;
- returns a typed `BitrixObjectList`.

#### Missing Linked Records

If a parent object has an underlying primary key but the bulk query does not return the linked object, `select_related()` does not fail immediately.

The original placeholder with the primary key remains in the link cache.

Subsequent lazy access to an unloaded field of this placeholder follows the linked object's regular logic and may raise its `DoesNotExist` exception.

#### `select()` and `select_related()`

These are different operations:

- `select()` specifies regular fields of the current object;
- `select_related()` specifies links to load in bulk.

You can combine them:

```python
departments = (
    Department.objects
    .select_related(
        "uf_head",
    )
    .all()
)
```

For managers that support `select()`, you can combine it with `select_related()`. Even if the underlying foreign key field is not included in `.select()`, the SDK adds it automatically when required by `select_related()`.

```python
workgroups = (
    Workgroup.objects
    .select(
        "bitrix_id",
        "name",
    )
    .select_related(
        "owner",
    )
    .all()
)
```

#### Terminal Links and Terminal Fields

The difference:

```python
select_related("uf_head")
```

loads the linked object's regular dataset.

Whereas:

```python
select_related(
    "uf_head.name",
    "uf_head.email",
)
```

passes an explicit selection of those fields to the linked manager.

If terminal fields require `select()`, the linked manager must expose a public `select()` method.

#### Requirements

To load linked objects in bulk, the linked manager must support `from_pks()`.

If a valid bulk `from_pks()` query is not possible, `select_related()` raises an object-layer error instead of silently falling back to N+1 queries.

The link can still be accessed lazily in the usual way.

#### `select_related()` and Fast Mode

A fast query may be a single-pass generator.

`select_related()` needs to traverse the parent objects more than once, so the parent fast stream is materialized into a `BitrixObjectList` when necessary.

Linked records may still be loaded through a fast bulk query using `from_pks(...).as_fast()`.

### `BitrixObjectList`

Materializing a query:

```python
users = (
    User.objects
    .filter(active=True)
    .to_list()
)
```

The result is a typed `BitrixObjectList[User]`.

Available operations:

```python
users.length
users.exists()
users.first()
users.last()
users.to_pks()
```

You can persist local object changes in a batch operation:

```python
for user in users:
    user.title = "Developer"

result = users.update()
```

Objects without local changes are skipped.

You can delete all objects in the list if the API method supports deletion:

```python
result = users.delete()
```

### Batch Creation

Managers that support creation expose `add()`:

```python
department = Department.objects.add(
    name="Development",
    parent_id=1,
    sort=100,
)
```

Bulk creation:

```python
result = Department.objects.add_many([
    {
        "name": "Backend",
        "parent_id": 1,
        "sort": 100,
    },
    {
        "name": "Frontend",
        "parent_id": 1,
        "sort": 200,
    },
])
```

`BitrixObjectBatchAddResult[T]` exposes:

```python
result.results
result.errors
result.is_success
result.has_errors
```

`result.results` is a typed `BitrixObjectList[T]`.

If the input is a sequence, errors are keyed by the original zero-based index.

If the input is a dictionary, `errors` preserves its keys.

### Batch Updates and Deletion

A manager query:

```python
result = (
    User.objects
    .filter(name__contains="Old")
    .update(name="Renamed")
)
```

or:

```python
result = (
    Department.objects
    .filter(bitrix_pk__in=[10, 11, 12])
    .delete()
)
```

Returns `BitrixObjectBatchWriteResult[T]`:

```python
result.result
result.result_error

result.results
result.errors

result.is_success
result.has_errors
```

`result.result` and `result.result_error` are keyed by the SDK object instances themselves.

`result.results` and `result.errors` are typed `BitrixObjectList[T]` collections.

### Custom Object Subclasses

You can extend SDK objects for a specific account:

- add custom fields;
- add object methods;
- fix the context for a specific entity;
- override the manager when necessary.

#### Example: a Specific Universal List

Suppose an account has a universal list:

```text
IBLOCK_ID = 123
```

with the following properties:

```text
PROPERTY_101  Customer
PROPERTY_102  Status
PROPERTY_103  Budget
PROPERTY_104  Attachment
```

You can define an application-specific object:

```python
from b24pysdk.objects import (
    FileField,
    FloatField,
    ListField,
    TextField,
)
from b24pysdk.objects.list.element import ListElement
from b24pysdk.schemas.list.field import ListElementFile


class ProjectElement(ListElement):
    IBLOCK_ID = 123

    customer = TextField("PROPERTY_101")
    status = ListField("PROPERTY_102")
    budget = FloatField("PROPERTY_103")
    attachment = FileField[ListElementFile](
        "PROPERTY_104",
        file_class=ListElementFile,
    )

    def customer_label(self) -> str:
        return f"{self.name}: {self.customer or '-'}"
```

A separate manager is not required.

The inherited manager automatically binds to the specific subclass:

```python
project = (
    ProjectElement.objects
    .filter(status=7)
    .first()
)

# type: ProjectElement | None
```

Materialized result:

```python
projects = (
    ProjectElement.objects
    .filter(status=7)
    .to_list()
)

# type: BitrixObjectList[ProjectElement]
```

Creation:

```python
project = ProjectElement.objects.add(
    element_code="PROJECT-001",
    name="New project",
    customer="Acme",
    status=7,
    budget=150000.0,
)

# type: ProjectElement
```

### Custom Manager That Preserves Typing

If your application needs custom manager methods, you can specialize the manager for a specific object type.

An additional `Generic` / `TypeVar` is not needed in the usual case:

```python
from b24pysdk.objects.list.element import (
    ListElement,
    ListElementManager,
)


class ProjectElementManager(
    ListElementManager["ProjectElement"],
):
    def with_status(
        self,
        status_id: int,
    ) -> "ProjectElementManager":
        return self.filter(status=status_id)

    def create_project(
        self,
        *,
        code: str,
        name: str,
        customer: str,
        status_id: int,
        budget: float,
    ) -> "ProjectElement":
        return self.add(
            element_code=code,
            name=name,
            customer=customer,
            status=status_id,
            budget=budget,
        )


class ProjectElement(ListElement):
    IBLOCK_ID = 123

    objects: "ProjectElementManager" = ProjectElementManager()

    customer = TextField("PROPERTY_101")
    status = ListField("PROPERTY_102")
    budget = FloatField("PROPERTY_103")
```

Typing is preserved:

```python
query = (
    ProjectElement.objects
    .with_status(7)
    .limit(20)
)

# type: ProjectElementManager

project = query.first()
# type: ProjectElement | None

projects = query.to_list()
# type: BitrixObjectList[ProjectElement]
```

Custom creation method:

```python
project = ProjectElement.objects.create_project(
    code="PROJECT-002",
    name="Typed project",
    customer="Acme",
    status_id=7,
    budget=250000.0,
)

# type: ProjectElement
```

`add_many()` also preserves the generic object type:

```python
result = ProjectElement.objects.add_many([
    {
        "element_code": "PROJECT-003",
        "name": "Another project",
        "customer": "Acme",
        "status": 7,
        "budget": 100000.0,
    },
])

# type: BitrixObjectBatchAddResult[ProjectElement]
```

An additional generic manager is needed only if the application intends to subclass `ProjectElement` further and preserve that subtype through the same custom manager.

### Universal List Registration and Discriminator

For `ListElement`, the object family uses the following discriminator:

```text
(IBLOCK_TYPE_ID, IBLOCK_ID)
```

The generic universal list class already has a fixed list type, while its specific `IBLOCK_ID` may be `None`.

In an application-specific subclass:

```python
class ProjectElement(ListElement):
    IBLOCK_ID = 123
```

a specific implementation is registered only for this list.

The model lookup order is:

```text
(lists, 123)
(lists, None)
(None, None)
None
```

A custom `ProjectElement` therefore does not replace the models for all other universal lists.

If a new class is registered for the same exact `OBJECT_KEY + discriminator` pair, it must inherit from the class already registered for that pair.

Remember to import the module containing custom objects before first use.

### Important Rule for Updating `PROPERTY_*`

For universal list elements, a REST update may clear custom property values that are missing from the complete set of submitted data.

If your application updates specific list elements, declare descriptors for all current `PROPERTY_*` values returned by the account.

If the API returns an undeclared property, the object layer must stop the update and require a field definition rather than risk clearing another custom property.

### Object-Layer Exceptions

The main exceptions are defined in `b24pysdk.objects.errors`.

For example:

- `BitrixObjectError`;
- `BitrixObjectDoesNotExist`;
- `BitrixObjectMultipleObjectsReturned`;
- `BitrixObjectClientError`;
- `BitrixObjectFieldError`;
- `BitrixObjectFilterError`;
- `BitrixObjectFieldReadOnlyError`;
- `BitrixObjectFieldNotLoadedError`.

Specific object classes also expose their own exceptions:

```python
User.DoesNotExist
User.MultipleObjectsReturned
```


## Django Integration {#django}

The Django integration provides decorators for view functions. They collect request parameters, validate the payload, and add typed Bitrix24 data to `request`.

Opening an application or a widget:

```python
from django.http import JsonResponse

from b24pysdk.integrations.django.decorators import placement_required
from b24pysdk.integrations.django.types import PlacementRequest


@placement_required
def placement_view(request: PlacementRequest):
    return JsonResponse({
        "domain": request.oauth_placement_data.domain,
    })
```

Event handler:

```python
from django.http import JsonResponse

from b24pysdk.integrations.django.decorators import event_required
from b24pysdk.integrations.django.types import EventRequest


@event_required
def event_view(request: EventRequest):
    return JsonResponse({
        "event": request.oauth_event_data.event,
    })
```

Workflow automation rule:

```python
from django.http import JsonResponse

from b24pysdk.integrations.django.decorators import workflow_required
from b24pysdk.integrations.django.types import WorkflowRequest


@workflow_required
def workflow_view(request: WorkflowRequest):
    return JsonResponse({
        "workflow_id": request.oauth_workflow_data.workflow_id,
    })
```

Verifying inbound data via `app.info`:

```python
from django.http import JsonResponse

from b24pysdk import BitrixApp
from b24pysdk.integrations.django.decorators import event_required
from b24pysdk.integrations.django.types import EventRequest

bitrix_app = BitrixApp(
    client_id="put-your-client-id-here",
    client_secret="put-your-client-secret-here",
)


@event_required(bitrix_app=bitrix_app)
def event_view(request: EventRequest):
    return JsonResponse({
        "event": request.oauth_event_data.event,
    })
```

The integration returns validation errors as `401 Unauthorized` and unexpected errors as `500 Internal Server Error`.

## FastAPI Integration {#fastapi}

The FastAPI integration uses dependencies. They return typed `OAuthPlacementData`, `OAuthEventData`, or `OAuthWorkflowData`.

Opening an application or a widget:

```python
from typing import Annotated

from fastapi import Depends, FastAPI

from b24pysdk.credentials import OAuthPlacementData
from b24pysdk.integrations.fastapi.dependencies import placement_dependency

app = FastAPI()


@app.post("/placement")
async def placement_handler(
    placement: Annotated[OAuthPlacementData, Depends(placement_dependency)],
):
    return {
        "domain": placement.domain,
    }
```

Event handler:

```python
from typing import Annotated

from fastapi import Depends, FastAPI

from b24pysdk.credentials import OAuthEventData
from b24pysdk.integrations.fastapi.dependencies import event_dependency

app = FastAPI()


@app.post("/event")
async def event_handler(
    event: Annotated[OAuthEventData, Depends(event_dependency)],
):
    return {
        "event": event.event,
    }
```

Workflow automation rule:

```python
from typing import Annotated

from fastapi import Depends, FastAPI

from b24pysdk.credentials import OAuthWorkflowData
from b24pysdk.integrations.fastapi.dependencies import workflow_dependency

app = FastAPI()


@app.post("/workflow")
async def workflow_handler(
    workflow: Annotated[OAuthWorkflowData, Depends(workflow_dependency)],
):
    return {
        "workflow_id": workflow.workflow_id,
    }
```

Verifying inbound data via `app.info`:

If you need to pass `bitrix_app`, use `get_*_dependency(...)` with the required argument:

```python
from typing import Annotated

from fastapi import Depends, FastAPI

from b24pysdk import BitrixApp
from b24pysdk.credentials import OAuthEventData
from b24pysdk.integrations.fastapi.dependencies import get_event_dependency

app = FastAPI()

bitrix_app = BitrixApp(
    client_id="put-your-client-id-here",
    client_secret="put-your-client-secret-here",
)


@app.post("/event")
async def event_handler(
    event: Annotated[
        OAuthEventData,
        Depends(get_event_dependency(bitrix_app=bitrix_app)),
    ],
):
    return {
        "event": event.event,
    }
```

The integration returns validation errors as `401 Unauthorized` and unexpected errors as `500 Internal Server Error`.

## Flask Integration {#flask}

The Flask integration provides decorators for routes and helper functions for typed data access. Data is stored in `flask.g`.

Opening an application or a widget:

```python
from flask import Flask

from b24pysdk.integrations.flask.decorators import placement_required
from b24pysdk.integrations.flask.dependencies import get_oauth_placement_data

app = Flask(__name__)


@app.post("/placement")
@placement_required
def placement_handler():
    return {
        "domain": get_oauth_placement_data().domain,
    }
```

Event handler:

```python
from flask import Flask

from b24pysdk.integrations.flask.decorators import event_required
from b24pysdk.integrations.flask.dependencies import get_oauth_event_data

app = Flask(__name__)


@app.post("/event")
@event_required
def event_handler():
    return {
        "event": get_oauth_event_data().event,
    }
```

Workflow automation rule:

```python
from flask import Flask

from b24pysdk.integrations.flask.decorators import workflow_required
from b24pysdk.integrations.flask.dependencies import get_oauth_workflow_data

app = Flask(__name__)


@app.post("/workflow")
@workflow_required
def workflow_handler():
    return {
        "workflow_id": get_oauth_workflow_data().workflow_id,
    }
```

Verifying inbound data via `app.info`:

```python
from flask import Flask

from b24pysdk import BitrixApp
from b24pysdk.integrations.flask.decorators import event_required
from b24pysdk.integrations.flask.dependencies import get_oauth_event_data

app = Flask(__name__)

bitrix_app = BitrixApp(
    client_id="put-your-client-id-here",
    client_secret="put-your-client-secret-here",
)


@app.post("/event")
@event_required(bitrix_app=bitrix_app)
def event_handler():
    return {
        "event": get_oauth_event_data().event,
    }
```

The integration returns validation errors as `401 Unauthorized` and unexpected errors as `500 Internal Server Error`.

## Token Events {#token-events}

The SDK can automatically refresh an OAuth token or change the account domain if Bitrix24 returns a redirect to a new domain. You can subscribe to these actions.

To subscribe to signals, install the SDK with the optional `signals` dependencies:

```bash
pip install "b24pysdk[signals]"
```

Without these dependencies, regular REST calls and automatic OAuth token refresh still work, but directly importing `b24pysdk.signals` raises an `ImportError` with installation instructions.

```python
from b24pysdk.events import OAuthTokenRenewedEvent, PortalDomainChangedEvent


def on_token_renewed(event: OAuthTokenRenewedEvent):
    print(event.renewed_oauth_token.oauth_token.access_token)


def on_domain_changed(event: PortalDomainChangedEvent):
    print(event.old_domain, event.new_domain)


bitrix_token.oauth_token_renewed_signal.connect(on_token_renewed)
bitrix_token.portal_domain_changed_signal.connect(on_domain_changed)
```

This is necessary if the application stores OAuth tokens in a database or configuration and must update the stored value after an automatic refresh.

## Configuring Timeouts, Retries, and Logging {#config}

`Config` configures the SDK for the current execution thread: timeouts, retry counts, retry delays, logging, and the time zone.

```python
from b24pysdk import Config
from b24pysdk.log import StreamLogger

logger = StreamLogger()

Config().configure(
    default_connect_timeout=3.05,
    default_read_timeout=10,
    default_max_retries=3,
    default_initial_retry_delay=1,
    default_retry_delay_increment=1,
    logger=logger,
    secure_log=True,
)
```

Current default values:

- connection timeout - `3.05` seconds;
- response read timeout - `10` seconds;
- maximum number of attempts - `3`;
- initial retry delay - `1` second;
- delay increment for each retry - `1` second.

The `secure_log` parameter is enabled by default. In this mode, the SDK masks OAuth tokens, client secrets, auth parameters, and credentials in webhook URLs in logs. Disable it only in a controlled environment where logs will not be publicly accessible.

Requests are retried for HTTP 503. The delay between attempts increases: `default_initial_retry_delay` sets the first pause, and `default_retry_delay_increment` is then added to it.

A timeout can also be set for a specific client or an individual call:

```python
client = Client(
    bitrix_token,
    timeout=(3.05, 10),
    max_retries=3,
    initial_retry_delay=1,
    retry_delay_increment=1,
)

result = client.crm.deal.get(
    bitrix_id=1,
    timeout=5,
).result
```

## Constants {#constants}

`b24pysdk.constants` contains constants for frequently used API values. These are useful when you want to avoid passing numeric identifiers or string codes manually:

```python
from b24pysdk.constants.crm import EntityTypeID

fields = client.crm.item.fields(
    entity_type_id=EntityTypeID.DEAL,
    use_original_uf_names=False,
).result
```

Constants are not mandatory, but they make the code more readable and reduce the risk of typos in frequently repeated values.
