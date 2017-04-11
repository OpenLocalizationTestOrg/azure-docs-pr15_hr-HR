<properties
    pageTitle="Otklanjanje poteškoća s Proxy aplikacije | Microsoft Azure"
    description="Obuhvaća upute za otklanjanje pogrešaka u Proxy aplikacije za Azure AD."
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
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>Otklanjanje poteškoća s Proxy aplikacije

Ako dođe do pogreške u pristupa objavljen ili u aplikacijama za objavljivanje, provjerite sljedeće mogućnosti da biste vidjeli radi li ispravno proxy poslužitelj sustava Microsoft Azure AD aplikacije:

- Otvorite konzolu za servise sustava Windows i provjerite je li omogućen servis **Microsoft AAD aplikacije Proxy Connector** i pokrenut. Preporučujemo vam i možete pregledati na stranici Svojstva servisa Proxy aplikacije kao što je prikazano na sljedećoj slici:  
  ![Snimka zaslona prozora svojstava poveznik za Microsoft AAD aplikacije proxy poslužitelja](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- Otvorite preglednik događaja i potražite proxy poslužitelj aplikacije poveznika događaja u **Zapisnici programa i servisa** > **Microsoft** > **AadApplicationProxy** > **poveznik** > **administrator**.
- Ako je potrebno, detaljnije zapisnike dostupni su uključivanjem analize i ispravljanje pogrešaka zapisnika te uključivanjem zapisnik sesije poveznik za Proxy aplikacije.

## <a name="the-page-is-not-rendered-correctly"></a>Stranice se pravilno prikazati

Ako ne čujete na određenu poruku o pogrešci, mogu i dalje imate problema s aplikacijom vizualizacije ili nepravilno funkcioniranje. To se može dogoditi ako ste objavili članak put, ali aplikacije potreban je sadržaj koji postoji izvan taj put.

Ako, na primjer, ako objavljujete https://yourapp/app put, ali aplikacije poziva slike u https://yourapp/media, oni ne može prikazati. Provjerite je li objavljujete aplikacije pomoću najvišu razinu put morate unijeti sve relevantne sadržaje. U ovom primjeru bi http://yourapp/.

Ako promijenite put uključite referentni sadržaj, ali i dalje potrebna korisnicima krenuti dublju vezu na putu, potražite u članku na blogu [Postavljanje desnom vezu za aplikacije Proxy aplikacije u ploču za pristup Azure AD i pokretač aplikacija za Office 365](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="general-errors"></a>Općenite pogreške

Pogreška | Opis | Razlučivost
--- | --- | ---
Nije moguće pristupiti tvrtke aplikacije. Nemate ovlasti za pristup te aplikacije. Autorizacija nije uspjelo. Provjerite je li korisnici s pristupom dodijeliti ovu aplikaciju. | Možda nije Dodijeljeno korisniku za tu aplikaciju. | Idite na karticu **aplikacije** pa u odjeljku **korisnici i grupe**, dodijeliti ovog korisnika ili grupu korisnika te aplikacije.
Nije moguće pristupiti tvrtke aplikacija. Nemate ovlasti za pristup te aplikacije. Autorizacija nije uspjelo. Provjerite ima li korisnik licence za Azure Active Directory Premium ili Basic. | Korisnici mogu se sljedeća pogreška pri pokušaju pristupiti aplikaciju ste objavili ako nisu izričito dodjeljuje im Premium/Basic licencom administrator pretplatnika. | Idite na karticu pretplatnika Active Directory **licenci** i provjerite je li ovo korisnik ili grupa korisnika je li dodijeljena Premium ili osnovni licencu.


## <a name="connector-troubleshooting"></a>Poveznik za otklanjanje poteškoća
Ako registracija ne uspije tijekom instalacije čarobnjak poveznika, dva su načina da biste vidjeli razlog neuspjeha. Potražite u zapisniku događaja u odjeljku **aplikacije i servise Logs\Microsoft\AadApplicationProxy\Connector\Admin**ili pokrenite sljedeću naredbu komponente Windows PowerShell.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Pogreška | Opis | Razlučivost |
| --- | --- | --- |
| Nije uspjela registracija poveznik: Provjerite je li omogućeno Proxy aplikacije na portalu za upravljanje Azure i ispravno unijeli servisa Active Directory korisničko ime i lozinku. Pogreška: "jednu ili više pogrešaka došlo je do." | Zatvorite prozor Registracija bez provedbe prijave za Azure AD. | Ponovno pokrenite čarobnjak za poveznik i registrirati poveznik. |
| Nije uspjela registracija poveznik: Provjerite je li omogućeno Proxy aplikacije na portalu za upravljanje Azure i ispravno unijeli Active Directory korisničko ime i lozinku. Pogreška: "AADSTS50001: resursa `https://proxy.cloudwebappproxy.net /registerapp` je onemogućen." | Proxy poslužitelj aplikacije je onemogućen.  | Provjerite je li omogućiti Proxy aplikacije na portalu za Azure klasični prije no što pokušate registrirati poveznik. Dodatne informacije o omogućivanju Proxy aplikacije, potražite u članku [Omogućivanje Proxy aplikacije servisa](active-directory-application-proxy-enable.md). |
| Nije uspjela registracija poveznik: Provjerite je li omogućeno Proxy aplikacije na portalu za upravljanje Azure i ispravno unijeli servisa Active Directory korisničko ime i lozinku. Pogreška: "jednu ili više pogrešaka došlo je do." | Ako prozor Registracija Otvori i odmah zatvara bez omogućujući vam da biste se prijavili, dobit ćete vjerojatno ta se pogreška. Ta se pogreška pojavljuje kada je mreža pogreške na računalu. | Provjerite je li je moguće povezati putem preglednika javnog web-mjesta i priključke otvoreno kao što je navedeno u [preduvjeti Proxy aplikacije](active-directory-application-proxy-enable.md). |
| Nije uspjela registracija poveznik: Provjerite je li računalo povezano s Internetom. Pogreška: "pojavio bez krajnjoj točki slušanje pri `https://connector.msappproxy.net :9090/register/RegisterConnector` koje nije moguće prihvatiti poruku. Uzrok je često sadrže netočnu adresu ili SOAP akcije. Ako postoji više pojedinosti u odjeljku InnerException,. " | Ako se prijaviti pomoću Azure AD korisničko ime i lozinku, ali prikaže Ova pogreška, možda ćete se blokiraju svi priključci iznad 8081. | Provjerite je li potrebno priključke otvoreno. Dodatne informacije potražite u članku [preduvjeti Proxy aplikacije](active-directory-application-proxy-enable.md). |
| Ukloni pogreške prikazuju se u prozoru Registracija. Ne može nastaviti – samo da biste zatvorili prozor. | Uneseni pogrešan korisničko ime i lozinku. | pokušaj ponovo. |
| Nije uspjela registracija poveznik: Provjerite je li omogućeno Proxy aplikacije na portalu za upravljanje Azure i ispravno unijeli servisa Active Directory korisničko ime i lozinku. Pogreška: "AADSTS50059: nema podataka o klijentu prepoznavanje pronaći u zahtjevu za ili implicitnih tako da sve navedene vjerodajnice, a pretraživanje po servis načelo URI nije uspjela. | Pokušavate se prijaviti pomoću Microsoftova Account i umjesto domene koja je dio ID tvrtke ili ustanove u imenik želite pristupiti. | Provjerite je li administrator je dio istog naziva domene kao klijentu domene, na primjer, ako je domena Azure AD contoso.com, administrator mora biti admin@contoso.com. |
| Dohvaćanje trenutnog izvođenja pravila za izvođenje skripti komponente PowerShell nije uspjelo. | Instalacija poveznika ne uspije, provjerite da biste bili sigurni da PowerShell izvođenja pravila je onemogućen. | Otvorite uređivaču pravila grupe. Idite na **Konfiguracije računala** > **Administrativni predlošci** > **Komponente Windows** > **Komponente Windows PowerShell** i dvokliknite **uključite skripte izvršavanja**. To možete postaviti **Nije konfiguriran** ili **omogućeno**. Ako postavite na **omogućeno**, provjerite je li u odjeljku mogućnosti pravila izvođenja postavljena ili **Dopusti lokalne skripte i Udaljena traje skripte** ili **sve**skripti. |
| Preuzimanje konfiguracije poveznika nije uspjelo. | Certifikat klijenta na poveznika koji se koristi za provjeru autentičnosti, istekao. To se može dogoditi i ako imate poveznik instaliran iza proxyja. U ovom slučaju poveznik ne možete pristupiti s Internetom i nećete moći omogućuje aplikacijama udaljenih korisnika. | Obnavljanje pouzdanost ručno pomoću na `Register-AppProxyConnector` cmdlet u ljusci Windows PowerShell. Ako je vaš poveznik iza proxy poslužitelj, nije potrebno da biste dodijelili pristup Internetu račune poveznik "mrežnim servisima" i "lokalni sustav". To je moguće napraviti tako da ih omogućivanju pristupa Proxy ili tako da postavite ih da biste zaobišli proxy poslužitelj. |
| Nije uspjela registracija poveznik: Provjerite jeste li vi globalni Administrator sustava Active Directory da biste registrirali poveznik. Pogreška: "zahtjev za registraciju odbijen." | Pseudonim se pokušavate prijaviti u nije administrator na ovoj domeni. Poveznik za vaše uvijek instaliran direktorij vlasništvu korisnikove domene. | Provjerite da administrator sustava se pokušavate prijaviti kao globalni dozvolama Azure AD klijent.|


## <a name="kerberos-errors"></a>Pogreške Kerberos

| Pogreška | Opis | Razlučivost |
| --- | --- | --- |
| Dohvaćanje trenutnog izvođenja pravila za izvođenje skripti komponente PowerShell nije uspjelo. | Instalacija poveznika ne uspije, provjerite da biste bili sigurni da PowerShell izvođenja pravila je onemogućen. | Otvorite uređivaču pravila grupe. Idite na **Konfiguracije računala** > **Administrativni predlošci** > **Komponente Windows** > **Komponente Windows PowerShell** i dvokliknite **uključite skripte izvršavanja**. To možete postaviti **Nije konfiguriran** ili **omogućeno**. Ako postavite na **omogućeno**, provjerite je li u odjeljku mogućnosti pravila izvođenja postavljena ili **Dopusti lokalne skripte i Udaljena traje skripte** ili **sve**skripti. |
| 12008 - azure AD prekoračilo maksimalni broj dopušteno pokušaja provjere autentičnosti Kerberos s poslužiteljem pozadinske. | Ovaj događaj mogu upućivati pogrešne konfiguracije između Azure AD i pozadinskog poslužitelja aplikacije ili problema u datum i vrijeme konfiguraciju na oba računala. | Pozadinskog poslužitelja odbili ulaznice Kerberos stvorio Azure AD. Provjerite je li taj Azure AD i pozadinskog poslužitelja aplikacije pravilno konfigurirani. Provjerite jesu li datum i vrijeme konfiguraciju na Azure AD i pozadinskog poslužitelja aplikacije sinkroniziraju. |
| 13016 - azure AD nije moguće dohvatiti Kerberos karata ime korisnika jer nema UPN token rub ili u kolačića programa access. | Došlo je do problema s konfiguracijom STS. | Ispravite konfiguraciju zahtjeva UPN u na STS. |
| 13019 - azure AD nije moguće dohvatiti Kerberos karata ime korisnika zbog sljedeće općenite pogreške API-JA. | Ovaj događaj mogu upućivati pogrešne konfiguracije između Azure AD i poslužitelja kontroler domene ili problema u datum i vrijeme konfiguraciju na oba računala. | Kontrolerom domene odbili ulaznice Kerberos stvorio Azure AD. Provjerite je li taj Azure AD i pozadinskog poslužitelja aplikacija konfigurirana pravilno, osobito SPN konfiguracije. Provjerite je li Azure AD domene istu domenu kao kontrolerom domene da biste bili sigurni da kontrolerom domene uspostavlja pouzdanosti Azure AD se pridružite. Provjerite jesu li datum i vrijeme konfiguraciju na Azure AD i sinkroniziraju kontrolerom domene. |
| 13020 - azure AD nije moguće dohvatiti Kerberos karata ime korisnika jer nije definiran pozadinskog poslužitelja SPN. | Ovaj događaj mogu upućivati pogrešne konfiguracije između Azure AD i poslužitelja kontroler domene ili problema u datum i vrijeme konfiguraciju na oba računala. | Kontrolerom domene odbili ulaznice Kerberos stvorio Azure AD. Provjerite je li taj Azure AD i pozadinskog poslužitelja aplikacija konfigurirana pravilno, osobito SPN konfiguracije. Provjerite je li Azure AD domene istu domenu kao kontrolerom domene da biste bili sigurni da kontrolerom domene uspostavlja pouzdanosti Azure AD se pridružite. Provjerite jesu li datum i vrijeme konfiguraciju na Azure AD i sinkroniziraju kontrolerom domene. |
| 13022 - azure AD ne provjere autentičnosti korisnika jer pozadinskog poslužitelja odgovori pokušaja provjere autentičnosti Kerberos s pogreškom HTTP 401. | Ovaj događaj mogu upućivati pogrešne konfiguracije između Azure AD i pozadinskog poslužitelja aplikacije ili problema u datum i vrijeme konfiguraciju na oba računala. | Pozadinskog poslužitelja odbili ulaznice Kerberos stvorio Azure AD. Provjerite je li taj Azure AD i pozadinskog poslužitelja aplikacije pravilno konfigurirani. Provjerite jesu li datum i vrijeme konfiguraciju na Azure AD i pozadinskog poslužitelja aplikacija sinkroniziraju. |
| Na web-mjestu ne može se prikazati na stranici. | Korisnički možda se sljedeća pogreška pri pokušaju pristupiti aplikaciju ste objavili ako aplikacija nije IWA aplikacije. Definirani SPN za ovu aplikaciju netočni. | Za aplikacije IWA: provjeriti je li on točan SPN konfiguriran za ovu aplikaciju. |
| Na web-mjestu ne može se prikazati na stranici. | Korisnički možda se sljedeća pogreška pri pokušaju pristupiti aplikaciju ste objavili ako aplikacija nije aplikaciju OWA. To može uzrokovati nešto od sljedećeg: <br> -Definirani SPN za ovu aplikaciju nije valjana. <br> -Korisnika koji pokušavate pristup aplikaciji koristi Microsoftov račun umjesto proper korporativni račun za prijavu ili korisnik je privremeni korisnik. <br> -Korisnika koji pokušavate pristup aplikaciji nije ispravno definiran za ovu aplikaciju na strani lokalno. | Koraci u skladu s tim smanjenju: <br> – Provjerite jesu li točni SPN konfiguriran za ovu aplikaciju. <br> -Provjerite je li korisnik se prijavi pomoću njihovih korporativni račun za koji odgovara domene objavljenom programu. Web-mjesto Microsoftova Account korisnika i u okvir za goste ne može pristupiti IWA aplikacije. <br> – Provjerite jesu li da taj korisnik ima odgovarajuće dozvole onako kako su definirana za tu aplikaciju pozadinskog na lokalno računalo. |
| Nije moguće pristupiti tvrtke aplikacije. Nemate ovlasti za pristup te aplikacije. Autorizacija nije uspjelo. Provjerite je li korisnici s pristupom dodijeliti ovu aplikaciju. | Korisnici mogu se sljedeća pogreška pri pokušaju pristupiti ste objavili ako koriste Microsoftovi računi umjesto njihove korporativni račun za prijavu u aplikaciju. Ova se pogreška može pojaviti i korisnicima gostima. | Web-mjesto Microsoftova Account korisnika i u okvir za goste ne može pristupiti IWA aplikacije. Provjerite je li korisnik se prijavi pomoću njihovih korporativni račun za koji odgovara domene objavljenom programu. |
| Tvrtke aplikacija ne može pristupiti odmah. Pokušajte ponovno kasnije... Poveznik isteklo. | Korisnici mogu se sljedeća pogreška pri pokušaju pristupiti aplikaciji ste objavili ako oni nisu ispravno definirane za ovu aplikaciju na strani lokalno. | Provjerite imaju li vaši korisnici odgovarajuće dozvole onako kako su definirana za tu aplikaciju pozadinskog na lokalno računalo. |


## <a name="see-also"></a>Vidi također

- [Omogućivanje Proxy aplikacije za Azure Active Directory](active-directory-application-proxy-enable.md)
- [Objavljivanje aplikacija pomoću Proxy aplikacije](active-directory-application-proxy-publish.md)
- [Omogućivanje jedinstvenu prijavu](active-directory-application-proxy-sso-using-kcd.md)
- [Omogućivanje pristupa uvjetno](active-directory-application-proxy-conditional-access.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
