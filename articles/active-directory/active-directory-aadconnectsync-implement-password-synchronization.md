<properties
    pageTitle="Implementacijom sinkronizaciju lozinke za sinkronizaciju Azure AD Connect | Microsoft Azure"
    description="Pruža informacije o tome kako funkcionira sinkronizacija lozinku i kako je omogućiti."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Implementacijom sinkronizaciju lozinke za sinkronizaciju Azure AD Connect
Ova tema sadrži informacije koje morate sinkronizirati korisničke lozinke iz programa lokalnog servisa Active Directory (AD) u oblaku Azure Active Directory (Azure AD).

## <a name="what-is-password-synchronization"></a>Što je sinkronizacija lozinku
Vjerojatnost povezana su blokira obavljanje posla zbog zaboravljene lozinke je broj različite lozinke morate zapamtiti. Morate zapamtiti viša vjerojatnost zaboraviti nešto više lozinki. Pitanja i pozive o lozinki i ostalih problema vezanih uz lozinku potražnje Većina služba za resurse.

Sinkronizaciju lozinke je značajka za sinkronizaciju korisničkih lozinki iz lokalnog Active Directory u oblaku Azure Active Directory (Azure AD).
Ova značajka omogućuje vam da biste se prijavili servisa Azure Active Directory (kao što su Office 365, Microsoft Intune, CRM Online i Azure servisa Active Directory Domain Services) koristeći istu lozinku koju koristite za prijavu na lokalni Active Directory.

