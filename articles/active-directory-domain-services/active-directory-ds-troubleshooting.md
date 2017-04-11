<properties
    pageTitle="Azure Active Directory Domain Services: Vodič za otklanjanje poteškoća | Microsoft Azure"
    description="Vodič za Azure AD domenske servise za otklanjanje poteškoća"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure servisa Active Directory Domain Services – vodič za otklanjanje poteškoća
Ovaj članak sadrži savjete za otklanjanje poteškoća za probleme koji se mogu pojaviti prilikom postavljanja ili administriranje Azure Active Directory (AD) domenske servise.


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Ne možete omogućiti Azure servisa Active Directory Domain Services za Azure AD direktorija
U ovom se odjeljku pomaže pri otklanjanju pogreške kada pokušate omogućiti Azure servisa Active Directory Domain Services za direktorija, a ne uspije ili dobiti preklapa natrag na "Onemogućeno".

Odaberite korake za otklanjanje poteškoća koje odgovaraju na koji se pojaviti poruka o pogrešci.

|**Poruka o pogrešci**|**Razlučivost**|
|---|:---|
|*Naziv contoso100.com već se koristi u ovu mrežu. Navedite naziv koji se ne koristi.*|[Sukob naziva domene u virtualne mreže](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*U klijentu za Azure AD neće biti omogućene domenske servise. Servis nema potrebne dozvole za aplikaciju pod nazivom 'Sinkronizaciju servisa Azure AD domene'. Brisanje aplikacije pod nazivom "Sinkronizaciju servisa Azure AD domene", a zatim pokušajte da biste omogućili domenske servise za vaš klijent Azure AD.*|[Domenski servisi nemate odgovarajuće dozvole za aplikaciju za sinkronizaciju servisa Azure AD domene](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*U klijentu za Azure AD neće biti omogućene domenske servise. Aplikacija domenske servise u klijentu za Azure AD nema potrebne dozvole za omogućivanje domenske servise. Brisanje aplikacije s identifikator d87dcbc6-a371-462e-88e3-28ad15ec4e64 aplikacije, a zatim pokušajte da biste omogućili domenske servise za vaš klijent Azure AD.*|[Aplikacija za domenske servise nije ispravno konfigurirana na klijentu](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*U klijentu za Azure AD neće biti omogućene domenske servise. Aplikacija Microsoft Azure AD je onemogućena u klijentu za Azure AD. Omogućivanje aplikacijom identifikator 00000002-0000-0000-c000-000000000000 aplikacije, a zatim pokušajte da biste omogućili domenske servise za vaš klijent Azure AD.*|[Aplikacija Microsoft Graph je onemogućena u klijentu za Azure AD](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Sukob naziva domene
**Poruka o pogrešci:**

*Naziv contoso100.com već se koristi u ovu mrežu. Navedite naziv koji se ne koristi.*

**Olakšava:**

Provjerite je li imati postojeću domenu s istim nazivom domene dostupan na tom virtualne mreže. Na primjer, pretpostavimo imate domenu pod nazivom "contoso.com" već dostupna na odabrani virtualne mreže. Naknadno pokušate omogućiti Azure servisa Active Directory Domain Services upravljanih domene s istim nazivom domene (to jest, "contoso.com") na tom virtualne mreže. Naiđete na pogrešku prilikom pokušaja omogućiti Azure servisa Active Directory Domain Services.

To je zbog sukoba imena za naziv domene na tom virtualne mreže. U tom slučaju morate koristiti neki drugi naziv da biste postavili domenu upravljanih Azure servisa Active Directory Domain Services. Umjesto toga poništite Dodjela resursa za postojeću domenu, a zatim nastavite s omogućivanjem Azure servisa Active Directory Domain Services.


### <a name="inadequate-permissions"></a>Nedovoljne dozvole
**Poruka o pogrešci:**

*U klijentu za Azure AD neće biti omogućene domenske servise. Servis nema potrebne dozvole za aplikaciju pod nazivom 'Sinkronizaciju servisa Azure AD domene'. Brisanje aplikacije pod nazivom "Sinkronizaciju servisa Azure AD domene", a zatim pokušajte da biste omogućili domenske servise za vaš klijent Azure AD.*

**Olakšava:**

Provjerite prikazuje li se aplikacija pod nazivom "Azure AD domene servisa sinkronizacije' u direktoriju Azure AD. Ako postoji ovu aplikaciju, izbrišite je, a zatim ponovno omogućiti Azure servisa Active Directory Domain Services.

Izvršite sljedeće korake da biste provjerili prisutnosti aplikacije, a da biste je izbrisali, ako postoji aplikacija:

  1. Dođite do **Azure klasični portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Odaberite čvor **Servisa Active Directory** u lijevom oknu.
  3. Odaberite klijenta Azure AD (imenik) za koju želite omogućiti Azure servisa Active Directory Domain Services.
  4. Idite na karticu **aplikacije** .
  5. Na padajućem popisu, odaberite mogućnost **aplikacije Moja tvrtka vlasništvu** .
  6. Provjerite ima li aplikacija pod nazivom **sinkronizaciju servisa Azure AD domene**. Ako aplikacija postoji, nastavite da biste je izbrisali.
  7. Nakon što ste izbrisali aplikaciju, pokušajte ponovno Omogućivanje servisa Azure AD domene.


### <a name="invalid-configuration"></a>Konfiguracija nije valjana
**Poruka o pogrešci:**

*U klijentu za Azure AD neće biti omogućene domenske servise. Aplikacija domenske servise u klijentu za Azure AD nema potrebne dozvole za omogućivanje domenske servise. Brisanje aplikacije s identifikator d87dcbc6-a371-462e-88e3-28ad15ec4e64 aplikacije, a zatim pokušajte da biste omogućili domenske servise za vaš klijent Azure AD.*

**Olakšava:**

Provjerite koristite li aplikacija pod nazivom AzureActiveDirectoryDomainControllerServices (s identifikator aplikacije d87dcbc6-a371-462e-88e3-28ad15ec4e64) u direktoriju Azure AD. Ako postoji ovu aplikaciju, morate izbrisati, a zatim ponovno omogućiti Azure servisa Active Directory Domain Services.

Koristite sljedeću skriptu komponente PowerShell da biste pronašli aplikacije i izbrisati.

> [AZURE.NOTE] Ova skripta koristi cmdleti za **Azure AD PowerShell verzije 2** . Potpuni popis sve dostupne cmdleta i preuzimanje modula, pročitajte [dokumentaciju referenca AzureAD PowerShell](https://msdn.microsoft.com/library/azure/mt757189.aspx).

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph onemogućena
**Poruka o pogrešci:**

U klijentu za Azure AD neće biti omogućene domenske servise. Aplikacija Microsoft Azure AD je onemogućena u klijentu za Azure AD. Omogućivanje aplikacijom identifikator 00000002-0000-0000-c000-000000000000 aplikacije, a zatim pokušajte da biste omogućili domenske servise za vaš klijent Azure AD.

**Olakšava:**

Provjerite jesu li onemogućeni aplikacije s identifikatorom 00000002-0000-0000-c000-000000000000. Ove aplikacije je Microsoft Azure AD aplikacije i omogućuje pristup grafikonu API klijentu za Azure AD. Azure servisa Active Directory Domain Services mora biti omogućen za sinkronizaciju Azure AD klijentu na domenu upravljanih ovu aplikaciju.

Da biste riješili ovu pogrešku, omogućite ovu aplikaciju, a zatim pokušajte da biste omogućili domenske servise za vaš klijent Azure AD.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Korisnici su Prijava na domenu upravljanih Azure servisa Active Directory Domain Services
Ako je jedan ili više korisnika na klijentu Azure AD su prijava novostvorenu upravljanih domenu, provedite sljedeće korake za otklanjanje poteškoća:

- **Prijavu pomoću UPN oblika:** Pokušajte se prijaviti pomoću oblika UPN-a (Ako, na primjer, 'joeuser@contoso.com') umjesto SAMAccountName oblik ('CONTOSO\joeuser'). Na SAMAccountName može biti automatski generira za korisnike čiji se prefiks UPN pretjerano predugačak ili je isti kao neki drugi korisnik upravljanog domeni. Oblikovanje UPN se zajamčeno jedinstvena u klijent za Azure AD.

> [AZURE.NOTE] Preporučujemo korištenje oblik UPN-a za prijavu na domenu upravljanih Azure servisa Active Directory Domain Services.

- Provjerite imate li [omogućena sinkronizacija lozinku](active-directory-ds-getting-started-password-sync.md) skladu korake navedene u vodiču za početak rada.

- **Vanjskog računa:** Provjerite je li problematične korisnički račun nije vanjski računa u klijentu za Azure AD. Vanjski računi Primjeri Microsoftova računa (, na primjer, 'joe@live.com') ili korisničke račune s vanjskog Azure AD direktorija. Budući da Azure servisa Active Directory Domain Services Nemate vjerodajnice za pretraživanje kao korisničke račune, ti korisnici ne možete prijaviti upravljanih domene.

- **Sinkronizirali račune:** Ako problematične korisničke račune se sinkroniziraju iz lokalnog imenika, provjerite sljedeće:
    - Možete uvesti ili ažurirati na [najnovije preporučene izdanje Azure AD Connect](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect).

    - Ste konfigurirali Azure AD Connect za [izvođenje potpune sinkronizacije](active-directory-ds-getting-started-password-sync.md).

    - Ovisno o veličini direktorija, on potrajati neko vrijeme za korisničke račune i vjerodajnica raspršivanja da bi bio dostupan u Azure AD domenske servise. Provjerite je li čekate dovoljno dugo provesti provjere autentičnosti (ovisno o veličini direktorij – od nekoliko sati dana ili dvije za velike direktorija).

    - Ako se problem nastavi pojavljivati i nakon potvrđivanja prethodnih koraka, pokušajte ponovno servisa Microsoft Azure AD sinkronizaciju. Na računalu sinkronizaciju pokretanje naredbeni redak i izvršiti sljedeće naredbe:
      1. Neto Zaustavi "Microsoft Azure AD sinkronizaciju"
      2. Neto početka "Microsoft Azure AD sinkronizaciju"

- **Samo oblak računi**: problematične korisnički račun je samo oblak korisnički račun, provjerite da korisnik Promijeni lozinku nakon što ste omogućili Azure servisa Active Directory Domain Services. Ovaj korak uzrokuje vjerodajnicu raspršivanja potrebne za Azure AD domenske servise generiranje.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Korisnici koji se uklanjaju iz klijenta sustava Azure AD se uklanjaju iz upravljanih domene
Azure AD štiti od nenamjerno brisanje objekata korisnika. Kad izbrišete korisnički račun s klijentom Azure AD, odgovarajući korisničkom objektu se premješta u koš za smeće. Kada se ovaj postupak brisanja sinkronizira upravljanih domene, uzrokuje odgovarajuće korisnički račun na kojem se označiti onemogućene. Ta značajka pomaže vam oporavak ili kasnije vratiti korisnički račun.

Da biste potpuno uklonili korisnički račun s upravljanih domene, korisnik trajno izbrisati s klijentom Azure AD. Pomoću cmdleta ljuske PowerShell Ukloni-MsolUser s na "-RemoveFromRecycleBin' mogućnosti, kao što je opisano u ovom [članku MSDN](https://msdn.microsoft.com/library/azure/dn194132.aspx).


## <a name="contact-us"></a>Obratite nam se
Obratite se timu za proizvod Azure Active Directory Domain Services da biste [Slanje povratnih informacija ili radi podrške za] (Aktivno-direktorija-ds-kontakt-us.md).
