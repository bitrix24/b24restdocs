# How to Prepare a Custom Template

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the methods: to complete the entire scenario, the user needs the strictest permission listed below — permission to export websites
>
> - [landing.site.fullExport](../site/landing-site-full-export.md) — a user with permission to export websites
> - [landing.demos.register](./landing-demos-register.md) and [landing.demos.getList](./landing-demos-get-list.md) — a user with View permission in the Sites section

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A custom template is a ready-made blueprint for a website or page that can be added to the site creation wizard. The wizard is the screen where a Bitrix24 user selects a design when creating a new website or page. Your own template appears in this list once an application registers it.

The template is created based on an existing website or page from the [Sites section](../site/index.md). First, the website is exported — its structure is unloaded into a data set that can be saved and passed on. This data set is then registered using the [`landing.demos.*` methods](./index.md) from the Bitrix24 application.

Verifiable result: the template is registered for the application, returned by `landing.demos.getList`, and displayed in the website or page creation wizard.

The scenario consists of three steps:

1. Export an existing website using [landing.site.fullExport](../site/landing-site-full-export.md)
2. Pass the export result to [landing.demos.register](./landing-demos-register.md)
3. Verify the registration using [landing.demos.getList](./landing-demos-get-list.md)

The call order matters: `landing.demos.register` accepts the `result` object returned by `landing.site.fullExport`, and `landing.demos.getList` verifies the registration result.

## When to Use a Custom Template

Use a custom template if you need to:

- add your own template to the website creation wizard
- distribute a ready-made website or page through the application
- reuse the same set of pages, blocks, and settings

If the template only needs to be installed in one Bitrix24 without reuse, it is sufficient to create and configure the website or page in the usual way.

{% note tip "" %}

If the template is distributed as an application with a website, refer to the articles [Installing Website Templates](../../../settings/app-installation/site-templates-installation.md) and [Website Requirements Before Publishing](../../../market/preparing-to-publish/requirements-sites.md).

{% endnote %}

## How a Custom Template Works

A template is made up of three interconnected parts:

- **the source website or page** — the basis for the template, prepared in the [Sites section](../site/index.md)
- **the export** — the website structure as a data set, created by the [landing.site.fullExport](../site/landing-site-full-export.md) method
- **the registered template** — an entry that appears in the wizard after calling [landing.demos.register](./landing-demos-register.md)

The section's methods handle the individual steps of working with a template:

- [landing.demos.register](./landing-demos-register.md) — registers the template in the website and page creation wizard
- [landing.demos.getList](./landing-demos-get-list.md) — returns the registered templates and lets you verify the registration result. If the method is called from an application, the response includes only that application's templates
- [landing.demos.getSiteList](./landing-demos-get-site-list.md) — returns the website templates available in the wizard for the selected site type. The list includes both built-in Bitrix24 templates and matching templates you have registered. For example, for the `store` type, the method returns online store templates
- [landing.demos.getPageList](./landing-demos-get-page-list.md) — the same, but for individual page templates
- [landing.demos.unregister](./landing-demos-unregister.md) — deletes a registered template

## How to Prepare the Template

Before exporting, check the website itself:

- pages are correctly linked to each other
- the necessary blocks and themes are used
- images and external resources are accessible via working URLs
- title, description, and preview data are prepared for the template. Preview data is the preview images used to identify the template in the wizard's list

If the website is multi-page, use a single theme for all pages. This helps maintain a consistent appearance after the template is installed.

## Prepare the Data

Before starting, prepare:

- the website identifier that will serve as the template source
- an external template code, for example `myfirstsite2026`
- the URL of a published page for `preview_url`
- an installed application with OAuth authorization and the `landing` permission
- an installed and initialized SDK: [B24JsSDK](../../../sdk/b24jssdk/index.md), [B24PhpSDK](../../../sdk/b24phpsdk/index.md), or [B24PySDK](../../../sdk/b24pysdk/index.md)

You can get the website identifier using [landing.site.getList](../site/landing-site-get-list.md) or from the result of [landing.site.add](../site/landing-site-add.md). The external code must contain only lowercase Latin letters and digits without separators.

