# Create Digital Workspace crm.automatedsolution.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a Bitrix24 administrator or a user with the "Edit automation solutions" or "Edit settings" permission for "Automated solutions" in CRM access permissions

The method `crm.automatedsolution.add` creates a digital workspace.

In cloud Bitrix24, the maximum number of digital workspaces depends on the plan. In the on-premise version, the limit is set by the `automated_solution_limit` option of the `crm` module, 200 by default. The limit counts all workspaces, including those installed from the Automated solution gallery.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values [(detailed description)](#fields) for creating a digital workspace in the form of a structure:

```js
"fields": {
    "title": "value",
    "typeIds": []
}
```
 ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **title***
[`string`](../../data-types.md) | The name of the digital workspace. The link to the workspace section in Bitrix24 is built from the name ||
|| **typeIds**
[`crm_dynamic_type.id[]`](../data-types.md) | An array of SPA type identifiers (`id`) to link to this workspace. If not passed, the workspace is created without SPAs. Non-existent identifiers are ignored without an error.

If an SPA is already linked to another workspace or to CRM, it is removed from its previous place when linked to the new workspace. To move an SPA from CRM, the "User can edit preferences" CRM permission is also required.

A digital workspace without SPAs is not displayed in the left menu, but it can be found in the list of digital workspaces ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

1. Create a digital workspace and immediately link SPAs to it

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"title":"HR","typeIds":[1,2,3]}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"title":"HR","typeIds":[1,2,3]},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.add
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type AutomatedSolutionAddResult = {
          automatedSolution: {
            id: number
            title: string
            typeIds: number[]
          }
        }

        try {
          const response = await $b24.actions.v2.call.make<AutomatedSolutionAddResult>({
            method: 'crm.automatedsolution.add',
            params: {
              fields: {
                title: 'HR',
                typeIds: [1, 2, 3],
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info(result.automatedSolution)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
        ```

    - JS (UMD)

        ```html
        <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function addAutomatedSolution() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.automatedsolution.add',
                params: {
                  fields: {
                    title: 'HR',
                    typeIds: [1, 2, 3],
                  },
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info(result.automatedSolution)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', addAutomatedSolution)
        </script>
        ```

    - Python

        Example

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.automatedsolution.add(
                fields={
                    "title": "HR",
                    "typeIds": [1, 2, 3],
                },
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
    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.automatedsolution.add',
            [
                'fields' =>
                [
                    'title' => 'HR',
                    'typeIds' => [1, 2, 3]
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```


    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "crm.automatedsolution.add", b24.Params{
        	"fields": b24.Params{
        		"title":   "HR",
        		"typeIds": []int{1, 2, 3},
        	},
        })
        if err != nil {
        	return fmt.Errorf("crm.automatedsolution.add: %w", err)
        }

        // The method wraps the response in an object with the "automatedSolution" key.
        raw, ok := b24.Unwrap(res.Result, "automatedSolution")
        if !ok {
        	return fmt.Errorf("no automatedSolution key in the response")
        }

        var item struct {
        	ID    b24.ID `json:"id"`
        	Title string `json:"title"`
        }
        if err := json.Unmarshal(raw, &item); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        fmt.Println(item.ID, item.Title)
        ```

    {% endlist %}

2. Create a digital workspace without SPAs

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"title":"HR"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.add
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"fields":{"title":"HR"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.add
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type AutomatedSolutionAddResult = {
          automatedSolution: {
            id: number
            title: string
            typeIds: number[]
          }
        }

        try {
          const response = await $b24.actions.v2.call.make<AutomatedSolutionAddResult>({
            method: 'crm.automatedsolution.add',
            params: {
              fields: {
                title: 'HR',
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info(result.automatedSolution)
          }
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
        ```

    - JS (UMD)

        ```html
        <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
        <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
        <script>
          async function addAutomatedSolution() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.automatedsolution.add',
                params: {
                  fields: {
                    title: 'HR',
                  },
                },
                requestId: B24Js.Text.getUuidRfc4122()
              })

              // The payload is available only on a successful response
              if (!response.isSuccess) {
                console.error(response.getErrorMessages().join('; '))
                return
              }

              const result = response.getData().result
              console.info(result.automatedSolution)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', addAutomatedSolution)
        </script>
        ```

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.automatedsolution.add(
                fields={
                    "title": "HR",
                },
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

    - PHP

        ```php
        require_once('crest.php');

        $result = CRest::call(
            'crm.automatedsolution.add',
            [
                'fields' =>
                [
                    'title' => 'HR'
                ]
            ]
        );

        echo '<PRE>';
        print_r($result);
        echo '</PRE>';
        ```

    - Go

        ```go
        // client and ctx are already created — see the Go SDK section
        res, err := client.Core().Call(ctx, "crm.automatedsolution.add", b24.Params{
        	"fields": b24.Params{
        		"title": "HR",
        	},
        })
        if err != nil {
        	return fmt.Errorf("crm.automatedsolution.add: %w", err)
        }

        // The method wraps the response in an object with the "automatedSolution" key.
        raw, ok := b24.Unwrap(res.Result, "automatedSolution")
        if !ok {
        	return fmt.Errorf("no automatedSolution key in the response")
        }

        var item struct {
        	ID    b24.ID `json:"id"`
        	Title string `json:"title"`
        }
        if err := json.Unmarshal(raw, &item); err != nil {
        	return fmt.Errorf("parse response: %w", err)
        }
        fmt.Println(item.ID, item.Title)
        ```

    {% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "automatedSolution": {
            "id": 1,
            "title": "HR",
            "typeIds": [
                1,
                2,
                3
            ]
        }
    },
    "time": {
        "start": 1715849396.642359,
        "finish": 1715849396.954623,
        "duration": 0.31226396560668945,
        "processing": 0.0068209171295166016,
        "date_start": "2024-05-16T11:49:56+03:00",
        "date_finish": "2024-05-16T11:49:56+03:00",
        "operating_reset_at": 1715849996,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **automatedSolution**
[`object`](../../data-types.md) | The created digital workspace. The object is nested in `result` [(detailed description)](#automatedSolution) ||
|| **time**
[`time`](../../data-types.md) | Information about the execution time of the request ||
|#

#### Object automatedSolution {#automatedSolution}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the created digital workspace. Pass it to the `id` parameter of the [crm.automatedsolution.update](./crm-automated-solution-update.md), [crm.automatedsolution.get](./crm-automated-solution-get.md), and [crm.automatedsolution.delete](./crm-automated-solution-delete.md) methods ||
|| **title**
[`string`](../../data-types.md) | Name of the digital workspace ||
|| **typeIds**
[`crm_dynamic_type.id[]`](../data-types.md) | Identifiers of the SPAs linked to the workspace. If `typeIds` was not passed on creation, an empty array is returned ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "BX_EMPTY_REQUIRED",
    "error_description":"The field Name is required."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `LIMIT_EXCEEDED` | The number of available digital workspaces has been exceeded ||
|| `BX_EMPTY_REQUIRED` | A required field is not filled ||
|| `0` | SPAs cannot be moved from workspaces installed from the Automated solution gallery ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-automated-solution-update.md)
- [{#T}](./crm-automated-solution-get.md)
- [{#T}](./crm-automated-solution-list.md)
- [{#T}](./crm-automated-solution-delete.md)
- [{#T}](./crm-automated-solution-fields.md)