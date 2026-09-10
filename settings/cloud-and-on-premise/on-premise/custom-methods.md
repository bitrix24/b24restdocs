# Adding Custom Methods to the REST API of the On-Premise Version of Bitrix24

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The on-premise version of Bitrix24 gives access to the server-side code, so the REST API can be extended: you can add your own methods and your own permissions — scopes. A custom method is called the same way as a standard one: at an address such as `/rest/mycompany.warehouse.get`, with regular authorization by a [webhook](../../../local-integrations/local-webhooks.md) or an application token.

The mechanism fits the cases where the data resides in the on-premise Bitrix24 itself and no standard method covers it. Typical tasks:

- expose the data of your own module or table: warehouse stock, price lists, records of an accounting system
- collect in a single call the data that would otherwise have to be retrieved with several requests and merged on the application side
- give an external service narrow access: your own scope with two or three methods instead of permissions for the entire CRM

A custom method can be added only in the on-premise version — the cloud Bitrix24 provides no access to the server-side code. In the cloud, the same tasks are handled with standard tools: custom fields and [CRM smart processes](../../../api-reference/crm/universal/index.md) to retain your own data, or an external service that the application calls directly.

What you need:

- an on-premise Bitrix24 with the **rest** module installed
- access to the server file system with permission to edit `/bitrix/php_interface/init.php`. These are server administrator permissions, not Bitrix24 administrator permissions
- knowledge of PHP and the Bitrix24 core API

To make a custom method work, three steps are required:

