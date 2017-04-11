<properties
pageTitle="Kako ažurirati servis u oblaku | Microsoft Azure"
description="Saznajte kako ažurirati servise u oblaku u Azure. Saznajte kako nastavlja ažuriranje na neki servis u oblaku da biste bili sigurni dostupnost."
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>Kako ažurirati servis u oblaku

## <a name="overview"></a>Pregled
10 000 stopa ažuriranje servis u oblaku i njegove uloge te goste OS, biti u tri koraka. Najprije binarne datoteke i konfiguracijske datoteke za novi servis u oblaku ili verzija OS-a morate prenijeti. Nakon toga Azure rezervira računalnim i mrežni resursi za servisa u oblaku potrebama novu verziju servisa oblaka. Na kraju, Azure izvodi rolling nadogradnje postupno ažurirati klijent u novu verziju ili gost OS, uz čuvanje vašu dostupnost. U ovom se članku govori o detalje o ovom posljednji korak – rolling nadogradnje.

## <a name="update-an-azure-service"></a>Azure servis za ažuriranje

Azure organizira instance vaša uloga u logičke grupe naziva domene nadogradnje (UD). Nadogradnja domene (UD) su logičkih skupova instance uloge koje će se ažurirati kao grupu.  Azure ažurira oblaka servisa jedan UD odjednom, koji omogućuje instance u drugim UDs da biste nastavili posluživanje promet.

