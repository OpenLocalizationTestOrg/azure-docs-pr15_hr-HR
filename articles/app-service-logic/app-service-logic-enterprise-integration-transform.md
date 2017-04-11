<properties 
    pageTitle="Pregled paketa za integraciju Enterprise | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
    description="Korištenje značajki Enterprise Integracija s programom paketa da biste omogućili postupak i integracija scenariji za tvrtke putem servisa za aplikaciju Microsoft Azure" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-xml-transforms"></a>Enterprise Integracija s XML pretvorbe

## <a name="overview"></a>Pregled
Poveznik za pretvorbu Enterprise Integracija pretvara podatke iz jednog oblika u drugi oblik. Na primjer, možda ulaznu poruku koja sadrži trenutni datum u obliku YearMonthDay. Pretvaranjem omogućuju preoblikovati datum koji će biti u obliku MonthDayYear.

## <a name="what-does-a-transform-do"></a>Čemu služi pretvaranjem?
Pretvorbe koje se nazivaju i kartu, sastoji se od izvorne XML sheme (input) i ciljne XML sheme (Izlaz). Da biste jednostavnije upravljali i upravljanje podacima, uključujući niz operacije, uvjetno dodjele, aritmetički izraze, datum vrijeme formatters pa čak i ponavljanje konstrukta možete koristiti različite ugrađene funkcije.

## <a name="how-to-create-a-transform"></a>Kako stvoriti pretvaranjem?
Pretvorba/Mapiraj možete stvoriti i pomoću Visual Studio [SDK Integracija Enterprise](https://aka.ms/vsmapsandschemas). Kada završite stvaranje i testiranje transformacija, transformacija prenijeti na račun za integraciju. 

## <a name="how-to-use-a-transform"></a>Kako koristiti pretvaranjem
Nakon transformacija prenijeti na račun za integraciju, možete je koristiti za stvaranje logike aplikacije. Aplikaciju logike pokreće pretvorbama svaki put kada se pokrene aplikaciju logike (i unos sadržaja koje je potrebno pretvoriti).

**Evo nekoliko koraka da biste koristili pretvaranjem**:

### <a name="prerequisites"></a>Preduvjeti 
U pretpregledu, morat ćete:  

-  [Stvaranje spremnika programa Azure funkcije] (https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Stvaranje spremnika programa Azure funkcije")  
-  [Dodavanje funkcija u spremnik funkcije Azure] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "Ovaj predložak stvara webhook temelji C# azure funkcije s mogućnostima pretvorbe da biste koristili u scenarijima za integraciju logike aplikacije")    
-  Stvorite račun integracije i dodati kartu  

>[AZURE.TIP] Obratite pozornost na naziv funkcije Azure spremnik i funkciju Azure, morate ih u sljedećem koraku.  

Sad kad ste snimili brigu o preduvjete, vrijeme je za stvaranje logike aplikacije:  

1. Stvorite logiku aplikacije i [povezati ga na račun za integraciju](./app-service-logic-enterprise-integration-accounts.md "Naučite povezivanje poslovnog subjekta Integracija logike aplikaciju") koja sadrži kartu.
2. Dodavanje okidač **zahtjev - kada an HTTP zahtjev je primljen** logike aplikacije  
![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. Dodajte akciju **Transformacija XML** tako da prvo odabir **dodali akciju**   
![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. Unesite riječ *transformirati* u okvir za pretraživanje da bi se sve akcije u onu koju želite koristiti za filtriranje  
![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Odaberite akciju **Transformacija XML**   
![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)  
6. Odaberite **SPREMNIK FUNKCIJA** koja sadrži funkciju koju ćete koristiti. To je naziv spremnika Azure funkcije koju ste stvorili ranije u sljedećim koracima.
7. Odaberite **FUNKCIJU** koju želite koristiti. To je naziv funkcije Azure koji ste prethodno stvorili.
8. Dodavanje **sadržaja** koji će transformacija XML. Imajte na umu da možete koristiti bilo koji XML podataka koje ste primili u HTTP zahtjev kao na **sadržaja**. U ovom primjeru odaberite tijelu zahtjeva za HTTP koja ga je pokrenula logike aplikacije.
9. Odaberite naziv **mape** koju želite koristiti za izvođenje transformaciju. Karte već mora biti na vašem računu integracije. U prethodnom koraku već dodijelili pristup logike aplikacije na račun za integraciju koja sadrži kartu.
10. Spremanje rezultata rada  
![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

Sada ste završili postavljanje karti. U aplikaciji stvarnog svijeta, trebali biste spremiti transformiranih podataka u aplikaciji LOB kao što su SalesForce. Možete jednostavno kao akciju da biste poslali izlaz transformacija Salesforce. 

Sada možete testirati i vaše pretvorbe tako da zahtjev HTTP krajnjoj.  

## <a name="features-and-use-cases"></a>Značajke i korištenje slučajeva

- Transformaciju stvorene u kartu mogu biti jednostavne, kao što su kopiranje ime i adresu iz jednog dokumenta u drugu. Ili možete stvoriti složenije transformacije pomoću karte Izlaz u-tvorničke operacije.  
- Više mapa operacije ili funkcije dostupnih lako, uključujući nizovi, Funkcije datuma vremena i tako dalje.  
- To možete učiniti kopiju Izravni podataka između sheme. Mapiranje obuhvaćeno SDK, riječ je lako i svodi nacrtate crtu koja povezuje elemenata u shemi izvora njihove verzija u shemi odredište.  
- Kada stvorite kartu, prikazujete grafički prikaz karte, koji pokazuju sve odnose i veza koje ste stvorili.
- Pomoću značajke Test karte da biste dodali ogledne XML poruke. Jednostavne klikom, možete testirati kartu koju ste stvorili i pogledajte generirani izlaza.  
- Prijenos postojeće karte  
- Obuhvaća podršku za XML oblik.


## <a name="learn-more"></a>uči više
- [Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o paket Enterprise Integracija")  
- [Dodatne informacije o kartama] (./app-service-logic-enterprise-integration-maps.md "Informirajte se o enterprise Integracija karte")  
 