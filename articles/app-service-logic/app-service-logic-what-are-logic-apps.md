<properties 
    pageTitle="Što su logika aplikacije?" 
    description="Dodatne informacije o logike servisa aplikacija" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>Što su logike aplikacije?

Logika aplikacija omogućuje pojednostaviti i implementirati skalabilni integracije i tijekova rada u oblaku. Pruža vizualnog dizajnera modela i automatizirati proces kao niz koraka naziva tijeka rada.  Postoji [mnogo poveznika](../connectors/apis-list.md) preko oblaka i lokalne da biste brzo integriraju svim servisima i protokoli.  Logika aplikacije počinje okidača (poput "prilikom dodavanja računa za Dynamics CRM") i nakon firing možete započeti s mnogo kombinacije Akcije, pretvorbe i logike uvjet.

Prednosti korištenja aplikacije logike obuhvaćaju sljedeće:  

- Čime se štedi vrijeme dizajniranjem složene procesa pomoću jednostavno da biste shvatili Alati za dizajn
- Jednostavno implementacijom obrazaca i tijekova rada koji bi se inače teško implementirati u kodu
- Početak rada brzo iz predložaka
- Prilagodba logiku aplikacije s vlastite prilagođene API-ji, kod i akcije
- Povezivanje i aktivna raznovrsne sustavi putem lokalnog poslužitelja u oblak i
- Sastavljanje s BizTalk server, upravljanje API-JA, funkcija Azure i Bus servisa Azure s podrškom za jednostavno prva liga Integracija

Logika aplikacija je potpuno upravljanih iPaaS (Integracija platforme kao Service) dopuštanja razvojnim inženjerima da ne morate brinuti o izradi hosting, skalabilnost, dostupnost i upravljanje.  Logika aplikacija će proširenjem automatski zadovoljavaju zahtjev.

![Dizajner tijeka aplikacije](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Kao što je rečeno, s aplikacijama logike možete automatizirati poslovni proces. Evo nekoliko primjera:  
 
* Premještanje datoteke u prostor za pohranu Azure prenijeli na FTP poslužitelj
* Postupak i usmjeravanje narudžbe putem lokalnog i sustava u oblaku
* Praćenje svih tweets o određenim temama, analizirali šalju i stvaranje upozorenja i zadataka za stavke koje je potrebno krajnji.

Scenariji kao što je to moguće je konfigurirati sve s vizualnog alata za dizajniranje i bez pisanja s jednim retkom kod. Početak rada [sastavljanjem logiku aplikacijom sada][create].  Kada sastavljene - logike aplikacije može biti [brzo implementiran i rekonfigurirati](app-service-logic-create-deploy-template.md) preko više okruženja u kojima se i područja.

## <a name="why-logic-apps"></a>Zašto logike aplikacije?

Logika aplikacije objedinjuje brzine i skalabilnost u prostor za integraciju enterprise.  Jednostavnost korištenja dizajnera različite dostupne okidača i akcije i alata za upravljanje Napredna provjerite središnje vaše API-ji jednostavnije nego ikad.  Tvrtke pomičete prema digitalization, logike aplikacije radite dopušta povezivanje sustavi stariji i najnovije zajedno.

Uz to, s oglednim [Enterprise Integracija račun] [ biztalk] mogu mijenjati veličinu da biste sadržaj za odrasle scenarija integracije pomoću broja u [porukama XML][xml], [trgovina partnera upravljanja][tpm], i još mnogo toga.

- **Alati za dizajn jednostavno je za korištenje** - logike aplikacije može biti definiran završetka do kraja u pregledniku ili pomoću alata za Visual Studio. Započnite s okidač - iz jednostavne rasporeda stvaranja GitHub problem. Zatim orkestrirali bilo koji broj od akcija pomoću obogaćenog galerije poveznike.

- **Povezivanje API-ji jednostavno** -čak i sastavljanje zadatke koji su jednostavni opisuju teško implementirati u kodu. Logika aplikacije olakšava povezivanje raznovrsne sustave. Želite povezati svoje oblaka marketing rješenje lokalnih sustava za naplatu? Želite li centralizirano messaging preko API-ji te sustavima s programa Bus usluge tvrtke? Logika aplikacije su način najbrže, većina pouzdanog isporuka rješenja tih problema.

- **Početak rada brzo iz predložaka** - vam pomoći da započnete smo ste naveli [Galerija predložaka] [ templates] koje omogućuju brzo stvaranje neke uobičajena rješenja. Iz Napredno B2B rješenja za jednostavne SaaS povezivanje, i čak nekoliko koje su samo 'za zabavu' – galerije je najbrži način za početak rada s power logika aplikacije.

- **Proširivanje pečeni-u** – morate connector ne prikazuje? Logika aplikacije namijenjen je korištenju vlastitim API-ji i koda; jednostavno možete stvoriti vlastite API aplikacije da biste koristili kao prilagođeni poveznik ili poziva u [Azure funkcija](https://functions.azure.com) izvršiti Isječci kod na zahtjev. 

- **Konjska snaga realni integraciju** – pokrenite jednostavno, a svakodnevnim da vam je potrebna. Logika aplikacije možete jednostavno prednosti programi BizTalk, Microsoftovim početne Integracija rješenje da biste omogućili integraciju profesionalce da biste sastavili rješenja koje su im potrebne. Saznajte više o [Enterprise Integracija s programom paketa](./app-service-logic-enterprise-integration-overview.md).

## <a name="logic-app-concepts"></a>Logika aplikacije koncepti

Slijede neki od ključne dijelova koje čine sučelje logike aplikacije. 

- **Tijek rada** – logika aplikacija omogućuje grafički modela poslovnih procesa u obliku niza koraka ili tijeka rada.
- **Upravljani poveznika** - logika aplikacije potreban je pristup podatke i usluge. Upravljani poveznika se stvaraju izričito da biste olakšali kada se povezujete s i raditi s podacima. Pogledajte popis poveznika sada dostupna u [upravlja poveznika][managedapis].
- **Okidača** - upravlja poveznici za neke također može poslužiti kao okidač. Okidač pokreće novu instancu tijeka rada koji se temelji na određenim događajima, kao što su dolazak poruke e-pošte ili promjena na vašem računu Azure prostora za pohranu.
-  **Akcije** - svaki korak nakon okidača u tijeku rada zove akciju. Svaku akciju obično mapira operaciju na upravljanih poveznika ili prilagođenih aplikacija API-JA.
- **Paket Enterprise Integracija** - napredniji scenarija integracije, logiku aplikacije sadrži mogućnosti izlaganja s BizTalk. BizTalk je Microsoftov industrijskih početne integracije. Paket Enterprise Integracija poveznika omogućuju vam da jednostavno obuhvaćaju provjere valjanosti, transformaciju i drugih značajki u da biste u aplikaciji logiku tijekove rada.

## <a name="getting-started"></a>Početak rada  

- Za početak rada s aplikacijama logiku slijedite [Stvaranje logiku aplikacije] [ create] vodič.  
- [Primjeri uobičajenih prikaza i scenarijima](app-service-logic-examples-and-scenarios.md)
- [Možete automatizacija poslovnih procesa pomoću aplikacije logike](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Saznajte kako integrirati vaše sustavi logiku aplikacije](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md
