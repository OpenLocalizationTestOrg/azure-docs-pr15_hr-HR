<properties
   pageTitle="Azure Automatizacija pogreškama | Microsoft Azure"
   description="Ovaj članak sadrži osnovne pogreške rukovanje korake za otklanjanje poteškoća i ispravak uobičajenih pogrešaka Azure automatizaciju."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="Pogreška automatizacije pogreškama"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Savjeti za uobičajene pogreške Azure Automatizacija rukovanja pogreškama

U ovom se članku objašnjava neke od uobičajenih pogrešaka Azure Automatizacija se mogu pojaviti i navedeni moguće pogreškom korake.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Otklanjanje pogrešaka za provjeru autentičnosti prilikom rada s runbooks Automatizacija Azure  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Scenarij: Prijavite se na račun za Azure nije uspjela

**Pogreške:** Prilikom rada s cmdleti za dodavanje AzureAccount ili prijava AzureRmAccount prikazat će se pogreška "Unknown_user_type: Nepoznata vrsta korisnika".

**Razloga za pogrešku:** Ta se pogreška pojavljuje ako naziv resursa vjerodajnica nije valjan ili korisničko ime i lozinku koju ste koristili za postavljanje vjerodajnica resursa za automatizaciju nisu valjani.

**Savjeti za otklanjanje poteškoća:** Da biste odredili što nije u redu, provedite sljedeće korake:  

1. Provjerite je li nemate posebne znakove, uključujući na **@** znaka u nazivu Automatizacija vjerodajnica resursa koji koristite za povezivanje s Azure.  

2. Provjerite da koristite korisničko ime i lozinku koju se pohranjuju u Azure Automatizacija vjerodajnica u lokalnom uređivaču Očisti filtar. To možete učiniti tako da pokrenete sljedeće Cmdlete u Očisti filtar:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Ako vaš ne uspije provjera autentičnosti lokalno, to znači da se vjerodajnice Azure Active Directory još niste postavili pravilno. Pogledajte članak na blogu [Authenticating za Azure pomoću servisa Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) da biste na račun servisa Azure Active Directory pravilno postavljeni.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Scenarij: Nije moguće pronaći Azure pretplate

**Pogreške:** Prikazat će se pogreška "pretplate pod nazivom ``<subscription name>`` nije moguće pronaći" kada radite s cmdleta odaberite AzureSubscription ili odaberite AzureRmSubscription.

**Razloga za pogrešku:** Ta se pogreška pojavljuje ako naziv pretplate nije valjan ili kao administrator pretplate nije konfiguriran Azure Active Directory korisnika pokušava pojedinosti pretplate.

**Savjeti za otklanjanje poteškoća:** Da biste odredili ako ste pravilno autorizirani za Azure i imaju pristup pretplati pokušavate da biste odabrali, provedite sljedeće korake:  

1. Provjerite je li pokrenuti **Dodaj AzureAccount** prije pokretanja cmdlet **Odaberite AzureSubscription** .  

2. Ako i dalje vidite ovu poruku o pogrešci, izmijenite kod dodavanjem cmdlet **Get-AzureSubscription** pratiti cmdlet za **Dodavanje AzureAccount** i izvršiti kod.  Sada ako izlaz Get-AzureSubscription provjerite sadrži li pojedinosti pretplate.  
    * Ako ne vidite sve Detalji o pretplati u izlaz, to znači da pretplate još nije pokrenut.  
    * Ako vidite Detalji o pretplati u izlaz, provjerite koristite ID ili naziv odgovarajuće pretplate s cmdlet **Odaberite AzureSubscription** .   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Scenarij: Provjera autentičnosti za Azure nije uspjelo jer je omogućen višestruka provjera autentičnosti

**Pogreške:** Prikazat će se pogreška "Dodavanje AzureAccount: AADSTS50079: potrebna je registracija Jaka provjera autentičnosti (dokaz gore)" prilikom provjere autentičnosti Azure s Azure korisničko ime i lozinku.

**Razloga za pogrešku:** Ako imate višestruke provjere autentičnosti na račun za Azure, nećete moći koristiti korisnik programa Azure Active Directory za provjeru autentičnosti Azure.  Umjesto toga, morate koristiti certifikat ili glavni servis za provjeru autentičnosti Azure.

