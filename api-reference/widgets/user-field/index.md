# Embedding as Custom Field Types

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits are needed to meet writing standards

{% endnote %}

{% endif %}

{% note info "Custom Fields" %}

**Scope**: depending on the [`placement`](../../scopes/permissions.md) **Execution Permissions**: `for everyone`.

{% endnote %}

Applications that have access to the **placement** scope can register their custom field types.

Currently, cloud accounts support the use of such fields in both the new and old detail forms of **CRM** entities. Applications can create custom fields of standard types and those registered by the application itself. Account administrators can create fields of any registered types, including field types registered by applications. When registering a type, the application specifies the handler address that will open in a frame at the field's output location, and further operations are virtually indistinguishable from those of a regular embedding.

| Method | Description | Version |
|--------|-------------|---------|
| [userfieldtype.add](./userfieldtype-add.md) | Register a new custom field type. | |
| [userfieldtype.list](./userfieldtype-list.md) | Retrieve a list of custom field types registered by the application. | |
| [userfieldtype.update](./userfieldtype-update.md) | Modify the settings of a custom field type registered by the application. | |
| [userfieldtype.delete](./userfieldtype-delete.md) | Remove a custom field type registered by the application. | |

## Additional Information

1. For more details on embedding as custom field types, refer to the [training course](https://training.bitrix24.com/support/training/course/index.php?COURSE_ID=169&LESSON_ID=20070&LESSON_PATH=13643.20052.20068.20070).

2. You have registered a new custom field type. If, when trying to create a field with the new type:

- [userfieldtype.list](./userfieldtype-list.md) returns your new custom field type
- and yet you encounter an error: `Error! 400: ERROR_CORE: Invalid custom type specified. (400)`

, it means that the [application](*application) is not fully installed. Call the method [app.info](../../common/system/app-info.md) and ensure that the result returns `INSTALLED = true`. If the application has an interface, to complete the installation, you need to execute the following JS code on the application page:

```javascript
BX24.init(function(){
    BX24.installFinish();
});
```

[*application]: Local applications refer to applications that are described and added directly to a specific Bitrix24.