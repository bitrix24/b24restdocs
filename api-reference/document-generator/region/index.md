# Regions in the Document Generator: Overview of Methods

Regions define local settings for document generator templates. They allow you to configure:
- the region's language
- date and datetime formats
- the full name template
- a set of localized phrases for the document

There are two types of regions in the system: predefined and custom. A predefined region contains a ready-made set of settings, while a custom region allows you to specify your own settings.

> Quick Navigation: [All Methods](#all-methods)

## Getting Started

1. Retrieve the list of available regions using the [documentgenerator.region.list](./document-generator-region-list.md) method.
2. If a predefined region suits your needs, use its code when [creating a template](../templates/document-generator-template-add.md) or [updating a template](../templates/document-generator-template-update.md) for documents.
3. If you need your own local settings, create a custom region using the [documentgenerator.region.add](./document-generator-region-add.md) method.
4. Obtain the settings for the desired region using the [documentgenerator.region.get](./document-generator-region-get.md) method.
5. Modify the parameters of the custom region using the [documentgenerator.region.update](./document-generator-region-update.md) method.
6. Delete any unnecessary custom region using the [documentgenerator.region.delete](./document-generator-region-delete.md) method.

## Available Regions

The [documentgenerator.region.list](./document-generator-region-list.md) method returns two types of regions:

1. Predefined regions with a symbolic code, such as `fr` or `en`. In the method's response, the code is specified in the `code` field.

    ```json
    "fr": {
        "code": "fr",
        "title": "France",
        "languageId": "fr"
    },
    ```

2. Custom regions with a numeric identifier, such as `5`. In the method's response, the identifier is specified in the `id` field.

    ```json
    "1": {
        "id": "1",
        "title": "United States (Custom)",
        "languageId": "en",
        "formatDate": "MM/DD/YYYY",
        "formatDatetime": "MM/DD/YYYY HH:MI:SS",
        "formatName": "#LAST_NAME# #FIRST_NAME# #MIDDLE_NAME#",
        "code": "1"
    }
    ```

## Relationship of Regions with Other Objects

**Document Templates.** The region value is stored in the template data and defines the local settings that will be used when working with that template. To associate a region with a template, you need to pass the value in the `region` field when creating the template using the [documentgenerator.template.add](../templates/document-generator-template-add.md) method. For a predefined region, use `code`, and for a custom region, use `id`. You can change the template's region using the [documentgenerator.template.update](../templates/document-generator-template-update.md) method.

## Considerations When Modifying and Deleting a Region

The [documentgenerator.region.update](./document-generator-region-update.md) and [documentgenerator.region.delete](./document-generator-region-delete.md) methods only work with custom regions.

Deleting a region using the [documentgenerator.region.delete](./document-generator-region-delete.md) method may result in an error if templates are associated with it. Reassign the templates to another region or delete any unnecessary templates.

## Overview of Methods {#all-methods}

> Scope: [`documentgenerator`](../../scopes/permissions.md)
>
> Who can execute the methods: a user with permission to modify document generator templates

#|
|| **Method** | **Description** ||
|| [documentgenerator.region.add](./document-generator-region-add.md) | Adds a custom region ||
|| [documentgenerator.region.update](./document-generator-region-update.md) | Updates a custom region ||
|| [documentgenerator.region.get](./document-generator-region-get.md) | Returns region data by identifier or code ||
|| [documentgenerator.region.list](./document-generator-region-list.md) | Returns a list of predefined and custom regions ||
|| [documentgenerator.region.delete](./document-generator-region-delete.md) | Deletes a custom region ||
|#