<properties
   pageTitle="Migracija s Orchestrator Azure Automatizacija | Microsoft Azure"
   description="U članku se opisuje kako migrirati paketi runbooks i Integracija sa Orchestrator centar sustava za automatizaciju Azure."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migracija s Orchestrator Azure Automatizacija (Beta)

Runbooks u [Orchestrator centar sustava](http://technet.microsoft.com/library/hh237242.aspx) temelje se na aktivnosti iz Integracija s programom paketa koji se pišu namijenjenu Orchestrator dok runbooks u automatizaciji Azure temelje se na Windows PowerShell.  [Grafički runbooks](automation-runbook-types.md#graphical-runbooks) u automatizaciji Azure imaju slične izgled Orchestrator runbooks s njihovim aktivnostima koji predstavlja PowerShell cmdleti za podređeni runbooks i resursi.

[Komplet alata migracije Orchestrator za centar sustava](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) sadrži alate za pomoć u Pretvorba runbooks iz Orchestrator Azure automatizaciju.  Osim pretvorbe runbooks sami, morate pretvoriti paketi za integraciju s aktivnostima koje koriste u runbooks module za integraciju pomoću cmdleta ljuske Windows PowerShell.  

Slijedi Osnovni postupak pretvaranja Orchestrator runbooks Azure automatizaciju.  Svaki od ovih koraka opisani u sljedećim odjeljcima.

1.  Preuzimanje [Alata za migraciju sustava centar za Orchestrator](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) koji sadrži alate i moduli koji se spominju u ovom članku.
2.  Uvesti [aktivnosti standardni modul](#standard-activities-module) Azure automatizaciju.  To obuhvaća pretvorene verzijama standardni Orchestrator aktivnosti koje se može koristiti pretvorene runbooks.
3.  Uvesti [Sustava centra Orchestrator Integracija modula](#system-center-orchestrator-integration-modules) Azure Automatizacija za te paketi Integracija koristi vaš runbooks koji pristupiti centru sustava.
4.  Pretvorite prilagođeni i drugih proizvođača paketi Integracija [Integracija s programom paketa pretvornika](#integration-pack-converter) i uvesti Azure automatizaciju.
5.  Pretvaranje Orchestrator runbooks [Runbook pretvornik](#runbook-converter) i instalirajte u automatizaciji Azure.
6.  Ručno stvoriti potrebne Orchestrator imovine u Azure Automatizacija jer pretvornik Runbook pretvoriti ove resurse.
7.  Konfiguriranje [Radnih Runbook hibridnog](#hybrid-runbook-worker) u centru za lokalnih podataka da biste pokrenuli pretvorene runbooks koja će imati pristup Lokalni resursi.

## <a name="service-management-automation"></a>Automatizacija usluga upravljanja

[Automatizacija usluga upravljanja](http://technet.microsoft.com/library/dn469260.aspx) (SMA) sprema i pokreće se u Centar za lokalne podatke kao što su Orchestrator runbooks, a kao Azure Automatizacija koristi iste Integracija module. [Pretvornik Runbook](#runbook-converter) pretvara Orchestrator runbooks grafički runbooks kroz koje nisu podržane u SMA.  Možete i dalje instalirati [Standardni modul aktivnosti](#standard-activities-module) i [Sustava centra Orchestrator Integracija module](#system-center-orchestrator-integration-modules) u SMA, ali morate ručno [dopune vaše runbooks](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hibridno Runbook tempiranja

Runbooks u Orchestrator su pohranjena na poslužitelju baze podataka i izvoditi na poslužiteljima runbook oboje u centru za lokalne podatke.  Runbooks u automatizaciji Azure se pohranjuju u Azure oblaka i jer se može pokrenuti u centru za lokalne podatke pomoću [Hibridnog Runbook tempiranja](automation-hybrid-runbook-worker.md).  Ovo je kako će pokrenuti obično runbooks pretvoriti iz Orchestrator jer osmišljeni su pokrenuti na lokalne poslužitelje.

## <a name="integration-pack-converter"></a>Integracija s programom paketa pretvornika

Pretvornik paket Integracija pretvara Integracija pakete koje su stvorene pomoću [Alata Orchestrator Integracija (OIT)](http://technet.microsoft.com/library/hh855853.aspx) za module za integraciju komponente Windows PowerShell možete uvesti u Automatizacija Azure ili Automatizacija usluga upravljanja na temelju.  

Kada pokrenete paketa pretvornika integracije, predstavljanja pomoću čarobnjaka za omogućit će vam da biste odabrali datoteku paketa (.oip) integracije.  Čarobnjak zatim popisi aktivnosti uključeni u taj paket integracije i omogućuje odabir koji će se premjestiti.  Kada dovršite čarobnjak, stvara sustava Integracija modul koji sadrži odgovarajuće cmdlet za svaku aktivnosti u izvornom Integracija s programom paketa.


### <a name="parameters"></a>Parametri

Svojstva aktivnosti u paketu za integraciju pretvaraju se u parametre odgovarajuće cmdlet u modulu za integraciju.  Windows PowerShell cmdleti imaju skup [uobičajenih parametre](http://technet.microsoft.com/library/hh847884.aspx) koje je moguće koristiti sve cmdleta sustava.  Na primjer, opširno parametar - uzrokuje cmdlet za izlaz detaljne informacije o njegov operacija.  Nema cmdlet možda parametar s istim nazivom kao uobičajenih parametar.  Ako aktivnost svojstvo s istim nazivom kao uobičajenih parametar, čarobnjak će zatraži da unesete neki drugi naziv parametra.

### <a name="monitor-activities"></a>Praćenje aktivnosti

Monitor runbooks u Orchestrator pokrenuti s [monitora aktivnosti](http://technet.microsoft.com/library/hh403827.aspx) i koristiti neprestano čekanje treba pozvati određeni događaj.  Azure Automatizacija ne podržava monitor runbooks, pa će se pretvoriti sve nadzor aktivnosti u paket za integraciju.  Umjesto toga rezervirano mjesto cmdlet se stvara u modulu Integracija nadzornik aktivnosti.  Ovaj cmdlet sadrži funkciju, no njime bilo koji pretvorenu runbook koji koristi za instalaciju.  Ovaj runbook će moći pokrenuti u automatizaciji Azure, ali se može instalirati tako da ga možete izmijeniti.

### <a name="integration-packs-that-cannot-be-converted"></a>Integracija s programom paketa koji nije moguće pretvoriti

Integracija s programom paketa koje su stvorene pomoću OIT nije moguće pretvoriti pomoću pretvornika za integraciju paketa. Postoje neki paketi Integracija pruža Microsoft koji trenutno nije moguće pretvoriti u ovom alatu.  Pretvoreni verzijama te paketi za integraciju su [za preuzimanje](#system-center-orchestrator-integration-modules) da bi se instaliraju Automatizacija Azure ili Automatizacija usluga upravljanja.


## <a name="standard-activities-module"></a>Modul za standardne aktivnosti

Orchestrator sadrži skup [standardne aktivnosti](http://technet.microsoft.com/library/hh403832.aspx) koje nisu obuhvaćeni paketa za integraciju, ali koristi mnogo runbooks.  Modul za standardne aktivnosti je modul za integraciju koja obuhvaća cmdlet koji je jednak za svaku od sljedećih aktivnosti.  Morate instalirati modul za integraciju u Azure automatizacije prije uvoza bilo koji pretvorenu runbooks koje koriste standard aktivnosti.

Osim podrške pretvorene runbooks cmdleta u modulu standardne aktivnosti mogu se netko upoznati s Orchestrator izraditi novi runbooks u automatizaciji Azure.  Dok funkcionalnost sve standardne aktivnosti može izvoditi cmdleta sustava, oni mogu drugačije funkcionirati.  Cmdleti za u modulu pretvorene standardne aktivnosti će funkcionira na isti način kao njihove odgovarajuće aktivnosti i koristite iste parametre.  To može pomoći postojeće autora runbook Orchestrator u svoje prijelaz Automatizacija Azure runbooks.

## <a name="system-center-orchestrator-integration-modules"></a>Centar za sustav Orchestrator Integracija moduli

Microsoft pruža [Integracija paketi](http://technet.microsoft.com/library/hh295851.aspx) za izgradnju runbooks da biste automatizirali komponente centar sustava i druge proizvode.  Neke od tih paketi za integraciju trenutno temeljen na OIT, ali ne trenutno može pretvoriti u module za integraciju zbog poznatim problemima.  [Sustav centar Orchestrator Integracija module](https://www.microsoft.com/download/details.aspx?id=49555) obuhvaća pretvorene verzijama te integraciju paketi koji se mogu uvesti u Automatizacija Azure i Automatizacija usluga upravljanja.  

RTM verzijom ovaj alat ažurirane verzije paketa za integraciju OIT koju je moguće pretvoriti pomoću pretvornika za integraciju paket na temelju će se objaviti.  Smjernice i se navesti kao pomoć u pretvaranje runbooks pomoću aktivnosti iz paketa Integracija s programom koji se ne temelji na OIT.

## <a name="runbook-converter"></a>Pretvornik Runbook

Pretvornik Runbook pretvara Orchestrator runbooks u [grafički runbooks](automation-runbook-types.md#graph-runbooks) moguće uvesti u Azure automatizaciju.  

Pretvornik Runbook je implementirana kao modul ljuske PowerShell s cmdlet naziva **ConvertFrom SCORunbook** izvodi pretvorbu.  Kada instalirate alat, će stvoriti prečac za sesije PowerShell koja učitava cmdlet.   

Slijedi Osnovni postupak pretvorite u runbook Orchestrator, a uvesti u Automatizacija Azure.  U sljedećim se odjeljcima dodatno dati pojedinosti o pomoću alata i radu s pretvorene runbooks.

1. Jedan ili više runbooks izvezite iz Orchestrator.
2. Nabavite Integracija modula za sve aktivnosti u na runbook.
3. Pretvaranje runbooks Orchestrator u izvezenu datoteku.
4. Pregledajte podatke u zapisnicima da biste provjerili valjanost pretvorbu i da biste odredili obavezne ručno zadatke.
4. Automatizacija Azure uvesti pretvorene runbooks.
5. Stvorite sve potrebne imovine u automatizaciji Azure.
6. Uređivanje runbook u automatizaciji Azure da biste izmijenili sve potrebne aktivnosti.

### <a name="using-runbook-converter"></a>Pretvornik Runbook

Sintaksa **ConvertFrom SCORunbook** je na sljedeći način:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - put do izvezene datoteke koja sadrži runbooks za pretvaranje.
- Modul - zarezom popisa koji sadrži aktivnosti u na runbooks module za integraciju.
- OutputFolder - put do mape da biste stvorili pretvoriti grafički runbooks. 


Sljedeći primjer naredbe pretvara runbooks u datoteku izvoza pod nazivom **MyRunbooks.ois_export**.  Ove runbooks pomoću paketi za integraciju servisa Active Directory i Upravitelj zaštite podataka.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Datoteke zapisnika

Pretvornik Runbook će stvoriti sljedeće zapisničke datoteke na istom mjestu kao pretvorene runbook.  Ako već postoji datoteka, zatim ih prebrisat će se s podacima iz posljednjeg pretvaranja.

| Datoteka | Sadržaj |
|:---|:---|
| Pretvornik Runbook - Progress.log | Detaljne upute pretvorbe uključujući podatke za svaku aktivnost uspješno pretvoriti i upozorenje za svaku aktivnosti ne pretvoriti. |
| Pretvornik Runbook - Summary.log  | Sažetak posljednjeg pretvaranja uključujući sva upozorenja i praćenje zadataka koje morate izvesti kao što su stvaranje potrebne za pretvoreni runbook tjednog prikaza kalendara.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Izvoz runbooks iz Orchestrator

Pretvornik Runbook funkcionira s izvezene datoteke iz Orchestrator koja sadrži jedan ili više runbooks.  Stvorit će odgovarajuće runbook Azure Automatizacija za svaki runbook Orchestrator u izvezene datoteke.  

Da biste izvezli u runbook iz Orchestrator, desnom tipkom miša kliknite naziv kompilacije u dizajneru Runbook pa **Izvoz**.  Da biste izvezli sve runbooks u mapi, desnom tipkom miša kliknite naziv mape, a zatim odaberite **Izvoz**.


### <a name="runbook-activities"></a>Runbook aktivnosti

Pretvornik Runbook pretvara svaki aktivnosti u Orchestrator runbook odgovarajuće aktivnosti u automatizaciji Azure.  Za aktivnosti koji nije moguće pretvoriti, rezervirano mjesto aktivnosti se stvara u runbook tekstom upozorenje.  Nakon uvesti pretvorene runbook Azure Automatizacija, morate bilo koji od sljedećih aktivnosti zamijeniti valjani aktivnosti koje obavljaju tu funkciju.

Pretvorit će se sve Orchestrator aktivnosti u [Standardni modul aktivnosti](#standard-activities-module) .  Postoje neke standardne Orchestrator aktivnosti koje nisu u ovom modulu kroz te se ne pretvaraju.  **Slanje platformu događaj** , na primjer, ima bez ekvivalentne Automatizacija Azure Budući da je specifične za Orchestrator događaja.

[Praćenje aktivnosti](https://technet.microsoft.com/library/hh403827.aspx) ne pretvaraju jer nema jednaku vrijednost na njih u Azure automatizaciju.  Iznimka nadzor aktivnosti u [pretvoriti Integracija paketi](#integration-pack-converter) koji će se pretvoriti u aktivnosti rezervirano mjesto.

Aktivnost iz programa [pretvoriti paket za integraciju](#integration-pack-converter) pretvorit će se ako unesete put do modula za integraciju s parametrom **module** .  Za paketi za integraciju sustava centar možete koristiti [Sustava centra Orchestrator Integracija module](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Orchestrator resursi

Pretvornik Runbook samo pretvara runbooks, ne ostali resursi Orchestrator kao što su mjerača, varijable ili veze.  Mjerača nisu podržane u automatizaciji Azure.  Podržane su varijable i veze, ali ih morate stvoriti ručno.  Datoteka zapisnika će obavijestiti ako na runbook zahtijeva takve resursa i navedite odgovarajuće resursa koje su vam potrebne za stvaranje Azure Automatizacija za pretvoreni runbook ispravno funkcioniranje.

Ako, na primjer, na runbook možda koristi varijabla za popunjavanje određenu vrijednost u aktivnost.  Pretvoreni runbook će pretvoriti aktivnosti i navedite varijable resursa u automatizaciji Azure s istim nazivom kao varijabla Orchestrator.  To će navedeno u datoteci **Runbook pretvornik - Summary.log** koja je stvorena nakon pretvaranja.  Morat ćete ručno stvoriti ovaj varijable resursa u Azure automatizacije prije korištenja u runbook. 

 
### <a name="input-parameters"></a>Ulaznih parametara

Runbooks u Orchestrator prihvatiti ulaznih parametara s aktivnosti **Inicijalizacija podataka** .  Ako runbook pretvara obuhvaća aktivnost, [ulazni parametar](automation-graphical-authoring-intro.md#runbook-input-and-output) u runbook Azure Automatizacija se stvara za svaki parametar u aktivnosti.  [Kontrole za skripte tijeka rada](automation-graphical-authoring-intro.md#activities) aktivnosti se stvara u pretvorene runbook koje dohvaća i vraća svaki parametar.  Željene aktivnosti u na runbook pomoću ulazni parametar odnose se na Izlaz iz ove aktivnosti.

Razlog koristi se ova strategije je najbolje odražavati funkcionalnost Orchestrator runbook.  Aktivnosti u novi grafički runbooks moraju se izravno pozivaju parametara pomoću izvora za unos podataka Runbook za unos.


### <a name="invoke-runbook-activity"></a>Pozivanje Runbook aktivnosti

Runbooks u Orchestrator druge runbooks počinju **Pozivanje Runbook** aktivnosti. Ako runbook pretvara sadrži ove aktivnosti i postavljanja **čekanja za dovršetak** mogućnosti, zatim runbook aktivnosti stvoriti za njega u pretvorene runbook.  Ako mogućnost **čekanja za dovršetak** nije postavljeno, zatim skripte tijeka rada aktivnosti stvoriti koji koristi **Start AzureAutomationRunbook** da biste započeli s runbook.  Kada je uvesti pretvorene runbook Azure Automatizacija, morate promijenite aktivnost informacije navedene u aktivnost.




## <a name="related-articles"></a>Povezani članci

- [Sustav centar 2012 – Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Automatizacija usluga upravljanja](https://technet.microsoft.com/library/dn469260.aspx)
- [Hibridno Runbook tempiranja](automation-hybrid-runbook-worker.md)
- [Standardni aktivnosti orchestrator](http://technet.microsoft.com/library/hh403832.aspx)
- [Centar za preuzimanje sustava komplet alata za migraciju Orchestrator](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
