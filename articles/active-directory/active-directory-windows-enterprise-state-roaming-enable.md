<properties
    pageTitle="Omogući Enterprise stanje Roaming u Azure Active Directory | Microsoft Azure"
    description="Najčešća pitanja o Roaming stanje Enterprise postavkama u uređaje sa sustavom Windows. Enterprise stanje Roaming korisnicima pruža sučelje za Sjedinjeno komuniciranje u svim svojim uređajima Windows web-mjesta i smanjuje vremena potrebnog za konfiguriranje novi uređaj."
    services="active-directory"
    keywords="Stanje Enterprise roaming, windows oblaka, omogućivanje enterprise stanje roaming"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Omogući Enterprise stanje Roaming servisa Azure Active Directory

Enterprise stanje Roaming dostupna je za sve tvrtke ili ustanove s pretplatom Premium Azure Active Directory (Azure AD). Dodatne informacije o tome kako se pretplate za Azure AD, pročitajte članak [Azure AD proizvoda stranice](https://azure.microsoft.com/services/active-directory).

Kada omogućite Roaming stanje Enterprise, vašoj tvrtki ili ustanovi će automatski dobiti licenci za pretplatu slobodni, ograničeno koristi za upravljanje pravima za Azure. U ovom besplatna pretplata ograničeno je na šifriranje i dešifriranje postavke enterprise i sinkronizirati tako da Enterprise stanje servis za Roaming sustava; podataka aplikacije morate imati plaćenu pretplatu da biste koristili cijeli mogućnosti upravljanja pravima za Azure.

Nakon dobivanja Premium Azure AD pretplatu, slijedite ove korake da biste omogućili Roaming stanje Enterprise:

1. Prijava na portal Azure klasični.
2. Na lijevoj strani odaberite **Servisa ACTIVE DIRECTORY**, a zatim direktorija za koju želite omogućiti Roaming stanje Enterprise.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Idite na karticu **KONFIGURACIJA** na vrhu.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  Pomaknite se prema dolje po stranici odaberite **KORISNICI mogu SINKRONIZIRATI postavke i podaci Aplikacije za ENTERPRISE**, i zatim kliknite **SPREMI**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Za Windows 10 uređaj da biste premještati s servis za Roaming za Enterprise stanje, uređaj mora autentičnost pomoću identitet Azure AD. Za uređaje koji su se pridružili Azure AD primarni Prijava korisnika je identiteta Azure AD pa potrebna je bez dodatna konfiguracija. Za uređaje koji koriste tradicionalni lokalni Active Directory, administrator mora biti [Povezivanje uređaje domene pridruženo za Azure AD za Windows 10 sadržaje](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Spremanje podataka za sinkronizaciju
Enterprise stanje Roaming podataka nalazi se u jednu ili više [područja Azure](https://azure.microsoft.com/regions/ ) koji najbolje poravnava s vrijednost države/regije u instancu Azure Active Directory. Enterprise stanje Roaming podataka particije koji se temelje na tri glavna zemljopisna područja: Sjeverna Amerika, EMEA i APAC. Enterprise stanje Roaming podataka za klijent lokalno nalazi s zemljopisnom području pa je replicirati preko područja.  Na primjer, korisnike koji imaju svoje države/regije vrijednost postavite na jednu zemalja EMEA kao što su "Francuska" ili "Zambija" će imati njegovi podaci nalaze u jednoj ili Azure regije u Europi.  Korisnici koji su njihove vrijednost države/regije postavite u Azure AD na jednu zemalja Sjeverna Amerika, kao što su "Sad" ili "Kanada" će imaju svoje podatke smješten u jedan ili više Azure područja unutar Sjedinjenih DRŽAVA.  Korisnici koji su njihove vrijednost države/regije postavite u Azure AD na neki od APAC države, kao što su "Australija" ili "Novi Zeland" će imaju svoje podatke smješten u jednu ili više Azure područja unutar Azija.  Južnoameričkih zemalja i Antarktika podataka će se nalaziti u jednu ili više Azure područja unutar Sjedinjenih DRŽAVA.  Vrijednost Država/regija postavljeno kao dio postupak stvaranja direktorija za Azure AD, a nije moguće naknadno izmijeniti. 

Ako trebate dodatne informacije o mjesto za pohranu podataka, datoteka karata s [Azure podržava](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Upravljanje Roaming stanje Enterprise
Azure AD globalni administratori mogu Omogućivanje i onemogućivanje Enterprise stanje Roaming Azure klasični portalu.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globalni administratori možete ograničiti postavke sinkronizacije za određene sigurnosne grupe.

Globalni administratori možete prikazati izvješća o statusu po korisniku uređaj sinkronizaciju odabirom određenom korisniku na popisu za **KORISNIKE** instanca servisa Active Directory i klikom na kartici **uređaji** i odaberete prikaz **uređaja sinkroniziranje postavke i podaci aplikacije za tvrtke**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>Zadržavanje podataka
Sinkronizirani s Azure putem Enterprise stanje Roaming će podaci biti zadržani beskonačno osim ako se provodi operacija brisanja ručno ili podatke u pitanju ustanovi da je zastarjele. 

**Eksplicitnih brisanja:** Podaci se brišu kad administrator Azure briše korisnika ili direktorij ili administrator izričito zatraži da podaci će se izbrisati.

- **Brisanje korisnika**: korisnik brisanjem u Azure AD korisnički račun roaming podataka će se označiti za brisanje i izbrisat će se između 90 za 180 dana. 
- **Brisanje direktorija**: brisanje cijelom direktoriju u Azure AD je operaciju odmah. Svi podaci postavke pridruženo koji direktorija će se označiti za brisanje i izbrisat će se između 90 za 180 dana. 
- **Na zahtjev za brisanje**: Ako administrator Azure AD želi da biste ručno izbrisali određenog korisnika ili postavke podataka, administrator datoteke karata s [Azure podržava](https://azure.microsoft.com/support/). 

**Brisanje zastarjele podataka**: podaci koji nisu pristupanja za jednu godinu ("razdoblje zadržavanja") će se smatrati zastarjele i može se izbrisati iz Azure. Razdoblje zadržavanja mogu se promijeniti, ali će biti manje od 90 dana. Zastarjele podatke možda se određeni skup postavke sustava Windows i aplikacije ili sve postavke za korisnika. Ako, na primjer:
 
- Ako nema uređaja pristupiti zbirku određeni postavke (npr., aplikacija je uklonjena s uređaja ili grupu postavke kao što su "Tema" nije omogućen za sve korisničke uređaje), zatim tu zbirku će postati zastarjele nakon razdoblje zadržavanja i može se izbrisati. 
- Ako korisnik ima isključeno postavke sinkronizacije na svojem uređajima, pa nema podataka postavke pristupa, a sve postavke podatke za tog korisnika neće biti zastarjele i može se izbrisati nakon razdoblje zadržavanja. 
- Ako administrator direktorija Azure AD isključujete Enterprise stanje Roaming za u cijelom direktoriju, zatim Svi korisnici u imenik će se prestati sinkronizirati postavke, a svi podaci postavke za sve korisnike će postati zastarjele i može se izbrisati nakon razdoblje zadržavanja. 

**Oporavak izbrisane podataka**: pravila zadržavanja podataka nije moguće konfigurirati. Kada se podaci trajno izbrisana, neće biti koje se mogu vratiti. Međutim, važno je Imajte na umu da podataka postavke će se izbrisati samo iz Azure, ne uređaj krajnjeg korisnika. Ako je bilo kojeg uređaja kasnije ponovno poveže servis za Roaming za Enterprise stanje, postavke će ponovno se sinkronizirati i pohranjena u Azure.


## <a name="related-topics"></a>Povezane teme
- [Pregled Roaming stanje Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
- [Postavke i podaci za roaming najčešća Pitanja](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Postavke pravilnika i MDM za sinkronizaciju postavke grupe](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Referenca za Windows 10 roaming postavke](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
