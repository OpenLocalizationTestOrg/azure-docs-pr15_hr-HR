
<properties 
    pageTitle="Kako koristiti pretplatu na Office 365 s Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako koristiti pretplatu na Office 365 Azure RemoteApp za zajedničko korištenje aplikacija sustava Office."
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Kako koristiti pretplatu na Office 365 s Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Jeste li znali da možete koristiti postojeću pretplatu na Office 365 Azure RemoteApp za zajedničko korištenje aplikacija sustava Office iz oblaka? Čitati na dodatne informacije o sustavu Office 365 + Azure RemoteApp mogućnosti, uključujući veze na članke o sustavu Office 365 koje olakšavaju budućeg najviše pretplate.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Kako pomoću računa za Office 365 za Azure RemoteApp?
Pogledajte novi članak Petar sve informacije: [Korištenje RemoteApp Azure s korisničkim računima u sustavu Office 365](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Mogu li se pretplata na Office 365 koristiti i pokretanje aplikacija sustava Office u Azure RemoteApp?

Da! Korištenje pretplate na Office 365 zapravo je jedini način da bi se prikazala aplikacijama sustava Office za Azure RemoteApp.

(Napomena: Ako implementaciju sustava Azure RemoteApp isporuke po hostinga partnera ih moći vam ponuditi [Licencnog ugovora davatelja usluge](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)na temelju licence za Office)


Sjajan stvar o pretplatu na Office 365 je da ga možete koristiti isti korisničke licence na mnogo različitih platforme i okruženja, uključujući Azure oblaka. Prilikom korištenja aplikacije sustava Office u Azure RemoteApp ne morate kupiti dodatne licence ili konfigurirajte postojeće licence na bilo koji način posebno. Sve što trebate je pretplatu na Office 365 koja obuhvaća [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Office 365 ProPlus omogućuje [aktivaciju zajedničkom računalu](https://technet.microsoft.com/library/Dn782860.aspx) – Ova značajka omogućuje privremene operacijskim sustavom aktivacije za Office u virtualne i okruženja u kojima oblaku kao što su Azure RemoteApp (i udaljene radne površine).

Koje tarife za Office 365 obuhvaćaju Office 365 ProPlus? Pogledajte tablicu [dostupnost servisa unutar svake plan](https://technet.microsoft.com/library/office-365-plan-options.aspx) . Imajte na umu da sve tarife obuhvaćaju Office 365 ProPlus (na primjer, tarifu Office 365 Business). Ako vaša tarifa ne, razmislite o nadogradnji na plan koji ne (na primjer, Office 365 Education E3).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>U redu, kako se moj Office 365 ProPlus licenci koje se koristi s Azure RemoteApp?

Svaka korisnička licenca za Office 365 ProPlus omogućuje jednom korisniku aktivacija aplikacije sustava Office na najviše 5 računala i tablete i mobitele. Svaki aktivaciju je registriran s korisnikom dok deaktivirati Office na uređaju. (Korisnici mogu upravljati svoje uređaje na [portalu sustava Office 365](https://portal.office365.com/).)

S Azure RemoteApp jednog korisnika mogu prijaviti u nekoliko računala na isti dan bez uključila. To je zato servis automatski upravlja i mijenja veličinu resursa u oblaku, dok korisnik će vidjeti samo aplikacije i programe koje ste omogućili zajedničko korištenje. Za Office 365 ProPlus scenarij nudi način za aktivaciju zajedničkom računalu – to znači da se taj korisnik ne morate učiniti sve upravljanje licencama za pristup te resurse i koje pojedinačna računala ne smanjuju raspoloživu aktivaciju ograničenje 5 računala.

Dok god (administratora) dodijelite licence za Office 365 ProPlus korisnika, mogu koristiti Office na njihove osobnih uređaja te putem vaše zbirke Azure RemoteApp.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Koje aplikacije sustava Office možete koristiti sa sustavom Office 365 i Azure RemoteApp?

Pretplatu na Office 365 možete koristiti da biste aktivirali i zajedničko korištenje sustava Office 2013 u implementacijama Azure RemoteApp. Trenutno ne podržavamo pomoću drugih verzija sustava Office s Azure RemoteApp. To obuhvaća Office 2003, Office 2007, Office 2010 i Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Što je s Visio Pro ili projekta Pro?

Office 365 ProPlus slika obuhvaćeno pretplatom na RemoteApp uključuje i Visio Pro i Project Pro. No pretplate na Office 365 ProPlus ne možete koristiti da biste aktivirali te programe – svaki imaju vlastite licence. Aktiviranje ih na [portalu sustava Office 365](https://portal.office365.com/). 

Ne morate licence te programe ako ne želite da ih koristiti. Samo aktivirati programe koje želite koristiti -, a zatim prijeđite na drugi. Prikazivat će se i dalje na slici, ali ne možete ih koristiti. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Kako započeti rad sa sustavom Office 365 i Azure RemoteApp?

Sad kad znate Detalji o licenci za Office 365, recimo početak rada s u Azure RemoteApp – vrlo je jednostavno:

Kada stvarate vaše zbirke Azure RemoteApp, koristite **Office 365 ProPlus (potrebna je pretplata)** slike.

![Azure RemoteApp slika sustava Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


Na ovoj se slici prikazuje najnoviju verziju sustava Windows Server i Office 365 ProPlus. Nakon što konfigurirate zbirka (uključujući objavljivanje web-aplikacije), korisnici jednostavno prijavite Azure RemoteApp (pomoću klijentskog RemoteApp) i navedite svoje vjerodajnice za Office 365 za sve aplikacije sustava Office. Licence automatski isporučuju bez svakog skupa prema gore ili upravljanje obavezno.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Je li moguće stvoriti prilagođenu sliku za Office 365 ProPlus?

Možete stvoriti prilagođenu sliku za vašu zbirku koja sadrži Office 365 ProPlus. Postoje mogućnosti 2 – pomoću Azure galerije dajemo ili možete stvoriti vlastitu prilagođenu sliku i instalacija sustava Office 365 ProPlus postoji.

### <a name="use-the-azure-gallery-image"></a>Korištenje Azure galerije

Najjednostavniji način za implementaciju sustava Office 365 ProPlus zbirka je [započnite jednim Azure Galerija slika](remoteapp-image-on-azurevm.md) obuhvaćeno pretplatom na Azure RemoteApp. Provjerite jeste li odabrali sliku **unaprijed instaliran Windows Server udaljene radne površine sesiju glavnog računala za Office 365 ProPlus** . Nakon toga instalirati sve aplikacije na toj slici i spremni ste za slanje.

### <a name="use-a-custom-image"></a>Koristite prilagođenu sliku

Uvijek možete stvoriti prilagođenu sliku – možete stvoriti [Azure VM](remoteapp-image-on-azurevm.md) ili [Stvaranje slike lokalno](remoteapp-create-custom-image.md) i prijenos Azure. U svakom slučaju, provjerite je li instalacije sustava Office 365 ProPlus koji pomoću čvor aktivaciju zajedničkom računalu. Pomoću [Alata za implementaciju sustava Office](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) i slijedite [upute](https://technet.microsoft.com/library/Dn782858.aspx) za instalaciju.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Onemogućivanje automatskog ažuriranja za Office 365 ProPlus u prilagođenu sliku – VAŽNE

Prilagođenu sliku koristi Azure RemoteApp kao predložak za dodavanje dodatne resurse kao na zahtjev iz vaše povećava korisnika. Da biste spriječili kašnjenja i problema s povezivanjem, onemogućite Automatska ažuriranja za Office na slici. Ako ne, a zatim svaki resurs stvorene pomoću tog predlošku automatski će se ažurirati prilikom pokretanja. Umjesto toga koristite standardni proces Azure RemoteApp o ažuriranju prilagođenu sliku. Na taj način ažurirati aplikacije sustava Office na jednom slika predložak, a omogućuju Azure RemoteApp pobrinuti početak ažuriranja svojim korisnicima.

Da biste onemogućili automatsko ažuriranje, dodajte sljedeće u konfiguracijskoj datoteci alata za implementaciju sustava Office:

        <Updates Enabled="FALSE" />

Odmah konfiguracijskoj datoteci mora sadržavati te retke:
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Kako ažurirati slike za Office 365 ProPlus?

Postoji mnogo razloga da biste ažurirali slike u sklopu zbirke. Evo tek nekoliko:

- Dohvaćanje najnovijih ažuriranja za Windows 
- Preuzeti najnovija ažuriranja za Office 365 ProPlus aplikacije
- Ažurirajte prilagođene aplikacije
- Promijeniti ostale postavke konfiguracije za samu sliku

Idite na kraj do kraja korake za ažuriranje vaše zbirke da biste koristili sliku ste ažurirali, [ovdje](remoteapp-update.md). No informacije o ažuriranju slike i Office 365 ProPlus, pogledajte sljedeće informacije.

Koje su vam dvije mogućnosti za ažuriranje sliku – zamijenite sliku s potpuno novi ili ručno ažurirati postojeće slike.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>Zamijenite sliku s najnovijim Azure galerije + Dodaj prilagodbe
Uz tu se mogućnost omogućuju Microsoft voditi brigu o ažuriranja za Windows Server i Office 365 ProPlus. Umjesto ažuriranje postojeću sliku, stvorit ćete posve nove slike koji se temelji na najnovije galerije. Pa ponovite neki niste prije prilagodite sliku – instalacija prilagođenih aplikacija izmjena konfiguracije slike, itd.

Slika galerije redovito ažuriraju, tako da položite jednostavno, znati jesu li aplikacija sustava Windows Server i Office 365 ProPlus u tijeku. Naravno, u trade-off je ćete morati Primjena prilagodbi svaki put dobijete novu sliku. Možete stvoriti skripte da biste automatizirali postavku prilagodbe.

### <a name="manually-update-your-existing-image"></a>Ručno ažuriranje postojeće slike

Uz tu se mogućnost da biste primijenili ažuriranja sliku pomoću standardne Alati za Windows. Za Office 365 ProPlus pomoću alata za implementaciju sustava Office da biste preuzeli i instalirali najnovija ažuriranja i verzije sustava Office 365 ProPlus.

> [AZURE.IMPORTANT] Imajte na umu – onemogućivanje Office 365 ProPlus Automatska ažuriranja.

Dodatne informacije o korištenju alata za implementaciju sustava Office ima li ažuriranja je potrebno?

- [Implementacija značajke "klikom do cilja" za proizvode iz sustava Office 365 pomoću alata za implementaciju sustava Office](https://technet.microsoft.com/library/JJ219423.aspx)
- [Implementacija i ažuriranje sustava Office 365 ProPlus pomoću alata za implementaciju sustava Office](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (videozapis)
- [Konfiguriranje postavki ažuriranja za Office 365 ProPlus](https://technet.microsoft.com/library/dn761708.aspx)
