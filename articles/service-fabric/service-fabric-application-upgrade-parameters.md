
<properties
   pageTitle="Nadogradnja aplikacije: Nadogradnja parametara | Microsoft Azure"
   description="U članku se opisuje parametara vezane uz nadogradnju servisa tkanina aplikaciju, uključujući web-mjesto provjere stanja da biste izvršili i u okvir za pravila automatski poništiti nadogradnju."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>



# <a name="application-upgrade-parameters"></a>Parametri za nadogradnju aplikacije

U ovom se članku opisuju razne parametre koje se primjenjuju tijekom nadogradnje aplikacije tkanina servisa Azure. Parametri obuhvaćaju naziv i verziju aplikacije. Su knobs koja određuju istek i provjere stanja koja su primijenjena tijekom nadogradnje, a mogu navesti pravila koja morate zatvoriti kada ne uspije nadogradnju.


<br>

| Parametar | Opis |
| --- | --- |
| ApplicationName | Naziv aplikacije u kojoj se nadograđuje. Primjeri: tkanina: / VisualObjects, tkanina: / ClusterMonitor  |
| TargetApplicationTypeVersion | Upišite verziju aplikacije koje nadogradnje ciljeve. |
| FailureAction | Akcija zauzima tkanina servisa prilikom nadogradnje neće uspjeti. Program možda vraćen prije ažuriranja verzija (Vraćanje) ili nadogradnje može se zaustaviti na trenutnu domenu za nadogradnju. U potonjem slučaju načina nadogradnje će se promijeniti ručno. Dopuštene vrijednosti su vraćanje i ručno. |
| HealthCheckWaitDurationSec | Vrijeme čekanja (u sekundama) nakon nadogradnje je završio nadogradnje domeni prije tkanina servisa vrednuje stanja aplikacije. U ovom trajanje i može se smatrati vrijeme aplikaciju trebao bi biti pokrenut prije nego što ga možete smatrati dobar. Ako provjera stanja prolazi, postupak nadogradnje nastavlja se na sljedeći nadogradnje domenu.  Ako provjera stanja ne uspije, servis tkanina čeka interval (UpgradeHealthCheckInterval) provesti Provjera stanja ponovno dok je otvoriti u HealthCheckRetryTimeout. Zadane i preporučena vrijednost je 0 sekundi. |
| HealthCheckRetryTimeoutSec | Trajanje (u sekundama) taj servis tkanina i dalje da biste izvršili procjenu stanja prije deklariranje nadogradnje kao nije uspjelo. Zadano je 600 sekundi. U ovom trajanje pokreće kad zbirka dostigne HealthCheckWaitDuration. U ovom HealthCheckRetryTimeout tkanina servis možda izvođenje višestruke provjere stanja od stanja računala. Zadana vrijednost je 10 minuta i trebali biste moguće je prilagoditi komponenta za svoju aplikaciju. |
| HealthCheckStableDurationSec | Trajanje (u sekundama) da biste provjerili je li aplikacija stabilan prije premještanja na sljedeći nadogradnje domenu ili dovršavanje nadogradnje. Da biste spriječili neprepoznate promjene stanja desnom nakon provjere stanja provodi koristi se tog trajanja čekanja. Zadana vrijednost je 120 sekundi, a trebali biste moguće je prilagoditi komponenta za svoju aplikaciju. |
| UpgradeDomainTimeoutSec | Maksimalno vrijeme (u sekundama) za nadogradnju jedan nadogradnje domene. Ako je dostigne ovog vremenskog ograničenja, nadogradnje zaustavlja i nastavlja na temelju postavku za UpgradeFailureAction. Zadana vrijednost je nikad (beskonačno) i mora se prilagoditi komponenta za svoju aplikaciju. |
| UpgradeTimeout | Prekoračeno (u sekundama) koji se primjenjuje za cijelu nadogradnju. Ako je dostigne ovog vremenskog ograničenja, nadogradnje tabulatora i UpgradeFailureAction se pokreće. Zadana vrijednost je nikad (beskonačno) i mora se prilagoditi komponenta za svoju aplikaciju. |
| UpgradeHealthCheckInterval | Učestalost status stanja potvrđen. Ovaj parametar je navedena u odjeljku ClusterManager _klaster_ _manifesta_, a nije naveden kao dio nadogradnje cmdlet. Zadana vrijednost je 60 sekundi.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Servis stanja procjenu tijekom nadogradnje aplikacije

<br>
Procjena kriterij stanja nije obavezno. Ako kriterij za procjenu stanje nisu navedeni prilikom pokretanja nadogradnju, servis tkanina koristi pravila stanja za programe navedene u ApplicationManifest.xml instance aplikacije.


<br>

