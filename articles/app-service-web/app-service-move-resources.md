<properties
    pageTitle="Premještanje web-aplikacije resurse u drugu grupu resursa"
    description="U članku se opisuje scenariji kojima možete premjestiti Web aplikacija i servisa aplikacija iz jedne grupe resursa u drugu."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Podržani premještanje konfiguracije

Možete premjestiti resurse Azure web-aplikacije pomoću [OKVIRA premještanje resursi API -ja](../resource-group-move-resources.md).

Azure web-aplikacije trenutno podržava sljedeće scenarije Premjesti:

* Premještanje cijeli sadržaj grupu resursa (web-aplikacije, aplikacije servisa tarife i certifikati) u drugu grupu resursa 
    * Napomena: Odredišnu grupu resursa može sadržavati nikakve Microsoft.Web resurse u ovom scenariju
* Premještanje pojedinačnih web-aplikacije u grupu različite resursa dok još uvijek nalaze u svoje trenutne aplikacije tarifa usluge (plan usluge za aplikaciju ostaje u grupi stare resursa)
