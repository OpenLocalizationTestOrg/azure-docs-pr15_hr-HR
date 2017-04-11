<properties 
    pageTitle="Kakvu vrstu zbirke potrebne za Azure RemoteApp? | Microsoft Azure" 
    description="Dodatne informacije o vrstama zbirki dostupno u sklopu Azure RemoteApp." 
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



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Kakvu vrstu zbirke potrebne za Azure RemoteApp?

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp omogućuje zajedničko korištenje aplikacija i resursa s korisnicima na svim uređajima. Ne možemo to učiniti stvaranjem zbirki prema držite aplikacija i resursa, a zatim zbirki šaljete drugim korisnicima. Postoje 2 mogućnosti za različite zbirke, s drugog mrežnog i mogućnosti provjere autentičnosti - koji vam najviše odgovara?

Pogledajmo voditi kroz različite pitanja vezana uz i mogućnosti koje morate donijeti da biste izvukli što više iz vaše zbirke Azure RemoteApp. 


## <a name="quick-differences-between-the-collection-types"></a>Brzi razlike između vrste zbirke

|           | Oblak | Hibridno |
|-----------|-------|--------|
|Korištenje postojeće VNET| Da| Da|
|Potreban je AD povezani računi (DirSync)| ne| Da|
|Potreban je spoj domene| ne| Da|
|Potreban je dostupna VNET kontroler domene| ne| Da|

## <a name="cloud-collections"></a>Oblak zbirki
- Brzo stvaranje - zbirka je brzo dodjeli, što znači da se aplikacija korisnicima brže Nabavljanje.
- Premjesti vlastite aplikacije ni zajednički koristiti naše. Možete koristiti prilagođenu sliku (ugrađen iz programa Azure VM) ili jednu od slike u sklopu pretplate.
- Ne morate konfigurirati vezu između vaše zbirke i lokalne domene.
- No možete po želji vlastite Azure VNET da biste omogućili pristup u lokalnog okruženja za zajedničko korištenje podataka ili koristili provjeru autentičnosti sustava – Windows u resurse kao što je SQL Server (koristi provjera autentičnosti baze podataka).


U redu, kako stvoriti jednu?

- Samo oblak? Stvaranje s mogućnošću **Brzo stvaranje** na portalu.
- Oblak + VNET? Stvarati pomoću mogućnosti za **Stvaranje s VNET** , ali ne odaberete pridruživanje domeni.

## <a name="hybrid-collections"></a>Hibridno zbirki
- Navedite puni pristup lokalne mreže + Azure VNET.
- Obuhvaća domene uključite pristup za aplikacije i podataka. Udaljena aplikacije možete provjere autentičnosti na temelju lokalni Active Directory – zatim mogu pristupiti resursa u svojoj domeni.
- Omogućivanje napredne nadzor i upravljanje s postojeća rješenja centar sustava i pravilnicima za grupe sustava Windows (putem prilagođenu sliku utemeljena na sustavu Windows Server 2012 R2)
- Podržava [ExpressRoute](https://azure.microsoft.com/services/expressroute/) povezati svoje Azure VNET vaše lokalne VNET.

Stvaranje pomoću mogućnosti za **Stvaranje s VNET** , a zatim odaberite da biste se pridružili domeni.

## <a name="authentication-options"></a>Mogućnosti provjere autentičnosti
Azure RemoteApp podržava Microsoftovi računi i računi servisa Azure Active Directory, ali ne i svih zbirki podržava sve metode. 

| Vrsta računa                      |                                                             | Oblak | Oblak + VNET | Hibridno |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Microsoftov račun                 |                                                             | Da   | Da          | ne     |
| Azure Active Directory (Azure AD) |                                                             |       |              |        |
|                                   | Azure AD                                               | Da   | Da          | ne     |
|                                   | AD Connect uz sinkronizaciju lozinke                               | Da   | Da          | Da    |
|                                   | AD Connect bez sinkronizaciju lozinke                            | Da   | Da          | ne     |
|                                   | AD Connect AD fs                                       | Da   | Da          | Da    |
|                                   | Davatelji identiteta Azure podržane na 3 proizvođača (primjerice Ping) | Da   | Da          | Da    |
| Višestruka provjera autentičnosti       |                                                             | Da   | Da          | Da    |



### <a name="cloud-and-cloud--vnet"></a>Oblak i oblaka + VNET 
S zbirke oblak, možete koristiti Microsoftovi računi, poslovni subjekti Azure AD ili kombinacije dviju. Pomoću računa koji najbolje za korisnike.

Postoje bez preduvjete za putem Microsoftova računa. 

Ako želite koristiti Azure AD računi morate da biste bili sigurni da vaš klijent Azure AD odgovara onom povezan s pretplatom. Pri stvaranju pretplate Azure RemoteApp klijentu Azure AD koje ste koristili je automatski povezuju s pretplatom. Bilo koji korisnik Azure AD dati dozvolu mora biti iste klijentu. Ako je potrebno, možete [promijeniti klijentu Azure AD](remoteapp-changetenant.md) povezan s pretplatom.
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hibridno (ili oblaka + Azure AD + AD)

Korištenje Azure AD + lokalna servisa Active Directory preduvjeta za hibridno zbirku. Morate koristiti AD Connect za integraciju direktorija za dva. No možete imati neke kada je riječ o kako konfigurirati AD Connect. 

Postoje 2 AD scenariji za povezivanje – korištenje sinkronizaciju lozinke ili korištenje vanjski pristup za AD. Pogledajte [AD Connect informacije](../active-directory/active-directory-aadconnect.md) da biste utvrdili koju od tih funkcionira najbolje odgovara.

Možete koristiti i Azure AD + AD s oblaka zbirke. Provjerite je li poduzmite iste korake za postavljanje.

Pogledajte [Azure AD + preduvjeti za Active Directory za Azure RemoteApp](remoteapp-ad.md) korake potrebne da biste konfigurirali Azure AD i servisa Active Directory.

## <a name="go-create-your-collection"></a>Otvorite stvaraju vašu zbirku!
U redu, mislim smo ste shvatili je sada pa samo jednu stvar ulijevo da biste učinite – da biste stvorili prvu zbirka Azure RemoteApp.

[Stvaranje oblaka zbirke](remoteapp-create-cloud-deployment.md) ili [stvorite zbirku hibridnog](remoteapp-create-hybrid-deployment.md) - početak stvaranja.
