
<properties
    pageTitle="Korištenje atributa za stvaranje naprednih pravila | Microsoft Azure"
    description="Kako – da biste je stvaranju naprednih pravila za grupu uključujući podržane operatorima pravilo izraza i parametre."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules"></a>Korištenje atributa za stvaranje naprednih pravila

Azure klasični portal omogućuje stvaranje naprednih pravila da biste omogućili složenije utemeljen na atribut dinamički članstva za grupe Azure Active Directory (Azure AD).  

Kada se procjenjuje za sve atribute promjena korisnika u sustavu sva pravila dinamički grupe u direktoriju da biste vidjeli ako promjenu atributa korisnika želite pokrenuti bilo koje grupe dodaje ili uklanja. Ako korisnik zadovoljava pravilo na grupu, ona se dodaju kao član u toj grupi. Ako se više ne zadovoljava pravila grupe su članovi, oni se uklanjaju kao na članove iz te grupe.

## <a name="to-create-the-advanced-rule"></a>Stvaranje naprednih pravila

1. [Azure klasični portala](https://manage.windowsazure.com)odaberite **Active Directory**, a zatim otvorite imenik tvrtke ili ustanove.

2. Odaberite karticu **grupe** , a zatim otvorite grupu koju želite urediti.

3. Odaberite karticu **Konfiguracija** odaberite mogućnost **Napredne pravilo** , a zatim unesite pravilo za napredne u tekstni okvir.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Izgradnje tijelo naprednih pravila

Napredni pravilo koje možete stvoriti za dinamičku članstva za grupe je zapravo binarni izraz koji se sastoji od tri dijela i rezultat true ili false rezultat. Tri dijela su:

- Lijevi parametra
- Binarni operator
- Konstante udesno

Dovršavanje naprednih pravila izgleda ovako: (leftParameter binaryOperator "RightConstant"), pri čemu su potrebni za cijeli izraz binarni otvorene i zatvorene zagrade, dvostrukih navodnika su potrebni za desno konstantu, a vidjet ćete da sintaksa za parametar lijevom user.property. Naprednih pravila može se sastojati od više od jedne binarni izraza odvojene- a, - ili, i – ne logički operatori.
Slijede primjeri ispravno izgrađene naprednih pravila:

- (user.department - eq "Prodaja") – ili (user.department - eq "Marketing")
- (user.department - eq "Prodaja") – i – ne (user.jobTitle-sadrži "SDE")

Popis svih podržanih parametre i operatori pravila izraz, potražite u odjeljcima.

Ukupne duljine tijelo napredne pravilo ne može biti dulji od 2048 znakova.

> [AZURE.NOTE]
>Niz i regex operacije su slova. Možete izvesti i provjere Null, pomoću $null kao konstantu, primjerice, user.department - eq $null.
Nizovi koji sadrže ponude "potrebno unijeti prespojni znak pomoću ' znakova, na primjer, user.department - eq \`"Prodaja".

## <a name="supported-expression-rule-operators"></a>Operatori podržani izraz pravila
U sljedećoj su tablici navedeni sve operatore pravila podržani izraza i njihovih sintaksa koja će se koristiti u tijelu naprednih pravila:

| Operator        | Sintaksa         |
|-----------------|----------------|
| Nije jednako      | -ne            |
| Jednako          | -eq            |
| Ne počinje s | -notStartsWith |
| Počinje s     | -startsWith    |
| Ne sadrži    | -notContains   |
| Sadrži        | -sadrži      |
| Ne podudaraju       | -notMatch      |
| MATCH           | -podudaraju         |


## <a name="query-error-remediation"></a>Olakšava pogreške upita
U sljedećoj su tablici navedeni potencijalne pogreške i kako ih ispravili ako nastaju

| Pogreška raščlanjivanja upita     | Korištenje pogreške       | Korištenje ispravnog             |
|-----------------------|-------------------|-----------------------------|
| Pogreška: Atribut nisu podržani.                                      | (user.invalidProperty - eq "Vrijednost")       | (user.department - eq "vrijednost")<br/>Svojstvo mora podudarati s [podržane popis svojstava](#supported-properties).                          |
| Pogreška: Operator nije podržana na atribut.                       | (user.accountEnabled-prikazuje logičku vrijednost true)                                                                               | (user.accountEnabled - eq true)<br/>Svojstvo je upišite Booleove vrijednosti. Koristite podržani operatore (-eq ili - ne) na Booleove vrste iznad popisa.                                                                                                                                   |
| Pogreška: Pogreška sastavljanje upita.                                      | (user.department - eq "Prodaja")- a (user.department - eq "Marketing")(user.userPrincipalName-match"*@domain.ext") | (user.department - eq "Prodaja")- a (user.department - eq "Marketing")<br/>Logički operator moraju podudarati nešto s gornjeg popisa podržanih svojstva. (user.userPrincipalName-odgovaraju ".*@domain.ext")or(user.userPrincipalName -odgovaraju "@domain.ext$")Error u uobičajeni izraz. |
| Pogreška: Binarni izraz nije odgovarajući oblik.                     | (user.department – eq "Prodaja") (user.department - eq "Prodaja") (user.department-eq "Prodaja")                             | (user.accountEnabled - eq true)- a (user.userPrincipalName-sadrži"alias@domain")<br/>Upit sadrži više pogrešaka. Zagrada nije na pravom mjestu.                                                                                                                            |
| Pogreška: Tijekom postavljanja dinamički članstva došlo je do nepoznate pogreške. | (user.accountEnabled - eq "True" i user.userPrincipalName-sadrži"alias@domain")                               | (user.accountEnabled - eq true)- a (user.userPrincipalName-sadrži"alias@domain")<br/>Upit sadrži više pogrešaka. Zagrada nije na pravom mjestu.                                                                                                                            |

## <a name="supported-properties"></a>Podržana svojstva
Slijede svojstava korisničkih koje možete koristiti u pravilu za napredne:

### <a name="properties-of-type-boolean"></a>Svojstva upišite Booleove vrijednosti

Dopuštene operatora

* -eq


* -ne


| Svojstva     | Dopuštena vrijednost  | Korištenje                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | true false      | user.accountEnabled - eq true)  |
| dirSyncEnabled | true false null | (user.dirSyncEnabled - eq true) |

### <a name="properties-of-type-string"></a>Svojstva vrste niz

Dopuštene operatora

* -eq


* -ne


* -notStartsWith


* -StartsWith


* -sadrži


* -notContains


* -podudaraju


* -notMatch

| Svojstva                 | Dopuštena vrijednost                                                                                        | Korištenje                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Grad                       | Bilo koja vrijednost niza ili $null                                                                           | (user.city - eq "vrijednost")                                   |
| države                    | Bilo koja vrijednost niza ili $null                                                                            | (user.country - eq "vrijednost")                                |
| odjelu                 | Bilo koja vrijednost niza ili $null                                                                          | (user.department - eq "vrijednost")                             |
| riješiti problem                | Bilo koja vrijednost niza                                                                                 | (user.displayName - eq "vrijednost")                            |
| facsimileTelephoneNumber   | Bilo koja vrijednost niza ili $null                                                                           | (user.facsimileTelephoneNumber - eq "vrijednost")               |
| givenName                  | Bilo koja vrijednost niza ili $null                                                                           | (user.givenName - eq "vrijednost")                              |
| jobTitle                   | Bilo koja vrijednost niza ili $null                                                                           | (user.jobTitle - eq "vrijednost")                               |
| pošta                       | Bilo koja vrijednost niza ili $null (SMTP adresa korisnika)                                                  | (user.mail - eq "vrijednost")                                   |
| mailNickName               | Bilo koja vrijednost niza (pošta pseudonim korisnika)                                                            | (user.mailNickName - eq "vrijednost")                           |
| mobilni                     | Bilo koja vrijednost niza ili $null                                                                           | (user.mobile - eq "vrijednost")                                 |
| ID objekta                   | GUID korisničkom objektu                                                                            | (user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Nema DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Bilo koja vrijednost niza ili $null                                                                            | (user.physicalDeliveryOfficeName - eq "vrijednost")             |
| Poštanski broj                 | Bilo koja vrijednost niza ili $null                                                                            | (user.postalCode - eq "vrijednost")                             |
| preferredLanguage          | ISO kod 639-1                                                                                        | (user.preferredLanguage - eq "HR-HR")                      |
| sipProxyAddress            | Bilo koja vrijednost niza ili $null                                                                            | (user.sipProxyAddress - eq "vrijednost")                        |
| Stanje                      | Bilo koja vrijednost niza ili $null                                                                            | (user.state - eq "vrijednost")                                  |
| streetAddress              | Bilo koja vrijednost niza ili $null                                                                            | (user.streetAddress - eq "vrijednost")                          |
| Prezime                    | Bilo koja vrijednost niza ili $null                                                                            | (user.surname - eq "vrijednost")                                |
| telephoneNumber            | Bilo koja vrijednost niza ili $null                                                                            | (user.telephoneNumber - eq "vrijednost")                        |
| usageLocation              | Dva povlačenje pozivni broj zemlje                                                                           | (user.usageLocation - eq "Sad")                             |
| userPrincipalName          | Bilo koja vrijednost niza                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| userType                   | član goste $null                                                                                    | (user.userType - eq "Člana")                              |

### <a name="properties-of-type-string-collection"></a>Svojstva zbirke vrsta niza

Dopuštene operatora

* -sadrži


* -notContains

| Poperties      | Dopuštena vrijednost                        | Korištenje                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Bilo koja vrijednost niza                      | (user.otherMails-sadrži"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-sadrži "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Proširenje atribute i prilagođene atribute
Proširenje atribute i prilagođene atribute podržani su u dinamičku članstvo pravila.

Proširenje atributi sinkroniziraju s u lokalnu prozor poslužitelj AD i poduzmite oblik "ExtensionAttributeX", gdje X jednako je 1-15.
Primjer pravila koja koristi atribut kućni broj bio

(user.extensionAttribute15 - eq "Marketing")

Prilagođeni atributi sinkroniziraju s na lokalnu instancu programa Windows Server AD ili iz povezanih SaaS aplikacije i oblik "user.extension_[GUID]\__ [atribut]", gdje je jedinstveni identifikator u AAD za aplikaciju koja je stvorila atribut u AAD [GUID] i [] je atribut naziv atributa kao što je stavka stvorena.
Primjer pravila koja koristi prilagođeni atribut

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Naziv prilagođenog atributa mogli pronaći u imeniku tijekom postavljanja upita korisniku je atribut pomoću programa Explorer grafikonu i traženje naziv atributa.

## <a name="direct-reports-rule"></a>Koje su izravno odgovorne pravila
Sada možete popuniti članovi grupe na temelju atribut upravitelja korisnika.

**Da biste konfigurirali grupe kao grupu "Upravitelja"**

1. Azure klasični portalu kliknite **Active Directory**, a zatim naziv imenik tvrtke ili ustanove.

2. Odaberite karticu **grupe** , a zatim otvorite grupu koju želite urediti.

3. Odaberite karticu **Konfiguracija** , a zatim odaberite **Dodatna pravila**.

4. Upišite pravilo sljedeću sintaksu:

    Izravni izvješća za *koje su izravno odgovorne za {obectID_of_manager}*. Primjer je valjan pravilo za koje su izravno odgovorne

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    pri čemu je "62e19b97-8b3d-4d4a-a106-4ce66896a863" ID objekta upravitelja. ID objekta pronaći ćete u Azure AD **kartica profila** na stranici Dodavanje korisnika za korisnika koji je upravitelj.

3. Prilikom spremanja ovo pravilo, svi korisnici koji zadovoljavaju pravilo će biti pridruženo kao članovi grupe. To može potrajati nekoliko minuta za grupu za prethodno popunjavanje.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Stvaranje pravila za objekata uređaja pomoću atributa

Možete stvoriti i pravilo koje odabire objekata uređaja za članstvo u grupi. Sljedećim atributima uređaja mogu koristiti:

| Svojstva              | Dopuštena vrijednost                  | Korištenje                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| riješiti problem             | bilo koja vrijednost niza                | (device.displayName - eq "Robert Iphone")                       |
| deviceOSType            | bilo koja vrijednost niza                | (device.deviceOSType - eq "IOS")                             |
| deviceOSVersion         | bilo koja vrijednost niza                | (uređaj. OSVersion - eq "9,1")                                |
| isDirSynced             | true false null                 | (device.isDirSynced - eq "true")                             |
| isManaged               | true false null                 | (device.isManaged - eq "neistinito")                              |
| isCompliant             | true false null                 | (device.isCompliant - eq "true")                             |
| deviceCategory          | bilo koja vrijednost niza                | (device.deviceCategory - eq "")                              |
| deviceManufacturer      | bilo koja vrijednost niza                | (device.deviceManufacturer - eq "Microsoft")                 |
| deviceModel             | bilo koja vrijednost niza                | (device.deviceModel - eq "IPhone 7 +")                        |
| deviceOwnership         | bilo koja vrijednost niza                | (device.deviceOwnership - eq "")                             |
| Naziv domene              | bilo koja vrijednost niza                | (device.domainName - eq "contoso.com")                       |
| enrollmentProfileName   | bilo koja vrijednost niza                | (device.enrollmentProfileName - eq "")                       |
| isRooted                | true false null                 | (device.deviceOSType - eq "true")                            |
| managementType          | bilo koja vrijednost niza                | (device.managementType - eq "")                              |
| organizationalUnit      | bilo koja vrijednost niza                | (device.organizationalUnit - eq "")                          |
| deviceId                | valjani deviceId                | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d" |

> [AZURE.NOTE]
> Ta pravila uređaja nije moguće stvoriti pomoću padajućeg izbornika "jednostavno pravilo" na portalu za Azure klasični.


## <a name="additional-information"></a>Dodatne informacije
Ti članci sadrže dodatne informacije na Azure Active Directory.

* [Dinamični članstva u grupama za otklanjanje poteškoća](active-directory-accessmanagement-troubleshooting.md)

* [Upravljanje pristupom resursima s grupama Azure Active Directory](active-directory-manage-groups.md)

* [Cmdleti za Azure Active Directory za konfiguriranje postavki grupe](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)

* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
