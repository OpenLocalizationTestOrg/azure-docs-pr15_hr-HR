<properties
   pageTitle="Azure katalog podataka napomene | Microsoft Azure"
   description="Napomene za Azure kataloga podataka."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Azure napomene kataloga podataka

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Katalog podataka Azure izdanje bilješke u studenom 20 2015.

### <a name="opening-data-sources-in-power-bi-desktop"></a>Otvaranje podatkovnih izvora u Power BI Desktop

Kada pomoću mogućnosti "Otvori u Power BI Desktop" s portala za **Katalog podataka Azure** korisnici naići na jedan od dva problema u aplikaciji Power BI Desktop:

- Prikazuje se dijaloški okvir s naslovom "Nije moguće u otvorenom dokumentu"
- Otvorit će se aplikacija Power BI Desktop, ali čini se prazna datoteka

Za svaki situaciju problem možete riješiti tako da preuzmete i instalirate najnoviju verziju dodatka Power BI Desktop iz [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Katalog podataka Azure izdanje bilješke u studenom 13 2015.

### <a name="registering-and-connecting-to-teradata"></a>Registriranje i povezivanje s Teradata

Prilikom povezivanja s Teradata izvorima podataka korisnicima morate imati instaliran odgovarajući Teradata ODBC upravljački program koji odgovaraju bitness (32-bitna ili 64-bitni) softvera koristi.

Od tog datuma izdanje ADC najnovije [Teradata ODBC upravljački program za windows (verzija 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) kompatibilan sa sustavom Office 2013, ali ne i s Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Katalog podataka Azure izdanje bilješke u srpnju 13 2015.

### <a name="registering-and-connecting-to-oracle-database"></a>Registriranje i povezivanje s bazom podataka tvrtke Oracle

Prilikom povezivanja s izvorima podataka za baze podataka tvrtke Oracle korisnici morate imati instaliran odgovarajuće Oracle upravljačke programe koje odgovaraju bitness (32-bitna ili 64-bitni) koristi softvera.

-   Kada Registracija izvora podataka Oracle na računalu s 32-bitnom sustavu Windows, koristit će se 32-bitne upravljačke programe za Oracle
-   Kada Registracija izvora podataka Oracle na računalu sa 64-bitnom sustavu Windows, koristit će se 64-bitne upravljačke programe za Oracle
-   Prilikom povezivanja s izvorima podataka tvrtke Oracle pomoću programa Excel na računalu sa sustavom 32-bitnu verziju sustava Microsoft Office, uključujući na 64-bitni Windows upravljačke programe za Oracle 32-bitni koristit će se
-   Kada se povezujete s izvorima podataka tvrtke Oracle pomoću programa Excel na računalu sa 64-bitnu verziju sustava Microsoft Office, koristit će se upravljačke programe za Oracle 64-bitni

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registriranje i povezivanje s SQL Server Reporting Services

Podrška za izvore podataka za SQL Server Reporting Services (SSRS) trenutno ograničeno je na samo poslužitelje Nativnom načinu rada. Podrška za SSRS u načinu rada sustava SharePoint dodaje se u novije verzije.

### <a name="opening-data-assets-in-excel"></a>Otvaranje imovine podataka u programu Excel

Prilikom otvaranja imovine podataka u programu Microsoft Excel s portala za **Katalog podataka Azure** , korisnici možda morati unijeti dijaloški okvir **Sigurnosno upozorenje programa Microsoft Excel** . To je standardni, očekivano i korisnici mogu odaberite **Omogući** da biste nastavili.

Dodatne informacije potražite u članku [Omogućivanje i onemogućivanje sigurnosnih upozorenja o vezama i datotekama sa sumnjivih web-mjesta](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Konfiguracija proxy poslužitelja i pravila i podataka izvora Registracija

Korisnicima se mogu pojaviti situaciji gdje prijave na portal Azure katalog podataka, ali kada pokušaju prijavite se na alat Registracija izvora podataka koji se pojavi poruka o pogrešci koja ih sprječava prijave.

Postoje dva razloga potencijalne takvo ponašanje problem:

**Uzrok 1: Konfiguriranje Active Directory Federation Services** Alat za registraciju izvora podataka koristi provjeru autentičnosti obrazaca za provjeru valjanosti korisničkog imena za prijavu protiv servisa Active Directory. Za uspješan prijavu provjere autentičnosti obrazaca mora biti omogućen u globalni pravila za provjeru autentičnosti administrator servisa Active Directory.

U nekim slučajevima to pogreška može dogoditi samo kad je korisnik na mreži tvrtke ili samo kada korisnik povezuje s izvan mreže tvrtke. Globalni pravilo provjere autentičnosti omogućuje metoda provjere autentičnosti koje će biti omogućene zasebno za intranet i ekstranet veze. Pogreške prilikom prijave može pojaviti ako Provjera autentičnosti obrazaca nisu omogućeni u mreži iz koje se povezuje korisnika.

Dodatne informacije potražite u članku [Konfiguriranje pravila za provjeru autentičnosti](https://technet.microsoft.com/library/dn486781.aspx).

**Uzrok 2: mrežna konfiguracija proxy** Ako poslovnoj mreži koristi proxy poslužitelj, alat za registraciju možda nećete moći povezivanje sa servisom Azure Active Directory putem proxy poslužitelj. Korisnici mogu Pobrinite se da alat za registraciju uređivanjem alata konfiguracijska datoteka dodavanjem u ovom se odjeljku datoteku:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Da biste pronašli datoteku RegistrationTool.exe.config, pokretanje alata za registraciju, a zatim otvorite uslužni upravitelja zadataka sustava Windows. Na kartici Detalji u upravitelju zadataka, desnom tipkom miša kliknite RegistrationTool.exe i skočnom izborniku odaberite Otvori mjesto datoteke.
