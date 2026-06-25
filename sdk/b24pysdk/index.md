# Installation and Usage of B24PySDK

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

B24PySDK is the official Python SDK for the Bitrix24 REST API. It provides a convenient Python interface to API methods, supports authorization via incoming webhooks and the OAuth protocol, validates request types and parameters before sending, returns data in standard Python structures, and unifies REST API error handling.

The SDK includes integrations for Django, FastAPI, and Flask: these help validate data sent by Bitrix24 when opening an application, calling event handlers, or working with workflows.

Use B24PySDK if:

- You are developing an application, integration, or automation in Python;
- You need to work with the Bitrix24 REST API without manually constructing HTTP requests;
- IDE autocompletion, request parameter validation, and predictable response structures are important;
- You are planning a backend application that needs to reliably handle authorization, events, and API errors.

B24PySDK supports:

1. Authorization via [incoming webhooks](../../local-integrations/local-webhooks.md) and the [OAuth protocol](../../settings/oauth/index.md);
2. Type hints and IDE autocompletion for available methods and parameters;
3. Argument type validation before sending a request;
4. Pagination for list methods via `.as_list()` and `.as_list_fast()`;
5. Batch requests and unified REST API error handling.

## Core SDK Modules

B24PySDK consists of several groups of modules that cover different aspects of working with the REST API:

- `b24pysdk.Client` - the entry point for calling REST API methods. Through the client, scopes such as `crm`, `user`, `department`, `disk`, `bizproc`, `tasks`, and others are available.
- `b24pysdk.credentials` - authorization classes: `BitrixWebhook`, `BitrixToken`, `BitrixTokenLocal`, `BitrixApp`, `BitrixAppLocal`, as well as OAuth data models received from Bitrix24.
- `b24pysdk.api.requests` - request objects returned by method wrappers: standard requests, list requests, and batch requests.
- `b24pysdk.api.responses` - response objects that provide access to `result`, `time`, `total`, `next`, and other response data.
- `b24pysdk.errors` - a unified exception hierarchy for network errors, HTTP responses, JSON, REST API, OAuth, and parameter validation errors.
- `b24pysdk.integrations` - integrations for Django, FastAPI, and Flask: decorators, dependencies, and helper functions for processing inbound requests from Bitrix24.
- `b24pysdk.constants` - constants for CRM, users, tasks, telephony, and other API sections.
- `b24pysdk.events` and `b24pysdk.signals` - SDK events, such as OAuth token updates or automatic portal domain changes after a redirect.
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

## Installation

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

The SDK repository is available on GitHub: [bitrix24/b24pysdk](https://github.com/bitrix24/b24pysdk).

## Using with Incoming Webhooks

To connect the SDK to an incoming webhook, provide the account domain and the webhook code in the `user_id/webhook_key` format. For example, for a webhook `https://example.bitrix24.com/rest/1/abcdef/` the domain will be `example.bitrix24.com`, and the webhook code is `1/abcdef`.

```python
from b24pysdk import BitrixWebhook, Client

bitrix_token = BitrixWebhook(
    domain="example.bitrix24.com",
    webhook_token="1/webhook_key",
)

client = Client(bitrix_token)
```

Once the client is initiated, it can be used to call various Bitrix24 REST API methods. In the example below, the variable `result` will receive the deal ID as a result of its creation:

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

## Obtaining Bitrix_Token in Marketplace Applications

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

## Obtaining Bitrix_Token in Local Applications

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

In the case of a local application, the values for `domain`, `client_id`, and `client_secret` are taken from the local application settings in your Bitrix24. The list of permissions is defined there and determines which REST methods will be available to the obtained token.

After initiating the `client` object, you can use it to call various Bitrix24 REST API methods:

```python
result = client.crm.deal.add(
    fields={
        "TITLE": "New deal",
        "TYPE_ID": "SALE",
        "STAGE_ID": "NEW",
    }
).result
```

## REST API Versions

`Client` can work with different REST API versions. If no version is specified, the SDK uses the default version. If you need to explicitly prefer API v3, pass `prefer_version=3`:

```python
client = Client(bitrix_token, prefer_version=3)
```

When selecting API v3, the SDK uses the v3 wrapper if available. If a method does not yet have a separate v3 wrapper, the SDK may use the available wrapper from a previous version.

## Responses and Requests

SDK methods do not send a request at the moment the object is created. They return a request object. The call is executed when you access `.response`, `.result`, or `.time`.

```python
request = client.crm.deal.get(bitrix_id=1)

deal = request.result
duration = request.time.duration
```

`request.response` contains the entire response object:

- `response.result` - the data returned by the REST method;
- `response.time` - service information regarding execution time;
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

## Method Parameters and Type Checking

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

## List Methods and Large Selections

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
    filter={">ID": 0},
    order={"ID": "ASC"},
).as_list_fast().result

