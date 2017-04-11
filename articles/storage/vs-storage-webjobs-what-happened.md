<properties
    pageTitle="Što se dogodilo s WebJob projektu (Visual Studio Azure prostora za pohranu povezani servisa)? | Microsoft Azure"
    description="U članku se opisuje što se dogodilo u programu project Azure WebJob nakon povezivanja s računom za pohranu pomoću Visual Studio povezani servisi"
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

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Što se dogodilo s WebJob projektu (Visual Studio Azure prostora za pohranu povezani servisa)?

## <a name="references-added"></a>Reference dodali

NuGet Azure prostora za pohranu paketa dodati ili ažurirati u projektu Visual Studio.  
Paket dodaje sljedeće .NET reference:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Niz za povezivanje za pohranu Azure dodali
U datoteci App.config projekta, stavke **AzureWebJobsStorage** i **AzureWebJobsDashboard** su će se ažurirati niz za povezivanje računa odabrane prostora za pohranu i ključa.

Dodatne informacije potražite u članku [Azure WebJobs dokumentaciju resursi](http://go.microsoft.com/fwlink/?linkid=390226).
