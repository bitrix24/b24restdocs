# Overview of Methods

> Scope: [`depending on the placement`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Applications that have access to the **placement** scope can register their own types of custom fields.

Currently, cloud accounts support the use of such fields in both the new and old detail forms of **CRM** entities. Applications can create custom fields of standard types as well as those registered by the application itself. Account administrators can create fields of any registered types, including field types registered by applications. When registering a type, the application specifies the handler address that will open in a frame at the location of the field output, and further operations are virtually indistinguishable from those of a standard widget.

#|
|| **Method** | **Description** ||
|| [userfieldtype.add](./userfieldtype-add.md) | Register a new type of custom fields ||
|| [userfieldtype.list](./userfieldtype-list.md) | Retrieve a list of types of custom fields registered by the application ||
|| [userfieldtype.update](./userfieldtype-update.md) | Modify the settings of a type of custom fields registered by the application ||
|| [userfieldtype.delete](./userfieldtype-delete.md) | Remove a type of custom fields registered by the application ||
|#

## Additional Information

1. Example of embedding in the tutorial [Embed a widget in a lead as a custom property](../../../tutorials/crm/crm-widgets/widget-as-field-in-lead-page.md)

2. You have registered a new type of custom fields. If, when trying to create a field with the new type, [userfieldtype.list](./userfieldtype-list.md) returns your new custom field type and you still encounter an error: `Error! 400: ERROR_CORE: Invalid custom type specified. (400)`, it means that the [application](*application) is not fully installed. Call the method [app.info](../../common/system/app-info.md) and ensure that the result returns `INSTALLED = true`. If the application has an interface, to complete the installation, you need to execute the following js code on the application page:

```javascript
BX24.init(function(){
    BX24.installFinish();
});
```

[*application]: Local applications refer to applications that are described and added directly to a specific *Bitrix24*.