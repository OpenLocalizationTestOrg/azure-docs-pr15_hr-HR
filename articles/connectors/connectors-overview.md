<properties
    pageTitle="Pregled logike aplikacije poveznika | Microsoft Azure"
    description="Pregled poveznika koji se mogu koristiti u aplikaciji logike"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>U aplikaciji logike pošte pomoću poveznika

Poveznika omogućuje brz pristup događaja, podataka i akcije services, protokola i platformama.  Cijeli popis poveznika koji podržava logiku aplikacije mogu [se tu nalaze](apis-list.md).  Poveznika mogu se koristiti kao okidač ili akcija u aplikaciji logike, a možda ćete morati konfigurirani *vezu* da biste koristili (na primjer: dopuštanja Twitter račun za pristup ili objavili na njih u vaše ime).

## <a name="basics"></a>Osnove

Poveznika su smještene services možete pristupiti kao dio logike aplikacije za integraciju s drugih servisa kao što je Dynamics, Azure, Salesforce, [i više](apis-list.md).  Su implementiran i upravlja Microsoft, pa možete izraditi u tijekove rada integracije s mjerilo, propusnost i sigurnost snimanja brigu o.  Možete dodati poveznik logike aplikacije pretraživanje, a zatim odaberete poveznik akcija ili pokretanje u odjeljku **Prikaži Microsoft upravljani API-ji**.

![Izbornik akcija za odabir okidača][1]

Svaki poveznik akcija ili okidača će imati njegov skup svojstava koja možete konfigurirati.  Kliknite gumbe informacije da biste saznali više o akcija ili referencu njegov dokumentaciju [Da biste saznali više](apis-list.md).

Ako želite integrirati s usluga ili API koja nije još poveznika, možete proširiti logike aplikacije putem [prilagođenog poveznika](../app-service-logic/app-service-logic-create-api-app.md) ili samo putem protokola kao što su HTTP poziva izravno na servis.

## <a name="triggers"></a>Okidača

Neke poveznike imati okidač, što znači da se događaj iz tog poveznika će pokrenuti logike aplikacije i prenesite podatke kao dio okidača.  Okidač uvijek je u prvom koraku u aplikaciji logike.  Popularne okidača obuhvaćaju operacija kao što su:
 
 * Ponavljanje - pokrenuti svaki sat
 * Kada je primljen HTTP zahtjev
 * Kada se stavke dodaju reda čekanja
 * Kada je primio poruku e-pošte
 
Nekih okidača će pokrenuti izravnih poruka putem obavijest aplikaciju logike će se dogoditi događaja, a drugi će vam intervala ponavljanja konfigurirali učestalosti aplikaciju logike provjerit će služba za događaj (najviše svakih 15 sekundi).  

Nakon primitka događaja će pokrenuti aplikaciju logike pokrenuti i pokrenut će se akcije u tijeku rada.  I moći pristupiti podacima iz okidača u tijeku rada (primjerice okidača 'na novu tweet' proći kroz na tweet u Pokreni).

## <a name="actions"></a>Akcija

Većina poveznika imati jedan ili više akcija koje možete izvršavati u sklopu tijeka rada.  Akcije su sve korake koji se pokreću kada je Pokreni pokrenuta iz okidač.  Da biste dodali programa kliknite akcijski gumb **Novog koraka** i potražiti poveznik koji želite koristiti.  Kada je odabrana (i nakon konfiguriranja sve [veze](#connections) koje možda će vam trebati) vidjet ćete na kartici akcije možete konfigurirati.  Odaberite podatke iz prethodnih koraka tako da kliknete na bilo kojoj od tokena izlaze ili unesite u bilo kojem konfiguraciji po potrebi.

![Konfiguriranje poveznik akcija][2]

## <a name="connections"></a>Veze

Većina poveznika zahtijevati konfigurirati *vezu* da biste koristili poveznik.  Konfiguracija sve korisničko ime ili vezu potrebna za pristup poveznik nije *veze* .  Poveznika koje koriste OAuth, stvorite vezu znači da Prijava na servis (kao što je Office 365, Salesforce ili GitHub) koju mogu šifrirane i sigurno pohranjene u Azure tajnu spremištu token za pristup.  Ostale poveznika (kao što su FTP i SQL) zahtijevaju vezu koja sadrži konfiguracije kao što su adresa poslužitelja, korisničko ime i lozinku.  Ove konfiguracije pojedinosti veze i šifriraju se i sigurno pohranjuju.  Veza će se omogućiti pristup servisu za sve dok se omogućuje servis.  Za Azure Active Directory OAuth veze (kao što je Office 365 i Dynamics) nastavit možete beskonačno osvježavanje token za pristup.  Stavite ograničenja ostalim servisima možda koliko možete koristiti token bez osvježavanja.  Općenito govoreći određene akcije kao što su promjena lozinke prestaju svih tokena programa access.  

Veza možete prikazati i upravlja u Azure kliknite **Pregledaj** , a zatim odaberete **API veze**.  API veze resursa možete prikaz, uređivanje, ažuriranje i ponovno autorizirali sve veze koje ste stvorili.

## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje prve logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Informirajte se zajednički koristi i primjeri logike aplikacije](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Početak rada s okidačima Integracija enterprise i akcije](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png