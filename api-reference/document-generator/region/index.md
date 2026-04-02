# Regions in the Document Generator: Overview of Methods

The methods in this section allow you to retrieve a list of regions, view a region by its code or identifier, as well as create, modify, and delete custom regions.

> Quick Navigation: [All Methods](#all-methods)

## What Defines a Region

Each template in the document generator is tied to a region. The region influences how data is formatted in the document:

- region language `languageId`
- date format `formatDate`
- date and time format `formatDatetime`
- full name format `formatName`
- set of service phrases `phrases` used during text generation

Predefined regions are stored in the system by region code, for example, `de`. Custom regions are created via REST and receive a numeric identifier. The method [documentgenerator.region.get](./document-generator-region-get.md) accepts both the region code and `id`.

## Connection with Templates

The region is used in document generator templates and defines local rules for text and field value formatting.

To work with templates, use the methods in the [Templates](../templates/index.md) section. After modifying the region parameters, the new settings will be applied during subsequent document generation in templates of that region.

## Operational Features

A region cannot be deleted if there are active templates associated with it.

## Predefined Regions

The document generator offers the following predefined regions:

- `de` — Germany
- `by` — Belarus
- `kz` — Kazakhstan
- `ua` — Ukraine
- `br` — Brazil
- `sp` — Spain
- `mx` — Mexico
- `fr` — France
- `uk` — United Kingdom
- `pl` — Poland

## Overview of Methods {#all-methods}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the methods: users with permission to modify document generator templates

#| 
|| **Method** | **Description** ||
|| [documentgenerator.region.add](./document-generator-region-add.md) | Adds a custom region ||
|| [documentgenerator.region.update](./document-generator-region-update.md) | Updates a custom region ||
|| [documentgenerator.region.get](./document-generator-region-get.md) | Returns region data by identifier or code ||
|| [documentgenerator.region.list](./document-generator-region-list.md) | Returns a list of predefined and custom regions ||
|| [documentgenerator.region.delete](./document-generator-region-delete.md) | Deletes a custom region ||
|#