# Add Page by Template landing.landing.addByTemplate

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with "edit" access permission for the site

The method `landing.landing.addByTemplate` creates a page on a site based on the template code and returns its identifier. The new page is created unpublished (`ACTIVE = "N"`). The exception is when a published page is passed in `fields.ID`: then the new page is published immediately. You can publish the page later with the [landing.landing.publication](./landing-landing-publication.md) method.

If you need to fully manage the fields of the created page, use [landing.landing.add](./landing-landing-add.md).

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **scope**
[`string`](../../../data-types.md) | Internal landing parameter: the type of sites the method works with. It is not related to the REST scope `landing`.

For Knowledge Base pages, pass `KNOWLEDGE`; for group knowledge base pages, pass `GROUP`. The value is case-insensitive. Without `scope`, the method cannot find a Knowledge Base site and returns the `SITE_ERROR` error. For sites of the `PAGE`, `STORE`, `SMN`, and `VIBE` types, the parameter is not needed [(detailed description)](../../types.md) ||
|| **siteId***
[`integer`](../../../data-types.md) | Identifier of the site where the page needs to be created.

The site identifier can be obtained using the [landing.site.getList](../../site/landing-site-get-list.md) method or from the result of the [landing.site.add](../../site/landing-site-add.md) method ||
|| **code***
[`string`](../../../data-types.md) | Template code for the page, for example `krayt.monotovar@KraytPetShop`. The [landing.demos.getPageList](../../demos/landing-demos-get-page-list.md) method returns the list of templates available in Bitrix24.