1. [Register the event handler](#step-handler) that describes the method
2. [Write the handler function](#step-callback) that does the work
3. [Grant access to your scope](#step-scope) and reset the permission cache

## How It Works

Four elements are involved in the mechanism:

| Element | Role |
|---|---|
| The `OnRestServiceBuildDescription` event of the **rest** module | Builds the description of all REST API methods. Your handler adds your own scopes and methods to that description |
| Method description | An array with the `callback` and `options` keys. It tells the **rest** module which PHP function to call for the method |
| Method handler function | Does the work and returns the data that goes into the response |
| The `\CRestServer` object | An instance of the current REST server. It is passed to the handler function and provides access to the request and authorization data |

A request to a custom method is processed as follows:

1. The **rest** module fires the `OnRestServiceBuildDescription` event and merges the arrays returned by all handlers of the event. The outcome is a combined description of methods grouped by scope
2. The module looks for the called method in that description. If the method is not there, the [onFindMethodDescription](#on-find-method-description) event fires and can supply a description on the fly. If it returns nothing either, the request fails with the `ERROR_METHOD_NOT_FOUND` error
3. The module checks the authorization and the access to the scope the method belongs to. If the application or the webhook does not have the required scope, the call fails with the `insufficient_scope` error
4. The module calls the handler function and passes three parameters to it
5. The value returned by the function goes into the `result` field of the response. An exception is converted into a REST error with the `error` and `error_description` fields

The descriptions of different event handlers are merged recursively. Three rules follow from this:

- identical scope names are safe, the methods of both handlers end up in the same scope
- an identical method name in two descriptions does not raise an error: the call still works, but only one of the handlers runs and the other one is silently ignored
- for the same reason, a custom description can silently override a standard method: one of the two handlers runs, and which one is not determined in advance. Such an override cannot be relied upon, whereas someone else's scenario can be broken by accident

Start method names with the name of your scope — that way they collide neither with custom methods of others nor with standard ones.

{% note warning "" %}

The **rest** module checks only the authorization and the access to the scope. It does not check permissions for specific data: if the application has been granted your scope, the method will be called. Check permissions inside the handler function yourself. By the time of the call, the `$USER` object is already initialized with the user the token or the webhook is bound to — rely on it and on the API of your own module.

{% endnote %}

## Step 1. Register the Event Handler {#step-handler}

The `OnRestServiceBuildDescription` event handler is registered in the `/bitrix/php_interface/init.php` file with the `AddEventHandler` function. The class with the methods of your API is declared in the same file or included in `init.php` as a separate file — before the `AddEventHandler` call. The function takes the module name, the event name, and the callable of the handler:

```php
AddEventHandler(
    'rest',
    'OnRestServiceBuildDescription',
    ['MyClass', 'onRestServiceBuildDescription']
);
```

`OnRestServiceBuildDescription` is an event of the old format, which is why it is registered with `AddEventHandler` and not through the event manager of the current core.

The handler returns an array with the following structure:

```php
return [
    'scope_name' => [
        'scope_name.object.action' => [
            'callback' => ['MyClass', 'firstMethod'],
            'options' => [],
        ],
        'scope_name.object.other_action' => [
            'callback' => ['MyClass', 'secondMethod'],
            'options' => [],
        ],
    ],
];
```

| Key | Description |
|---|---|
| Scope name | An arbitrary permission name, `scope_name` in the example. How to grant access to your scope is described in [step 3](#step-scope). A method can also be placed into a standard scope, `crm` for example — then any application with that permission calls it, and access cannot be restricted separately. Create your own scope for your own data |
| Method name | An arbitrary method name, `scope_name.object.action` in the example. Case does not matter: the **rest** module converts method names to lowercase. The traditional naming convention is scope name, object, action |
| `callback` | The PHP [callable](https://www.php.net/manual/en/language.types.callable.php) type. Use the same forms as the standard handlers: a function name or an array of the form `[class, method]` |
| `options` | An array of additional method configurations. One key is currently supported — `private`. With the value `true`, the method does not appear in the [methods](../../../api-reference/common/system/methods.md) output but remains available for calls. This is how service and internal methods that should not be listed are marked |

A method can be made available to any application without a separate permission. To do this, specify the `\CRestUtil::GLOBAL_SCOPE` constant instead of a scope name.

{% note warning "" %}

A method in `\CRestUtil::GLOBAL_SCOPE` is called by any application and any webhook of this Bitrix24, and access to it cannot be restricted with permissions. Place there only the methods that are safe to open to everyone. For everything else, create your own scope.

{% endnote %}

## Step 2. Write the Handler Function {#step-callback}

The handler function receives three parameters:

| Parameter | Type | Description |
|---|---|---|
| `$query` | array | An associative array of call parameters without the authorization parameters. The `start` parameter is removed from it |
| `$start` | int | The value of the `start` parameter from the request. Zero if the parameter is not passed. Used for [pagination](#navigation) |
| `$server` | \CRestServer | The object of the current REST server |

The `$server` object provides access to the request and authorization data:

| Method | What It Returns |
|---|---|
| `getScope()` | The scope in which the called method was found |
| `getMethod()` | The name of the called method in lowercase |
| `getQuery()` | The same array of call parameters that arrives in the handler as the first parameter |
| `getAuthType()` | The authorization type of the call: `oauth` — an application, `apauth` — an inbound webhook, `sessionauth` — a call authorized by the session of the current user |
| `getAppId()` | The identifier of the application on behalf of which the call is made. Returns `null` for a webhook |

The handler function can:

- return an array or a scalar value — it goes into the `result` field of the response and is converted to the json or xml format
- throw an exception — it is caught and returned to the client as a REST error

### How to Return an Error

To set the HTTP status of an error, throw the `\Bitrix\Rest\RestException` exception. The constructor takes three parameters: the error message, the error code, and the HTTP status.

```php
throw new \Bitrix\Rest\RestException(
    'Parameter id is required',
    'WAREHOUSE_ID_REQUIRED',
    \CRestServer::STATUS_WRONG_REQUEST
);
```

The message goes into the `error_description` field of the response, and the code — into the `error` field. If the status is not specified, the response is returned with the `400 Bad Request` status.

HTTP statuses are defined by the constants of the `\CRestServer` class. Only error statuses are passed into an exception — the first two constants refer to a successful response and are not used in `RestException`.

| Constant | HTTP status |
|---|---|
| `STATUS_OK` | 200 OK |
| `STATUS_CREATED` | 201 Created |
| `STATUS_WRONG_REQUEST` | 400 Bad Request |
| `STATUS_UNAUTHORIZED` | 401 Unauthorized |
| `STATUS_PAYMENT_REQUIRED` | 402 Payment Required |
| `STATUS_FORBIDDEN` | 403 Forbidden |
| `STATUS_NOT_FOUND` | 404 Not Found |
| `STATUS_TO_MANY_REQUESTS` | 429 Too Many Requests |
| `STATUS_INTERNAL` | 500 Internal Server Error |

Any other exception is converted into a REST error as well. By default this is the `400 Bad Request` status and the `ERROR_CORE` code, but if the exception has a code of its own, that code goes into the response. Core exceptions are handled separately: `\Bitrix\Main\ArgumentException` produces the `ERROR_ARGUMENT` code, and an SQL error produces the `ERROR_CORE` code and the `500 Internal Server Error` status.

{% note warning "" %}

If the core generated an old-style error through `$APPLICATION->ThrowException()` before the exception was thrown, it overwrites the code and the message in the response. Make sure that no unhandled old core errors are left in the method handler function.

{% endnote %}

## Step 3. Grant Access to Your Scope {#step-scope}

Your scope appears in the list of permissions when a [local application](../../../local-integrations/local-apps.md) or an [inbound webhook](../../../local-integrations/local-webhooks.md) is created — in the *Applications > Developer resources* section. It is displayed by its code, `mycompany` for example: it has no title in the interface language.

The list of scopes is cached for seven days. The cache is force-reset only when modules are installed or removed, and an event handler in `init.php` is not a module. Therefore, after adding a new scope, reset the cache by calling `\Bitrix\Rest\Engine\ScopeManager::cleanCache()`.

The call is needed once, after a new scope is registered. Do not leave it in `init.php`: there it fires on every request to Bitrix24 and resets the cache permanently. Run it once — for example, as a separate script in the site root:

```php
<?php
require_once $_SERVER['DOCUMENT_ROOT'] . '/bitrix/modules/main/include/prolog_before.php';

\Bitrix\Main\Loader::includeModule('rest');
\Bitrix\Rest\Engine\ScopeManager::cleanCache();
```

Open the script in a browser once and delete it from the server. If the Bitrix24 cache is stored in files, a full cache cleanup gives the same result: the `/bitrix/admin/cache.php` page of the administrative section, the *Delete Cache Files* tab, the *All* option.

The same `cleanCache()` call also resets the cache of `method.get` results — it resides in the same cache directory. There is no need to clear it separately.

Verify the result with the calls:

- [method.get](../../../api-reference/common/system/method-get.md) with the `name` parameter — it returns `isExisting` and `isAvailable`: whether the method is registered and whether it is available with the current permissions
- [scope](../../../api-reference/common/system/scope.md) with the `full` parameter — the new scope must appear in the full list of permissions. Without parameters, the method returns only the permissions already granted to the application or the webhook

How to read the `method.get` response:

- `isExisting: false` — the method is not registered. Check whether the event handler is connected and whether the cache is reset
- `isExisting: true` and `isAvailable: false` — the method exists, but the required scope is not granted to the application or the webhook. When called, such a method returns the `insufficient_scope` error

## Example: A Custom Scope and Method

The code registers the `mycompany` scope and the `mycompany.warehouse.get` method in it. The method checks a required parameter and the user permissions, and then returns the data.

```php
class MyCompanyRestApi
{
    public static function onRestServiceBuildDescription(): array
    {
        return [
            'mycompany' => [
                'mycompany.warehouse.get' => [
                    'callback' => [__CLASS__, 'getWarehouse'],
                    'options' => [],
                ],
            ],
        ];
    }

    public static function getWarehouse($query, $start, \CRestServer $server): array
    {
        global $USER;

        $warehouseId = (int)($query['id'] ?? 0);

        if ($warehouseId <= 0)
        {
            throw new \Bitrix\Rest\RestException(
                'Parameter id is required',
                'WAREHOUSE_ID_REQUIRED',
                \CRestServer::STATUS_WRONG_REQUEST
            );
        }

        // the handler checks data permissions itself, the rest module does not do it
        if (!$USER->IsAdmin())
        {
            throw new \Bitrix\Rest\RestException(
                'Access to warehouse data denied',
                'ACCESS_DENIED',
                \CRestServer::STATUS_FORBIDDEN
            );
        }

        return [
            'id' => $warehouseId,
            'title' => 'Central warehouse',
            'scope' => $server->getScope(),
        ];
    }
}

AddEventHandler(
    'rest',
    'OnRestServiceBuildDescription',
    ['MyCompanyRestApi', 'onRestServiceBuildDescription']
);
```

Requests to a custom method are built by the general REST rules — they are described in the [{#T}](../../how-to-call-rest-api/general-principles.md) article. A successful call:

```http
GET /rest/mycompany.warehouse.get?auth=**put_access_token_here**&id=12

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
```

```json
{
    "result": {
        "id": 12,
        "title": "Central warehouse",
        "scope": "mycompany"
    },
    "time": {
        "start": 1791540000.123456,
        "finish": 1791540000.234567,
        "duration": 0.111111,
        "processing": 0.021,
        "date_start": "2026-10-09T12:00:00+02:00",
        "date_finish": "2026-10-09T12:00:00+02:00"
    }
}
```

A call without the required parameter, in the xml format:

```http
GET /rest/mycompany.warehouse.get.xml?auth=**put_access_token_here**

HTTP/1.1 400 Bad Request
Content-Type: text/xml; charset=utf-8
```

```xml
<?xml version="1.0" ?>
<response>
    <error>WAREHOUSE_ID_REQUIRED</error>
    <error_description>Parameter id is required</error_description>
</response>
```

## Pagination in Custom Methods {#navigation}

If the method returns a list, inherit the class from `\IRestService` and use its `getNavData` and `setNavData` methods. They build the same pagination as the standard methods: the page size is 50 records, the position of the next page is returned in the `next` field, and the total number — in the `total` field. Your method behaves the same way as the standard list methods — their common contract is described in the [{#T}](../../how-to-call-rest-api/list-methods-pecularities.md) article.

`getNavData` takes two parameters: the `start` value from the request and the flag of an ORM class. With the value `true` the method returns an array with the `limit` and `offset` keys for ORM methods, and with the value `false` — an array with the `nPageSize` and `iNumPage` keys for the methods of the old core.

`setNavData` takes the selected records and a pagination array with the keys `count` — the total number of records, and `offset` — the offset of the current page. The method adds the `next` and `total` fields to the result, which the **rest** module lifts to the top level of the response.

```php
\Bitrix\Main\Loader::includeModule('rest');

class MyCompanyRestList extends \IRestService
{
    public static function onRestServiceBuildDescription(): array
    {
        return [
            'mycompany' => [
                'mycompany.user.list' => [
                    'callback' => [__CLASS__, 'getUserList'],
                    'options' => [],
                ],
            ],
        ];
    }

    public static function getUserList($query, $start, \CRestServer $server): array
    {
        $navData = static::getNavData($start, true);

        $result = \Bitrix\Main\UserTable::getList([
            'filter' => $query['filter'] ?? [],
            'select' => $query['select'] ?? ['ID', 'NAME', 'LAST_NAME'],
            'order' => $query['order'] ?? ['ID' => 'ASC'],
            'limit' => $navData['limit'],
            'offset' => $navData['offset'],
            'count_total' => true,
        ]);

        return static::setNavData(
            $result->fetchAll(),
            [
                'count' => $result->getCount(),
                'offset' => $navData['offset'],
            ]
        );
    }
}

AddEventHandler(
    'rest',
    'OnRestServiceBuildDescription',
    ['MyCompanyRestList', 'onRestServiceBuildDescription']
);
```

The `\Bitrix\Main\Loader::includeModule('rest')` call is required before the class declaration: without it the `\IRestService` class is not loaded yet and the code in `init.php` fails with an error.

Example request:

```http
GET /rest/mycompany.user.list?auth=**put_access_token_here**&order[ID]=ASC&filter[<ID]=1000&select[]=ID&select[]=NAME&start=50
```

In the response, `next` appears only if there is a next page. The `result` array in the example is shortened to two records out of fifty:

```json
{
    "result": [
        {"ID": "51", "NAME": "Klaus"},
        {"ID": "52", "NAME": "Petra"}
    ],
    "next": 100,
    "total": 137
}
```

## How to Supply a Method Description on the Fly {#on-find-method-description}

The `onFindMethodDescription` event allows you to describe a method at the moment of the call instead of in advance. It is needed when the set of methods is unknown at startup: for example, when the method name is built from the name of an object created by a user.

The handler receives two parameters: the name of the called method in lowercase and the requested scope. To handle the call, return an array with the `scope` key and the method description. To decline handling, return `null` — then the **rest** module polls the remaining handlers.

```php
AddEventHandler('rest', 'onFindMethodDescription', 'myCompanyFindMethodDescription');

function myCompanyFindMethodDescription($method, $scope)
{
    if (mb_strpos($method, 'mycompany.dynamic.') !== 0)
    {
        return null;
    }

    return [
        'scope' => 'mycompany',
        'callback' => ['MyCompanyDynamicRest', 'getDynamicItem'],
        'options' => [],
    ];
}
```

The `MyCompanyDynamicRest` class with the `getDynamicItem` method is written the same way as the method handler from step 2: the same three input parameters, the same way to return data or an error.

Such methods do not appear in the [methods](../../../api-reference/common/system/methods.md) output: they are not present in the combined description.

## Continue Learning

- [{#T}](index.md)
- [{#T}](versions.md)
- [{#T}](custom-auth-provider.md)
- [{#T}](../../../local-integrations/local-apps.md)
- [{#T}](../../../api-reference/common/system/method-get.md)
- [{#T}](../../../api-reference/common/system/scope.md)
