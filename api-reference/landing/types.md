# Working with Site Types and Scopes

The type of site determines its purpose and how it interacts with Bitrix24. This affects how the site is created, which pages, blocks, and settings are available, and which `landing` methods can be used. For instance, different capabilities are available for a knowledge base and an online store.

{% note info "" %}

The `scope` parameter in landing methods is not related to the REST scope `landing` that you provide to the application or webhook.

{% endnote %}

## What You Need to Know

- The `scope` parameter is not required for `PAGE`, `STORE`, and `SMN` types.
- For `KNOWLEDGE`, `GROUP`, and `MAINPAGE`, you need to pass the top-level `scope` parameter.
- If `scope` is not provided, methods will only work with `PAGE`, `STORE`, and `SMN` types.
- The value of `scope` must match the site type or the filter value by type.
- The scope affects not only the list of sites and pages but also the set of available blocks and site settings.
- The `GROUP` type is only available on accounts that support group knowledge bases.

## Site Types

#| 
|| **Type** | **Description** | **`scope` in REST request** ||
|| `PAGE` | Regular site | Do not pass ||
|| `STORE` | Store | Do not pass ||
|| `SMN` | Site of the Sites24 section in 1C-Bitrix: Site Management | Do not pass ||
|| `KNOWLEDGE` | Knowledge base | `KNOWLEDGE` ||
|| `GROUP` | Group knowledge base | `GROUP` ||
|| `MAINPAGE` | Main page or vibe | `MAINPAGE` ||
|#

## How Scope Works

Without the `scope` parameter, REST methods operate in standard mode and only see sites of types `PAGE`, `STORE`, and `SMN`. If you provide `scope`, the methods switch to the corresponding site type. For example, with `scope = "KNOWLEDGE"`, the lists of sites, pages, permissions, and available blocks will only work for knowledge bases.

#| 
|| **scope in request** | **Which site types the methods work with** | **When to use** | **Public path** ||
|| do not pass | `PAGE`, `STORE`, `SMN` | Regular sites and stores | Depends on the domain and site settings ||
|| `KNOWLEDGE` | `KNOWLEDGE` | Knowledge bases | `/knowledge/` ||
|| `GROUP` | `GROUP` | Group knowledge bases | `/knowledge/group/` ||
|| `MAINPAGE` | `MAINPAGE` | Main page and vibe | `/vibe/` ||
|#

When creating a site, the value of `scope` must match `fields.TYPE`. When retrieving a list of sites or pages, the value of `scope` must match the filter value by type.

## What Changes in Special Scopes

Special scopes not only change the selection of sites and pages.

#| 
|| **Scope** | **What Changes for the User** ||
|| `KNOWLEDGE` | Methods only work with knowledge bases. Some site settings are unavailable, such as SEO fields, analytics, pixels, custom code, favicon, cookies banner, Bitrix24 widget, catalog settings, and optimization. ||
|| `GROUP` | Methods only work with group knowledge bases. The restrictions on settings are the same as for `KNOWLEDGE`. ||
|| `MAINPAGE` | Methods only work with the main page and vibe. In addition to general restrictions, some design settings are unavailable, such as font themes, page appearance animation, and the back-to-top button. ||
|#

## How to Pass Scope in REST

For regular sites, the `scope` parameter is not needed:

```json
{
    "params": {
        "filter": {
            "TYPE": "PAGE"
        }
    }
}
```

For a knowledge base, pass the top-level `scope`:

```json
{
    "scope": "KNOWLEDGE",
    "params": {
        "filter": {
            "TYPE": "KNOWLEDGE"
        }
    }
}
```

When creating a special type of site, `scope` and `fields.TYPE` must match:

```json
{
    "scope": "MAINPAGE",
    "fields": {
        "TITLE": "Company Main Page",
        "CODE": "mainpage",
        "TYPE": "MAINPAGE"
    }
}
```

## Continue Your Learning

- [{#T}](./site/landing-site-add.md)
- [{#T}](./site/landing-site-get-list.md)
- [{#T}](./page/methods/landing-landing-get-list.md)
- [{#T}](./block/methods/landing-block-get-repository.md)
- [{#T}](./page/block-methods/landing-landing-add-block.md)