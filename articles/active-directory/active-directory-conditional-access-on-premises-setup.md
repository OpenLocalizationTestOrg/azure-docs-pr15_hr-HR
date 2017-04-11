<properties
    pageTitle="Postavljanje pristupa uvjetno lokalnog pomoću Azure Active Directory Registracija uređaja | Microsoft Azure"
    description="Postupni vodič da biste omogućili uvjetno pristup lokalnog aplikacije koje se koriste servis Active Directory Federation (AD FS) u sustavu Windows Server 2012 R2."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>Postavljanje pristupa uvjetno lokalnog pomoću Azure Active Directory Registracija uređaja

Osobno vlasništvu uređaji korisnika može biti označen zna da vašoj tvrtki ili ustanovi tako da zahtijevate od korisnika da radno okruženje spoj svoje uređaje sa servisom Azure Active Directory Registracija uređaja. U nastavku je Postupni vodič da biste omogućili uvjetno pristupa lokalnim aplikacije koje se koriste servis Active Directory Federation (AD FS) u sustavu Windows Server 2012 R2.

> [AZURE.NOTE]
> Licence za Office 365 ili Azure AD Premium potreban je kada pomoću uređaja registrira za pravila za uvjetno pristup servisa Azure Active Directory Registracija uređaja. To obuhvaća pravila nametnuo Active Directory Federation Services (AD FS) na lokalni resurse.