In the examples, replace `326`, `myfirstsite2026`, and the preview URL with your values. Run the examples from the selected tab sequentially in the same script: the variable containing the export result is used in the next step.

{% note warning "" %}

An OAuth token grants access to Bitrix24. Store it in the application settings or environment variables, and do not add it to the source code.

{% endnote %}

## 1. Export the Website

Call [landing.site.fullExport](../site/landing-site-full-export.md). Pass the website identifier in the `id` parameter and the external template code in `params.code`. The method returns the complete website structure in the `result` field.

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    // $b24 is a previously initialized B24JsSDK instance
    const exportResponse = await $b24.actions.v2.call.make({
      method: 'landing.site.fullExport',
      params: {
        id: 326,
        params: {
          code: 'myfirstsite2026',
          name: 'Auto Repair Website',
          preview_url: 'https://example.com/previews/myfirstsite2026'
        }
      }
    })

    if (!exportResponse.isSuccess) {
      throw new Error(exportResponse.getErrorMessages().join('; '))
    }

    const exportData = exportResponse.getData().result
    ```

- PHP

    ```php
    // $b24Service is a previously initialized B24PhpSDK instance
    $response = $b24Service->core->call(
        'landing.site.fullExport',
        [
            'id' => 326,
            'params' => [
                'code' => 'myfirstsite2026',
                'name' => 'Auto Repair Website',
                'preview_url' => 'https://example.com/previews/myfirstsite2026',
            ],
        ]
    );

    $exportData = $response->getResponseData()->getResult();
    ```

- Python

    ```python
    # client is a previously initialized B24PySDK instance
    export_data = client.landing.site.full_export(
        bitrix_id=326,
        params={
            "code": "myfirstsite2026",
            "name": "Auto Repair Website",
            "preview_url": "https://example.com/previews/myfirstsite2026",
        },
    ).response.result
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": {
        "charset": "UTF-8",
        "code": "myfirstsite2026",
        "name": "Auto Repair Website",
        "type": "page",
        "version": 3,
        "items": {
            "myfirstsite2026": {
                "code": "myfirstsite2026",
                "name": "Auto Repair Website",
                "type": "page",
                "version": 3,
                "items": {}
            }
        }
    }
}
```

Save the entire `result` object, not individual fields. The `exportData`, `$exportData`, and `export_data` variables contain data for the next step.

## 2. Register the Template

Pass the saved export object in the `data` parameter of [landing.demos.register](./landing-demos-register.md). Do not rebuild the structure manually: it already contains the external `code`, the page map in `items`, fields, blocks, and website settings.

The examples continue the code from the first step.

{% list tabs %}

- JS

    ```js
    const registerResponse = await $b24.actions.v2.call.make({
      method: 'landing.demos.register',
      params: {
        data: exportData
      }
    })

    if (!registerResponse.isSuccess) {
      throw new Error(registerResponse.getErrorMessages().join('; '))
    }

    const registeredTemplateIds = registerResponse.getData().result
    if (registeredTemplateIds.length === 0) {
      throw new Error('Template was not registered')
    }
    ```

- PHP

    ```php
    $response = $b24Service->core->call(
        'landing.demos.register',
        [
            'data' => $exportData,
        ]
    );

    $registeredTemplateIds = $response->getResponseData()->getResult();
    if ($registeredTemplateIds === []) {
        throw new RuntimeException('Template was not registered');
    }
    ```

- Python

    ```python
    registered_template_ids = client.landing.demos.register(
        data=export_data,
    ).response.result

    if not registered_template_ids:
        raise RuntimeError("Template was not registered")
    ```

{% endlist %}

A successful response contains the identifiers of the created or updated templates:

```json
{
    "result": [5]
}
```

Save the `result` array. If it is empty, do not proceed to the UI check; verify the request data first.

## 3. Verify the Registration

Call [landing.demos.getList](./landing-demos-get-list.md) and find the entry whose `XML_ID` matches `myfirstsite2026`. When called from an application, the method returns only that application's templates.

{% list tabs %}

- JS

    ```js
    const listResponse = await $b24.actions.v2.call.make({
      method: 'landing.demos.getList',
      params: {
        params: {
          select: ['ID', 'XML_ID', 'TITLE', 'TYPE']
        }
      }
    })

    if (!listResponse.isSuccess) {
      throw new Error(listResponse.getErrorMessages().join('; '))
    }

    const template = listResponse
      .getData()
      .result
      .find((item) => item.XML_ID === 'myfirstsite2026')

    if (!template) {
      throw new Error('Template was not found')
    }
    ```

- PHP

    ```php
    $response = $b24Service->core->call(
        'landing.demos.getList',
        [
            'params' => [
                'select' => ['ID', 'XML_ID', 'TITLE', 'TYPE'],
            ],
        ]
    );

    $templates = $response->getResponseData()->getResult();
    $template = array_values(array_filter(
        $templates,
        static fn(array $item): bool => $item['XML_ID'] === 'myfirstsite2026'
    ))[0] ?? null;
    if ($template === null) {
        throw new RuntimeException('Template was not found');
    }
    ```

- Python

    ```python
    templates = client.landing.demos.get_list(
        params={
            "select": ["ID", "XML_ID", "TITLE", "TYPE"],
        },
    ).response.result

    template = next(
        (item for item in templates if item["XML_ID"] == "myfirstsite2026"),
        None,
    )

    if template is None:
        raise RuntimeError("Template was not found")
    ```

{% endlist %}

Abbreviated response:

```json
{
    "result": [
        {
            "ID": "5",
            "XML_ID": "myfirstsite2026",
            "TITLE": "Auto Repair Website",
            "TYPE": "page"
        }
    ]
}
```

## Verify the Result

Check that:

- `landing.demos.register` returned a non-empty array of identifiers
- `landing.demos.getList` returned a template with the expected `XML_ID`, `TITLE`, and `TYPE` values
- the template appeared in the website or page creation wizard and its preview opens

## Errors and Troubleshooting

- `BX_EMPTY_REQUIRED` at the second step — check `data.code` and the `code` field of every page in `data.items`
- `REGISTER_ERROR_DATA` at the second step — pass the complete `result` object from `landing.site.fullExport` in `data`
- `CONTENT_IS_BAD` at the second step — check the template content with `landing.repo.checkcontent`, then register it again
- `AI_SITE_EXPORT_NOT_ALLOWED` at the first step — exporting AI websites is not supported. Select another website
- `ACCESS_DENIED` at the first step — check the user's permission to export websites; at the second and third steps, check the View permission in the Sites section
- **The template was not found at the third step** — check `XML_ID`, the application context, and the result of `landing.demos.register`, then repeat the third step
- **The preview does not open** — check that `preview_url` is available without authorization

## Important Notes {#important}

- for a multi-page website, pass the complete result of `landing.site.fullExport` in `data`, including the page map in `items`
- the `type` field defines the template purpose, while `tpl_type` defines its location in the wizard: `S` for a website and `P` for a page
- external images and `preview_url` must remain available after the template is registered
- pass OAuth tokens only through application settings or environment variables; do not add them to source code
- to localize the title and description, pass the `lang` and `lang_original` parameters to `landing.demos.register`
- to delete a template, retrieve its external code from the `XML_ID` field using `landing.demos.getList`, then pass the code to [landing.demos.unregister](./landing-demos-unregister.md)

## Continue Learning

- [Overview of Custom Template Methods](./index.md)
- [Register a Template in the Website Creation Wizard](./landing-demos-register.md)
- [Get a List of Registered Templates](./landing-demos-get-list.md)
- [Get a List of Templates for Website Creation](./landing-demos-get-site-list.md)
- [Get a List of Templates for Page Creation](./landing-demos-get-page-list.md)
- [Delete a Registered Template](./landing-demos-unregister.md)
- [Export a Website](../site/landing-site-full-export.md)
- [Template Localization](./localization.md)