If there is no template with this code, the page is not created, and `result` contains `null` without an error ||
|| **fields**
[`object`](../../../data-types.md) | Additional parameters for creating the page [(detailed description)](#fields) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TITLE**
[`string`](../../../data-types.md) | Overrides the title of the created page and the SEO titles of the template.

If not provided, values from the template will be used ||
|| **DESCRIPTION**
[`string`](../../../data-types.md) | Overrides the SEO descriptions of the template.

The value is recorded in the additional fields `METAOG_DESCRIPTION` and `METAMAIN_DESCRIPTION`, but not in the `DESCRIPTION` field of the page itself ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of an existing page in the same site. The new page takes the folder and the publication state from it.

If the page is in a folder, the new page will be created in the same folder, and the `fields.FOLDER_ID` parameter is ignored. For a page without a folder, the method will take the value from `fields.FOLDER_ID`, if this parameter is specified.

If a published page is provided, the new page will be published automatically after creation.

The page identifier can be obtained using the [landing.landing.getList](./landing-landing-get-list.md) method.

A folder identifier from [landing.site.getFolders](../../site/landing-site-get-folders.md) does not work here: the method looks for a page in `fields.ID` and usually returns the `LANDING_ERROR` error. Pass the folder in `fields.FOLDER_ID` ||
|| **FOLDER_ID**
[`integer`](../../../data-types.md) | Identifier of the folder where the page needs to be created. The folder must be on the site specified by `siteId`, otherwise the method will return the `FOLDER_NOT_FOUND` error.

The folder identifier can be obtained using the [landing.site.getFolders](../../site/landing-site-get-folders.md) method.

If the page specified by `fields.ID` is in a folder, the parameter is ignored ||
|| **SITE_TYPE**
[`string`](../../../data-types.md) | Type of templates from which the page is created. Site types from the article [Working with Site Types and Scopes](../../types.md) are used.

If not provided, the site type is taken. For sites of types `STORE` and `SMN`, the default value is `PAGE` ||
|| **ADD_IN_MENU**
[`string`](../../../data-types.md) | Whether to add the created page to the site menu: `Y` to add, `N` not to add. The default is `N`.

The page is added to the menu only if `TITLE` is provided and the page is not being added to a block menu through `BLOCK_ID` and `MENU_CODE` ||
|| **BLOCK_ID**
[`integer`](../../../data-types.md) | Together with `MENU_CODE`, adds a link to the created page in the menu of the block with the specified identifier ||
|| **MENU_CODE**
[`string`](../../../data-types.md) | Together with `BLOCK_ID`, specifies the menu code within the block where the link to the created page should be added ||
|#

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 157,
        "code": "krayt.monotovar@KraytPetShop",
        "fields": {
          "TITLE": "Spring Promotion",
          "DESCRIPTION": "SEO description of the promotion page",
          "FOLDER_ID": 95
        }
      }' \
      "https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/landing.landing.addByTemplate.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "siteId": 157,
        "code": "krayt.monotovar@KraytPetShop",
        "fields": {
          "TITLE": "Spring Promotion",
          "DESCRIPTION": "SEO description of the promotion page",
          "FOLDER_ID": 95
        },
        "auth": "**put_access_token_here**"
      }' \
      "https://**put_your_bitrix24_address**/rest/landing.landing.addByTemplate.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number | null>({
        method: 'landing.landing.addByTemplate',
        params: {
          siteId: 157,
          code: 'krayt.monotovar@KraytPetShop',
          fields: {
            TITLE: 'Spring Promotion',
            DESCRIPTION: 'SEO description of the promotion page',
            FOLDER_ID: 95,
          },
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Created page ID:', result)
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
      async function addLandingByTemplate() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.landing.addByTemplate',
            params: {
              siteId: 157,
              code: 'krayt.monotovar@KraytPetShop',
              fields: {
                TITLE: 'Spring Promotion',
                DESCRIPTION: 'SEO description of the promotion page',
                FOLDER_ID: 95,
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
          console.info('Created page ID:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addLandingByTemplate)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.landing.landing.add_by_template(
            site_id=157,
            code="krayt.monotovar@KraytPetShop",
            fields={
                "TITLE": "Spring Promotion",
                "DESCRIPTION": "SEO description of the promotion page",
                "FOLDER_ID": 95,
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
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.landing.addByTemplate',
                [
                    'siteId' => 157,
                    'code' => 'krayt.monotovar@KraytPetShop',
                    'fields' => [
                        'TITLE' => 'Spring Promotion',
                        'DESCRIPTION' => 'SEO description of the promotion page',
                        'FOLDER_ID' => 95,
                    ],
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding landing page by template: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.landing.addByTemplate',
        {
            siteId: 157,
            code: 'krayt.monotovar@KraytPetShop',
            fields: {
                TITLE: 'Spring Promotion',
                DESCRIPTION: 'SEO description of the promotion page',
                FOLDER_ID: 95
            }
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.landing.addByTemplate',
        [
            'siteId' => 157,
            'code' => 'krayt.monotovar@KraytPetShop',
            'fields' => [
                'TITLE' => 'Spring Promotion',
                'DESCRIPTION' => 'SEO description of the promotion page',
                'FOLDER_ID' => 95,
            ],
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "landing.landing.addByTemplate", b24.Params{
    	"siteId": 157,
    	"code":   "krayt.monotovar@KraytPetShop",
    	"fields": b24.Params{
    		"TITLE":       "Spring Promotion",
    		"DESCRIPTION": "SEO description of the promotion page",
    		"FOLDER_ID":   95,
    	},
    })
    if err != nil {
    	return fmt.Errorf("landing.landing.addByTemplate: %w", err)
    }

    var value b24.ID
    if err := json.Unmarshal(res.Result, &value); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("result:", value)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 2229,
    "time": {
        "start": 1773700566,
        "finish": 1773700567.67178,
        "duration": 1.6717801094055176,
        "processing": 1,
        "date_start": "2026-03-17T01:36:06+01:00",
        "date_finish": "2026-03-17T01:36:07+01:00",
        "operating_reset_at": 1773701166,
        "operating": 1.4516642093658447
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) \| `null` | Identifier of the created page. If there is no template with the code from `code`, the method returns `null` ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "Site was not found or access was denied",
    "error_description": "SITE_ERROR"
}
```

For the errors in the table, the code is returned in `error_description`, and the text in `error`, as in the example above. The exception is `MISSING_PARAMS`: its code is returned in `error`, as with system errors such as `TYPE_ERROR` and with other methods.

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `MISSING_PARAMS` | Required parameters are missing: `siteId`, `code`, or both parameters are absent in the request ||
|| `SITE_ERROR` | Site was not found or access was denied: the required `scope` is not passed, a non-existent site is provided in `siteId`, or the user does not have access to it ||
|| `LANDING_ERROR` | Page not found: `fields.ID` contains an identifier that does not match any page on the site specified by `siteId`. This happens if you pass a folder identifier from [landing.site.getFolders](../../site/landing-site-get-folders.md) ||
|| `ACCESS_DENIED` | Access to create the page is denied: the user does not have "edit" permission for the specified site ||
|| `FOLDER_NOT_FOUND` | Folder not found: a folder that does not belong to the specified site or does not exist is provided in `fields.FOLDER_ID` ||
|| `WRONG_DATA` | Invalid template data: unable to retrieve template data for the provided `code` ||
|| `SLASH_IS_NOT_ALLOWED` | Slash is not allowed in the landing address: the template contains an invalid address for the created page ||
|| `CANT_BE_EMPTY` | The page address cannot be empty: the template contains an empty address for the created page ||
|| `WRONG_CODE_FORMAT` | Invalid page address: the template contains an address in the format `<characters>_<number>_<number>`, for example `code_12_34` ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [landing.demos.getPageList](../../demos/landing-demos-get-page-list.md)
- [landing.site.getList](../../site/landing-site-get-list.md)
- [landing.site.getFolders](../../site/landing-site-get-folders.md)
- [{#T}](./landing-landing-add.md)
- [{#T}](./landing-landing-copy.md)
- [{#T}](./landing-landing-delete.md)
- [{#T}](./landing-landing-get-additional-fields.md)
- [{#T}](./landing-landing-get-list.md)
- [{#T}](./landing-landing-update.md)