Zadani broj nadogradnje domena je 5. Možete navesti različit broj nadogradnje domene tako da uvrstite atribut upgradeDomainCount u datoteku na servis definicije (.csdef). Dodatne informacije o atribut upgradeDomainCount potražite u članku [WebRole sheme](https://msdn.microsoft.com/library/azure/gg557553.aspx) ili [WorkerRole shemu](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Izvođenjem lokalno ažuriranje jednu ili više uloga na servisu Azure ažurira skupove instanci uloga prema nadogradnje domene koje pripadaju. Azure ažuriranja sve instance u određenom nadogradnje domeni – prekid ih ih ažurirati da ih ponovno mreži – prelazi na sljedeću domene. Po zaustavljanje samo instance izvodi u trenutnoj nadogradnje domeni Azure jamči da ažuriranje pojavljuju se s najmanje moguće utjecaj na pokrenuti servis. Dodatne informacije potražite u članku [kako se nastavlja ažuriranje](#howanupgradeproceeds) u nastavku ovog članka.

> [AZURE.NOTE] Dok uvjete **Ažuriranje** i **Nadogradnja** ima malo drugačiju značenje u kontekstu Azure, moguće ih je koristiti naizmjence za procese i opise značajke u ovom dokumentu.

Na servisu morate definirati barem dvije instance uloge za uloge ažurirati lokalno bez isključiti. Ako servis sastoji se od samo jedna pojava jednu ulogu, na servisu neće biti dostupno dok se ne dovrši ažuriranje na istome mjestu.

U ovoj se temi objašnjava sljedeće informacije o Azure ažuriranja:

-   [Dopuštena promjenama servisa tijekom ažuriranje](#AllowedChanges)
-   [Kako se nastavlja nadogradnje](#howanupgradeproceeds)
-   [Vraćanje ažuriranje](#RollbackofanUpdate)
-   [Pokretanje više mutating operacije u tijeku implementacije](#multiplemutatingoperations)
-   [Distribucija uloge svim nadogradnje domenama](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>Dopuštena promjenama servisa tijekom ažuriranje
Sljedeća tablica prikazuje dopuštene promjene na neki servis tijekom ažuriranja:

|Promjena ovlasti za hostiranje, servise i uloge|Lokalno ažuriranje|Postupne (VIP zamjena)|Izbriši, a zatim ponovno implementacije|
|---|---|---|---|
|Verzija operacijskog sustava|Da|Da|Da
|Razina pouzdanosti .NET|Da|Da|Da|
|Veličina virtualnog računala<sup>1</sup>|Da<sup>2</sup>|Da|Da|
|Lokalno spremište postavke|Povećavanje samo<sup>2</sup>|Da|Da|
|Dodavanje ili uklanjanje uloge servisa|Da|Da|Da|
|Broj instanci odgovarajućom ulogom|Da|Da|Da|
|Broj ili vrstu krajnje točke za uslugu|Da<sup>2</sup>|ne|Da|
|Nazive i vrijednosti konfiguracijske postavke|Da|Da|Da|
|Vrijednosti (ali ne imena) konfiguracija postavki|Da|Da|Da|
|Dodavanje novog certifikata|Da|Da|Da|
|Promjena postojećeg certifikata|Da|Da|Da|
|Implementirajte novu šifru|Da|Da|Da|
<sup>1</sup> Promjena veličine ograničen na podskup veličine dostupna za servis u oblaku.

<sup>2</sup> Potreban je Azure SDK 1,5 ili novije verzije.

> [AZURE.WARNING] Promjena veličine virtualnog računala će uništiti lokalnih podataka.


Tijekom ažuriranje ne podržava sljedeće stavke:

-   Promjena naziva uloge. Uklanjanje, a zatim dodajte uloga pod novim nazivom.
-   Promjena count nadograditi domene.
-   Smanjivanje veličine lokalnog resursa.

Ako želite napraviti ostala ažuriranja definicije funkcioniranju servisa, kao što su smanjivanje veličine lokalnog resursa, morate izvršiti ažuriranja za Zamijeni VIP. Dodatne informacije potražite u članku [Zamjena implementacije](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Kako se nastavlja nadogradnje
Možete odlučiti želite li ažurirati sve uloga na servisu ili jedan uloga u servisu. U svakom slučaju, sve instance ulogama koji se nadograđuje pripadate prva nadogradnje domena su zaustavljena, nadograditi, a ne unese natrag na Internetu. Kada su vratite u mrežni instance na drugu domenu nadogradnje su zaustavljena, nadograditi, a ne unese natrag na Internetu. Servis u oblaku može sadržavati najviše aktivni po jednu nadogradnju. Nadogradnja uvijek izvršiti protiv najnoviju verziju servisa u oblaku.

Sljedeći dijagram prikazuje kako nadogradnje nastavlja Ako nadograđujete svih uloga u servisu:

![Nadogradnja servisa] (media/cloud-services-update-azure-service/IC345879.png "Nadogradnja servisa")

Ovaj sljedeći dijagram prikazuje kako ažuriranje prihod Ako nadograđujete samo jedan uloga:

![Nadogradnja uloga] (media/cloud-services-update-azure-service/IC345880.png "Nadogradnja uloga")  

> [AZURE.NOTE] Prilikom nadogradnje usluge iz jedne instance više instanci na servisu će biti uvezeni prema dolje tijekom izvođenja nadogradnje zbog servisa Azure nadogradnje način. Dostupnost guaranteeing usluge servisa razine ugovor odnosi se samo na servise koji su raspoređeni s više od jedne instance. Sljedeći popis sadrži opis kako podataka na svakom pogonu utječe svaki scenarij nadogradnje servisa Azure:
>
>Ponovno pokrenite računalo VM:
>
-   C: zadržava
-   D: zadržava
-   E: zadržava
>
>Ponovno pokretanje portala:
>
-   C: zadržava
-   D: zadržava
-   E: uništava
>
>Portala Reimage:
>
-   C: zadržava
-   D: uništava
-   E: uništava

>Nadogradnja na mjestu:
>
-   C: zadržava
-   D: zadržava
-   E: uništava
>
>Migracija čvor:
>
-   C: uništava
-   D: uništava
-   E: uništava

>Imajte na umu da, na gornjem popisu pogon E: predstavlja željenu ulogu korijenskom pogonu, a ne smije biti programiranih. Umjesto toga koristite varijablu okruženja % RoleRoot % predstavlja pogon.

>Da biste minimizirali na nedostupnost prilikom nadogradnje servisa jednokratni, implementacije novog servisa više instanci pripremni poslužitelj i izvoditi VIP Zamijeni.

Tijekom automatsko ažuriranje kontrolerom tkanina Azure povremeno vrednuje stanje servisa u oblaku da biste odredili kada je sigurno da biste prošli sljedeći UD. Ovaj procjenu stanja se izvode na temelju po ulogama i smatra instance u sklopu najnovije verzije (odnosno instance iz UDs koji već walked). Provjerava koji Minimalni broj instanci uloge, za svaku ulogu ste postići zadovoljavajuće terminal stanje.

### <a name="role-instance-start-timeout"></a>Uloga Instance Start ograničenja
Kontrolerom tkanina će Pričekajte 30 minuta za svaku instancu uloga dosegne rada stanje. Ako trajanje vremenskog ograničenja minutama, kontrolerom tkanina će i dalje objašnjenje sljedeću pojavu uloge.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>Vraćanje ažuriranje
Azure pruža fleksibilnost u Upravljanje uslugama tijekom ažuriranje tako da dopušta pokretanje dodatne operacije na neki servis, nakon početnog ažuriranje zahtjev je prihvatio kontrolerom tkanina Azure. Vraćanje može izvršiti samo kada ažuriranje (promjena konfiguracije) ili Nadogradnja je u stanju **u tijeku** na implementaciju. Ažuriranje ili nadogradnja smatra se da je u tijeku dok god je barem jedan instanca servisa koji još nije ažuriran u novu verziju. Da biste provjerili je li dopušten je vraćanje, provjerite vrijednost zastavice RollbackAllowed, vratio [Dobiti implementacije](https://msdn.microsoft.com/library/azure/ee460804.aspx) i [Dobili svojstva oblaka servisa](https://msdn.microsoft.com/library/azure/ee460806.aspx) operacija, postavljen na true.

> [AZURE.NOTE] Samo smisla pozivanje vraćanje na **lokalno** ažuriranje ili nadograditi jer VIP zamjena nadogradnje obuhvaćaju zamjene jedan cijeli pokrenute instance servisa s drugim.

Vraćanje u tijeku ažuriranje sastoji se od sljedećih efekata na implementaciju:

-   Sve instance uloge koje ste još nisu ažurirani ili nadograditi na novu verziju su ažurirani ili nadograditi, jer se slučajevima već koristite ciljne verzije servisa.
-   Sve instance uloge koje ste već ažurirati ili nadograditi na novu verziju paketa servisa (\*.cspkg) datoteke ili konfiguracija servisa (\*.cscfg) datoteke (ili obje datoteke) vratit će se prije nadogradnje verziju te datoteke.

To functionally daju se sljedeće značajke:

-   [Vraćanje ažuriranje ili nadogradnja](https://msdn.microsoft.com/library/azure/hh403977.aspx) operacije koja se može pozvati na web-mjesto ažuriranja konfiguracije (koji se prikazuje kada pozivanja [Konfiguraciju implementacije promjena](https://msdn.microsoft.com/library/azure/ee460809.aspx)) ili u okvir za nadogradnju (koji se prikazuje kada pozivanja [Implementaciju nadogradnje](https://msdn.microsoft.com/library/azure/ee460793.aspx)) dok god je barem jedan instancu servis koji još nije ažuriran u novu verziju.
-   Zaključano element i RollbackAllowed element koji se vraćaju kao dio tijelo odgovora operacije [Dobiti implementacije](https://msdn.microsoft.com/library/azure/ee460804.aspx) i [Dobili svojstva oblaka servisa](https://msdn.microsoft.com/library/azure/ee460806.aspx) :
    1.  Element zaključano omogućuje prepoznavanje situacije u operaciji mutating mogu se pozvati na navedeni implementacije.
    2.  RollbackAllowed element omogućuje da biste otkrili kad operacija [Vraćanje ažuriranje ili nadogradnja](https://msdn.microsoft.com/library/azure/hh403977.aspx) možete pozvati na navedeni implementacije.

    Da biste izvršili vraćanje, nemate da biste provjerili na zaključano i RollbackAllowed elemente. Ga suffices da biste potvrdili da je RollbackAllowed postavljen na true. Tih elemenata vraćaju samo ako su ovih metoda pozvati pomoću zaglavlje zahtjev postavljena na "x ms verzija: 2011-10-01" ili noviju verziju. Dodatne informacije o zaglavljima verzijama potražite u članku [Servis za upravljanje verzijama](https://msdn.microsoft.com/library/azure/gg592580.aspx).

U nekim situacijama gdje je vraćanje ažuriranje ili nije podržana Nadogradnja, ovo su na sljedeći način:

-   Smanjenje Lokalni resursi – ako ažuriranje povećava Lokalni resursi za ulogu Azure platforme omogućuju povratak. 
-   Ograničenja kvote – ako je ažuriranje skalu dolje postupak koji se više ne može imati dovoljno kvote računalnim da biste dovršili postupak vraćanja. Azure pretplate je pridružen kvote koja određuje najveći broj jezgri koji se može trošiti tako da sve glavnom računalu servise koji pripadaju tu pretplatu. Ako izvođenje vraćanje zadanog ažuriranja želite odgoditi pretplate putem kvote, a zatim koji vraćanje nije omogućen.
-   Uvjet trkaći - ako početne ažuriranje dovrši, vraćanje nije moguće.

Primjer kada vraćanje ažuriranje može biti korisno je ako koristite postupka [Nadogradnje implementacije](https://msdn.microsoft.com/library/azure/ee460793.aspx) u načinu za ručno kontrolu Brzina kojom glavne nadogradnju na mjestu na vašem Azure hostira servisa izvršavanja odgovor.

Tijekom uvođenje nadogradnje poziva [Implementaciju nadogradnje](https://msdn.microsoft.com/library/azure/ee460793.aspx) u načinu rada za ručno i počnete da biste prošli nadogradnje domene. Ako u nekom trenutku tijekom praćenje nadogradnje, zabilježite pojedinim uloga u prvom nadogradnje domenama pregledate ste prestati reagirati, da biste uputili poziv postupka [Vraćanje ažuriranje ili nadogradnju](https://msdn.microsoft.com/library/azure/hh403977.aspx) na implementaciju će ostaviti nepromijenjen instanci koje su još nisu nadograđena i vraćanje instanci koje ste nadograđena na prethodni paketa i konfiguraciji.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Pokretanje više mutating operacije u tijeku implementacije
U nekim slučajevima možda želite da biste započeli više istodobno mutating operacije u tijeku implementaciji. Na primjer, može imati ažuriranje servisa i dok se ažuriranje se izvršavanja preko usluge, želite li promijeniti neke, npr. vratiti natrag ažuriranje primijenili različite ažuriranje ili čak i brisanje uvođenje. Slučaja u kojoj to može biti potrebno je ako nadogradnje servisa sadrži buggy kod koji uzrokuje instancu nadograđena uloga pritišćite rušiti. U ovom slučaju kontrolerom tkanina Azure nećete moći da biste tijeka primjene koje je nadograditi jer su dobar dovoljno broj instanci nadograđena domeni. Ovaj status je se nazivaju u *zamrzne implementacije*. Uvođenje možete unstick povratak ažuriranje ili svježim ažuriranje preko vrha na Neuspjeh jedan.

Nakon početnog zahtjeva za ažuriranje ili nadogradnje usluge primila kontrolerom tkanina Azure, možete pokrenuti sljedeći mutating operacije. To jest, nemate ćete morati pričekati početne postupak da biste dovršili prije no što počnete druga mutating operacija.

Pokretanje drugi postupak ažuriranja dok je prvog ažuriranja u tijeku će izvesti slično postupak vraćanja. Ako je drugi ažuriranja u način rada za automatsko, prva nadogradnje domena nadogradit će se odmah, vjerojatno oni mogu dovesti do instance iz više domena nadogradnje u tijeku izvanmrežni na istom mjestu u vremenu.

Mutating operacije su na sljedeći način: [Konfiguraciju implementacije promjena](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Implementaciju nadogradnje](https://msdn.microsoft.com/library/azure/ee460793.aspx), [Status uvođenja ažuriranje](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Brisanje implementacijom](https://msdn.microsoft.com/library/azure/ee460815.aspx)i [Vraćanje ažuriranje ili nadogradnja](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Dvije operacije, [Dobiti implementacije](https://msdn.microsoft.com/library/azure/ee460804.aspx) i [Dobili svojstva oblaka servisa](https://msdn.microsoft.com/library/azure/ee460806.aspx), vratite zastavicu zaključano koje možete pregledavaju da biste odredili hoće li mutating postupak mogu se pozvati na navedeni implementacije.

Da biste nazvali verzija od ovih metoda vraća zastavice zaključano, morate postaviti zahtjev za zaglavlje "x ms verzija: 2011-10-01" ili novijeg. Dodatne informacije o zaglavljima verzijama potražite u članku [Servis za upravljanje verzijama](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Distribucija uloge svim nadogradnje domenama
Azure ravnomjerno distribuira pojavljivanja uloge preko određeni broj nadogradnje domene koje se može konfigurirati kao dio datoteke definicije (.csdef) servisa. Maksimalni broj nadogradnje domene iznosi 20 i Zadana vrijednost je 5. Dodatne informacije o načinu izmjene datoteku definicije servisa potražite u članku [Azure servisa definicija sheme (.csdef datoteka)](cloud-services-model-and-package.md#csdef).

Ako, na primjer, ako vaša uloga ima deset instance, po zadanom svaku nadogradnje domenu sadrži dvije instance. Ako vaša uloga ima 14 instance, zatim četiri nadogradnje domene sadrži tri instance, a petoj domene sadrži dvije.

Nadogradnja domena koji su povezani s polje index: prvi nadogradnje domena ima Identifikacijskim brojem 0, a drugi nadogradnje domena ima Identifikacijskim brojem 1 i tako dalje.

Sljedeći dijagram prikazuje kako distribuirati servisa ne sadrži dvije uloge kada servis definira dvije nadogradnje domene. Servis je pokrenut osam instance web uloge i devet pojavljivanja ulogu suradnika.

![Distribucija nadogradnje domene] (media/cloud-services-update-azure-service/IC345533.png "Distribucija nadogradnje domene")

> [AZURE.NOTE] Imajte na umu Azure određuje kako se pojave dodijeliti svim nadogradnje domenama. Nije moguće navesti domene u kojoj se dodijeliti koje će se instance.

## <a name="next-steps"></a>Daljnji koraci
[Kako upravljati servise u Oblaku](cloud-services-how-to-manage.md)  
[Upute za praćenje servise u Oblaku](cloud-services-how-to-monitor.md)  
[Konfiguriranje servisa u Oblaku](cloud-services-how-to-configure.md)  
