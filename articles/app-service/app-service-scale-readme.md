<properties
    pageTitle="Aplikacije servisa Azure: Skaliranje aplikacije servisnim aplikacijama"
    description="Saznajte tajne promjene veličine aplikaciju u aplikacije servisa."
    keywords="aplikacije servisa, azure aplikacije servisa, mjerilo skalabilni, plan aplikacije servisa, trošak servisa aplikacija"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Aplikacije servisa Azure: Skaliranje aplikacije servisnim aplikacijama

Aplikacija smješten u aplikacije servisa za Azure možete [postići pretraživanje velikog mjerilo](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Međutim, skaliranje aplikacija je složene problem koji imaju rješenje "veličine odgovara sve". Da biste pravilno skaliranje vaše aplikacije su 3 ključna područja koje će pridonose uspjehu aplikacije:

1. Objašnjenje arhitektura vašeg računala i njegov slabosti.
    * Je li vaša aplikacija s praćenjem stanja? Bez praćenja stanja?
    * Što su sve komponente aplikacije?
        * Gdje su grla u aplikaciji?
    * Prilikom učitavanja primjenjuje se na aplikaciju, što će prekinuti prvi?
2. Objašnjenje očekivani preduvjeti opterećenja i performanse.
    * Aplikacija mora vam poslužiti tisuću korisnika? ili milijun?
    * Promet isporučuje se s jednog zemljopisna mjesta ili globalno?
    * Postoje li sezonski varijacije? promet peaks?
    * Kako brzo treba odgovoriti aplikaciju? 1 sekunde? 1 od milisekundi?
3. Razumijevanje i ispravno pod utjecajem platformu nalaze aplikacije.
    * Koje značajke korištenje za da biste postigli Moji ciljevi skaliranje?

U ovom se odjeljku će vam olakšati razumijevanje čimbenika i pomoć razviju strategije koja vas vodi potrebne značajke aplikacije servisa da biste postigli ciljeve skalabilnost.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]
