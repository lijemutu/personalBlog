---
title: "Construyendo un gran archivo de configuración a partir de fragmento"
meta_title: "Construyendo un gran archivo de configuración a partir de fragmento"
description: "Se describe la problemática de un servicio complejo con amplias capacidades de configuración."
date: 2025-09-26T12:00:00Z
image: ""
categories: ["Software", "Engineering"]
author: "Erick López"
tags: ["software","configuration","dotnet","SOLID"]
draft: false
---

# Grandes configuraciones, grandes retos

Al trabajar en un sistema que requiere grandes archivos de configuración surge la pregunta: ¿cómo será mantenible esto en el futuro?

En el IDE (Visual Studio, en mi caso) podemos colapsar secciones y navegar por las regiones de interés, pero eso no soluciona la carga cognitiva que generan archivos enormes.

![visual studio collapsing features](/images/composability/composability.png)


## 100, 200, 500, 2,000 líneas

Leer archivos grandes de cientos o miles de líneas es habitual tanto para ingenieros novatos como experimentados. Sin embargo, en fases tempranas del proyecto conviene invertir tiempo en alternativas para evitar que esos archivos crezcan sin control.

Se habla mucho de los principios [SOLID](https://en.wikipedia.org/wiki/SOLID) como buenas prácticas en el desarrollo de software, en especial la responsabilidad única de métodos y clases. Ese enfoque rara vez se aplica a la configuración de un proyecto, donde podemos encontrar claves de API, mensajes personalizados, URL e incluso banderas que controlan funcionalidades complejas.

Concentrar todas esas configuraciones en un único archivo dificulta su mantenimiento.


## Solución

Al implementar internacionalización (soportar varios idiomas) es habitual usar archivos distintos y acceder al idioma correcto mediante claves. Dividir un gran archivo en fragmentos más pequeños es una solución efectiva para reducir la carga cognitiva.

Tomemos este JSON como ejemplo:

```json
{
  "StoreInfo": {
    "Name": "Tienda en Línea Genial",
    "ContactEmail": "soporte@tienda.com",
    "IsLive": true
  },
  "DatabaseSettings": {
    "ConnectionString": "Server=tcp:tuserver.database.windows.net,1433;Initial Catalog=ecommercedb;Persist Security Info=False;User ID=admin-user;Password={tu_contraseña};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;",
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


Al tener tres grandes grupos de claves podemos organizar los archivos así:

```
|-- /Configuracion
|   |-- StoreInfo.json
|   |-- DatabaseSettings.json
|   |-- ApiKeys.json
```


Esta organización nos ayuda a enfocarnos únicamente en la característica que vamos a revisar o modificar.

En .NET podemos implementar esto usando `ConfigurationBuilder` y el método `.AddJsonFile("StoreInfo.json")`.