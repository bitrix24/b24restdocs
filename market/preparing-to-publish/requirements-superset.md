# Create Your Reports for the BI Builder in Bitrix24

This is a great opportunity not only to monetize your knowledge and experience but also to help other businesses improve their analytics. Your template can become a valuable tool for other entrepreneurs looking to enhance their business performance.

## What Are Report Templates

**BI Builder** is a new tool in Bitrix24 that helps businesses make informed decisions based on data. Users will be able to aggregate various metrics important to their business sector, perform calculations, visually compare data, and more.

**Report template** is a specific type of solution in the Bitrix24 Marketplace that does not require programming and does not use the Bitrix24 REST API. When such a solution is installed, clients receive an analytical dashboard ready to use with their data.

## What’s Included in a Report Template

{% cut "Datasets" %}

These are pre-installed datasets from Bitrix24 available for building reports. Currently, they include information about leads, deals, phone calls, etc. The user documentation describes the complete list of datasets. The BI Builder also allows the creation of custom virtual datasets based on SQL queries to the pre-installed datasets.

{% endcut %}

{% cut "Dashboards" %}

These are visual panels where you should display key metrics and charts. Dashboards enable clients to quickly assess the state of their business and identify trends and anomalies.

{% endcut %}

{% cut "Metrics" %}

These are aggregating numerical indicators that help measure business performance. For example, this could include revenue, average check, conversion to sale, and other metrics.

{% endcut %}

{% cut "Calculable Fields" %}

These are computed values created based on existing data in the BI system. They allow for complex mathematical operations, data aggregation, applying logical conditions, and transforming information to derive new indicators and metrics.

{% endcut %}

{% cut "Filters" %}

They allow you to select specific segments of data for analysis. For example, you can filter data by a specific region or time period.

{% endcut %}

{% cut "Table Charts" %}

Don’t forget about the option to present data in table format, helping users see details about important business metrics.

{% endcut %}

## What Are the Requirements for Reports

In addition to the [main regulations with requirements for solution formatting](https://vendors.bitrix24.com/doc/en/moderator_rules_rest.php), there are several specific requirements for publishing report templates:

- **DATASET NAMES, METRIC CODES, AND CALCULABLE FIELD CODES**. In the names of virtual datasets, in the metric codes, and in the codes of calculable fields, use the partner prefix followed by a dot. For example: _virtual dataset_ - `mycode.revenue_dynamic`, _metric_ - `mycode.avg_bill`;
- **NAMES AND DESCRIPTIONS OF METRICS AND FIELDS**. Metrics and calculable fields must have a clear name and description, and the "Approved by" field must be filled with the partner's name (the same as in the application card);
- **DASHBOARD NAMES**. The name of the dashboard must include a postfix with the partner code in square brackets. Example "Dynamics of Key Sales Indicators [`mycode`]";
- **ADDING METRICS AND FIELDS**. Metrics and calculable fields should be added not on the chart but in the data source for further use in any charts;
- **USING FILTERS IN CHARTS**. Filters by date range must be applied in charts to limit the volume of loaded data. The recommended period is the current year.
- **SETTING A LIMIT IN THE CHART**. In the chart settings, it is necessary to specify a row limit. The recommended value is 1000.
- **USING SERVER-SIDE PAGINATION**. In the table chart, it is recommended to enable server-side pagination for faster performance.
- **EXPLICIT FILTER IN THE DASHBOARD**. An explicit date range filter that affects the used charts must also be added to the dashboard.
- **LIMITING THE RANGE FOR THE FILTER IN THE FORM OF A DROPDOWN LIST**. When forming the dashboard filter as a dropdown list, the filter settings should apply a selection based on a limited date range to restrict the volume of loaded data. The recommended period is the current year.

## How to Create and Publish a Report in the Marketplace

To create such a solution, it is sufficient to set up a dashboard with reports in Bitrix24 using standard user tools while considering the aforementioned requirements, and then export these settings using a special built-in tool in the form of an archive. Next, you need to add the application in the Developer's area and attach the obtained archive in the version card of the added application. Then fill out the solution card and submit it for moderation.

## Continue Learning

- [{#T}](common-requirements.md)