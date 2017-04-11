<properties
    pageTitle="Visoke gustoće hostiranje na aplikacije servisa za Azure | Microsoft Azure"
    description="Visoke gustoće hostiranje na aplikacije servisa za Azure"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>Visoke gustoće hostiranje na aplikacije servisa za Azure

Prilikom korištenja aplikacije servisa za aplikaciju je samostalne iz kapaciteta ga tako da dva pojma dodijeljen:

- **Aplikacije:** Predstavlja aplikaciju i svoju konfiguraciju runtime. Na primjer, sadrži verziju .NET koja se učitava vremena izvođenja, postavki aplikacije, itd.

- **Aplikacije servisa za planiranje:** Definira karakteristike kapacitetu, skup dostupnih značajki i kraj aplikacije. Ako, na primjer, značajke možda velike računala (četiri jezgri), četiri primjerka, Premium značajkama u SAD-a na.

Aplikaciju uvijek je povezana s tarifu aplikacije servisa za, ali tarifu aplikacije servisa za pružaju kapaciteta neke aplikacije.

Zbog toga platforme pruža fleksibilnost izdvajanja aplikacije u jednu ili više aplikacije Omogućivanje zajedničkog korištenja resursa slanjem tarifu aplikacije servisa za su.

Međutim, kada više aplikacije zajedničko korištenje tarifu aplikacije servisa, instance komponente tu aplikaciju pokreće sve instance tog aplikacije servisa za planiranje.

## <a name="per-app-scaling"></a>Po skaliranje aplikacije
*Po skaliranje aplikacija* je značajke koje se mogu omogućena na razini aplikacije servisa za planiranje i zatim koristiti svaku aplikaciju.

Po aplikacije skaliranje mijenja veličinu aplikacije neovisno iz aplikacije servisa za plan koji mu. Na taj način tarifu aplikacije servisa za moguće je konfigurirati za pružanje 10 instance, ali aplikacije možete postaviti za promjenu veličine da biste samo 5 od njih.

Sljedeći predložak Voditelj resursa Azure stvara tarifu aplikacije servisa za koja je neproporcionalno 10 slučajeve i aplikaciju koja je konfiguriran za korištenje po skaliranje aplikacije i Skaliraj samo 5 instance.

Aplikacije servisa za planiranje je svojstvo postavite **Skaliranje po web-mjesta** na true ( `"perSiteScaling": true`). Aplikaciju postavlja **broj zaposlenici zaduženi** za 5 (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Preporučena konfiguracija za hostiranje visoke gustoće

Po skaliranje aplikacija je značajka koja je omogućena u javnim Azure regije i okruženja aplikacije servisa. Preporučena strategija je da biste koristili aplikaciju servisa okruženja da iskoristite prednost njihove napredne značajke i veće grupe kapaciteta.  

Slijedite ove korake da biste konfigurirali visoke gustoće hosting aplikacija:

1. Konfigurirati okruženje servisa aplikacija, a zatim odaberite skup tempiranja posvećenu visoke gustoće hostinga scenarij.

1. Stvorite jedan plan aplikacije servisa za i promijenite joj da biste koristili dostupan kapacitet na skup tempiranja.

1. Postavljanje zastavice skaliranja po web-mjesta na true u planu aplikacije servisa.

1. Nova web-mjesta su stvoreni i dodijeljeni te aplikacije servisa za tarifu s svojstvo **numberOfWorkers** postavljeno na **1**. Pomoću ovog konfiguracije rezultira najviših gustoće moguće na ovom radnih grupa aplikacija.

1. Broj zaposlenici zaduženi za samostalno može konfigurirati po web-mjestu da biste dodijelili dodatne resurse prema potrebi. Najviša-korištenje web-mjesta, na primjer, možda postavljen **numberOfWorkers** **3** da bi više obrada kapaciteta za tu aplikaciju dok niska koristite web-mjesta želite postaviti **numberOfWorkers** **1**.