for deal in deals:
    print(deal["ID"], deal["TITLE"])
```

`.as_list_fast()` returns a generator and makes requests lazily during iteration. This is convenient for large volumes of data when you do not need to keep the entire selection in memory.

## Batch Requests

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

## Error Handling

All SDK exceptions inherit from `BitrixSDKException`. In practice, it is useful to categorize errors into several levels:

- `BitrixRequestError` and `BitrixRequestTimeout` - network issue or timeout;
- `BitrixResponseError` - the server returned an HTTP response that could not be processed as a valid Bitrix24 REST API JSON response;
- `BitrixAPIError` - the REST API returned an error in the standard `error` and `error_description` format;
- `BitrixAPIExpiredToken` - the OAuth token has expired; for `BitrixToken`, the SDK can update it automatically if `refresh_token` and application data are available;
- `BitrixAPIInsufficientScope` - the application lacks sufficient permissions;
- `BitrixAPITooManyRequests`, `BitrixAPIQueryLimitExceeded`, `BitrixAPIOverloadLimit` - limits exceeded or the account is temporarily overloaded.

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

## Validation of Inbound Data From Bitrix24

Bitrix24 passes data to the application when an application is opened within the interface, when a widget is opened, when event handlers are called, and during the execution of workflows. The ready-to-use SDK integrations collect inbound request parameters, validate the payload, and return typed data:

- `OAuthPlacementData` - data from opening an application or a widget;
- `OAuthEventData` - event handler data;
- `OAuthWorkflowData` - workflow automation rule data;
- `OAuth`, `EventOAuth`, `WorkflowOAuth`, `RenewedOAuth` - OAuth data within inbound payloads.

If you pass `bitrix_app` to Django, FastAPI, or Flask integrations, the SDK can additionally verify inbound OAuth data via `app.info`.

## Django Integration

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

The integration returns validation errors as `401 Unauthorized`, and unexpected errors like `500 Internal Server Error`.

## FastAPI Integration

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

The integration returns validation errors as `401 Unauthorized`, and unexpected errors like `500 Internal Server Error`.

## Flask Integration

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

The integration returns validation errors as `401 Unauthorized`, and unexpected errors like `500 Internal Server Error`.

## Token Events

The SDK can automatically refresh an OAuth token or change the account domain if Bitrix24 returns a redirect to a new domain. You can subscribe to these actions:

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

## Configuring Timeouts, Retries, and Logging

`Config` sets the SDK configurations for the current execution flow: timeouts, number of retries, delays between retries, logging, and the time zone.

```python
from b24pysdk import Config
from b24pysdk.log import StreamLogger

logger = StreamLogger()

Config().configure(
    default_timeout=(3.05, 10),
    default_max_retries=3,
    default_initial_retry_delay=1,
    default_retry_delay_increment=1,
    logger=logger,
)
```

A timeout can also be set for a specific customer or an individual call:

```python
client = Client(bitrix_token, timeout=(3.05, 10))

result = client.crm.deal.get(
    bitrix_id=1,
    timeout=5,
).result
```

## Constants

`b24pysdk.constants` contains constants for frequently used API values. These are useful when you want to avoid passing numeric identifiers or string codes manually:

```python
from b24pysdk.constants.crm import EntityTypeID

fields = client.crm.item.fields(
    entity_type_id=EntityTypeID.DEAL,
    use_original_uf_names="N",
).result
```

Constants are not mandatory, but they make the code more readable and reduce the risk of typos in frequently repeated values.