| Parametar | Opis |
| --- | --- |
| ConsiderWarningAsError | Zadana je vrijednost False. Smatraj događaje stanja upozorenje za aplikaciju pogreške prilikom procjene stanja računala tijekom nadogradnje. Prema zadanim postavkama tkanina usluge rezultirati upozorenje stanja događaji biti pogreške (pogreške) tako da je nadogradnja možete nastaviti čak i ako postoje događaje upozorenja.   |
| MaxPercentUnhealthyDeployedApplications | Zadane i preporučena vrijednost je 0. Navedite najveći broj (pogledajte [odjeljak stanje](service-fabric-health-introduction.md)) distribuiranih aplikacija koje se mogu dobro prije aplikacije smatra dobro, a ne uspije nadogradnje. Ovaj parametar određuje stanja aplikacije na čvor i pomaže u otkrivanju probleme tijekom nadogradnje. Obično replike aplikacije se raspoređivačem na druge čvor, koji omogućuje računala da se prikazuje dobar zato što omogućuje nadogradnju da biste nastavili. Navođenjem izričite MaxPercentUnhealthyDeployedApplications stanje servisa tkanina možete brzo otkrije problem s Aplikacijski paket i lakše stvarali nije uspjelo brzo nadogradnje. |
| MaxPercentUnhealthyServices | Zadane i preporučena vrijednost je 0. Navedite maksimalni broj services u instanci aplikacije koje mogu biti dobro prije aplikacije smatra dobro, a ne uspije nadogradnje. |
| MaxPercentUnhealthyPartitionsPerService | Zadane i preporučena vrijednost je 0. Navedite maksimalni broj particije na servisu koji mogu biti dobro prije nego što se smatra se dobro servis. |
| MaxPercentUnhealthyReplicasPerPartition | Zadane i preporučena vrijednost je 0. Odredite maksimalan broj replike u particije koja može biti dobro prije particija smatra dobro. |
| UpgradeReplicaSetCheckTimeout | **Bez praćenja stanja servisa**– unutar jedne domene nadogradnje servisa tkanina pokuša da biste bili sigurni da su dostupni dodatni instanci servisa. Ako je broj instanci cilj više od jedne, servis tkanina čeka više instanci da bi bio dostupan na vrijednost Maksimalna ograničenja. U ovom isteka naveden je pomoću UpgradeReplicaSetCheckTimeout svojstva. Ako Prekoračenje vremena istekne, servis tkanina nastavlja na nadogradnju, bez obzira na to koliko je instanci servisa. Ako je broj instanci cilj jedan, servis tkanina Pričekajte i odmah nastavlja na nadogradnju. **S praćenjem stanja servisa**– unutar jedne domene nadogradnje servisa tkanina pokušava provjerite imaju li skup replike kvorum. Servis tkanina čeka kvorum da bi bio dostupan do isteka Maksimalna vrijednost (određen svojstvo UpgradeReplicaSetCheckTimeout). Ako Prekoračenje vremena istekne, servis tkanina nastavlja na nadogradnju, bez obzira na to kvorum. Ta je postavka skupa kao nikad (beskonačno) kada vodoravnim naprijed i 900 sekunde kada povratak. |
| ForceRestart | Ako ažurirate konfiguraciju ili paketa podataka ne ažurirate kod usluge, servis je ponovno pokrenuti samo ako je svojstvo ForceRestart postavljeno na true. Po dovršetku ažuriranja servisa tkanina obavještava servis novog konfiguracije paketa ili paketa podataka dostupna. Servis je odgovorna za primjenu promjene. Ako je potrebno, ponovno će sam servis. |



<br>
<br>
MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService i MaxPercentUnhealthyReplicasPerPartition kriterija možete navesti po vrsti servisa za instancu aplikacije. Postavljanje tih parametara po usluga omogućuje za aplikaciju koja će sadržavati različite servise vrste s različitim procjene pravila. Na primjer, vrstu pristupnika bez praćenja stanja servisa može imati MaxPercentUnhealthyPartitionsPerService koji se razlikuje od servisa vrstu s praćenjem stanja modul za određenu aplikaciju instancu.

## <a name="next-steps"></a>Daljnji koraci

[Nadogradnja vašeg računala pomoću Visual Studio](service-fabric-application-upgrade-tutorial.md) vodit će vas kroz nadogradnju aplikacije pomoću Visual Studio.

[Nadogradnja aplikacije pomoću ljuske](service-fabric-application-upgrade-tutorial-powershell.md) vodit će vas kroz nadogradnju aplikacije pomoću komponente PowerShell.

Provjerite svoje nadogradnje aplikacije kompatibilne Naučite koristiti [Serijalizacije podataka](service-fabric-application-upgrade-data-serialization.md).

Saznajte kako koristiti napredne funkcije tijekom nadogradnje aplikacije za [Dodatne teme](service-fabric-application-upgrade-advanced.md).

Riješite uobičajene probleme u aplikaciji nadogradnje tako da upućuju na korake [Za otklanjanje poteškoća nadogradnje aplikacije](service-fabric-application-upgrade-troubleshooting.md).
 
