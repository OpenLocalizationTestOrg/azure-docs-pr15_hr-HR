<properties
   pageTitle="Upute za promjenu veličine Azure funkcije | Microsoft Azure"
   description="Objašnjenje kako funkcije Azure skaliranje potrebama vaše radnih opterećenja utemeljenih na događaj."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure funkcije, obrada događaj, webhooks, dinamični računalnim, funkcije serverless arhitekture"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Upute za promjenu veličine Azure funkcije

## <a name="introduction"></a>Uvod

Prednost funkcija Azure je koji su resursi računalnim samo potrošena po potrebi. To znači da ne plaćate neaktivnosti VMs ili imate rezervirati kapacitet kada bilo bi dobro. Umjesto toga platforme dodjeljuje računalnim power kada kod je pokrenut, mijenja veličinu koliko je potrebno učiniti učitavanja, a zatim mijenja veličinu kada kod je pokrenut.

Mehanizmi za tu mogućnost Nova je tarifa za dinamičku servis.  

Ovaj članak sadrži pregled kako funkcionira tarifa za dinamičku servis i kako mijenja veličinu platformu na zahtjev za pokretanje koda.

Ako niste niste upoznati s Azure funkcije, provjerite je li da biste provjerili u članku [Pregled Azure funkcija](functions-overview.md) da biste bolje razumjeli njegovim mogućnostima.

## <a name="configure-azure-functions"></a>Konfiguriranje Azure funkcije

Dvije glavne postavke vezane uz Skaliranje:

* [Planiranje aplikacije servisa za Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) ili dinamički tarifu korištenja servisa
* Veličina memorije okruženju izvođenja

Trošak funkcija mijenja se ovisno o tarifa za servis koji ste odabrali. Uz tarifu za servis za dinamičku trošak je funkcija vrijeme izvođenja, memorije i broj izvršavanja. Ako skupi troškove samo kada kod zapravo radi.

Tarifu aplikacije servisa za hostira vaš funkcionira na temelju postojeće VMs koji mogu se koristiti da biste pokrenuli drugi kod. Nakon plaćate za ove VMs svakog mjeseca, postoji bez dodatnih troškova za izvođenja funkcije na njima.

## <a name="choose-a-service-plan"></a>Odaberite tarifu za servis

Kada stvorite funkcije, možete odabrati da biste pokrenuli ih na tarifu za servis za dinamičku ili [aplikacije servisa za planiranje](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
U planu aplikacije servisa za vaše funkcije će se izvoditi na namjenski VM, baš kao web apps posla danas za Basic, Standard ili Premium SKU-ove.
U ovom namjenski VM je dodijeljen aplikacije i funkcije i uvijek dostupna je li kod je aktivno izvršena ili ne. Ovo je dobar izbor imate postojeće, u odjeljku-koristi VMs koji se već izvode drugi kod ili ako očekujete da biste pokrenuli funkcije neprekidno ili kontinuirano gotovo. Na VM decouples trošak veličini izvođenja i memorije. Zbog toga možete ograničiti trošak mnoge funkcije dugoročnih cijeni od jednog ili više VMs koji su pokrenuti na.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
