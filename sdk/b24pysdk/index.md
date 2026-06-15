# Installation and Usage of B24PySDK

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

B24PySDK is the official Python SDK for the Bitrix24 REST API. It provides a Python interface to API methods, supports authorization via webhooks and OAuth protocol, checks types and parameters of requests before sending, returns data in native Python data structures, and standardizes API error handling.

The SDK includes integrations for Django, FastAPI, and Flask: they help validate the data that Bitrix24 sends when opening an application, handling events, and working with workflows.

Use B24PySDK if you:

- Are developing an application, integration, or automation in Python,
- Need to work with the Bitrix24 REST API without manually assembling HTTP requests,
- Value IDE autocompletion, request parameter validation, and predictable response structures,
- Are planning a backend application that needs to reliably handle authorization, events, and API errors.

B24PySDK supports:

1. Authorization via [incoming webhooks](../../local-integrations/local-webhooks.md) and [OAuth protocol](../../settings/oauth/index.md);
2. Type hints and autocompletion in IDE for available methods and parameters;
3. Type checking of arguments before sending a request;
4. Pagination of list methods through `.as_list()` and `.as_list_fast()`;
5. Batch requests and unified error handling for REST API.

## Main SDK Modules

B24PySDK consists of several groups of modules that cover different aspects of working with the REST API:

- `b24pysdk.Client` - the entry point for calling REST API methods. Through the client, scopes such as `crm`, `user`, `department`, `disk`, `bizproc`, `tasks`, and others are accessible.
- `b24pysdk.credentials` - authorization classes: `BitrixWebhook`, `BitrixToken`, `BitrixTokenLocal`, `BitrixApp`, `BitrixAppLocal`, as well as OAuth data models received from Bitrix24.
- `b24pysdk.api.requests` - request objects that return method wrappers: standard requests, list requests, and batch requests.
- `b24pysdk.api.responses` - response objects that provide access to `result`, `time`, `total`, `next`, and other response data.
- `b24pysdk.errors` - a unified hierarchy of exceptions for network errors, HTTP responses, JSON, REST API, OAuth, and parameter validation.
- `b24pysdk.integrations` - integrations for Django, FastAPI, and Flask: decorators, dependencies, and helper functions for handling incoming requests from Bitrix24.
- `b24pysdk.constants` - constants for CRM, users, tasks, telephony, and other API sections.
- `b24pysdk.events` and `b24pysdk.signals` - SDK events, such as updating the OAuth token or automatically changing the portal domain after a redirect.
- `b24pysdk.log` and `b24pysdk.Config` - configuration for timeouts, retries, time zones, and logging.

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

The client returns a `BaseClient` object, which has methods grouped by REST scopes:

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

If you are using integrations with web frameworks, install the SDK with the necessary additional dependencies:

```bash
pip install "b24pysdk[django]"
pip install "b24pysdk[fastapi]"
pip install "b24pysdk[flask]"
```

