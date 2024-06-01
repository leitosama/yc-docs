---
title: "{{ datalens-full-name }} release notes for April 2024"
description: "Check out {{ datalens-full-name }} release notes for April 2024."
---

# {{ datalens-full-name }} release notes: April 2024


* [Changes in basic features](#base)
* [Changes available with the _Business_ service plan](#business)

## Changes in basic features {#base}


### Auto height of the _Text_ and _Heading_ widgets {#text-title-auto-height}

Added the auto height setting for the [Text](../dashboard/widget.md#text) and [Heading](../dashboard/widget.md#title) widgets. To enable auto height, while in dashboard edit mode, click ![image](../../_assets/console-icons/gear.svg), enable **Auto height** and click **Save**.

### Selecting a text color in Markdown {#colors-markdown}

Now you can select a text color for [Markdown](../dashboard/markdown.md#text-color). You can set the text color using either the visual editor or this formatting: `{color}(text)`.

### Markup functions in calculated fields {#calculations-markup}

Added new markup functions in [calculated fields](../concepts/calculations/index.md): [BR](../function-ref/BR.md), [COLOR](../function-ref/COLOR.md), and [SIZE](../function-ref/SIZE.md).


### Label for null values {#nulls-column-chart}

Fixed the display of labels for null values in the [column chart](../visualization-ref/column-chart.md).


### Usage statistics {#usage-analytics}

Added the connection to [{{ datalens-short-name }} Usage Analytics Light](../operations/connection/create-usage-tracking.md) that will allow you to analyze {{ datalens-short-name }} user behavior, i.e., view aggregated statistics on using the service instance for a limited time period (60 days).

### Workbooks and collections in new instances {#workbooks-in-new-instances}

Only [workbooks and collections](../workbooks-collections/index.md#enable-workbooks) are now available in new {{ datalens-short-name }} instances.

## Changes available with the _Business_ service plan {#business}

### UI customization {#combined-chart-format}

Added the [UI customization](../settings/ui-customization.md). Now you can customize your service instance user interface; this includes setting up colors, logo, and design of individual elements.

### Extended usage statistics {#usage-analytics-detailed}

Added the connection to [{{ datalens-short-name }} Usage Analytics Detailed](../operations/connection/create-usage-tracking.md) that will allow you to analyze {{ datalens-short-name }} user behavior, i.e., view both detailed and aggregated statistics on using the service instance for a long time period (180 days). It allows you to view detailed statistics on dataset queries and dashboard views.

### Secure embedding {#private-embedding}

Now you can [securely embed](../dashboard/embedded-objects.md#private-embedding) private charts into a website or app using special links with a [JWT token](https://en.wikipedia.org/wiki/JSON_Web_Token).

Embedding private charts only works in the new {{ datalens-short-name }} object model at the [workbook](../workbooks-collections/index.md) level and is only available to the workbook [administrator](../security/roles.md#workbooks-admin).

### Enterprise authentication {#sso}

[Enterprise authentication](../security/add-new-user.md#federated-user) is now available only with the _Business_ [service plan](../pricing.md#service-plans). For existing customers who have configured an identity federation and used a corporate account to log in to {{ datalens-short-name }} before April 22, 2024, enterprise authentication and SSO will be available for free as part of the _Community_ plan until December 31, 2024.

