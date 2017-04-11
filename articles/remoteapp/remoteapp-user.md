<properties
    pageTitle="Dodavanje korisnika u zbirku Azure RemoteApp | Microsoft Azure"
    description="Saznajte kako dodati korisnike u zbirku Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Upute za dodavanje korisnika u zbirku Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Prije nego što vaši korisnici mogu vidjeti i koristiti aplikacije RemoteApp Azure, morate dopustiti pristup zbirci web. Ovo je lako dijela: na kartici **Korisničkog pristupa** unesite podatke o računu za korisnika, a zatim kliknite kvačicu.

Podatke potrebne? Koja ovisi o vrsti zbirke koji ste stvorili (oblaka ili hibridnog) i tome koristite Office 365 ProPlus u toj zbirci.

## <a name="supported-user-identities"></a>Podržani korisničkih identiteta

Vrste drugu zbirku (oblaka nasuprot hibridnog) podržava korištenje drugog korisnika identiteta za pristup aplikacijama.  

Za hibridno skup RemoteApp ćete morati postaviti do infrastrukture domene servisa Active Directory na lokalno i klijent za Azure Active Directory s Integracija direktorija (i po želji jedan prijave). Uz to, morate stvoriti neke objektima servisa Active Directory u lokalnog imenika.  

Oblak zbirke RemoteApp, bilo koji korisnik koji ima Azure Active Directory podržava identiteta se dodjeljuju korisničkog pristupa RemoteApp da biste uključili Microsoft Accounts.  Pogledajte tablicu u nastavku.

Korisnici sustava Office 365 koje korisnici Azure Active Directory. Ako imaju hibridnog Azure Active Directory, direktorij sinkronizirati poslovne kontakte, oni mogu se dodijeliti pristup korisnika u hibridnoj instalaciji RemoteApp.   

U ovoj su tablici možete koristiti kao vodič kroz identiteta za koje je podržano u zbirci web i koji su preduvjeti za Active Directory.

|Korisnički računi |Oblak   |Hibridno|
|--------------|--------|------|
|Microsoftov račun|     Da|    ne|
|Azure Active Directory (Azure AD)| | |
|Samo Azure AD oblaka    |Da    |ne |
|ADsync uz sinkronizaciju lozinke  |Da    |Da    |
|ADsync bez sinkronizaciju lozinke|  Da |ne |
|ADsync AD fs  |Da    |Da    |
|[3 proizvođača Azure podržane davatelji identiteta](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (primjer Ping) |Da    |Da|
|Višestruka provjera autentičnosti    |Da    |Da    |

Pogledajte [Dodatne informacije](remoteapp-ad.md) o konfiguraciji servisa Active Directory za RemoteApp.


> [AZURE.NOTE] Azure Active Directory korisnici moraju se nalaziti iz klijenta koji je povezan s pretplatom. (Možete pogledati i izmjena pretplate na kartici **Postavke** na portalu. Potražite u članku [Promjena klijentu Azure Active Directory koriste RemoteApp](remoteapp-changetenant.md) dodatne informacije)

## <a name="office-365-proplus-user-account-information"></a>Podaci za Office 365 ProPlus korisničkog računa
Ako koristite Office 365 ProPlus predložak slike u svoju zbirku *ili* ako ste stvorili prilagođenu sliku koja koristi Office 365, samo dopušteno da biste dodali Azure Active Directory korisnici koji imaju pretplate na Office 365 za zadanu domenu pretplate. Dodatne informacije potražite u članku [korištenje sustava Office 365 s Azure RemoteApp](remoteapp-o365.md) .