The SDK repository is available on GitHub: [bitrix24/b24pysdk](https://github.com/bitrix24/b24pysdk).

## Using with Incoming Webhooks

To connect the SDK to an incoming webhook, pass the portal domain and webhook code in the format `user_id/webhook_key`. For example, for the webhook `https://example.bitrix24.com/rest/1/abcdef/`, the domain will be `example.bitrix24.com`, and the webhook code will be `1/abcdef`.

```python
from b24pysdk import BitrixWebhook, Client

bitrix_token = BitrixWebhook(
    domain="example.bitrix24.com",
    webhook_token="1/webhook_key",
)

client = Client(bitrix_token)
```

Once the client is initialized, it can be used to call various REST API methods. In the example below, the variable `result` will receive the value of the deal ID as a result of its creation:

```python
result = client.crm.deal.add(
    fields={
        "TITLE": "New Deal",
        "TYPE_ID": "SALE",
        "STAGE_ID": "NEW",
    }
).result
```

If there is no ready-made wrapper for the method you need in the SDK, you can call the REST method directly through `bitrix_token.call_method()`. This allows you to call any REST API method available to the current authorization:

```python
response = bitrix_token.call_method(
    api_method="crm.deal.add",
    params={
        "fields": {
            "TITLE": "New Deal",
            "TYPE_ID": "SALE",
            "STAGE_ID": "NEW",
        },
    },
)

result = response["result"]
```

In this case, you specify the name of the REST method and the request parameters yourself. The SDK will perform an authorized HTTP call, handle network errors, and return the JSON response from the REST API. Autocompletion for the specific method wrapper in the IDE will not work.

## Obtaining bitrix_token in Mass-Market Applications

For a mass-market application that opens within the Bitrix24 interface, use `BitrixApp` and the OAuth data from the incoming request. The SDK will take the portal domain from this data.

In the example below, `params` are the normalized parameters from the incoming POST request from Bitrix24. In the Django integration of the SDK, they are available as `request.params`; in other frameworks, it is the same dictionary collected from the incoming request. These parameters include `DOMAIN`, `PROTOCOL`, `LANG`, `APP_SID`, `AUTH_ID`, `AUTH_EXPIRES`, `REFRESH_ID`, `member_id`, `status`. For widgets, `PLACEMENT` and `PLACEMENT_OPTIONS` may also be included.

```python
from b24pysdk import BitrixApp, BitrixToken, Client
from b24pysdk.credentials import OAuthPlacementData

bitrix_app = BitrixApp(
    client_id="put-your-client-id-here",
    client_secret="put-your-client-secret-here",
)

placement_data = OAuthPlacementData.from_dict(params)
bitrix_token = BitrixToken.from_oauth_placement_data(
    oauth_placement_data=placement_data,
    bitrix_app=bitrix_app,
)

client = Client(bitrix_token)
```

The values for `client_id` and `client_secret` are taken from the application settings. `OAuthPlacementData.from_dict()` validates the incoming parameters, extracts the portal domain, and transforms the fields `AUTH_ID`, `REFRESH_ID`, `AUTH_EXPIRES` into the SDK OAuth token.

## Obtaining bitrix_token in Local Applications

For a local application, use `BitrixAppLocal` and `BitrixTokenLocal`. Unlike a mass-market application, `BitrixAppLocal` is tied to a specific portal. The variable `params` is the same dictionary of incoming parameters from Bitrix24:

```python
from b24pysdk import BitrixAppLocal, BitrixTokenLocal, Client
from b24pysdk.credentials import OAuthPlacementData

placement_data = OAuthPlacementData.from_dict(params)

bitrix_app = BitrixAppLocal(
    domain=placement_data.domain,
    client_id="put-your-client-id-here",
    client_secret="put-your-client-secret-here",
)

bitrix_token = BitrixTokenLocal.from_oauth_placement_data(
    oauth_placement_data=placement_data,
    bitrix_app=bitrix_app,
)

client = Client(bitrix_token)
```

In the case of a local application, you will need to obtain the values for `client_id`, `client_secret`, and the list of permissions from the corresponding settings of the local application in your Bitrix24. The values for `AUTH_ID` and `REFRESH_ID` come in the incoming OAuth data when opening an application, handling events, and working with workflows.

Once you have initialized the `client` object, you can use it to call various REST API methods:

```python
result = client.crm.deal.add(
    fields={
        "TITLE": "New Deal",
        "TYPE_ID": "SALE",
        "STAGE_ID": "NEW",
    }
).result
```

## REST API Versions

`Client` can work with different versions of the REST API. If the version is not specified, the SDK uses the default version. If you need to explicitly prefer API v3, pass `prefer_version=3`:

```python
client = Client(bitrix_token, prefer_version=3)
```

When selecting API v3, the SDK uses the v3 wrapper if available. If the method does not yet have a separate v3 wrapper, the SDK may use the available wrapper from the previous version.

## Responses and Requests

SDK methods do not send a request at the moment of object creation. They return a request object. The call is executed when you access `.response`, `.result`, or `.time`.

```python
request = client.crm.deal.get(bitrix_id=1)

deal = request.result
duration = request.time.duration
```

`request.response` contains the entire response object:

- `response.result` - the data returned by the REST method;
- `response.time` - service information about execution time;
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

In SDK wrappers, parameters are named as Python arguments. For example, the REST parameter `ID` in entity retrieval methods is usually passed as `bitrix_id`, while the `fields` object remains a dictionary:

```python
result = client.crm.deal.update(
    bitrix_id=1,
    fields={
        "TITLE": "New Deal Name",
    },
).result
```

The SDK checks the types of arguments before sending the request. This helps catch errors on the application side rather than after the REST API response. Pass values of the type specified in the IDE hints and method signature.

## List Methods and Large Datasets

A typical call to a list method returns one page of data. For the Bitrix24 REST API, this is usually up to 50 items:

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

`.as_list_fast()` returns a generator and makes requests lazily during iteration. This is convenient for large datasets when you do not need to hold the entire selection in memory.

## Batch Requests

If you need to perform multiple REST calls in one request, use `client.call_batch()`. It is suitable for up to 50 commands.

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

If there are more than 50 commands, use `client.call_batches()`. The SDK will split the requests into several batch calls. `call_batches()` also accepts a dictionary or list:

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

Batch requests accept the same lazy request objects returned by regular SDK methods.

## Error Handling

All SDK exceptions inherit from `BitrixSDKException`. In practice, errors can be grouped into several levels:

- `BitrixRequestError` and `BitrixRequestTimeout` - network issue or timeout;
- `BitrixResponseError` - the server returned an HTTP response that could not be processed as a valid JSON response from the REST API;
- `BitrixAPIError` - the REST API returned an error in the standard format `error` and `error_description`;
- `BitrixAPIExpiredToken` - the OAuth token has expired; for `BitrixToken`, the SDK can refresh it automatically if there is a `refresh_token` and application data;
- `BitrixAPIInsufficientScope` - the application lacks permissions;
- `BitrixAPITooManyRequests`, `BitrixAPIQueryLimitExceeded`, `BitrixAPIOverloadLimit` - limits exceeded or the portal is temporarily overloaded.

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
    print(f"Request exceeded timeout: {error.timeout}")
except BitrixAPIInsufficientScope as error:
    print(f"Insufficient permissions: {error.error_description}")
except (BitrixAPITooManyRequests, BitrixAPIQueryLimitExceeded) as error:
    print(f"Request limit exceeded: {error.error_description}")
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

If you need to log all unforeseen application errors, add a regular `Exception` after handling SDK errors:

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

## Validation of Incoming Data from Bitrix24

Bitrix24 sends data to the application when opening an application within the interface, opening a widget, handling events, and working with workflows. The SDK can parse this data into typed objects:

- `OAuthPlacementData` - data for opening an application or widget;
- `OAuthEventData` - data for the event handler;
- `OAuthWorkflowData` - data for the workflow automation rule;
- `OAuth`, `EventOAuth`, `WorkflowOAuth`, `RenewedOAuth` - models of OAuth data within incoming payloads.

If you are not using a ready-made integration with a web framework, you can manually validate the dictionary from the incoming request:

```python
from b24pysdk.credentials import OAuthPlacementData

try:
    placement_data = OAuthPlacementData.from_dict(params)
except OAuthPlacementData.ValidationError as error:
    print(f"Invalid application opening data: {error}")
else:
    print(placement_data.domain)
```

For a local application, you can create a token directly from the validated OAuth data:

```python
from b24pysdk import BitrixAppLocal, BitrixTokenLocal, Client
from b24pysdk.credentials import OAuthPlacementData

placement_data = OAuthPlacementData.from_dict(params)

bitrix_app = BitrixAppLocal(
    domain=placement_data.domain,
    client_id="put-your-client-id-here",
    client_secret="put-your-client-secret-here",
)

bitrix_token = BitrixTokenLocal.from_oauth_placement_data(
    oauth_placement_data=placement_data,
    bitrix_app=bitrix_app,
)

client = Client(bitrix_token)
```

If you pass `bitrix_app` in the Django, FastAPI, or Flask integrations, the SDK can additionally verify the incoming OAuth data through `app.info`.

## Integration with Django

The Django integration provides decorators for view functions. They collect request parameters, validate the payload, and add typed Bitrix24 data to `request`.

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

For events and workflow automation rules, use `event_required` and `workflow_required`:

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

If you need to validate incoming data through `app.info`, pass the application to the decorator:

```python
@placement_required(bitrix_app=bitrix_app)
def placement_view(request: PlacementRequest):
    return JsonResponse({
        "domain": request.oauth_placement_data.domain,
    })
```

Validation errors are returned as `401 Unauthorized`, while unforeseen errors are returned as `500 Internal Server Error`.

## Integration with FastAPI

The FastAPI integration uses dependencies. They return typed `OAuthPlacementData`, `OAuthEventData`, or `OAuthWorkflowData`.

```python
from typing import Annotated

from fastapi import Depends, FastAPI

from b24pysdk.credentials import OAuthPlacementData
from b24pysdk.integrations.fastapi.dependencies import get_placement_dependency

app = FastAPI()


@app.post("/placement")
async def placement_handler(
    placement: Annotated[OAuthPlacementData, Depends(get_placement_dependency())],
):
    return {
        "domain": placement.domain,
    }
```

For events and workflows, use `get_event_dependency()` and `get_workflow_dependency()`:

```python
from typing import Annotated

from fastapi import Depends

from b24pysdk.credentials import OAuthEventData
from b24pysdk.integrations.fastapi.dependencies import get_event_dependency


@app.post("/event")
async def event_handler(
    event: Annotated[OAuthEventData, Depends(get_event_dependency())],
):
    return {
        "event": event.event,
    }
```

Validation through `app.info` is enabled by passing `bitrix_app`:

```python
placement_dependency = get_placement_dependency(bitrix_app=bitrix_app)
```

## Integration with Flask

The Flask integration provides decorators for routes and helper functions for typed access to data. Data is stored in `flask.g`.

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

For events and workflow automation rules, use `event_required`, `workflow_required`, `get_oauth_event_data()`, and `get_oauth_workflow_data()`:

```python
from b24pysdk.integrations.flask.decorators import event_required
from b24pysdk.integrations.flask.dependencies import get_oauth_event_data


@app.post("/event")
@event_required
def event_handler():
    return {
        "event": get_oauth_event_data().event,
    }
```

As with other integrations, `bitrix_app` includes additional validation of incoming data:

```python
@placement_required(bitrix_app=bitrix_app)
def placement_handler():
    return {
        "domain": get_oauth_placement_data().domain,
    }
```

## Token Events

The SDK can automatically refresh the OAuth token or change the portal domain if Bitrix24 returns a redirect to a new domain. You can subscribe to these actions:

```python
from b24pysdk.events import OAuthTokenRenewedEvent, PortalDomainChangedEvent


def on_token_renewed(event: OAuthTokenRenewedEvent):
    print(event.renewed_oauth_token.oauth_token.access_token)


def on_domain_changed(event: PortalDomainChangedEvent):
    print(event.old_domain, event.new_domain)


bitrix_token.oauth_token_renewed_signal.connect(on_token_renewed)
bitrix_token.portal_domain_changed_signal.connect(on_domain_changed)
```

This is necessary if the application stores OAuth tokens in a database or configuration and needs to update the saved value after an automatic refresh.

## Configuring Timeouts, Retries, and Logging

`Config` sets SDK settings for the current execution thread: timeouts, number of retries, delays between retries, logging, and time zone.

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

Timeouts can also be set for a specific client or individual call:

```python
client = Client(bitrix_token, timeout=(3.05, 10))

result = client.crm.deal.get(
    bitrix_id=1,
    timeout=5,
).result
```

## Constants

In `b24pysdk.constants`, there are constants for frequently used API values. They are useful when you do not want to pass numeric identifiers or string codes manually:

```python
from b24pysdk.constants.crm import EntityTypeID

fields = client.crm.item.fields(
    entity_type_id=EntityTypeID.DEAL,
    use_original_uf_names="N",
).result
```

Constants are not mandatory, but they make the code clearer and reduce the risk of typos in frequently repeated values.
