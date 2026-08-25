# Get the List of Available Call Card Interface States CallCardGetListUiStates

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — registration of the call that raises the card
>
> Who can execute the command: any user

The `CallCardGetListUiStates` command returns the list of available interface states of the call card.

{% note info "" %}

The command operates within the application context in the `PAGE_BACKGROUND_WORKER` placement. This is a JS interface command, not a REST method: it cannot be invoked with a request to `/rest/`.

{% endnote %}

## How to Call the Command

The command is invoked from the widget with the [BX24.placement.call](../bx24-placement-call.md) method. The third argument is the callback function, which receives the result of the command.

```js
BX24.placement.call('CallCardGetListUiStates', {}, function (result) {
    console.log(result);
});
```

## Command Parameters

The command takes no parameters. Pass an empty object `{}` as the second argument.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% note info "" %}

It is recommended to call the command after the [BackgroundCallCard::initialized](./events/initialized.md) event

{% endnote %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.call('CallCardGetListUiStates', {}, function (result) {
                console.log(result);
            });
        });
    });
    ```

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide)
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    const uiStates = await $b24.placement.call('CallCardGetListUiStates') as string[]

    console.log(uiStates)
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        const result = await $b24.placement.call('CallCardGetListUiStates')

        console.log(result)
      })
    </script>
    ```


- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.placement.call(
            placement='CallCardGetListUiStates',
            params={},
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
{% endlist %}

## Command Result

```json
[
    "incoming",
    "transferIncoming",
    "outgoing",
    "connectingIncoming",
    "connectingOutgoing",
    "connected",
    "transferring",
    "transferFailed",
    "transferConnected",
    "error",
    "moneyError",
    "redial"
]
```

### Returned Data

The root element of the response is an array of strings with the available interface states of the card.

Possible values:

- `incoming` — incoming call
- `transferIncoming` — incoming call transfer
- `outgoing` — outgoing call
- `connectingIncoming` — connecting an incoming call
- `connectingOutgoing` — connecting an outgoing call
- `connected` — connection established
- `transferring` — the call is being transferred
- `transferFailed` — call transfer error
- `transferConnected` — the call transfer was connected successfully
- `error` — call error
- `moneyError` — an error caused by insufficient funds
- `redial` — redial

## Errors

The `CallCardGetListUiStates` command has no error codes of its own: it either runs or is not invoked at all.

- If the widget is open outside the `PAGE_BACKGROUND_WORKER` placement, the placement interface ignores the unknown command and the callback function is not invoked
- Check the command name with the correct capitalization: the list of commands available in the current placement is returned by [BX24.placement.getInterface](../bx24-placement-get-interface.md)

## Continue Learning

- [{#T}](./call-card-set-ui-state.md)
- [{#T}](./call-card-set-mute.md)
- [{#T}](./call-card-set-hold.md)
- [{#T}](./call-card-set-card-title.md)
- [{#T}](./call-card-set-status-text.md)
- [{#T}](./call-card-close.md)
- [{#T}](./events/index.md)
- [{#T}](./index.md)