**Savjeti za otklanjanje poteškoća:** Da biste koristili certifikat s cmdleti za upravljanje servisom Azure, pogledajte [Stvaranje i dodavanje certifikat za upravljanje uslugama za Azure.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Da biste koristili glavni servisa Azure Voditelj resursa cmdleta sustava, pogledajte [Stvaranje glavnicu servis pomoću portala za Azure](./resource-group-create-service-principal-portal.md) i [provjere autentičnosti glavni servis pomoću upravitelja za Azure resursa.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Otklanjanje uobičajenih pogrešaka prilikom rada s runbooks

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Scenarij: Runbook neće uspjeti zbog deserijalizirati objekta

**Pogreške:** Vaše runbook neće uspjeti zbog pogreške "ne mogu se povezati parametar ``<ParameterName>``. Nije moguće pretvoriti u ``<ParameterType>`` vrijednost vrste Deserialized ``<ParameterType>`` upišite ``<ParameterType>``".

**Razloga za pogrešku:** Ako je vaš runbook PowerShell tijeka rada, sprema složene objekte u obliku deserialized da bi se zadržavaju stanje runbook ako je obustavljena je tijek rada.  

**Savjeti za otklanjanje poteškoća:**  
Bilo koji od sljedeća tri rješenja će riješili taj problem:

1. Ako su piping složene objekte iz jednog cmdlet na drugu, prelamanje tih cmdleta u programa InlineScript.  
2. Prenesite ime ili vrijednosti koje su vam potrebne iz složene objekt umjesto prosljeđivanje cijeli objekt.  

3. Pomoću komponente PowerShell runbook umjesto tijeka rada PowerShell runbook.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Scenarij: Runbook posla nije uspjelo jer je premašena dodijeljene kvota

**Pogreške:** Vaš posao runbook neće uspjeti zbog pogreške "kvote za mjesečne posla Ukupno vrijeme izvođenja nije dostigao je za ovu pretplatu".

**Razloga za pogrešku:** Ta se pogreška pojavljuje kada izvođenja posao premašuje 500 minute besplatne kvote za vaš račun. U ovom kvote odnosi se na sve vrste posla izvršavanje zadataka kao što su testiranje posla, počevši posao na portalu izvršavanja posla pomoću webhooks i planiranje posla izvršiti pomoću Azure portal ili u vašem podatkovnog centra. Da biste saznali više o cijene za automatizaciju potražite u članku [Automation cijene](https://azure.microsoft.com/pricing/details/automation/).

**Savjeti za otklanjanje poteškoća:** Ako želite koristiti više od 500 minuta obrada mjesečno morat ćete promijeniti pretplate iz na slobodno sloju osnovni sloju. Možete nadograditi na osnovne sloju tako da poduzmete sljedeće korake:  

1. Prijavite se u pretplatu za Azure  
2. Odaberite želite li nadograditi račun za automatizaciju  
3. Kliknite **Postavke** > **sloju određivanje cijena i korištenje** > **sloju određivanje cijena**  
4. Na plohu **Odabir vaše cijene sloju** odaberite **osnovne**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Scenarij: Cmdlet nije prepoznata prilikom izvršavanja u runbook

**Pogreške:** Vaš posao runbook neće uspjeti zbog pogreške "``<cmdlet name>``: pojam ``<cmdlet name>`` nije prepoznata kao naziv cmdleta, funkcija, skriptna datoteka ili funkcionalan program."

**Razloga za pogrešku:** Ta se pogreška se događa kada modul ljuske PowerShell ne možete pronaći cmdlet koristite u vašem runbook.  Možda račun nema modul koji sadrži cmdlet, postoji sukob imena pod nazivom runbook ili cmdlet i dalje postoji u drugom modul i automatizaciju ne može odrediti naziv.

**Savjeti za otklanjanje poteškoća:** Bilo koji od sljedećih rješenja će riješiti problem:  

- Provjera ispravno unijeli naziv cmdleta.  

- Provjerite cmdlet postoji li na vašem računu Automatizacija i da nema sukoba. Da biste provjerili je li cmdlet izlaganje, otvorite je runbook u načinu uređivanja i traženje cmdlet koji želite pronaći u biblioteci ili na Pokreni **Get-naredba ``<CommandName>`` **.  Nakon što ste provjeriti dostupnoj cmdlet na račun, a koji nema sukoba naziv s drugim cmdleta ili runbooks, dodajte ga u područje crtanja i provjerite koristite li valjan parametar postavljanje u vašem runbook.  

- Ako imate sukob imena, a cmdlet dostupna je u dvije različite module, to možete riješiti pomoću potpuno kvalificiran naziv cmdlet. Na primjer, možete koristiti **ModuleName\CmdletName**.  

- Ako su izvršavaju na runbook lokalnog u radnoj grupi hibridnog, zatim provjerite je li modul/cmdlet je instaliran na računalu koje hostira tempiranja hibridnog.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Scenarij: Dugotrajne runbook dosljedno ne uspijeva uz iznimku: "posao ne može nastaviti s izvođenjem jer je više puta uklonjen iz iste Kontrolna točka".

**Razloga za pogrešku:** Ovo je dizajn ponašanjem zbog "Sajma zajedničko korištenje" nadzor procesa unutar Azure Automatizacija, koji se automatski obustavlja na runbook ako izvršava dulje od 3 sata. No poruku pogreške koju vraća nude "daljnji" mogućnosti. Na runbook moguće je obustavljena nekoliko razloga. Obustavlja se dogodi uglavnom zbog pogreške. Ako, na primjer, iznimku je neuhvaćenu na runbook, mrežni problem ili neočekivanog na tempiranja Runbook koji se izvodi na runbook sve uzrokovat će runbook biti obustavljeno i započnite s njegova zadnjeg Kontrolna točka kada nastaviti.

**Savjeti za otklanjanje poteškoća:** Dokumentirane rješenje da biste izbjegli taj problem jest korištenje Checkpoints u tijeku rada.  Da biste saznali dodatne informacije pogledajte [Tijekova rada za učenje PowerShell](automation-powershell-workflow.md#Checkpoints).  Dodatna objašnjenja "Sajma zajedničko korištenje" i kontrolne točke pronaći ćete u ovom članku na blogu [Pomoću Checkpoints u Runbooks](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Otklanjanje uobičajenih pogrešaka prilikom uvoza moduli

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Scenarij: Modul ne uspije da biste uvezli ili Cmdlete nije moguće izvršiti nakon uvoza

**Pogreške:** Modul ne uspije da biste uvezli ili uspješno uvezli, ali se izdvajaju bez cmdleta.

**Razloga za pogrešku:** Neki od uobičajenih razloga koje modul možda neće uvesti uspješno Azure Automatizacija su:  

- Struktura odgovarati strukturi Automatizacija potrebne da bi bio u.  

- Modul ovisi o drugi modul koji nije implementiran računa za automatizaciju.  

- Modul nema njezine ovisnosti u mapi.  

- Cmdlet **Novo AzureRmAutomationModule** koristi se za prijenos modul, a danih put puno prostora za pohranu ili pomoću URL-a za javno dostupnu nije učitan modul.  

**Savjeti za otklanjanje poteškoća:**  
Bilo koji od sljedećih rješenja će riješiti problem:  

- Provjerite je li modul nalazi iza sljedećem obliku:  
ModuleName.Zip **->** ModuleName ili broj verzije **->** (ModuleName.psm1, ModuleName.psd1)

- Otvorite datoteku .psd1 i ima li modul sve ovisnosti.  Ako je tako, prenesite te modula na račun za automatizaciju.  

- Provjerite jesu li sve referencirani .dlls u mapi modul.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>Otklanjanje uobičajenih pogrešaka prilikom rada s želji stanje konfiguracije (DSC)  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Scenarij: Čvor se Neuspjelo status o pogrešci "Nije pronađena"

**Pogreške:** Čvor ima izvješće s status **nije uspjelo** i s pogreškom "Pokušaj da biste akciju s poslužitelja https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction nije uspjelo jer valjane konfiguracije ``<guid>`` nije moguće pronaći."

**Razloga za pogrešku:** Ta se pogreška obično pojavljuje kada čvor vam je dodijeljen naziv konfiguracijske (npr. ABC) umjesto čvor konfiguracije naziva (npr. ABC. WebServer).  

**Savjeti za otklanjanje poteškoća:**  

- Provjerite je li dodjeljujete čvora s "čvor konfiguracije naziv" i nije "konfiguracija naziva".  

- Možete dodijeliti čvor konfiguracije čvor pomoću portala za Azure ili s cmdlet ljuske PowerShell.
    - Da biste dodijelili čvor konfiguracije čvor pomoću portala za Azure, otvorite plohu **DSC čvorove** , a zatim odaberite čvor, a zatim kliknite **Dodijeli čvor konfiguracije** gumba.  
    - Da biste dodijelili čvor konfiguracije čvor pomoću cmdleta ljuske PowerShell, koristite cmdlet **Skup AzureRmAutomationDscNode**


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Scenarij: Nema čvor konfiguracije (MOF datoteke) su proizvodi po prikupljaju konfiguraciju

**Pogreške:** Sastavljanje posla DSC obustavlja uz pogrešku: "sastavljanje je uspješno dovršena, ali pojavile su se ne .mofs konfiguracije čvor".

**Razloga za pogrešku:** Kada **čvor** ključne riječi u konfiguraciji DSC procjenjuje sljedeće izraz za $null, a zatim bez konfiguracije čvor će proizvesti.    

**Savjeti za otklanjanje poteškoća:**  
Bilo koji od sljedećih rješenja će riješiti problem:  

- Provjerite je li izraz pokraj **čvor** ključne riječi u definiciji konfiguracije ne procjene za $null.  
- Ako dodajete ConfigurationData za sastavljanje konfiguraciju, provjerite je li se prosljeđivanje očekivanim vrijednostima koje je potrebno za konfiguraciju iz [ConfigurationData](automation-dsc-compile.md#configurationdata).


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Scenarij: Izvješće čvor DSC postaje zamrzne "u tijeku" stanja

**Pogreške:** Agent za DSC Proizvodi "Nema instanca pronađen navedeni vrijednosti nekretnina."

**Razloga za pogrešku:** Vi ste nadogradili verziji WMF i oštetila WMI.  

**Savjeti za otklanjanje poteškoća:** Slijedite upute u unosu u blog [DSC Poznati problemi i ograničenja](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) da biste riješili problem.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Scenarij: Nije moguće koristiti vjerodajnice u konfiguraciji DSC

**Pogreške:** Vaš posao sastavljanja DSC je obustavljena uz pogrešku: "System.InvalidOperationException pri obradi svojstvo vjerodajnica vrste ``<some resource name>``: pretvorba i spremanje šifriranu lozinku kao običan tekst je dopušteno samo ako je PSDscAllowPlainTextPassword postavljen na true".

**Razloga za pogrešku:** Vjerodajnice ste koristili u konfiguraciji, ali niste omogućuje proper **ConfigurationData** da biste postavili **PSDscAllowPlainTextPassword** na true za svaki čvor konfiguracije.  

**Savjeti za otklanjanje poteškoća:**  
- Provjerite je li za prosljeđivanje u odgovarajuće **ConfigurationData** da biste postavili **PSDscAllowPlainTextPassword** na true za svaki čvor konfiguraciju spomenute u konfiguraciji. Dodatne informacije potražite [resursima u DSC Automatizacija Azure](automation-dsc-compile.md#assets).


## <a name="next-steps"></a>Daljnji koraci

Ako ste pratili navedene korake rješavanje problema, a je potrebna dodatna pomoć u ovom članku u bilo kojem trenutku, možete učiniti sljedeće:

- Zatražite pomoć od stručnjaka za Azure. Slanje problem da biste na [MSDN Azure ili stogu prelijevanje forume.](https://azure.microsoft.com/support/forums/)

- Datoteka incident Azure podršci. Idi na [Azure podržava web-mjesta](https://azure.microsoft.com/support/options/) , pa kliknite **podrška** **službe za podršku tehničke i naplatu**.

- Ako tražite rješenja programa automatizacije Azure runbook ili modul za integraciju, objavite zahtjev skripte na [Centar za skripte](https://azure.microsoft.com/documentation/scripts/) .

- Objavite povratne informacije ili značajku zahtjeva za automatizaciju Azure na [Govorne korisnika](https://feedback.azure.com/forums/34192--general-feedback).
