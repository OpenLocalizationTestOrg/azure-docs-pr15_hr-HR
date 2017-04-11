<properties
   pageTitle="Azure Active Directory nadzora izvješće događaje | Microsoft Azure"
   description="Nadzirati događaja koji su dostupni za prikaz i preuzimanje s Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory nadzora izvješće događaja

*Ovo je dokumentacija je dio [Azure Active Directory izvješćivanja vodič] (Aktivno-direktorija-izvješćivanje-guide.md).*

Azure Active Directory izvješća nadzora olakšava kupci prepoznavanje povlaštene akcije kojih je došlo u svoje Azure Active Directory. Povlaštene akcije uključuju promjene visinu (Ako, na primjer, stvaranje uloga ili lozinki), promjena konfiguracije pravila (na primjer lozinki) ili promjene konfiguracije direktorija (na primjer, promjene postavki vanjskog pristupa domene). Izvješća pružaju zapisa nadzora za naziv događaja, glumca koji se izvodi akciju resursa cilj utječu promjene, te datum i vrijeme (UTC-a). Korisnici će moći dohvatiti popis događaje nadzora za svoje Azure Active Directory putem [Portala za Azure](https://portal.azure.com/)kao što je opisano u [prikazu na zapisnika nadzora](active-directory-reporting-azure-portal.md).


## <a name="list-of-audit-report-events"></a>Popis nadzora izvješće događaja
<!--- audit event descriptions should be in the past tense --->

Događaji                               | Opis događaja
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Korisnik događaja**                      |
Dodavanje korisnika                             | Dodaje korisnika u imeniku.
Brisanje korisnika                          | Izbrisati korisnika iz imenika.
Postavljanje svojstava licence               | Postavljanje svojstava licence za korisnika u imeniku.
Ponovno postavljanje korisničke lozinke                  | Ponovno postavljanje lozinke za korisnika u imeniku.
Promjena lozinke za korisnika                 | Promijeniti lozinku za korisnika u imeniku.
Promjena korisničke licence                  | Promijenjen je licenca dodijeljena korisnika u imeniku. Da biste vidjeli licenci za koje su ažurirane, pogledajte ispod [korisnika ažuriranje](#update-user-attributes) svojstava
Ažuriraj korisnika                          | Ažuriranje korisnika u imeniku. [Potražite u nastavku](#update-user-attributes) atributa koji se može ažurirati.
Postavljanje prisilno promjena lozinke korisničkog       | Postavite svojstvo koje navodi korisnika da biste promijenili svoju lozinku na prijava.
Ažurirajte korisničke vjerodajnice        | Korisnik promijeniti lozinku  
**Događaja grupe**                     |
Dodavanje grupe                            | Stvorite grupu u direktoriju.
Ažuriranje grupe                         | Ažuriranje grupe u direktoriju. Da biste pogledali svojstva koje grupe su ažurirani, pogledajte [Nadzirati svojstva grupe](#update-group-attributes) u odjeljku u nastavku
Brisanje grupe                         | Izbrisati grupu iz imenika.
Dodavanje člana u grupu            | Da biste dodali člana u grupu u direktoriju.
Uklanjanje člana iz grupe         | Uklanjanje člana iz grupe u direktoriju.
CreateGroupSettings              | Postavke stvorene grupe
UpdateGroupSettings          | Ažuriranje postavki grupe. Da biste vidjeli što postavke grupe su ažurirani, pogledajte [Nadzirati svojstva grupe](#update-group-attributes) u odjeljku u nastavku
DeleteGroupSettings            | Postavke izbrisane grupe
SetGroupLicense                  | Postavljanje grupe licence.
SetGroupManagedBy              | Postavljanje grupu koju želite upravlja korisnika
AddGroupMember               | Dodane člana u grupu
RemoveGroupMember            | Uklanjanje člana iz grupe
AddGroupOwner                | Dodane vlasnika grupe
RemoveGroupOwner                 | Vlasnik uklonjeni iz grupe
**Događaji aplikacije**               |
Dodavanje usluge                | Dodaje glavni servis imenika.
Uklanjanje usluge             | Glavni servis uklonjena iz imenika.
Dodavanje upravitelja vjerodajnice servisa    | Dodane vjerodajnice za glavni servisa.
Uklanjanje upravitelja vjerodajnice servisa | Uklonjene vjerodajnice iz servisa glavni.
Dodavanje unosa delegiranje                 | Da biste stvorili [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) u direktoriju.
Postavljanje delegiranje unosa                 | Ažurirati [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) u direktoriju.
Uklanjanje delegiranje unosa              | Izbrisati [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) u direktoriju.
**Uloga događaja**                      |
Dodavanje člana uloge uloga              | Dodaje korisnika u imeniku ulogu.
Uklanjanje člana uloge iz uloge         | Uklanja korisnika iz imenika uloge.
Postavljanje tvrtke podaci za kontakt      | Postavite kontaktnih postavki na razini tvrtke. To obuhvaća adrese e-pošte za marketing, kao i tehničke obavijesti o Microsoft Online Services.
Dodavanje unosa delegiranje                 | Da biste stvorili [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) u direktoriju.
Postavljanje delegiranje unosa                 | Ažurirati [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) u direktoriju.
Uklanjanje delegiranje unosa              | Izbrisati [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) u direktoriju.
AddSevicePrincipalOwner          | Dodane vlasnika usluge.
RemoveSevicePrincipalOwner   | Vlasnik uklonjeni glavnicu servisa.
AddApplication  | Dodaj aplikaciju.
UpdateApplication | Ažuriranje aplikacije. Da biste vidjeli koje postavki aplikacije ažurirano, pogledajte [Nadzirati svojstva aplikacije](#update-application-attributes) u odjeljku u nastavku
DeleteApplication | Brisanje aplikacije.
RestoreApplication|Vraćanje aplikacije.
AddApplicationOwner   |     Dodajte vlasnik aplikaciju.
RemoveApplicationOwner    |     Uklanjanje vlasnika iz aplikacije.
**Uloga događaja**                         |
Dodavanje člana uloge uloga                 | Dodaje korisnika u imeniku ulogu.
Uklanjanje člana uloge iz uloge            | Uklanja korisnika iz imenika uloge.
AddRoleDefinition               | Definicije dodane uloge.
UpdateRoleDefinition                | Ažurirati definicije uloge. Da biste vidjeli koje uloga postavke ažurirano, pogledajte [Uloga definicija svojstva nadzirati](#update-role-definition-attributes) u odjeljak u nastavku
DeleteRoleDefinition                | Izbrisati definicije uloge.
AddRoleAssignmentToRoleDefinition | Dodjela dodane uloge definicije uloga.
RemoveRoleAssignmentFromRoleDefinition | Dodjela uloge za uklonjen iz definicije uloge.
AddRoleFromTemplate     | Dodane uloga iz predloška.
UpdateRole  | Ažurirani uloga.
AddRoleScopeMemberToRole    | Dodane opsegu član uloge.
RemoveRoleScopedMemberFromRole  | Uklanja opsegu člana iz uloge.
**Događaje uređaja (sve nove događaje)**                    |
AddDevice               | Dodane uređaj.
UpdateDevice            | Ažurirani uređaj. Da biste vidjeli koje svojstva uređaja ažurirano, pogledajte [Svojstva uređaja Audited](#update-device-attributes) u odjeljak u nastavku
DeleteDevice            | Izbrisane uređaj.
AddDeviceConfiguration      | Konfiguracija dodane uređaja.
UpdateDeviceConfiguration   | Konfiguracija ažurirane uređaja. Da biste vidjeli koje konfiguracija svojstava uređaja su ažurirani, pogledajte [Svojstva uređaja konfiguracije Audited](#update-device-configuration-attributes) u odjeljak u nastavku
DeleteDeviceConfiguration   | Konfiguracija izbrisane uređaja.
AddRegisteredOwner     | Dodane registrirani vlasnik uređaj.
AddRegisteredUsers     | Dodaje registrirane korisnike na uređaj.
RemoveRegisteredOwner    | Uklanjanje registrirani vlasnik s uređaja.
RemoveRegisteredUsers    | Uklanjanje registrirane korisnike s uređaja.
RemoveDeviceCredentials  | Uklonite uređaj vjerodajnice.
**B2B događaja**                       |
Pozivnice za obradu prenijeti.              | Administrator je prenijeti datoteku koja sadrži pozivnice koje se šalju korisnicima partnera.
Obrada pozivnice obrađuju.             | Datoteka koja sadrži pozivnicama za korisnike partnera obrađen.
Pozivanje vanjskih korisnika.                | Vanjskog korisnika koji je pozvan imenika.
Aktivacija pozivnice za vanjske korisnike.         | Vanjskog korisnika sadrži aktivacije njihove poziv u direktoriju.
Dodavanje vanjskih korisnika u grupu.          | Vanjskog korisnika dodijeljeno članstvom grupe u direktoriju.
Dodijelite vanjskog korisnika aplikacije. | Vanjskog korisnika dodijeljen izravan pristup aplikaciji.
Stvaranje širenjem virusa te klijenta.               | Novi klijent stvoren je u Azure AD po otkup poziv.
Stvaranje širenjem virusa te korisnika.                 | Korisnik je stvorena u postojećem klijentu u Azure AD po otkup poziv.
**Administrativni jedinice (sve nove događaje)**             |
AddAdministrativeUnit   |   Dodavanje administratora jedinicu.
UpdateAdministrativeUnit|   Ažurirajte administratora jedinicu. Da biste pogledali svojstva koje administratora jedinica su ažurirani, pogledajte [nadzirati administratora jedinica svojstva](#update-administrative-unit-attributes) u odjeljku u nastavku
DeleteAdministrativeUnit|   Brisanje administratora jedinicu.
AddMemberToAdministrativeUnit|  Da biste dodali člana administratora jedinicu.
RemoveMemberFromAdministrativeUnit|     Uklanjanje člana iz administratora jedinice.
**Događaji direktorija**                 |
Dodavanje partnera za tvrtke               | Dodaje partnera imenika.
Uklanjanje partnera iz tvrtke          | Uklanja partnera iz imenika.
DemotePartner   |   Smanjenje razine partnera.
Dodavanje domene u tvrtki                | Da biste dodali domenu u direktorij.
Uklanjanje domena iz tvrtke           | Iz imenika ukloniti domenu.
Ažuriranje domene                        | Ažuriranje domene u direktoriju. Da biste pogledali svojstva koje domene ažurirano, pogledajte [nadzirati svojstva domene](#update-domain-attributes) u odjeljku u nastavku
Provjera autentičnosti postavljanje domene            | Promijenjena zadana postavka domene za tvrtku.
Postavljanje tvrtke podaci za kontakt      | Postavite kontaktnih postavki na razini tvrtke. To obuhvaća adrese e-pošte za marketing, kao i tehničke obavijesti o Microsoft Online Services.
Postavljanje postavki za vanjski pristup domeni    | Ažuriranje postavki vanjski pristup domeni.
Potvrda domene                        | Potvrde domene u direktoriju.
Provjerite je li potvrđeni domena e-pošte         | Provjeriti domene na imenik pomoću provjere e-pošte.
Postavljanje zastavice DirSyncEnabled na tvrtke   | Postavite svojstvo koja omogućuje direktorij za Azure AD sinkronizaciju.
Postavljanje pravilnika za lozinke                  | Postavljanje ograničenja duljine i znak za korisničke lozinke.
Informacije o tvrtki za postavljanje              | Ažuriranih informacija na razini tvrtke. Cmdlet ljuske PowerShell [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) dodatne pojedinosti potražite u članku.
SetCompanyAllowedDataLocation  |    Postavljanje tvrtke dopušteno mjesto podataka.
SetCompanyDirSyncEnabled    |   Postavljanje zastavice DirSyncEnabled.
SetCompanyDirSyncFeature |  Postavljanje značajke DirSync.
SetCompanyInformation |   Informacije za postavljanje tvrtke.
SetCompanyMultiNationalEnabled    |     Postavljanje tvrtke multinacionalnoj omogućena.
SetDirectoryFeatureOnTenant   |     Postavljanje značajke imenika na klijentu.
SetTenantLicenseProperties  |   Postavljanje svojstava licence klijenta.
CreateCompanySettings   |   Stvaranje postavki za tvrtke
UpdateCompanySettings   |   Ažuriranje postavki tvrtke. Da biste vidjeli koje svojstva tvrtke ažurirano, pogledajte [nadzirati tvrtke svojstva](#update-company-attributes) u odjeljku u nastavku
DeleteCompanySettings   |   Brisanje postavki tvrtke
SetAccidentalDeletionThreshold   |  Postavite prag nenamjerno brisanje.
SetRightsManagementProperties   |   Postavite rights management svojstva.
PurgeRightsManagementProperties     |   Čišćenje rights management svojstva.
UpdateExternalSecrets   |   Ažuriranje vanjske tajne
**Pravilnik o događajima (sve nove događaje)**           |
AddPolicy   |   Dodavanje pravila.
UpdatePolicy    |   Ažurirajte pravila.
DeletePolicy    |   Brisanje pravila.
AddDefaultPolicyApplication     |   Dodavanje pravila za aplikaciju.
AddDefaultPolicyServicePrincipal    |   Dodavanje pravila za usluge.
RemoveDefaultPolicyApplication  |   Uklanjanje pravila iz aplikacije.
RemoveDefaultPolicyServicePrincipal     |   Uklanjanje usluge pravila.
RemovePolicyCredentials     |   Uklanjanje pravila vjerodajnice.

## <a name="audit-report-retention"></a>Zadržavanje nadzora izvješće
Događaji u izvješću Azure AD nadzora zadržavaju se za 180 dana. Dodatne informacije o zadržavanju na izvješća potražite u članku [Azure Active Directory izvješća o zadržavanju](active-directory-reporting-retention.md).

U slučaju korisnika zanimati spremanje svoje događaje nadzora za više razdoblja zadržavanja API izvješćivanje mogu se redovito uvesti događaje nadzora u zasebne podatke trgovine. Potražite u članku [Uvod u izvješća API](active-directory-reporting-api-getting-started.md) detalje.

## <a name="properties-included-with-each-audit-event"></a>Svojstva uključene svaki događaj nadzora

Svojstvo      | Opis
------------- | --------------------------------------------------------------
Datum i vrijeme | Datum i vrijeme do kojih je došlo događaj nadzora
Glumca         | Korisniku ili Upravitelj servisa koji će se izvršiti akciju
Akcija        | Akcija koju je izvršila
Cilj        | Korisniku ili Upravitelj servisa koji je akcija izvedena na


## <a name="update-user-attributes"></a>Atributi "Ažuriraj korisnika"
Događaj nadzora "Ažuriranje korisnik" sadrži dodatne informacije o koje korisničke atribute su ažurirani. Za svaki atribut prethodne vrijednosti i novu vrijednost uključen.

Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | Korisnik mogu se prijaviti u.
AssignedLicense                     | Sve licenci koje su dodijeljene korisnika.
AssignedPlan                        | Tarife za usluge dobivenima licence korisniku dodijeliti.
LicenseAssignmentDetail             | Detalje o licence korisniku dodijeliti. Ako, primjerice, ako grupne licenciranje je uključen, to će uključiti grupi dodijeljena licenca.
Mobile                              | Mobilni telefon korisnika.
OtherMail                           | Adresa zamjenska e-pošte za korisnika.
OtherMobile                         | Korisničke zamjenski mobilni telefon.
StrongAuthenticationMethod          | Popis metoda provjere konfigurirana tako da korisnik za višestruke provjere autentičnosti, kao što su govorne poziva, SMS ili potvrdu kod iz mobilne aplikacije.
StrongAuthenticationRequirement     | Ako se provodi višestruka provjera autentičnosti, omogućiti ili onemogućiti za tog korisnika.
StrongAuthenticationUserDetails     | Telefonski broj korisnika, zamjenski telefonski broj i e-pošte adresu koja se koristi za potvrdu ponovno postavljanje višestruke provjere autentičnosti i lozinku.
StrongAuthenticationPhoneAppDetail  | Aplikacije za telefon forof detalja registriran za izvođenje 2FA prijava
TelephoneNumber                     | Korisnikov telefonski broj.
AlternativeSecurityId               | Zamjenski sigurnosni ID objekta.
CreationType                        |Način stvaranja korisnika (ili poziv širenjem virusa te).
InviteTicket                        |Popis poziva karata za korisnika.
InviteReplyUrl                      |Popis URL adrese za odgovor na pozivnicu prihvatila.
InviteResources                     |Popis resursa na koji je pozvan korisnika.
LastDirSyncTime                     |Posljednjeg objekt ažuriranja zbog sinkroniziranje s na mjerodavne direktorija (Kupac, na lokaciji).
MSExchRemoteRecipientType           |Karte u MSO primatelja vrste. Pogledajte https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx [MSO primatelja vrste] za primatelja vrste
PreferredDataLocation               |Preferirani mjesto za korisnika, grupe, kontakta, javne mape ili uređaja podataka.
ProxyAddresses                      |Adresa objekta primatelja Exchange Server koji nije prepoznata u sustav vanjski pošte.
StsRefreshTokensValidFrom           |Prenosi Osvježi podatke tokena opoziva – sve tokeni osvježavanja STS izdavanja prije ovaj put smatraju istekle.
UserPrincipalName                   |UPN-a koji je ime za prijavu internetske za korisnika.
UserState                           |Stanje korisnika odobrenje nije riješeno/PendingAcceptance/prihvaćene/PendingVerification.
UserStateChangedOn                  |Vremenske oznake zadnju promjenu UserState. Služi za okidanje tijekova rada za životni ciklus.
UserType                            |Vrsta korisnika. Član (0), goste (1), Viral(2).

## <a name="update-group-attributes"></a>Atributi "Grupu update"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Klasifikacija            | Klasifikacija Unified grupe (HBI, MBI itd.).
Opis               | Ljudski čitljiv opisni izraze o objekt.  
Riješiti problem               |Zaslonski naziv za objekt
DirSyncEnabled            |Upućuje na to pojavljuje li se iz programa mjerodavne sinkronizacije direktorija (Kupac, na lokaciji).
GroupLicenseAssignment    |Dodjela licence grupe
GroupType                 |Vrstu grupe, Sjedinjeno komuniciranje (0)
IsMembershipRuleLocked    |Pokazuje da je svojstvo MembershipRule je postavio samostalno grupu servisa za upravljanje i korisnici ne možete mijenjati. Odnosi se samo na grupe gdje GroupType uključuje GroupType.DynamicMembership
IsPublic                  |Označite da biste naznačili ako je grupa javno/privatno.
LastDirSyncTime           |Posljednjeg objekt ažuriranja uslijed sinkroniziranje s na mjerodavne direktorija (kupac u lokalnu).
Pošta                      |Adresa e-pošte primarnog
MailEnabled               |Označava hoće li ovu grupu ima mogućnost e-pošte.
MailNickname              |Nadimak za objekt adresara adresu, obično dio prethodnog nazivu e-pošte na "@" simbol.   
MembershipRule            |Niz na kojem se iznosi ono kriterije servisa za upravljanje samostalno grupe da biste utvrdili koji članovi moraju pripadaju ovu grupu. Vidi također IsMembershipRuleLocked. Odnosi se samo na grupe gdje GroupType uključuje GroupType.DynamicMembership.
MembershipRuleProcessingState| Redni broj definira servisa za upravljanje samostalno grupe koji definira stanje članstva obrade za tu grupu. Odnosi se samo na grupe gdje GroupType uključuje GroupType.DynamicMembership.
ProxyAddresses            |Adresa objekta primatelja Exchange Server koji nije prepoznata u sustav vanjski pošte.
RenewedDateTime           |Vremenska oznaka zapis kada je grupu nedavno obnovljena.   
SecurityEnabled           |Označava hoće li članstvo u grupi može utjecati autorizacije odluka.
WellKnownObject           |Prikazana objekt direktorija, označite oznakom kao jedan od unaprijed definiranih skupa.

## <a name="update-device-attributes"></a>Atributi "Ažuriranje uređaj"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Označava hoće li sigurnosni upravitelj možete provjeru autentičnosti.
CloudAccountEnabled | Označava hoće li sigurnosni upravitelj možete provjeru autentičnosti. Kada je uređaj usvojili u lokalnu napisao InTune.
CloudDeviceOSType | Vrsta uređaja na temelju OS-a – primjerice sa sustavom Windows RT, iOS. Kada postavite tako da neki servis u oblaku (kao što je Intune), taj atribut postaje mjerodavne u imeniku za DeviceOSType.
CloudDeviceOSVersion | Verzija OS-a. Kada postavite tako da neki servis u oblaku (kao što je Intune), taj atribut postaje mjerodavne u imeniku za DeviceOSVersion.
CloudDisplayName | Vrijednost atributa LDAP riješiti problem. Kada postavite tako da neki servis u oblaku (kao što je Intune), taj atribut postaje mjerodavne u direktoriju za riješiti problem.
CloudCreated |Označava hoće li se objekt je stvorena pomoću servisa u oblaku.
CompliantUntil | Do koje se doba uređaj se smatra usklađen.
DeviceMetadata |Prilagođeni metapodataka za uređaj
DeviceObjectVersion | Taj atribut koristi se za identificiranje verzije sheme uređaja.
DeviceOSType |Vrsta uređaja na temelju OS-a – primjerice sa sustavom Windows RT, iOS. Napisao servis za registraciju i namijenjen ažurirati tako da u MDM servisa za upravljanje ili STS Osnovno servisa za upravljanje.
DeviceOSVersion |Verzija OS-a. Napisao servis za registraciju i namijenjen ažurirati tako da u MDM servisa za upravljanje ili STS Osnovno servisa za upravljanje.
DevicePhysicalIds |Atribut s više vrijednosti predviđene za identifikatore fizički uređaj za pohranu. To može obuhvaćati BIOS ID-ove, TPM thumbprints, hardver određene ID-a, itd.
DirSyncEnabled | Upućuje na to pojavljuje li se iz programa mjerodavne (Kupac, u lokalnu) sinkronizacije direktorija.
Riješiti problem |Zaslonski naziv za objekt
IsCompliant | Taj atribut koristi se za upravljanje statusom upravljanja mobilnim uređajima uređaja.
IsManaged |Taj atribut koristi za označavanje uređaj upravlja prikazom oblaka MDM.
LastDirSyncTime |Posljednjeg objekt ažuriranja zbog sinkroniziranje s na mjerodavne direktorija (kupac u lokalnu).

## <a name="update-device-configuration-attributes"></a>Atributi "Ažuriranje Konfiguracija uređaja"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Maksimalni broj dana koji se uređaj može biti Neaktivni prije smatra se za uklanjanje.
RegistrationQuota   |Pravila koja se koristi za ograničavanje broja registracije uređaja za jednog korisnika.

## <a name="update-service-principal-configuration-attributes"></a>Atributi "Ažuriranje glavni konfiguracija servisa"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Označava hoće li sigurnosni upravitelj možete provjeru autentičnosti.
AppPrincipalId | Vanjski, definira aplikacija identiteta za sigurnosni upravitelj.
Riješiti problem |Zaslonski naziv za objekt
ServicePrincipalName    | Glavni naziv servisa, koji sadrži "naziv/izdavanje" gdje naziv određuje klasu vrijednost aplikacije i za izdavanje certifikata sadrži najmanje glavno računalo [: priključak] "naziv" ili koji određuje identifikator za Upravitelj servisa.

## <a name="update-app-attributes"></a>Atributi "Ažurira aplikaciju"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Postavljanje adrese (preusmjeravanje URL-ovi) koje su dodijeljene glavni servisa.
ID programa                               |ID aplikacije aplikacije   
AppIdentifierUri | Aplikacija URI, koje označava aplikacije.  To je obično URL aplikacije programa access.
AppLogoUrl |Url za sliku logotipa aplikacije pohranjene u ustvari CDN.
AvailableToOtherTenants | TRUE aplikacija nije više klijentske aplikacije (odnosno mogu koristiti druge klijenata).
Riješiti problem | Zaslonsko ime za naziv aplikacije
Prava |Popis aplikacija prava.
ExternalUserAccountDelegationsAllowed |Zastavica koja označava li aplikacija resursa pouzdanih jedan i možete stvoriti delegiranje stavke za računima vanjskih korisnika.
GroupMembershipClaims |Pravila zahtjevima članstvo grupe.
PublicClient | TRUE ako je klijent ne možete zadržati tajnu (odnosno koje nisu povjerljive klijent u OAuth2.0)
RecordConsentConditions | Vrste pristanak uvjeta, kako je definirano odredbe ugovora: ništa (0), SilentConsentForPartnerManagedApp(1). Ta vrijednost će prikazanima u shemi API grafikonu, a može biti samo skup/promijenio administratore klijenta.
RequiredResourceAccess |Vrijednost svojstva RequiredResourceAccess XML sadržaj.
WebApp |Ako je true, znači da ovu aplikaciju web-aplikacijama.
WwwHomepage |Primarni web-stranicu.

## <a name="update-role-attributes"></a>Atributi "Ažuriranje uloga"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |Postavljanje adrese (preusmjeravanje URL-ovi) koje su dodijeljene glavni servisa.
BelongsToFirstLoginObjectSet |Ako je true, označava taj objekt pripada skup objekata koje su potrebne za omogućivanje prijave prvi administrator novi klijent.
Ugrađene |Označava hoće li vijek objekta vlasnik sustav.
Opis | Ljudski čitljiv opisni izraze o objekt.  
Riješiti problem |Zaslonski naziv za objekt
MailNickname | Nadimak za objekt adresara adresu, obično dio prethodnog nazivu e-pošte na "@" simbol.
RoleDisabled |Označava hoće li se ulogu zanemariti svrhu pristupa provjere.
RoleTemplateId | Identitet predloška uloge.
ServiceInfo |Servis informacije specifične za dodjelu resursa koji mogu biti troše MOAC i/ili druge usluge instance (vrste istom ili drugom servisa).
TaskSetScopeReference |Služi za identifikaciju na TaskSet i postavljanje dosega vezane uz uloga ili lokalizirati.
ValidationError |Podaci koji se objavljuje pridruženim servisa s opisom koji nisu prolaznim, specifične za servis pogreške vezane uz svojstva ili vezu od akcija objekta administrator da biste riješili.
WellKnownObject |Prikazana objekt direktorija, označite oznakom kao jedan od unaprijed definiranih skupa.

## <a name="update-role-definition-attributes"></a>Atributi "Ažuriranja definicije uloge"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |Zbirka autorizacije dosega koji se pozivati prilikom dodjele ovaj RoleDefinition sigurnosni upravitelj.
Riješiti problem     |Zaslonski naziv za objekt
GrantedPermissions  |Pravo ovaj RoleDefinition.


## <a name="update-administrative-unit-attributes"></a>Atributi "Ažuriranje administratora jedinica"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Opis |   Ovo svojstvo će se ažurirati kada promijenite opis administratora jedinice.
Riješiti problem |   Ovo svojstvo će se ažurirati kada promijenite naziv administratora jedinice.

## <a name="update-company-attributes"></a>Atributi "Ažuriranje tvrtke"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Mjesto dopušteno tvrtke korisnika da se dodjeli.
AuthorizedServiceInstance   | Nazive koji mogu biti implementirano plan instanci servisa.
DirSyncEnabled |Upućuje na to pojavljuje li se iz programa mjerodavne (Kupac, u lokalnu) sinkronizacije direktorija.
DirSyncStatus |Označava je li adresa adresara objekata u ovom kontekstu klijenta sinkronizaciji iz mjerodavne (klijentom, u lokalnu) direktorija; Proširenje DirSyncEnabled svojstva za objekte za tvrtke.
DirSyncFeatures |Zastavica bitne da biste pratili skup značajki omogućeni i onemogućene dirsync za klijenta.
DirectoryFeatures |Značajke omogućeno/onemogućeno direktorija.
DirSyncConfiguration |Sadrži sve DirSync konfiguracije specifične za trenutni klijent.
Riješiti problem |Zaslonski naziv za objekt
IsMnc |Booleova zastavice postavljeno na "true" iff tvrtke omogućena za značajku multinacionalnoj tvrtke.
ObjectSettings | Skup postavki primjenjuju na opseg objekta.
PartnerCommerceUrl |URL-a na partnersko web-mjesto trgovine.
PartnerHelpUrl |URL web-mjesto za pomoć i partnera.
PartnerSupportEmail | URL adresa e-pošte za podršku i partnera.
PartnerSupportTelephone |URL-a i partnera telefona za podršku.
PartnerSupportUrl |URL web-mjesto za podršku i partnera.
StrongAuthenticationDetails |Detalji o vezane uz StrongAuthentication.
StrongAuthenticationPolicy |Jaka provjera autentičnosti pravila za tvrtku.
TechnicalNotificationMail |Adresa e-pošte da biste obavijesti tehničkih problema odnose u tvrtki.
TelephoneNumber |Telefonski brojevi u skladu s E.123 preporuke ITU.
TenantType |Vrsta klijenta. Ako vrijednost nije naveden, klijent je tvrtka. U suprotnom moguće vrijednosti su: MicrosoftSupport (0), SyndicatePartner (1), a zatim BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |Postavljanje naziva domena DNS vezana uz u tvrtki.

## <a name="update-domain-attributes"></a>Atributi "Ažuriranje domene"
Atribut                           | Opis
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Mogućnosti    |Bit zastavice s opisom mogućnosti domenu, ako postoje.
Zadani |Označava je li domena zadanu vrijednost; na primjer, zadani UserPrincipalName sufiks kada administrator stvara novi korisnik u MOAC.
Početna |Označava je li domena na početnu domenu za tvrtke, kao što je koje OCP. Na početnu domenu je jedinstveni poddomenu Microsoft Online domene; e.g.contoso3.microsoftonline.com.
LiveType    |Vrsta odgovarajuće Windows Live naziva, ako postoje.
Ime    | Identifikator za krajnju točku.
PasswordNotificationWindowDays  |Broj dana prije lozinke isteka korisnik prima obavijest.
PasswordValidityPeriodDays  | Broj dana lozinka je dobro za prije mora se promijeniti.

Zapisi nadzora su potrebne kontrole za mnoge pravilnicima za usklađenost. Za korisnike koji se koriste Azure Active Directory nadzora izvješće da bi odgovarao njihove pravilnicima za usklađenost, preporučuje se da je korisnik odabrao poslati kopiju ove teme pomoći s kopijom kupca izvezene nadzora izvješće objasniti Detalji izvješća. Li na Revizor da biste shvatili pravilnicima za usklađenost koji trenutno ispunjava Azure, izravno Revizor [stranice usklađenost](https://azure.microsoft.com/support/trust-center/compliance/) sustava Microsoft Azure Trust Center.
