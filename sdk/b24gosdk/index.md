# Installation and Usage of B24GoSDK

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

[B24GoSDK](https://github.com/bitrix24/b24gosdk) is the official Go SDK for the Bitrix24 REST API. It handles authorization, retries on failures, list traversal, and response parsing, while a REST method is called by its name.

The SDK contains no wrappers for individual methods. This is deliberate: a method that Bitrix24 releases today is available in the SDK on the same day, and nothing has to be updated or regenerated as the API grows.

Use B24GoSDK:

- if you are developing an integration, an application, or a background service in Go,
- if you need authorization through [inbound webhooks](../../local-integrations/local-webhooks.md) or the [OAuth protocol](../../settings/oauth/index.md),
- if you need [batch requests](../../settings/how-to-call-rest-api/batch.md) and the export of large lists,
- if predictable behavior on connection drops and rate limits is important.

B24GoSDK supports:

1. Authorization through [inbound webhooks](../../local-integrations/local-webhooks.md) and the [OAuth protocol](../../settings/oauth/index.md) with automatic token renewal;
2. A universal call of any REST method by its name;
3. [Batch requests](../../settings/how-to-call-rest-api/batch.md) with `$result` chains and splitting into parts;
4. List traversal by the server cursor and by ID — for large exports;
5. Response parsing: IDs in both formats, unwrapping, and detecting the shape of a value;
6. Parsing of the data that Bitrix24 passes to an application: installation events and the POST data of the application page.

Go 1.21 or higher is required. The SDK has no external dependencies.

## How This SDK Differs From the Others

Read this section if you have already worked with B24PhpSDK, B24PySDK, or
B24JsSDK. The workflow here is different, and the examples on this page look
different for a reason.

| | B24PhpSDK, B24PySDK | B24GoSDK |
|---|---|---|
| Method call | a wrapper: `deal()->add(...)` | by name: `Call(ctx, "crm.deal.add", …)` |
| Method name and parameters | suggested by the IDE | taken from the REST documentation |
| Response | a typed object | raw JSON, you declare the struct |
| A new portal method | you wait for an SDK update | available right away |

The trade-off is clear: **there is no autocompletion for REST methods**, because a method name is
an ordinary string. In exchange, the whole of REST is available, including a method that
Bitrix24 released today, and the SDK does not lag behind the API or require regeneration.

## How to Write an Integration

The routine that replaces autocompletion.

### 1. Find the Method in the Documentation

Method names and their parameters are not stored in the SDK, and **you must not invent them**:
it is `crm.deal.add`, not `crm.deals.create`. The exact names, parameters, and response
shape are in the [REST API reference](../../api-reference/index.md).

The response shape is a fact of the documentation as well, not of the SDK: the fact that `crm.deal.add`
responds with a bare ID while `crm.deal.update` responds with a boolean is
written there, not here.

### 2. Call the Method by Its Name

```go
res, err := client.Core().Call(ctx, "crm.deal.get",
    b24.Params{"id": 1}, b24.WithIdempotent())
```

### 3. Look at What the Portal Actually Sent

This step is absent from other SDKs, and it is exactly what replaces IDE hints: the answer about the
response shape comes from the portal itself, not from the editor. The SDK provides tools for this:

```go
// What shape is this at all: an object, an array, a scalar?
fmt.Println(b24.Result(res.Result).Kind())

// Which fields actually arrived — spelled the way they were sent.
if keys, ok := b24.Keys(res.Result); ok {
    fmt.Println(keys)
}
```

The step costs one run and saves an hour of guesswork. The portal renames fields
between the request and the response, returns an ID sometimes as a number and sometimes as a string, and sends one and
the same field as an object or as an array depending on the data — seeing
this with your own eyes is faster than deducing it from the documentation.

### 4. Declare a Struct for the Response

Exactly the fields you need: extra fields in the response do not get in the way.

```go
type deal struct {
    ID    b24.ID `json:"ID"`
    Title string `json:"TITLE"`
    Stage string `json:"STAGE_ID"`
}

var d deal
if err := json.Unmarshal(res.Result, &d); err != nil {
    return err
}
```

The `b24.ID` type instead of `int` is not over-caution: IDs arrive sometimes as a number
and sometimes as a quoted string, occasionally within a single scenario.

### If You Are Writing With an AI Agent

Step 1 is exactly where an AI agent goes wrong most often: it confidently invents
a method name that does not exist. So, along with the task, give the agent the
[`llms.txt`](https://github.com/bitrix24/b24gosdk/blob/main/llms.txt) file from the root of the
repository and the [documentation MCP server](../../ai-tools/mcp.md): the first explains
how the SDK is built, the second keeps it from inventing a method.

## Installation

```bash
go get github.com/bitrix24/b24gosdk
```

The package is named `b24gosdk`, but in the examples it is imported under the short name `b24`:

```go
import b24 "github.com/bitrix24/b24gosdk"
```

The SDK repository is available on GitHub: [bitrix24/b24gosdk](https://github.com/bitrix24/b24gosdk). The API reference is on [pkg.go.dev](https://pkg.go.dev/github.com/bitrix24/b24gosdk).

## How to Run an Example From the Documentation

The examples on this page and in the tutorials are complete programs: they can be
copied as a whole, built, and run. However, `go get` will not work in an empty directory, and
this differs from the habits of other ecosystems: `npm install` and
`composer require` create the project file themselves, while Go requires the module to
already exist.

The full sequence in an empty directory:

```bash
go mod init example
go get github.com/bitrix24/b24gosdk
```

Next, save the example code to `main.go`, put the webhook path into an environment
variable, and run it:

```bash
export B24_WEBHOOK_URL='https://your-portal.bitrix24.com/rest/1/token/'
go run .
```

{% note warning "" %}

The webhook path grants full access to the portal. Keep it in an environment variable:
code with a webhook inside must never be committed or shown in an issue.

{% endnote %}

The individual steps in the tutorials are **excerpts** from the full example at
the end of the page: they refer to `ctx`, `client`, and the variables of the previous
steps, so you have to run the full example and read the steps. The tabs of other languages are
arranged the same way.

## Usage With Inbound Webhooks

The path of an inbound webhook is a secret that grants full access to the portal. Read it from an environment variable and never write it into the code or into logs:

```go
package main

import (
    "context"
    "encoding/json"
    "log"
    "os"

    b24 "github.com/bitrix24/b24gosdk"
)

func main() {
    client := b24.NewClient(os.Getenv("B24_WEBHOOK_URL"))

    res, err := client.Core().Call(context.Background(), "crm.deal.add", b24.Params{
        "fields": b24.Params{
            "TITLE":    "New deal",
            "TYPE_ID":  "SALE",
            "STAGE_ID": "NEW",
        },
    })
    if err != nil {
        log.Fatal(err)
    }

    var dealID b24.ID
    if err := json.Unmarshal(res.Result, &dealID); err != nil {
        log.Fatal(err)
    }
    log.Println("deal created", dealID)
}
```

A single `Client` object is created per portal and reused: it holds the HTTP client and the token state. Do not create it for every call.

`b24.Params` is a short name for `map[string]any`. It is declared as an alias, so both spellings are interchangeable and can be mixed within a single literal.

## Calling Methods

A REST method is called by its name. `Call` returns the whole response envelope, `CallJSON` returns only the `result` field, and `CallMultipart` uploads files:

```go
res, err := client.Core().Call(ctx, "crm.deal.list", params)
// res.Result — the data, res.Total — how many records there are in total,
// res.Next — the offset of the next page (nil once the data has run out)

raw, err := client.Core().CallJSON(ctx, "crm.deal.get", b24.Params{"id": 1})
```

None of these calls returns your struct: the response arrives as `json.RawMessage`, and you unmarshal it yourself. The SDK neither validates nor enumerates method names and their parameters — take them from the [REST API reference](../../api-reference/index.md).

## Parsing the Response

Four situations occur in Bitrix24 responses often enough for the SDK to help with them.

**IDs arrive sometimes as a number and sometimes as a string** — occasionally within a single scenario: `disk.*` returns `"ID": 6687`, while `tasks.*` returns `"id": "3711"`. A field of the `b24.ID` type parses both variants and serializes back as a number:

```go
var task struct {
    ID    b24.ID `json:"id"`
    Title string `json:"title"`
}
err := json.Unmarshal(res.Result, &task)
```

**Many methods wrap the response in an object with a single key** — `tasks.*` responds with an object with the `task` key, `catalog.*` with the `products` key. `Unwrap` removes such a wrapper without declaring a struct for the sake of one field:

```go
raw, ok := b24.Unwrap(res.Result, "task")
```

Keys are compared exactly. If the portal renamed a field between the request and the response — `UF_TASK_WEBDAV_FILES` goes into `select` while `ufTaskWebdavFiles` arrives in the response — use `UnwrapFold`, which ignores case and underscores, or `Keys` to see what actually arrived.

**An empty field** arrives as `null`, an empty string, `false`, an empty array, or an empty object — depending on the method and the field type. `b24.IsEmpty` covers all five variants; a numeric zero is not considered empty.

**One and the same field arrives in different shapes**, and which one depends on the data rather than on the method: a product property with a single value arrives as an object, and with several values as an array of such objects. `Result.Kind` tells you what exactly arrived, so that you can choose the decoding target:

```go
switch b24.Result(raw).Kind() {
case b24.KindArray:
    err = json.Unmarshal(raw, &values)
case b24.KindObject:
    var one value
    err = json.Unmarshal(raw, &one)
    values = []value{one}
case b24.KindNull:
    values = nil
}
```

## Traversing Lists

`Pages` walks the server cursor: it sends `start` and repeats `next` for as long as the server returns it.

```go
p, err := client.Core().Pages("crm.deal.list", b24.Params{
    "select": []any{"ID", "TITLE"},
})
if err != nil {
    return err
}
for p.Next(ctx) {
    for _, row := range p.Rows() {
        var d Deal
        if err := json.Unmarshal(row, &d); err != nil {
            return err
        }
    }
}
return p.Err()
```

Always check `Err` after the loop. `Next` returns `false` both at the end of the list and on an error, so a traversal that broke off looks like one that finished, and only `Err` tells them apart.

For large exports, use `Scan`. Page-by-page traversal makes the server count off every skipped row, so the last pages of a long list get slower and slower. `Scan` walks by ID — it sorts by the ID and filters by the last one seen — so every page costs the same:

```go
p, err := client.Core().Scan("crm.deal.list", nil)
```

The SDK knows the families with a non-standard response shape: `crm.item.*` returns rows inside the `items` key and a lowercase `id`, `tasks.task.*` returns `id` but sorts by `ID`, and `user` and `department` accept a top-level `SORT` and `ORDER`. For the remaining cases there are `WithRowPath`, `WithIDField`, and `WithCursorParam`.

If a method ignores the cursor, the traversal does not loop: it stops with the `ErrCursorStalled` error instead of requesting the same page over and over at the expense of the portal limits.

`WithDescending` returns the freshest records first. Do not reverse `Scan` by hand: it pages by ID, and both the sorting and the filter boundary have to be reversed — otherwise the traversal gets an empty second page and reports a full export that contains only the first page.

```go
p, err := client.Core().Scan("crm.deal.list", nil,
    b24.WithDescending(),
    b24.WithCallOptions(b24.WithTimeout(30*time.Second)),
)
```

`WithCallOptions` passes settings into every request of the traversal. This is the only way to limit a single page: the traversal takes one context for the whole loop, so otherwise the choice is a deadline for the entire export or no limit at all on a stalled page.

## Batch Requests

A batch spends one limit token instead of one per command, so fifty creating calls are a single request rather than fifty.

```go
b := b24.NewBatch()
idUser, _ := b.Add("user.current", nil)
b.AddAs("deals", "crm.deal.list", b24.Params{"filter": b24.Params{">ID": 5}})

res, err := client.Core().CallBatch(ctx, b)
raw, err := res.Get(idUser)
```

The commands are executed in the order they were added, whatever their names are. `Get` returns the raw result of a command. The service `next` and `total` values do not get inside it: the server moves them into separate sections of the response, and the SDK puts them into `res.Next` and `res.Total` keyed by the command ID. The sign of "there is another page" is the presence of the key rather than its value: a `next` with the value zero is a legitimate first offset.

The result of one command is substituted into the parameters of the next one through `Ref`:

```go
b := b24.NewBatch()
b.Halt = true

b.AddAs("c", "crm.contact.add", b24.Params{
    "fields": b24.Params{"NAME": "Anna"},
})
ref, err := b24.Ref("c")
if err != nil {
    return err
}
b.AddAs("note", "crm.timeline.comment.add", b24.Params{
    "fields": b24.Params{"ENTITY_ID": ref, "ENTITY_TYPE": "contact"},
})
```

The `Halt` field is mandatory for a chain. If the source command fails, its substitution does not become an error — the server substitutes the unresolved text as an ordinary value, and the next command runs with a corrupted parameter. `Halt` stops the batch instead.

`CallBatch` does not split the batch: if there are more than fifty commands, it returns an error. For an unlimited set of independent commands, use `CallBatchChunked` — it splits the set and joins the results. It is not suitable for chains: a `$result` substitution does not survive a part boundary.

A batch error is partial. The server responds with HTTP 200 and puts the failures of individual commands into the error section, so the result is returned together with the error and must not be discarded: repeating the whole batch would re-execute the commands that had already been committed.

```go
res, err := client.Core().CallBatch(ctx, b)

var be *b24.BatchError
if errors.As(err, &be) {
    // be.Failed — what failed, res — everything that ran.
    // Assemble a new batch from be.Failed and repeat only that.
}
```

`res.Executed` tells "the command ran and failed" apart from "the command did not run, because the batch stopped earlier".

## Phone Numbers and Emails

CRM stores them as a list of rows, and the set of keys in a row decides what happens. Rows that you did not mention stay as they were, so a deletion has to be explicit:

```go
"PHONE": []map[string]any{
    b24.MultifieldAdd("+49 30 1234567", "MOBILE"),
    b24.MultifieldSet(rowID, "+49 30 7654321"),
    b24.MultifieldDelete(rowID),
}
```

A row without an ID always adds: an existing phone number sent again without its ID creates a duplicate rather than updating the record.

## Files

Files travel as base64 inside the request parameters, and no separate helper is needed for this:

```go
"fileContent": []string{name, base64.StdEncoding.EncodeToString(content)},
```

This is a pair of the file name and its content. For large files, call `disk.folder.uploadfile` without `fileContent`, retrieve the upload address, and send the bytes there.

## Errors

An error reported by the REST API is returned by the SDK as `*b24.APIError` with the `Code`, `Description`, and `HTTPStatus` fields. Compare the code with `errors.Is` rather than by string equality: a typo in a literal compiles, runs, and silently takes execution down the wrong branch.

```go
if errors.Is(err, b24.ErrMethodNotFound) {
    // the method does not exist or is not available on this portal
}
if errors.Is(err, b24.Code("CREATE_DYNAMIC_TYPE_RESTRICTED")) {
    // any code that has no named constant
}
```

The comparison is case-insensitive — the portal sends some codes in uppercase and others in lowercase — and goes by the code alone. A check by HTTP status would confuse different situations: `OVERLOAD_LIMIT` and `QUERY_LIMIT_EXCEEDED` both arrive with the code 503, but the first one means a manual block and the second one means the request rate was exceeded.

There are named constants for the frequent codes: `ErrQueryLimitExceeded`, `ErrOperationTimeLimit`, `ErrExpiredToken`, `ErrInvalidToken`, `ErrInvalidGrant`, `ErrInsufficientScope`, `ErrMethodNotFound`, `ErrAccessDenied`, `ErrPaymentRequired`. The set is deliberately open: the portal releases new codes without warning, and `b24.Code` covers any of them.

### Retries

The SDK decides on a retry by the question "did the request run", not "is the error temporary".

- `QUERY_LIMIT_EXCEEDED` is a refusal before execution, so such a call is always retried: a retry duplicates nothing.
- A network failure, a timeout, an unreadable response body, or a 5xx error without a code are ambiguous — the request may have run. By default they are not retried, because repeating `crm.deal.add` would create a second deal.
- If a call is safe to repeat, say so explicitly:

```go
res, err := client.Core().Call(ctx, "crm.deal.get",
    b24.Params{"id": 42}, b24.WithIdempotent())
```

Pass `WithIdempotent` for reads and for writes of fixed values. Do not pass it for creation methods and for updates where the new value is calculated from the old one.

List traversal retries pages on its own: reading is safe to repeat, and without this a single connection drop on the fortieth page would abort the whole traversal. A batch is never retried: it may contain commands that have already been committed.

## Applications and the OAuth Protocol

An access token lives for about an hour. To have the SDK renew it on its own, pass `WithTokenRefresher`. The ready-made implementation is `oauth.NewRefresher`: it retains the current refresh token, because the authorization server issues a new pair on every renewal, and it hands the new pair to a function where it has to be retained:

```go
import (
    b24 "github.com/bitrix24/b24gosdk"
    "github.com/bitrix24/b24gosdk/oauth"
)

oauthClient := oauth.NewClient(clientID, clientSecret)
url := oauthClient.AuthorizeURL("example.bitrix24.com", state, redirectURI)

resp, err := oauthClient.ExchangeCode(ctx, code)

refresher := oauth.NewRefresher(oauthClient, resp.RefreshToken,
    func(t oauth.TokenResponse) {
        storeTokens(t.AccessToken, t.RefreshToken)
    })

client := b24.NewOAuthClient(resp.ClientEndpoint, resp.AccessToken,
    b24.WithTokenRefresher(refresher.Refresh))
```

Renewal does not run on a timer: the authorization server is called only when the token has actually expired. Concurrent requests share a single renewal, and a failed renewal is not repeated for the same token.

The data that Bitrix24 passes to an application is parsed by dedicated functions. The installation event arrives in the form format rather than as JSON:

```go
ev, err := b24.ParseOnAppInstallRequest(r)
// ev.Auth contains AccessToken, RefreshToken, ExpiresIn, Scope,
// MemberID, and ApplicationToken — retain them, this is the only moment
// when they arrive

ok := ev.Auth.VerifyApplicationToken(stored)
```

`VerifyApplicationToken` compares the values in constant time. Verify the token before doing anything with the event data.

The POST data of the application page is parsed by `ParseAppRequest`, and a client is created from it by a single function:

```go
req, err := b24.ParseAppRequest(r)
client, err := b24.NewClientFromAppRequest(req)
```

## Testing an Integration Without a Portal

The `b24test` package provides a fake portal and response fixtures, so that the integration code can be checked without the network and without a real portal:

```go
p := b24test.NewPortal(t)
p.On("crm.deal.get", b24test.Result(map[string]any{"ID": "42", "TITLE": "Deal"}))
p.OnError("crm.deal.add", "QUERY_LIMIT_EXCEEDED", "too frequent")

client := p.Client()
// ... run the code you are testing, then check what it sent:
p.CallsTo("crm.deal.get")[0].Params["id"]
```

Do not write stubs for the SDK types by hand. In an integration with Bitrix24 it is the wiring that breaks: an ID that arrived as a string; a list hidden under a key; a limit error with the HTTP status 503; a batch where one command failed inside a 200 response. A stub reproduces your assumptions, while `b24test` reproduces what the bytes from the portal actually look like.

The values in the fixtures are made up, taken from the documentation examples. Fixtures end up in the repository, and a real token in them would mean leaked access.

## Development With an AI Agent

The root of the repository contains the [`llms.txt`](https://github.com/bitrix24/b24gosdk/blob/main/llms.txt) file — an entry point for an AI agent that writes an integration. It is written for an agent rather than for a human, and it contains what an agent most often gets wrong: REST method names are not stored in the SDK and must not be invented, seven peculiarities of this particular SDK, and the traps that cost data rather than an error.

Pass this file to the agent along with the task.
