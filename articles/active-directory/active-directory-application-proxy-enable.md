<properties
    pageTitle="Omogućivanje Proxy aplikacije Azure AD | Microsoft Azure"
    description="Uključivanje Proxy aplikacije na portalu za Azure klasični i instalacija poveznika za povratnog proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>Omogućivanje Proxy aplikacije na portalu za Azure

U ovom se članku vodit će vas kroz korake da biste omogućili Proxy aplikacije Microsoft Azure AD za oblak direktorija u Azure AD.

Ako ne znate što Proxy aplikacije omogućuju učinili, Saznajte [Kako daljinski pristup sigurne lokalnog aplikacije](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Preduvjeti za aplikaciju proxy poslužitelja
Prije nego što možete omogućiti i koristiti servise Proxy aplikacije, morate koristiti:

- [Microsoft Azure AD osnovni ili premium pretplata](active-directory-editions.md) i u imeniku Azure AD za koju ste globalni administrator.
- Poslužitelj koji se izvodi Windows Server 2012 R2 ili Windows 8.1 ili noviji, na kojoj možete instalirati Proxy poveznik aplikacija. Poslužitelj šalje zahtjeve Proxy aplikacije servisa u oblaku, a potrebne HTTP ili HTTPS veze na aplikacije koju objavljujete.

    - Za jedinstvenu prijavu na vaše objavljene aplikacije, ovo računalo trebala bi biti domene-spojena u istom domenu AD aplikacije koje objavljujete.

- Postoji li vatrozid na putu, provjerite da je otvoren tako da se poveznik možete unijeti zahtjeva za HTTP (TCP) Proxy aplikacije. Poveznik koristi sljedeće priključke zajedno s poddomene koje su dio msappproxy.net više razine domene i servicebus.windows.net. Provjerite je li otvorite sljedeće priključke za **Izlazni** promet:

  	| Broj priključka | Opis |
  	| --- | --- |
  	| 80 | Omogućivanje odlazni promet HTTP provjere valjanosti za sigurnost. |
  	| 443 | Omogući provjeru autentičnosti korisnika u odnosu Azure AD (samo za postupka registracije poveznik obavezno) |
  	| 10100 – 10120 | Omogućivanje LOB HTTP odgovore šalje proxy poslužitelj |
  	| 9352, 5671 | Omogućivanje komunikacije između poveznik prema servisa Azure zahtjevi za razgovore. |
  	| 9350 | Obavezan, da biste omogućili bolje performanse za zahtjevi za razgovore |
  	| 8080 | Omogućivanje poveznik Samopokretanje niza i automatsko ažuriranje poveznika |
  	| 9090 | Omogućivanje poveznik Registracija (samo za postupka registracije poveznik obavezno) |
  	| 9091 | Omogućiti automatsku obnovu certifikata pouzdanost poveznika |

    Ako vatrozida nameće promet prema potječu korisnika, otvorite sljedeće priključke za promet koji dolaze iz Windows servise kao mrežni servis. Osim toga, provjerite je li omogućiti priključak 8080 za NT Authority\System.

- Ako vaša tvrtka ili ustanova koristi proxy poslužitelji da biste se povezali s Internetom, provjerite pogledajte bloga [Rad s postojećeg lokalnog proxy poslužitelji](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) detalje o tome kako ih konfigurirali.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Korak 1: Omogućavanje Proxy aplikacije u Azure AD
1. Prijavite se kao administrator [Azure klasični portal](https://manage.windowsazure.com/).
2. Idite na servisu Active Directory i odaberite direktoriju u kojem želite omogućiti Proxy aplikacije.

    ![Active Directory - ikona](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Odaberite **Konfiguriraj** na stranici direktorija i pomaknite se do odjeljka **Proxy aplikacije**.
4. Uključivanje i isključivanje **omogućite aplikacije Proxy servisa za taj imenik** **omogućena**.

    ![Omogućivanje Proxy aplikacije](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Odaberite **Preuzmi odmah**. Tako ćete doći do **Azure AD aplikacije Proxy poveznik za preuzimanje**. Pročitajte i prihvatite licencne odredbe i kliknite **Preuzmi** da biste spremili datoteku Windows Installer (.exe) za poveznik.

## <a name="step-2-install-and-register-the-connector"></a>Korak 2: Instalacija i registracija poveznik
1. Pokrenite **AADApplicationProxyConnectorInstaller.exe** na poslužitelju pripremljeni prema preduvjete.
2. Slijedite upute u čarobnjaku za instalaciju.
3. Tijekom instalacije će zatraži da biste registrirali poveznik s proxy poslužitelj aplikacije za vaš klijent Azure AD.

  - Unesite vjerodajnice za Azure AD globalni administrator. Vaš klijent globalni administrator mogu se razlikovati od vjerodajnice za Microsoft Azure.
  - Provjerite je li administrator tko registrira poveznik u imeniku isti kojima je omogućeno Proxy aplikacije servisa. Na primjer, ako je domena klijentu contoso.com, administrator mora biti admin@contoso.com ili neki drugi pseudonim u toj domeni.
  - **Poboljšana konfiguracija sigurnosti preglednika Internet Explorer** postavljen **na** na poslužitelju koje instalirate poveznik, blokirane možda zaslonu registracije. Slijedite upute u poruci o pogrešci da biste dopustili pristup. Provjerite je li Internet Explorer Poboljšano Security isključeno.
  - Ako registracija connector ne uspije, potražite u članku [Otklanjanje poteškoća s Proxy aplikacije](active-directory-application-proxy-troubleshoot.md).  

4. Kada se instalacija završi, dva nove servise dodat će se na vašem poslužitelju:

    - **Microsoft AAD aplikacija Proxy Connector** omogućuje povezivanje
    - **Microsoft AAD aplikacija Proxy poveznik za ažuriranje programa** je servis automatsko ažuriranje, koji se povremeno provjerava nove verzije poveznik i ažurira poveznik prema potrebi.

    ![Aplikacija poveznika Proxy usluge – snimka](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Kliknite **Završi** u prozoru instalacije.

Visoke dostupnosti svrhe treba uvesti najmanje dva poveznike. Da biste implementirali više poveznika, ponovite korake 2 i 3, iznad. Poveznik za svaki naziv mora biti registriran zasebno.

Ako želite da biste deinstalirali poveznik, deinstalirajte poveznik servisa i servis za ažuriranje programa. Ponovno pokrenite računalo da biste potpuno uklonili servis.


## <a name="next-steps"></a>Daljnji koraci

Sada ste spremni aplikacijama za [Objavljivanje s Proxy aplikacije](active-directory-application-proxy-publish.md).

Ako imate aplikacije koje se nalaze na zasebnom mreže ili drugo mjesto, možete koristiti connector grupe da biste organizirali različite poveznika u logičke jedinice. Dodatne informacije o [radu s poveznika Proxy aplikacije](active-directory-application-proxy-connectors.md).
