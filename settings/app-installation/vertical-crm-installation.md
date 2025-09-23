# Installation of Industry-Specific CRMs

An industry-specific CRM is a configuration solution based on CRM settings in the form of a set of deal types, lead stages, custom fields for all CRM entities, settings for Automation rules, workflows, etc., which can be saved as a single archive using the user functionality for exporting industry settings.

This archive can be uploaded in the Developer's area in the application cards and published as an industry solution in the Bitrix24 Marketplace.

The installation of such an application is carried out by Bitrix24 and occurs in several stages:

1. Automatic downloading of the archive from the Developer's area to the Bitrix24 where the installation takes place;
2. Unpacking the archive;
3. Applying the settings from the archive files to the current Bitrix24.

## Using REST Applications in the Functionality of Industry-Specific CRMs

When configuring the CRM before exporting the settings, the solution developer can install and use applications from the Bitrix24 Marketplace to implement the required functionality.

For example, you can utilize non-standard Automation rules that were added from the Marketplace solution. Alternatively, you can install solutions that format customer phone numbers in the CRM, etc. The convenience lies in the fact that you can focus specifically on the business logic of the CRM itself without delving into the complex development of individual Automation rules or specific integrations. At the same time, by using functionality from other Marketplace solutions, you can offer users of your industry-specific CRMs very flexible and highly functional solutions.

The list of used applications is saved in the industry-specific CRM file during export. Upon import, the applications from this list will be automatically installed on the same Bitrix24 where the industry-specific CRM is being installed.

If among these applications there are solutions available to the client only within the "Bitrix24 Marketplace" subscription, then when publishing your industry-specific CRM, you will also need to place it in the Bitrix24 Marketplace catalog under subscription terms.