---
title: "Building a Large Configuration File from Fragments"
meta_title: "Building a Large Configuration File from Fragments"
description: "Discusses the challenges of configuring a complex service with extensive configuration needs."
date: 2025-09-26T12:00:00Z
image: ""
categories: ["Software", "Engineering"]
author: "Erick López"
tags: ["software","configuration","dotnet","SOLID"]
draft: false
---

# Large configurations, big challenges

When you work on a system that requires large configuration files, the question arises: how will this be maintainable in the future?

In the IDE (Visual Studio, in my case) you can collapse sections and navigate to areas of interest, but that doesn't solve the cognitive load that huge files create.

![visual studio collapsing features](/images/composability/composability.png)


## 100, 200, 500, 2,000 lines

Reading large files of hundreds or thousands of lines is common for both junior and experienced engineers. However, in the early stages of a project it's worth investing time in alternatives to prevent those files from growing out of control.

The [SOLID](https://en.wikipedia.org/wiki/SOLID) principles are often discussed as good practices in software development, especially the single responsibility principle for methods and classes. That approach is rarely applied to a project's configuration, where we may find API keys, custom messages, URLs and even feature flags that control complex behavior.

Concentrating all of those settings in a single file makes maintenance harder.


## Solution

When implementing internationalization (supporting multiple languages) it's common to use separate files and access the correct language via keys. Splitting a large file into smaller fragments is an effective way to reduce cognitive load.

Let's take this JSON as an example:

```json
{
  "StoreInfo": {
    "Name": "Awesome Online Store",
    "ContactEmail": "support@store.com",
    "IsLive": true
  },
  "DatabaseSettings": {
    "ConnectionString": "Server=tcp:tuserver.database.windows.net,1433;Initial Catalog=ecommercedb;Persist Security Info=False;User ID=admin-user;Password={your_password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
    "Provider": "SqlServer",
    "EnableRetryOnFailure": true,
    "MaxRetryCount": 5,
    "MaxRetryDelay": "00:00:30",
    "CommandTimeout": 60,
    "EnableSensitiveDataLogging": false,
    "UseInMemoryDatabase": false,
    "DefaultSchema": "products",
    "EnableDetailedErrors": true
  },
  "ApiKeys": {
    "PaymentGateway": "pk_te...qRsTuVwXyZ",
    "ShippingProvider": "shp_li...7g8H9i0J",
    "EmailService": "SG.aBc...tUvWxYz",
    "AnalyticsService": "G-12345ABCDE",
    "MapService": "AIzaSyA...xyz",
    "SMSService": "AC123...def",
    "CRMIntegration": "crm-key-live-xyz",
    "InventoryManagement": "inv_mgr_prod_key",
    "SocialMediaAuth": "fb_app_id|client_token"
  }
}
```


With these three large groups of keys we can organize files like this:

```
|-- /Configuration
|   |-- StoreInfo.json
|   |-- DatabaseSettings.json
|   |-- ApiKeys.json
```


This organization helps us focus only on the feature we are going to review or modify.

In .NET we can implement this using `ConfigurationBuilder` and the `.AddJsonFile("StoreInfo.json")` method.
