<properties
   pageTitle="Pomoću poveznika Azure resursa u aplikacijama logike | Aplikacije servisa za Microsoft Azure"
   description="Kako stvoriti i konfigurirati aplikaciju Azure resursa poveznika ili API-JA i koristiti u aplikaciji logike u aplikacije servisa za Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Početak rada s poveznik za Azure resursa i njezino dodavanje logike aplikacije
>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju.

Konektora Azure resurs za lakše upravljanje resursima Azure unutar aplikacije logike.

## <a name="create-the-azure-resource-connector"></a>Stvaranje poveznik za Azure resursa
Da biste koristili aplikaciju API-jem poveznik za Azure resursa, morate najprije stvoriti instancu ga. To možete učiniti bilo u istoj razini prilikom stvaranja logike aplikacije ili tako da odaberete Azure resursa Upravitelj poveznik API aplikaciju iz trgovine Azure Marketplace.

Da biste konfigurirali, imate koje morate postaviti gore glavni servisa s dozvolama za učinite ono je želite raditi u Azure. Sve pozive bit će na-ime-od upravitelja ovaj servis koji ste postavili. Time se omogućuje vam opsega zajedničko korištenje samo točno što želite učiniti i ništa više.

Neven Ebbo sadrži napisan [sjajno blogu](http://blog.davidebbo.com/2014/12/azure-service-principal.html) o tome kako postaviti. Pratite sve upute i **ID klijenta**, **ID klijenta** i **tajna**. Ta tri polja, uz **ID pretplate**su koje su potrebne za konfiguriranje poveznik.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>Korištenje resursa poveznik Azure u dizajneru logike aplikacije
### <a name="trigger"></a>Pokretanje
Postoje dvije okidača koje su podržane u poveznik:

Ime | Opis
---- | -----------
Događaj | Pokretanje kada se pojavi događaj resursa u svoju pretplatu.
Metrika presjek praga. |  Pokretanje kada metrike ispunjava određeni prag.

### <a name="action"></a>Akcija

Isto tako, unesete velik broj akcija unutar Azure pretplatu:

Grupa **resursa** možete učiniti sljedeće:

Ime | Opis
---- | -----------
Popis grupa resursa | Popis svih grupa resursa u pretplatu.
Početak grupa resursa | Pronađite grupu resursa, njegov id.
Stvaranje grupa resursa | Stvaranje ili ažuriranje grupu resursa.
Brisanje grupe resursa | Izbrisati grupu resursa.

Za **resurse** možete učiniti sljedeće:

Ime | Opis
---- | -----------
Popis resursa | Popis resursa u svoju pretplatu tako da različite vrste filtara.
Početak resursa | Početak jedan resurs po resursa ID-a.
Stvaranje ili ažuriranje resursa | Stvaranje resursa ili ažurirati postojeće resursa. Navedite sva svojstva za taj resurs.
Akcija resursa |  Izvedite druge akcije na resurs. Morate znati naziv akcije i opseg koja vas vodi Time (ako ih ima).
Brisanje resursa | Brisanje resurs.

Davateljima **Resursa** možete učiniti sljedeće:

Ime | Opis
---- | -----------
Davatelji popis resursa | Popis svih davatelja usluga dostupnih resursa u pretplate.

U slučaju **Implementacije grupu resursa** možete učiniti sljedeće:

Ime | Opis
---- | -----------
Popis implementacije | Popis svih implementacije u grupu resursa.
Početak implementacije | Dobiti implementacije predložak tako da njegov ID-a.
Stvaranje implementacije | Stvori novu implementaciju grupiranje resursa unosom predloška.

Za **događaje** o resursima možete učiniti sljedeće:

Ime | Opis
---- | -----------
Početak događaja | Pronađite događaje u pretplatu ili resursa.

Za **određene parametre** o resursima možete učiniti sljedeće:

Ime | Opis
---- | -----------
Početak mjerenja | Početak metrike resursa ID-a.

## <a name="do-more-with-your-connector"></a>Bolje iskorištavanje vaše poveznika
Sad kad je poveznik stvorili, možete je dodati tvrtke tijek logike aplikacija. U odjeljku [koje su aplikacije logike?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Ako želite započeti s Azure logike aplikacije prije registracije za račun za Azure, idite na [pokušajte logike aplikacije](https://tryappservice.azure.com/?appservice=logic), gdje možete odmah stvorite short-lived starter logike aplikaciju u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

Prikaz reference Swagger REST API-JA na [poveznika i API aplikacija referenca](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md
