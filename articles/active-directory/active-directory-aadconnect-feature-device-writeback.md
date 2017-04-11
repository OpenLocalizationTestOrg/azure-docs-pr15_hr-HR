<properties
    pageTitle="Azure AD Connect: Omogućivanje upisima uređaja | Microsoft Azure"
    description="Kako omogućiti upisima uređaj pomoću Azure AD Connect detalje o ovom dokumentu"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Omogućivanje uređaj upisima

>[AZURE.NOTE] Za uređaj upisima potreban je pretplatu na Azure AD Premium.

Sljedeća dokumentacija pruža informacije o omogućivanju značajci upisima uređaj Azure AD Connect. Uređaj Upisima koristi se u sljedećim scenarijima:

- Omogućivanje uvjetno pristupa na temelju uređajima ADFS (2012 R2 ili noviji) zaštićeni aplikacije (potrebe za oslanjanjem strana trusts).

To pruža dodatne sigurnosti i jamstvo koji pristup aplikacijama moguć je samo na pouzdanih uređaja. Dodatne informacije o uvjetnog access potražite u članku [Upravljanje rizika pomoću uvjetnog programa Access](active-directory-conditional-access.md) i [Postavljanje pristupa uvjetnog lokalnog pomoću Azure Active Directory Registracija uređaja](https://msdn.microsoft.com/library/azure/dn788908.aspx).

>[AZURE.IMPORTANT]
<li>Uređaji moraju nalaziti u isti skup stabala kao korisnika. Budući da uređaja mora biti zapisane natrag jedan skup stabala, ta značajka trenutno ne podržava implementacija s više šuma korisnika.</li>
<li>Samo jedan uređaj Registracija konfiguracije objekt mogu dodati skupa stabala za lokalni Active Directory. Ta značajka nije kompatibilan s topologije gdje se lokalnog servisa Active Directory sinkronizirano s više Azure AD imenika.</li>

## <a name="part-1-install-azure-ad-connect"></a>Dio 1: Instalacija Azure AD povezivanje
1. Instalirajte Azure AD Connect pomoću prilagođenih ili Ekspresne postavke. Microsoft preporučuje započeti s svim korisnicima i grupama uspješno ne sinkroniziraju prije omogućivanja upisima uređaja.

## <a name="part-2-prepare-active-directory"></a>2.dio: Priprema servisa Active Directory
Poduzmite sljedeće korake da biste se pripremili za korištenje upisima uređaja.

1.  Na računalu na kojem je instaliran Azure AD Connect, pokrenite PowerShell u načinu rada za dodatnim.

2.  Ako modula Active Directory PowerShell nije instaliran, instalirajte ga pomoću naredbe za sljedeće:

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. Ako modul Azure Active Directory PowerShell nije instaliran, zatim ga preuzmite i instalirajte ga iz [Azure Active Directory Module za Windows PowerShell (64-bitnu verziju)](http://go.microsoft.com/fwlink/p/?linkid=236297). Komponenta je u opsegu u Pomoćnik za prijavu, koji se instalira uz Azure AD Connect.

4.  S enterprise administratorske vjerodajnice, pokrenite sljedeće naredbe, a zatim zatvorite PowerShell.

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Budući da su vam potrebne promjene konfiguracije naziva, potrebni su Enterprise administratorske vjerodajnice. Administrator sustava domena neće imati dovoljno dozvole.

![PowerShell za omogućivanje uređaj upisima](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Opis:

- Ako već postoji, stvara i konfigurira novog spremnika i objekata u odjeljku CN = Konfiguracija Registracija uređaja, CN = Servisi, CN = Configuration, [skupa stabala dn].
- Ako već postoji, stvara i konfigurira novog spremnika i objekata u odjeljku CN = RegisteredDevices, [domene dn]. U ovom spremniku stvorit će se objekata uređaja.
- Postavlja potrebne dozvole na račun za Azure AD poveznik za upravljanje uređajima na servisa Active Directory.
- Samo mora se izvoditi na jedan skup stabala, čak i ako se instalira Azure AD Connect na više šuma.

Parametri:

- Naziv domene: Domena aktivnog imenika kojem će se stvoriti objekata uređaja. Napomena: Svi uređaji za zadani skup stabala servisa Active Directory stvorit će se u jednu domenu.
- AdConnectorAccount: Servisa Active Directory račun koji će se koristiti tako da Azure AD Connect za upravljanje objektima u direktoriju. Ovo je račun koji se koristi sinkronizaciju Azure AD Connect povezati AD. Ako ste instalirali pomoću eksplicitnih postavki, je račun na mjestu s MSOL_.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Dio 3: Omogući uređaj upisima u Azure AD Connect
Da biste omogućili upisima uređaja u Azure AD Connect pomoću sljedećeg postupka.

1.  Ponovno pokrenite čarobnjak za instalaciju. Odaberite **Prilagodba mogućnosti za sinkronizaciju** s dodatnim zadacima stranice, a zatim kliknite **Dalje**.
![Prilagođene instalacije prilagodite mogućnosti sinkronizacije](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  Na stranici značajke upisima uređaj neće više biti zasivljene. Provjerite Imajte na umu da ako Azure AD Connect avansne korake su ne upisima dovršene uređaj će biti zasivljen smanjivati na stranici dodatne značajke. Potvrdite okvir upisima uređaja, a zatim kliknite **Dalje**. Ako potvrdni okvir i dalje je onemogućen, pogledajte [odjeljak za otklanjanje poteškoća](#the-writeback-checkbox-is-still-disabled).
![Prilagođena instalacija uređaja Upisima dodatne značajke](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  Na stranici upisima vidjet ćete unesene domene kao upisima skupa stabala zadani uređaj.
![Prilagođene instalacije uređaja upisima ciljni skup stabala](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  Dovršite instalaciju čarobnjaka bez ikakvih promjena dodatna konfiguracija. Ako je potrebno, pogledajte [prilagođenu instalaciju programa Azure AD Connect.](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Omogućivanje pristupa uvjetno
Detaljne upute za omogućivanje scenarij dostupnih unutar [Postavljanje pristupa uvjetno lokalnog pomoću Azure Active Directory Registracija uređaja](https://msdn.microsoft.com/library/azure/dn788908.aspx).

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Provjerite je li uređaja se sinkroniziraju sa servisom Active Directory
Trebali biste uređaj upisima sada funkcionira ispravno. Imajte na umu da može potrajati do 3 sata za uređaj objekte biti sastavljene natrag AD.  Da biste potvrdili da uređaje koji se sinkroniziraju ispravno, učinite sljedeće nakon dovršite pravila sinkronizacije:

1.  Pokretanje servisa Active Directory administracijski centar.
2.  Proširite RegisteredDevices unutar domene čije je u tijeku.
![Centar za administratore Active Directory registrirana uređaja](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  Postoji navest će se trenutni registrirani uređaja.
![Centar za administratore Active Directory registrirana popisa uređaja](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Otklanjanje poteškoća

### <a name="the-writeback-checkbox-is-still-disabled"></a>Potvrdni okvir upisima i dalje je onemogućen
Ako je potvrdni okvir upisima uređaja nije omogućena čak i ako ste pratili navedene korake, na sljedeći način će vas voditi kroz što je provjera Čarobnjak za instalaciju prije no što je omogućena okvir.

Prvi stvari prvi:

- Provjerite je li barem jedan skup stabala sadrži 2012R2 Windows Server. Vrsta objekta uređaj mora postojati.
- Ako se čarobnjak za instalaciju je već pokrenut, zatim promjene neće biti otkriveni. U ovom slučaju, dovršite Čarobnjak za instalaciju i ponovno ga pokrenite.
- Provjerite je li račun unesete u skriptu za inicijalizaciju zapravo ispravno korisničko koristi Active Directory Connector. Da biste to provjerili, slijedite ove korake:
    - S izbornika start otvorite **Servis za sinkronizaciju**.
    - Otvorite karticu **poveznike** .
    - Pronađite poveznik s vrstom Active Directory Domain Services, a zatim ga odaberite.
    - U odjeljku **Akcije**odaberite **Svojstva**.
    - Idite na **povezati skupa stabala Active Directory**. Provjerite je li domenu i korisničko ime na tu podudarnost zaslona navedena račun za skripte.
![Poveznik za račun u upravitelja za sinkronizaciju servisa](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Provjera konfiguracije u servisu Active Directory:
- Provjerite je li servis za registraciju uređaja nalazi na lokaciji ispod (CN = DeviceRegistrationService, CN = Servisi za registraciju uređaja, CN Konfiguracija Registracija uređaja, CN = Services, CN = = konfiguracije) u odjeljku kontekst imenovanja konfiguracije.

![Rješavanje problema DeviceRegistrationService u konfiguraciji naziva](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- Provjerite je li još samo jedan objekt konfiguracije pretraživanjem prostora za naziv konfiguracije. Ako postoji više od jedne, izbrišite duplikat.

![Otklanjanje poteškoća, potražite duplicirane objekata](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- Na objekt servis za registraciju uređaja, provjerite je li atribut msDS-DeviceLocation postoji i ima vrijednost. Traženje ovom mjestu i provjerite je li se prezentacija s vrstu objekta msDS-DeviceContainer.

![Rješavanje problema msDS DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Rješavanje problema klase RegisteredDevices objekta](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- Provjerite je li račun koji se koristi Active Directory poveznika ima potrebne dozvole na registriran uređaji spremnik pronašao prethodnom koraku. To je očekivani dozvole na ovom spremniku:

![Otklanjanje poteškoća s, provjerite je li dozvole za spremnik](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Račun servisa Active Directory provjerite ima li dozvole za u CN Konfiguracija Registracija uređaja, CN = = Servisi, CN = Konfiguracija objekta.

![Otklanjanje poteškoća s, provjerite je li dozvole za konfiguriranje Registracija uređaja](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Dodatne informacije
- [Upravljanje rizika pomoću uvjetnog programa Access](active-directory-conditional-access.md)
- [Postavljanje pristupa uvjetno lokalnog pomoću Azure Active Directory Registracija uređaja](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