![Što je Azure AD Connect](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Smanjivanjem broj lozinki koje korisnici moraju održavati samo jednom sinkronizaciju lozinke pomaže vam:

- Poboljšanje produktivnosti korisnika
- Smanjivanje troškove službom za korisnike  

Osim toga, ako odaberete da biste koristili [**pridruženi AD FS**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect), možete po želji omogućiti sinkronizaciju lozinke kao sigurnosnu kopiju u slučaju da ne uspije infrastruktura za AD FS.

Sinkronizacija lozinka je datotečni nastavak značajke sinkronizacije direktorija pomoću Azure AD Connect sinkronizaciju. Da biste koristili sinkronizaciju lozinke u svom okruženju, morate:

- Povezivanje Instaliraj Azure AD  
- Konfiguriranje sinkronizacije direktorija između lokalnih AD i Azure Active Directory
- Omogući sinkronizaciju lozinke

Dodatne informacije potražite u članku [integriranje vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md)

> [AZURE.NOTE] Dodatne informacije o Active Directory Domain Services podešena za sinkronizaciju FIPS i lozinku, potražite u članku [sinkronizaciju lozinke i FIPS](#password-synchronization-and-fips).

## <a name="how-password-synchronization-works"></a>Kako funkcionira sinkronizacija lozinke
U servisu Active Directory domain pohranjuje lozinke u obrazac za prikaz vrijednosti raspršivanje stvarni korisničku lozinku. Raspršivanje vrijednost je rezultat funkcije jednosmjerna matematičke (u "*raspršivanje algoritam*"). Ne postoji način da biste vratili rezultat funkcije jednosmjerna običnog teksta verziju lozinku. Raspršivanje za lozinku ne možete koristiti za prijavu na lokalnu mrežu.

Da biste sinkronizirali lozinku, Azure AD Connect sinkronizaciju izdvaja vaša lozinka raspršivanje iz lokalnog servisa Active Directory. Dodatna sigurnost Obrada primjenjuje se na raspršivanje lozinku prije nego što sinkronizirati servis za provjeru autentičnosti Azure Active Directory. Lozinke se sinkroniziraju na temelju po korisniku i kronološkim redoslijedom.

Tijek stvarnih podataka sinkronizacija lozinka je slična sinkronizacije korisničkih podataka kao što su riješiti problem ili adrese e-pošte. Međutim, lozinke sinkroniziraju češće od prozoru sinkronizacije standardne direktorija za ostale atribute. Sinkronizacija lozinku pokreće svaki dvije minute. Učestalost taj postupak ne možete mijenjati. Kada sinkronizirate lozinke, prebrisati postojeću lozinku oblaka.

Prvi put, omogućite značajku sinkronizaciju lozinke, provodi početne sinkronizacije lozinki svi korisnici u opseg. Izričito ne možete definirati podskup korisničkih lozinki koje želite sinkronizirati.

Kada promijenite lozinku na lokalni ažuriranu lozinku sinkronizira, najčešće u samo nekoliko minuta.
Značajke za sinkronizaciju lozinke automatski ponovnih pokušaja pokušaja sinkronizacije nije uspjelo. Ako se pojavi pogreška tijekom pokušaja sinkronizaciju lozinke, pogreška prijavljen je na vaš preglednik događaja.

Sinkronizaciju lozinke ima bez utjecaja na trenutno prijavljeni korisnik.
Trenutna sesija oblaka servisa ne odmah utječe promjena sinkronizirane lozinke koja se pojavljuje dok ste prijavljeni na servis u oblaku. Međutim, kada servis u oblaku zahtijeva provjeru autentičnosti ponovno, morate unijeti nove lozinke.

> [AZURE.NOTE] Sinkroniziranje lozinkom podržano je samo za korisnika vrsta objekta u servisu Active Directory. Nije podržana za vrstu objekta iNetOrgPerson.

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>Kako funkcionira sinkronizacija lozinku pomoću servisa Azure AD domene
Da biste sinkronizirali svoje lozinke lokalnog [Azure servisa Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md)možete koristiti i značajke za sinkronizaciju lozinke. Ovaj scenarij omogućuje Azure AD domenske servise za provjeru autentičnosti korisnika u oblaku s sve metode koje su dostupne u lokalnih AD. Sučelje za ovaj scenarij je slična pomoću u Active Directory migracije alat (ADMT) u okruženju na lokaciji.

### <a name="security-considerations"></a>Sigurnosna pitanja vezana uz
Prilikom sinkroniziranja lozinki, običan tekst verziju lozinku nije izložen značajku sinkronizaciju lozinke za Azure AD ili bilo koji od povezane usluge.

Osim toga, postoji bez zahtjeva na lokalni Active Directory za pohranu lozinku u obliku reversibly šifrirane. Sažetak od lozinku raspršivanje Active Directory koristi se za prijenos između na lokalni AD i Azure Active Directory. Sažetak raspršivanje lozinku nije moguće koristiti za pristup resursa u lokalnog okruženja.

### <a name="password-policy-considerations"></a>Razmatranja Pravilnik za lozinke
Postoje dvije vrste lozinki koje utječe omogućavanja sinkronizacije lozinke:

1. Pravila složenosti lozinki
2. Pravilnika za istek lozinke

**Pravila složenosti lozinki**  
Kada omogućite sinkronizaciju lozinke, složenosti lozinki servisa Active Directory lokalnog nadjačati pravilnike složenosti u oblaku za sinkronizirani korisnici. Da biste pristupili servisa Azure AD možete koristiti sve valjani lozinke lokalni Active Directory.

> [AZURE.NOTE] Lozinke za korisnike koji su stvorili izravno u oblak i dalje podložne su lozinki kako je definirano u oblaku.

**Pravilnika za istek lozinke**  
Ako je korisnik u opsegu sinkronizaciju lozinke, lozinku računa oblak je postavljeno na "*Nikada ne istječe*".
Možete i dalje da biste se prijavili oblak servisa pomoću sinkronizirane lozinku koju je istekao u lokalnog okruženja. Oblak lozinku ažurira se sljedeći put promijenite lozinku u lokalnog okruženja.

### <a name="overwriting-synchronized-passwords"></a>Prebrisivanje sinkronizirati lozinki
Administrator možete ručno ponovno postavljanje lozinke pomoću komponente Windows PowerShell.

U ovom slučaju s novom lozinkom nadjačava sinkronizirane lozinku, a sve lozinki u oblaku definiran primjenjuju se na novu lozinku.

Ako promijenite lozinku lokalnog ponovno s novom lozinkom sinkronizirani s oblakom te nadjačava ručno ažuriranu lozinku.

## <a name="enabling-password-synchronization"></a>Omogućivanje sinkronizacije lozinke
Lozinka je automatski omogućena sinkronizacija, prilikom instaliranja Azure AD Connect **Ekspresne postavke**. Dodatne informacije potražite u članku [Uvod u rad s Azure AD Connect korištenju eksplicitnih postavki](./connect/active-directory-aadconnect-get-started-express.md).

Ako koristite prilagođene postavke instalacije Azure AD Connect, omogućite sinkronizaciju lozinke na stranici za prijavu korisnika. Dodatne informacije potražite u članku [Instalacija Prilagođeno Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

![Omogućivanje sinkronizacije lozinke](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Sinkronizaciju lozinke i FIPS
Ako vaš poslužitelj zaključana prema dolje prema Savezna informacije Processing Standard (FIPS), zatim MD5 je onemogućen.

**Da biste omogućili MD5 za sinkronizaciju lozinke, poduzmite sljedeće korake:**

1. Idite na **%programfiles%\Azure AD Sync\Bin**.
2. Otvorite **miiserver.exe.config**.
3. Idite na čvor **konfiguracije/runtime** (na kraju datoteka).
4. Dodajte čvor sljedeće:`<enforceFIPSPolicy enabled="false"/>`
5. Spremite promjene.

Za referencu, ova izrezak je kako će izgledati kao:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Informacije o sigurnosti i FIPS potražite u članku [AAD Sync lozinke i šifriranja FIPS usklađenosti](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>Otklanjanje poteškoća s sinkronizaciju lozinke
Ako lozinke ne sinkronizirate prema očekivanjima, ona može biti za podskup korisnika ili za sve korisnike.

- Ako imate problema s pojedinačne objekte, pogledajte [Otklanjanje poteškoća s jednog objekta koji nije sinkroniziran lozinke](#troubleshoot-one-object-that-is-not-synchronizing-passwords).
- Ako imate problema u kojima se sinkroniziraju bez lozinke, pročitajte članak [Otklanjanje poteškoća koje se sinkroniziraju bez lozinke](#troubleshoot-issues-where-no-passwords-are-synchronized).

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Otklanjanje poteškoća s jednog objekta koji nije sinkroniziran lozinki
Jednostavno možete otklanjati poteškoće sinkronizaciju lozinke tako da pročitate status objekta.

Pokrenite **Korisnici servisa Active Directory i računala**. Traženje korisnika i provjerite je li **korisnik mora promijeniti lozinku prilikom sljedeće prijave** poništeni.
![Active Directory produktivni lozinki](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
Ako je odabrana, uputite korisnika za prijavu i promjena lozinke. Da biste Azure AD privremene lozinke ne sinkroniziraju.

Ako je izgledom u servisu Active Directory, zatim sljedeći je korak da biste pratili korisnika u modula za sinkroniziranje. Slijedeći korisnika iz lokalnog imeničkog servisa Active Directory za Azure AD, vidjet ćete ako postoji opisne pogreške na objekt.

1. Pokrenite **[Upravitelj servisa za sinkronizaciju](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Kliknite **poveznike**.
3. Odaberite **Active Directory poveznik** korisnik nalazi se u.
4. Odaberite **prostor poveznik za pretraživanje**.
5. Pronađite korisnika koji tražite.
6. Odaberite karticu **o podrijetlu** i provjerite je li barem jedno pravilo za sinkronizaciju prikazuje **Sinkronizaciju lozinke** kao **True**. U konfiguraciji zadani je naziv pravila za sinkronizaciju **u iz AD - AccountEnabled korisnika**.  
    ![Informacije o podrijetlu o korisniku](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. Zatim [slijedite korisnika](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) putem metaverse prostor Azure AD poveznik. Objekt prostora poveznika moraju imati izlazni pravila za **Sinkronizaciju lozinke** postavite na **True**. U konfiguraciji zadani naziv pravila sinkronizaciju je **Odjava AAD – uključivanje korisnika**.  
    ![Poveznik prostora na svojstvima korisnika](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. Da biste vidjeli detalje sinkronizaciju lozinke objekta za proteklog tjedna, kliknite **evidenciji...**.  
    ![Videokonferencija zapisnika](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

Stupac Stanje može imati sljedeće vrijednosti:

Status | Opis
---- | -----
Uspjeh | Lozinka uspješno je sinkronizirane.
FilteredByTarget | Lozinka postavljena tako da **korisnik mora promijeniti lozinku prilikom sljedeće prijave**. Lozinka nisu sinkronizirane.
NoTargetConnection | Nema objekata u na metaverse ili u prostoru poveznik Azure AD.
SourceConnectorNotPresent | Nema objekt nije pronađen u prostoru poveznik lokalnog servisa Active Directory.
TargetNotExportedToDirectory | Objekt u prostoru poveznik Azure AD niste još izvezena je.
MigratedCheckDetailsForMoreInfo | Stavka evidencije stvorena prije Sastavi 1.0.9125.0 te se prikazuje u naslijeđene stanju.

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Otklanjanje poteškoća koje se sinkroniziraju bez lozinke
Pokrenite ponovnim pokretanjem skripte u odjeljku [dohvatite status postavke sinkronizacije lozinku](#get-the-status-of-password-sync-settings). Vam daje pregled konfiguracije sinkronizaciju lozinke.  
![PowerShell skripte izlaza iz postavke sinkronizacije lozinke](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
Ako značajka nije omogućena u Azure AD ili stanje sinkronizacije kanala nije omogućena, pokrenite čarobnjak za instalaciju za povezivanje. Odaberite **mogućnosti Prilagodi sinkronizacije** i poništavanje sinkronizaciju lozinke. Ta promjena privremeno onemogućuje značajku. Zatim ponovno pokrenite čarobnjak i ponovno omogućiti sinkroniziranje lozinkom. Pokrenuti skriptu da biste provjerite jesu li konfiguraciju točni.

Ako je skripta pokazuje da je bez intervala sustava, zatim pokrenuti skriptu u [okidača cijelog Sinkroniziraj sve lozinki](#trigger-a-full-sync-of-all-passwords). Ova skripta može se koristiti i za druge scenariji kojima konfiguraciju ispravna, ali ne sinkroniziraju lozinke.

#### <a name="get-the-status-of-password-sync-settings"></a>Dohvatite status postavke sinkronizacije lozinke

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Pokretanje cijelog Sinkroniziraj sve lozinki
Možete pokrenuti cijelog Sinkroniziraj sve lozinki pomoću sljedeće skripte:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Daljnji koraci

* [Azure AD povezivanje sinkronizacije: Mogućnosti za prilagodbu sinkronizacije](active-directory-aadconnectsync-whatis.md)
* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