Dodatne informacije o scenarija uvjetno pristupa za lokalni potražite u članku [Pridruživanje s mjestom u bilo kojem uređaju za SSO i objedinjenog drugi faktor provjere autentičnosti putem tvrtke aplikacije](https://technet.microsoft.com/library/dn280945.aspx).

Te mogućnosti dostupne korisnicima koji kupite licencu za Azure Active Directory Premium su.

<a name="supported-devices"></a>Podržani uređaji
-------------------------------------------------------------------------
* Windows 7 domene pridruženo uređaja.
* Windows 8.1 osobno i domenu pridruženo uređaja.
* iOS 6 i noviji za preglednik Safari
* Android 4.0 ili noviji, Samsung GS3 ili iznad telefoni, bilješka Samsung 2 ili iznad tableta.


<a name="scenario-prerequisites"></a>Scenarij preduvjeti
------------------------------------------------------------------------
* Pretplata na Office 365 ili Premium Azure Active Directory
* Klijent za Azure Active Directory
* Windows Server Active Directory (Windows Server 2008 ili noviji)
* Ažurirani shemu u sustavu Windows Server 2012 R2
* Licence za Premium Azure Active Directory
* Windows Server 2012 R2 vanjski pristup Services konfigurirana za SSO za Azure AD
* Windows Server 2012 R2 Web aplikacije Proxy Microsoft Azure Active Directory povezivanje (Azure AD Connect). [Preuzimanje Azure AD Connect ovdje](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* Provjerene domene.

<a name="known-issues-in-this-release"></a>Poznati problemi u ovom izdanju
-------------------------------------------------------------------------------
* Pravila uvjetnog access uređaja koji se temelje zahtijevaju uređaj objekt pisanje-natrag sa servisom Active Directory iz servisa Azure Active Directory. Može potrajati i do 3 sata za uređaj objekte biti sastavljene natrag sa servisom Active Directory
* uređaje sa sustavom iOS 7 uvijek tražit će od korisnika da biste odabrali certifikat tijekom provjere autentičnosti certifikat klijenta.
* Neke verzije iOS8, prije iOS 8,3 ne funkcioniraju.

## <a name="scenario-assumptions"></a>Pretpostavke scenarija
Ovaj scenarij pretpostavlja da imate hibridnog okruženja sastoji se od klijent za Azure AD i lokalni Active Directory. Ove klijenata mora biti povezan pomoću Azure AD Connect i s provjerene domene i AD FS za SSO. Kontrolni popis ispod pronaći ćete konfigurirati vaše okruženje za radni prostor na prethodno opisan.

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>Kontrolni popis: Preduvjeti za uvjetno pristup scenarija
--------------------------------------------------------------
Povezivanje klijentu za Azure AD pomoću lokalni Active Directory.

## <a name="configure-azure-active-directory-device-registration-service"></a>Konfiguriranje servisa za registraciju uređaja Azure Active Directory
Koristite ovaj vodič možete implementirati i konfigurirati Azure Active Directory uređaj servis za registraciju za tvrtku ili ustanovu.

Ovaj vodič pretpostavlja da ste konfigurirali Windows Server Active Directory i ste se pretplatili na Microsoft Azure Active Directory. Pogledajte preduvjete iznad.

Da biste implementirali Azure Active Directory uređaj servis za registraciju s klijentom Azure Active Directory, dovršite zadatke u sljedeće kontrolnog popisa, redoslijedom. Nakon referentne veze vodi vas na temu konceptualni, vratite se na navedeni popis za provjeru kad pregledate konceptualni temu tako da možete nastaviti s preostale zadaci u navedeni popis za provjeru. Neke zadatke neće sadržavati stavku korak provjere valjanosti scenarij kojih možete potvrditi korak je uspješno dovršena.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>1.dio: Registracija uređaja Omogući Azure Active Directory

Slijedite kontrolnog popisa možete omogućiti i konfigurirati Azure Active Directory uređaj servis registraciju u nastavku.

| Zadatak                                                                                                                                                                                                                                                                                                                                                                                             | Referenca                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Omogućivanje Registracija uređaja u klijentu za Azure Active Directory da biste omogućili uređaje da biste se pridružili na radnom mjestu. Prema zadanim postavkama višestruka provjera autentičnosti nije omogućen za servis. Međutim, višestruka provjera autentičnosti se preporučuje ako Registracija uređaja. Prije omogućivanja višestruke provjere autentičnosti u ADRS, provjerite je li AD FS konfiguriran za davatelja usluga višestruke provjere autentičnosti. | [Omogućivanje Registracija uređaja Azure Active Directory](active-directory-conditional-access-device-registration-overview.md)               |
| Uređaji će otkrijte Azure Active Directory uređaj Registracija uslugu traženjem poznati DNS zapisa. Vaša tvrtka DNS-a morate konfigurirati tako da uređaji mogu otkriti Azure Active Directory uređaj Registracija uslugu.                                                                                                                                                   | [Konfiguriranje otkrivanje Azure Active Directory Registracija uređaja.](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Dio 2: Implementirati i konfigurirati Windows Server 2012 R2 Active Directory Federation Services i postavljanje vanjskog pristupa odnos s Azure AD

| Zadatak                                                                                                                                                                                                                                                                                                                                                                                             | Referenca                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Implementacija servisa Active Directory Domain Services domenu s proširenja za Windows Server 2012 R2 shema. Ne morate nadograditi bilo kontrolera domene u sustavu Windows Server 2012 R2. Nadogradnja sheme je samo zahtjev. | [Nadogradnja sheme domene servisa Active Directory](#upgrade-your-active-directory-domain-services-schema)               |
| Uređaji će otkrijte Azure Active Directory uređaj Registracija uslugu traženjem poznati DNS zapisa. Vaša tvrtka DNS morate konfigurirati tako da uređaji mogu otkriti Azure Active Directory uređaj Registracija uslugu.                                                                                                                                                   | [Priprema podrška za uređaje servisa Active Directory](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>Dio 3: Omogući uređaj upisima u Azure AD

| Zadatak                                                                                                                                                                                                                                                                                                                                                                                             | Referenca                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Dovršite 2.dio Omogućavanje uređaja upisima u Azure AD Connect. Po dovršetku, vratite to ovaj vodič. | [Omogućivanje upisima uređaja u Azure AD Connect](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[Neobavezni] Dio 4: Omogući višestruka provjera autentičnosti

Preporučuje se konfigurirati jednu od nekoliko mogućnosti za višestruke provjere autentičnosti. Ako želite da se traži MFA, potražite u članku [Odabir višestruku sigurnosno rješenje umjesto vas](../multi-factor-authentication/multi-factor-authentication-get-started.md). Obuhvaća opis svakog rješenja i veze za konfiguriranje rješenja po izboru.

## <a name="part-5-verification"></a>Dio 5: Provjera

Uvođenje je dovršena. Sada možete isprobavanje nekim scenarijima. Slijedite veze u nastavku da biste Poigrajte se sa servisom, a zatim se upoznali sa značajkama


| Zadatak                                                                                                                                                                                                                         | Referenca                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Uključiti neke uređaje s radnim mjestom putem Azure Active Directory Registracija uređaja. Možete se uključiti iOS, Windows i uređaje sa sustavom Android                                                                                         | [Uključivanje uređaja s radnim mjestom putem Azure Active Directory Registracija uređaja](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Možete pogledati i omogućiti ili onemogućiti registrirani uređajima pomoću portala za administratore. U ovom ćete zadatku će vidjeti neke registriranu uređaje pomoću portala za administratore.                                                        | [Pregled registracije uređaja za Azure Active Directory](active-directory-conditional-access-device-registration-overview.md)                             |
| Provjerite da objekata uređaja zapisuju natrag iz servisa Azure Active Directory za Windows Server Active Directory.                                                                                                                  | [Provjerite je li registrirani pravilno napisan natrag sa servisom Active Directory](#verify-registered-devices-are-written-back-to-active-directory)                  |
| Sad kad korisnici mogu registrirati svoje uređaje, možete stvoriti aplikaciju access pravila u AD FS koje omogućuju samo registrirani uređaje. U ovom ćete zadatku stvarate pravilo aplikacije programa access i prilagođeni pristup odbijen poruke | [Stvaranje pravila za aplikaciju programa access i prilagođeni pristup zabranjen poruke](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Azure Active Directory integrirati s lokalnim servisom Active Directory
To će vam pomoći integriranje klijentu za Azure AD s vašeg lokalnog servisa active directory, pomoću Azure AD Connect. Iako korake su dostupni na portalu za Azure klasični, zabilježite posebne upute navedene u ovom odjeljku.

1.  Prijavite se na portal za Azure klasični pomoću računa koji je globalni Administrator u Azure AD.
2.  U lijevom oknu odaberite **Servisa Active Directory**.
3.  Na kartici **direktorija** odaberite direktorija.
4.  Odaberite karticu **Integracija direktorija** .
5.  U odjeljku **Upravljanje i** slijedite korake od 1 do 3, čime se integrirati Azure Active Directory s lokalnog imenika.
  1.    Dodavanje domene.
  2.    Instalirajte i pokrenite Azure AD Connect: instalacija Azure AD Connect prema sljedećim uputama [prilagođenu instalaciju programa Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).
  3. Provjerite i upravljanje Sinkroniziranje direktorija. Jedan upute za prijavu dostupnih u ovom koraku.

  > [AZURE.NOTE]
  > Konfiguriranje vanjskog pristupa AD fs, kao što je vidljivo dokument povezan iznad. Ne morate konfigurirati neke značajke za pregled.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Nadogradnja sheme Active Directory Domain Services

> [AZURE.NOTE]
> Nadogradnja sheme servisa Active Directory nije moguće poništiti. Preporučuje se da najprije dovršite to u okruženje za testiranje.

1. Prijavite se na upravljaču domene pomoću računa s ovlastima administrator tvrtke i administratore sheme.
2. Kopirajte direktorija **\support\adprep [media]** i podređene direktorija nešto vaše kontrolera domene servisa Active Directory.
3. Pri čemu je [media] put do Windows Server 2012 R2 instalacijskog medija.
4. Iz naredbenog retka, idite do pokrenete i izvršavanje: **adprep.exe/forestprep**. Slijedite upute na zaslonu da biste dovršili nadogradnju shemu.

## <a name="prepare-your-active-directory-to-support-devices"></a>Priprema sustava Active Directory za podršku uređaja

>[AZURE.NOTE] Ovo je jednokratni postupak koji morate pokrenuti da biste pripremili servisa Active Directory skupa stabala za podršku uređaja. Morate biti prijavljeni s administratorskim dozvolama enterprise i skupa stabala sustava Active Directory moraju imati shemi Windows Server 2012 R2 da biste dovršili postupak.


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>Priprema Active Directory skupa stabala za podršku uređajima

> [AZURE.NOTE]
>Ovo je jednokratni postupak koji morate pokrenuti da biste pripremili servisa Active Directory skupa stabala za podršku uređaja. Morate biti prijavljeni s administratorskim dozvolama enterprise i skupa stabala sustava Active Directory moraju imati shemi Windows Server 2012 R2 da biste dovršili postupak.

### <a name="prepare-your-active-directory-forest"></a>Priprema skupa stabala na servisu Active Directory

1.  Na vanjskom poslužitelju, otvorite prozor naredbu komponente Windows PowerShell i upišite: inicijalizaciju ADDeviceRegistration
2.  Kada se zatraži **ServiceAccountName**, unesite naziv računa za servis koji ste odabrali kao račun servisa za AD FS. Ako je račun za gMSA, unesite račun u obliku **domain\accountname$** . Koristite oblik **domain\accountname**za račun domene.



### <a name="enable-device-authentication-in-ad-fs"></a>Omogući provjeru autentičnosti uređaja u AD FS

1. Na vanjskom poslužitelju otvorite konzola za AD FS i idite na **AD FS** > **Pravila za provjeru autentičnosti**.
2. Odaberite **Uredi globalni primarni autentičnosti...** u oknu **Akcije** .
3. Potvrdite **Omogući provjeru autentičnosti uređaja** , a zatim odaberite**u redu**.
4. Prema zadanim postavkama AD FS povremeno uklonit će Neiskorišteni uređaje iz servisa Active Directory. Ovaj zadatak morate onemogućiti prilikom korištenja Azure Active Directory Registracija uređaja tako da se upravlja uređaja u Azure.


### <a name="disable-unused-device-cleanup"></a>Onemogućivanje čišćenje Neiskorišteni uređaja
1. Na vanjskom poslužitelju, otvorite prozor naredbu komponente Windows PowerShell i upišite: postavljanje AdfsDeviceRegistration - MaximumInactiveDays 0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Priprema Azure AD Connect za uređaj upisima

1.  Dovršite 1.dio: Priprema Azure AD povezivanje.


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>Uključivanje uređaja s radnim mjestom putem Azure Active Directory Registracija uređaja

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>Uključivanje uređaju sa sustavom iOS pomoću Azure Active Directory Registracija uređaja

Azure Active Directory uređaj Registracija koristi postupka Registracija Over-the-Air profila za uređaje sa sustavom iOS. Ovaj postupak započinje s korisnikom povezivanju s URL registraciju profila pomoću web-preglednika Safari. Oblik URL-a je na sljedeći način:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Gdje `yourdomainname` je naziv domene koji ste konfigurirali s Azure Active Directory. Na primjer, ako vaš naziv domene je contoso.com, URL bio sljedeći:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Postoje različite načine komunikacije ovaj URL za korisnike. Preporučeni način je da biste objavili ovog URL-a u programu access prilagođenu aplikaciju zabranjen poruku u AD FS. To opisana su u odjeljku nadolazeće: [Stvori pravilo aplikacije programa access i prilagođeni pristup zabranjen poruke](#create-an-application-access-policy-and-custom-access-denied-message).

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>Uključivanje Windows 8.1 uređaja pomoću Azure Active Directory Registracija uređaja

1. Na uređaju Windows 8.1, idite na **Postavke PC -JA** > **mrežni** > **radnom mjestu**.
2. Unesite svoje korisničko ime u obliku UPN-a. Na primjer, dan@contoso.com..
3. Odaberite **Pridruži se**.
4. Kada se to od vas zatraži, prijavite se pomoću vjerodajnica. Uređaj sada uključili.

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>Uključivanje u sustavu Windows 7 uređaja pomoću Azure Active Directory Registracija uređaja
Da biste registrirali domenu u sustavu Windows 7 pridruženo uređaja morate implementaciju softvera paket za registraciju uređaja. Softverski paket naziva radnog mjesta uključivanje u sustavima Windows 7 i dostupna je za preuzimanje na sustava [Microsoft povezivanje web-mjesta](https://connect.microsoft.com/site1164). Upute kako pomoću značajke pakiranja dostupne su na [Konfiguriraj Registracija automatsko uređaja za domenu u sustavu Windows 7 pridruženo uređaja](active-directory-conditional-access-automatic-device-registration-windows7.md).

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>Uključivanje uređaju sa sustavom Android pomoću Azure Active Directory Registracija uređaja

[Azure autentikatora za Android tema](active-directory-conditional-access-azure-authenticator-app.md) sadrži upute o instalacija Azure autentikatora aplikacije na uređaju sa sustavom i dodavanje računa za rad. Kada poslovnog računa uspješno je stvorena na uređaju sa sustavom Android, uređaj je radnom mjestu pridruženo tvrtke ili ustanove.

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>Provjerite je li registrirani pravilno napisan natrag sa servisom Active Directory
Možete pogledati i provjerite koji uređaj objekte napisan natrag sa servisom Active Directory pomoću LDP.exe ili ADSI Edit. Oba dostupni su pomoću alata za Active Directory administratora.

Prema zadanim postavkama, uređaj objekte koji su sastavljene natrag iz servisa Azure Active Directory nalaziti u istoj domeni kao farmi AD FS.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Stvaranje pravila za aplikaciju programa access i prilagođeni pristup zabranjen poruke
Zamislite sljedeće: Stvaranje aplikacije komponente Relying strana pouzdanost u AD FS i konfiguriranje pravilo autorizacije izdavanja koja omogućuje samo registrirani uređaje. Sada samo uređaje registrirane dopušteno pristup aplikaciji. Da biste se lakše za korisnike da biste pristupili aplikaciju, konfigurirati prilagođeni pristup zabranjen poruku koja sadrži upute o tome kako uključiti svoj uređaj. Sada korisnici imati objedinjenog onako kako registrirati svoje uređaje da biste pristupili aplikacije.

Sljedeće će vam pokazati kako implementirati scenarij.

>[AZURE.NOTE]
U ovom se odjeljku pretpostavlja da ste već konfigurirali na potrebe za oslanjanjem strana pouzdanosti aplikacije u AD FS.

1. Otvorite alat za AD FS BLOG, a zatim otvorite AD FS > odnosima pouzdanosti > potrebe za oslanjanjem strana smatra pouzdanima.
2. Pronađite aplikaciju za koje će se primijeniti ovo pravilo programa access. Desnom tipkom miša kliknite program, a zatim odaberite Uređivanje pravila zahtjeva...
3. Odaberite karticu **Izdavanja pravila za provjeru autentičnosti** , a zatim odaberite **Dodaj pravilo...**
4. **Pravila zahtjeva** predložak padajućeg popisa, odaberite **Dopusti ili Nemoj dopustiti korisnicima na temelju dolaznih zahtjeva**. Odaberite **Dalje**.
5. Naziv pravila zahtjeva: polja vrste: **dopustiti pristup s registrirani uređaja**
6. Iz na dolazni Vrsta zahtjeva: padajućeg popisa, odaberite **Je registrirana korisnika**.
7. U na dolazni zahtjeva vrijednost: polja, upišite: **true**
8. Odaberite izborni gumb **dopustiti pristup korisnicima s ovom dolaznih zahtjeva** .
9. Odaberite **Završi** , a zatim **Zatvori**.
10. Uklanjanje svih pravila koja se više čemu od pravila koji ste upravo stvorili. Na primjer, uklonite zadano pravilo **Dopustiti pristup svim korisnicima** .

Vaša aplikacija sada konfigurirana dopustiti pristup samo kada korisnik dolazi s uređaja koji je registriran i im se pridružite na radnom mjestu. Za naprednije pristup pravila, potražite u članku [Upravljanje rizika s kontrola pristupa višestruke provjere](https://technet.microsoft.com/library/dn280949.aspx).

Nakon toga će konfigurirati u prilagođenu poruku o pogrešci za svoju aplikaciju. Poruka o pogrešci poslat će korisnici znali da ih morate pridružiti svoj uređaj na radnom mjestu prije nego što ih je li dopušten pristup aplikaciji. Možete stvoriti prilagođenu aplikaciju pristup zabranjen poruke pomoću prilagođenih HTML i komponente Windows PowerShell.

Na vanjskom poslužitelju, otvorite prozor naredbu komponente Windows PowerShell i upišite sljedeću naredbu. Stavke koje su specifične za sustav zamijenite dijelove naredbu:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Vaš uređaj morate registrirati mogli pristupiti te aplikacije.

**Ako koristite uređaju sa sustavom iOS, odaberite ovu vezu da biste se pridružili uređaju**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Uključivanje iOS uređaj na vašem radnom mjestu.


**Ako koristite Windows 8.1 uređaj**, možete se uključiti uređaj tako da otvorite **Postavke PC -JU ** >  **mrežni** > **radnom mjestu**.


Pri čemu "**potrebe za oslanjanjem strana pouzdanost naziv**" je naziv objekta potrebe za oslanjanjem pouzdanost proizvođača aplikacije u AD FS.
Pri čemu je **yourdomain.com** naziv domene koji ste konfigurirali s Azure Active Directory. Ako je, primjerice, contoso.com.
Ne zaboravite da biste uklonili sve prijelome redaka (ako ih ima) iz html sadržaj koji prenesite cmdlet **Skup AdfsRelyingPartyWebContent** .


Sada kada korisnici pristupaju aplikacije s uređaja koji je registriran za Azure Active Directory uređaj Registracija servis, primit će na stranicu koja izgleda slično zaslona ispod.

![Screeshot pogreške kada korisnici još nije registriran na svoj uređaj Azure AD](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
