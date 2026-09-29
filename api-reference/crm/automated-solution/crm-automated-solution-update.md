# Update Digital Workplace crm.automatedsolution.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: a Bitrix24 administrator or a user with the "Edit automation solutions" or "Edit settings" permission for "Automated solutions" in CRM access permissions

The method `crm.automatedsolution.update` modifies the digital workplace with the identifier `id`. Fields that are not passed retain their previous values.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../data-types.md) | Identifier of the digital workplace. You can retrieve it from the response of the [crm.automatedsolution.add](./crm-automated-solution-add.md) method (`result.automatedSolution.id`) or [crm.automatedsolution.list](./crm-automated-solution-list.md). In Bitrix24, the identifier is shown in the `ID` column of the digital workplace list ||
|| **fields***
[`object`](../../data-types.md) | Field values [(detailed description)](#fields) for modifying a digital workplace in the following structure:

```js
"fields": {
    "title": "value",
    "typeIds": []
}
```
||
|#

### Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **title**
[`string`](../../data-types.md) | Title of the digital workplace.

The link to the digital workplace is built from the title, so changing `title` also changes the link. If `title` is passed, it cannot be empty ||
|| **typeIds**
[`crm_dynamic_type.id[]`](../data-types.md) | Array of SPA type identifiers (`id`) to link to this workplace. Non-existent identifiers are ignored without an error.

{% note warning %}

The `typeIds` list is overwritten entirely: pass the full set of required SPAs. If `typeIds` is not passed, the links do not change. Unlinked SPAs return to CRM. To unlink an SPA or move it from CRM, the "User can edit preferences" CRM permission is also required

{% endnote %}

 ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

1. Change the title of the digital workplace

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"title":"HR & Customer Success"}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"title":"HR & Customer Success"},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.update
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type AutomatedSolutionUpdateResult = {
          automatedSolution: {
            id: number
            title: string
            typeIds: number[]
          }
        }

        try {
          const response = await $b24.actions.v2.call.make<AutomatedSolutionUpdateResult>({
            method: 'crm.automatedsolution.update',
            params: {
              id: 238,
              fields: {
                title: 'HR & Customer Success',
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info(result.automatedSolution.id, result.automatedSolution.title, result.automatedSolution.typeIds)
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
          async function updateAutomatedSolution() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.automatedsolution.update',
                params: {
                  id: 238,
                  fields: {
                    title: 'HR & Customer Success',
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
              console.info(result.automatedSolution.id, result.automatedSolution.title, result.automatedSolution.typeIds)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', updateAutomatedSolution)
        </script>
        ```

    - Python

        Example

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.automatedsolution.update(
                bitrix_id=238,
                fields={
                    "title": "HR & Customer Success",
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
            'crm.automatedsolution.update',
            [
                'id' => 238,
                'fields' =>
                [
                    'title' => 'HR & Customer Success'
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
        res, err := client.Core().Call(ctx, "crm.automatedsolution.update", b24.Params{
        	"id": 238,
        	"fields": b24.Params{
        		"title": "HR & Customer Success",
        	},
        })
        if err != nil {
        	return fmt.Errorf("crm.automatedsolution.update: %w", err)
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

2. Change the list of linked SPAs

    Suppose the SPAs with `id` = `14` and `158` are linked to the digital workplace with `id` = `238`. To keep only one of them, pass only the required SPAs in `typeIds`:

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"typeIds":[14]}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"typeIds":[14]},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.update
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type AutomatedSolutionUpdateResult = {
          automatedSolution: {
            id: number
            title: string
            typeIds: number[]
          }
        }

        try {
          const response = await $b24.actions.v2.call.make<AutomatedSolutionUpdateResult>({
            method: 'crm.automatedsolution.update',
            params: {
              id: 238,
              fields: {
                typeIds: [14],
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info(result.automatedSolution.id, result.automatedSolution.title, result.automatedSolution.typeIds)
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
          async function updateAutomatedSolution() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.automatedsolution.update',
                params: {
                  id: 238,
                  fields: {
                    typeIds: [14],
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
              console.info(result.automatedSolution.id, result.automatedSolution.title, result.automatedSolution.typeIds)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', updateAutomatedSolution)
        </script>
        ```

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.automatedsolution.update(
                bitrix_id=238,
                fields={
                    "typeIds": [
                        14,
                    ],
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
            'crm.automatedsolution.update',
            [
                'id' => 238,
                'fields' =>
                [
                    'typeIds' => [14]
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
        res, err := client.Core().Call(ctx, "crm.automatedsolution.update", b24.Params{
        	"id": 238,
        	"fields": b24.Params{
        		"typeIds": []int{14},
        	},
        })
        if err != nil {
        	return fmt.Errorf("crm.automatedsolution.update: %w", err)
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

3. Unlink all SPAs

    To unlink all SPAs from the digital workplace, pass an empty array in `typeIds`. The SPAs return to CRM.

    {% list tabs %}

    - cURL (Webhook)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"typeIds":[]}}' \
        https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.automatedsolution.update
        ```

    - cURL (OAuth)

        ```bash
        curl -X POST \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"id":238,"fields":{"typeIds":[]},"auth":"**put_access_token_here**"}' \
        https://**put_your_bitrix24_address**/rest/crm.automatedsolution.update
        ```

    - JS (TS)

        ```ts
        // This snippet is an ES module: top-level await requires type="module" or a bundler.
        // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
        import { Text } from '@bitrix24/b24jssdk'
        import type { B24Frame } from '@bitrix24/b24jssdk'

        declare const $b24: B24Frame

        // Shape of the payload returned in result (match the "response handling" section of the page)
        type AutomatedSolutionUpdateResult = {
          automatedSolution: {
            id: number
            title: string
            typeIds: number[]
          }
        }

        try {
          const response = await $b24.actions.v2.call.make<AutomatedSolutionUpdateResult>({
            method: 'crm.automatedsolution.update',
            params: {
              id: 238,
              fields: {
                typeIds: [],
              },
            },
            requestId: Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
          } else {
            const result = response.getData()!.result
            console.info(result.automatedSolution.id, result.automatedSolution.title, result.automatedSolution.typeIds)
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
          async function updateAutomatedSolution() {
            try {
              // Initialize the SDK inside a Bitrix24 frame
              const $b24 = await B24Js.initializeB24Frame()

              const response = await $b24.actions.v2.call.make({
                method: 'crm.automatedsolution.update',
                params: {
                  id: 238,
                  fields: {
                    typeIds: [],
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
              console.info(result.automatedSolution.id, result.automatedSolution.title, result.automatedSolution.typeIds)
            } catch (error) {
              // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
              console.error(error)
            }
          }

          document.addEventListener('DOMContentLoaded', updateAutomatedSolution)
        </script>
        ```

    - Python

        ```python

        from b24pysdk.errors import BitrixAPIError, BitrixSDKException

        try:
            bitrix_response = client.crm.automatedsolution.update(
                bitrix_id=238,
                fields={
                    "typeIds": [],
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
            'crm.automatedsolution.update',
            [
                'id' => 238,
                'fields' =>
                [
                    // CRest does not send an empty array, so pass an empty string
                    'typeIds' => ''
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
        res, err := client.Core().Call(ctx, "crm.automatedsolution.update", b24.Params{
        	"id": 238,
        	"fields": b24.Params{
        		"typeIds": []any{},
        	},
        })
        if err != nil {
        	return fmt.Errorf("crm.automatedsolution.update: %w", err)
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
            "id": 238,
            "title": "HR & Customer Success",
            "typeIds": [
                14
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
[`object`](../../data-types.md) | The digital workplace after the update. The object is nested in `result` [(detailed description)](#automatedSolution) ||
|| **time**
[`time`](../../data-types.md) | Information about the request execution time ||
|#

#### Object automatedSolution {#automatedSolution}

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../data-types.md) | Identifier of the digital workplace ||
|| **title**
[`string`](../../data-types.md) | Title of the digital workplace ||
|| **typeIds**
[`crm_dynamic_type.id[]`](../data-types.md) | Identifiers of the SPAs linked to the workplace. If no SPAs are linked, an empty array is returned ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error":"BX_EMPTY_REQUIRED",
    "error_description":"The field Name is required."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions ||
|| `BX_EMPTY_REQUIRED` | An empty `title` value was passed ||
|| `NOT_FOUND` | A digital workplace with this `id` was not found ||
|| `RESTRICTED_BY_TARIFF` | The workplace was installed from the Automated solution gallery and is locked: your current plan does not allow working with it ||
|| `0` | SPAs cannot be moved from workplaces installed from the Automated solution gallery ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./crm-automated-solution-add.md)
- [{#T}](./crm-automated-solution-get.md)
- [{#T}](./crm-automated-solution-list.md)
- [{#T}](./crm-automated-solution-delete.md)
- [{#T}](./crm-automated-solution-fields.md)