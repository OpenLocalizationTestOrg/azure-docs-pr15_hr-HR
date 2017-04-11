<properties
    pageTitle="Što se dogodilo s ASP.NET projektu? | Microsoft Azure | Visual Studio povezani servisi"
    description="U članku se opisuje što se događa nakon dodavanja Azure prostora za pohranu u ASP.NET projektu pomoću Visual Studio povezani servisi"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>Što se dogodilo s projektu ASP.NET (Visual Studio Azure prostora za pohranu povezani servisa)?

## <a name="references-added"></a>Reference dodali

Za projekt za Visual Studio dodan je paket NuGet Azure prostora za pohranu.  
Paket dodaje sljedeće .NET reference:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

##<a name="connection-string-for-azure-storage-added"></a>Niz za povezivanje za pohranu Azure dodali
U datoteci web.config projekta element stvorena s računom odabrane prostora za pohranu niz za povezivanje i ključa.

Dodatne informacije potražite u članku [ASP.NET](http://www.asp.net).
