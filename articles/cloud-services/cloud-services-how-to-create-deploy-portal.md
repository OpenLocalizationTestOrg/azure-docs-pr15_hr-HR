<properties
    pageTitle="Kako stvoriti i implementirati servis u oblaku | Microsoft Azure"
    description="Saznajte kako stvoriti i implementirati pomoću portala za Azure servis u oblaku."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Kako stvoriti i implementirati servis u oblaku

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-how-to-create-deploy-portal.md)
- [Azure klasični portal](cloud-services-how-to-create-deploy.md)

Portal za Azure nudi dva načina za stvaranje i implementacija servis u oblaku: *Brzo stvaranje* te *Stvaranje prilagođenih*.

U ovom se članku objašnjava kako koristiti metodu brzo stvaranje za stvaranje novog servis u oblaku, a zatim da biste prenijeli i implementacija u oblak paketa u Azure pomoću **Prijenos** . Kada koristite ovaj postupak, portala za Azure postaje dostupan praktičan veze za dovršavanje sve zahtjeve tijekom rada. Ako ste spremni za implementaciju servis u oblaku kad ga stvorite, to možete učiniti i u isto vrijeme pomoću prilagođenih stvaranje.

> [AZURE.NOTE] Ako namjeravate objaviti na servis u oblaku s Visual Studio tima Services (VSTS), koristite brzo stvaranje i postavite VSTS objavljivanje iz Azure brzi početak rada ili na nadzornoj ploči. Dodatne informacije potražite u članku [Neprekinuti isporuka Azure pomoću Visual Studio tim servisima][TFSTutorialForCloudService], ili potražite u članku pomoć za stranicu za **Brzo pokretanje** .

## <a name="concepts"></a>Koncepti
Tri komponente su potrebne za implementaciju aplikacije kao neki servis u oblaku u Azure:

- **Definicija servisa**  
  Datoteku definicije oblaka servisa (.csdef) definira modela servisa, uključujući broj uloge.

- **Konfiguracija servisa**  
  Oblak servisa konfiguracijska datoteka (.cscfg) nudi konfiguracijske postavke za oblak uloge servisa i pojedinačne, uključujući broj instanci uloge.

- **Paketa**  
  Paket servisa (.cspkg) sadrži kod aplikacije i konfiguracija i datoteku definicije servisa.

Dodatne informacije o ovim i kako stvoriti paket [ovdje](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Priprema aplikacije
Prije nego što možete implementirati servis u oblaku, morate stvoriti u oblak paketa (.cspkg) iz kodu aplikacije i u oblaku servisa konfiguracijska datoteka (.cscfg). Azure SDK nudi alate za pripremu te datoteke potrebne implementacije. Na stranici [Azure preuzimanja](https://azure.microsoft.com/downloads/) na jeziku koji se u kojoj želite razviti kodu aplikacije možete instalirati SDK-a.

Tri značajke servisa oblaka potrebno posebno konfiguracije prije izvoza paket servisa:

- Ako želite uvesti servis u oblaku koji koristi Secure Sockets Layer (SSL) za šifriranje podataka [konfigurirali aplikaciju](cloud-services-configure-ssl-certificate-portal.md#modify) za SSL.

- Ako želite konfigurirati veze udaljene radne površine na instance uloga [Konfiguriranje uloge](cloud-services-role-enable-remote-desktop.md) za udaljene radne površine. To možete napraviti na portalu klasični.

- Ako želite da biste konfigurirali opširno nadzor za servis u oblaku, omogućite Azure Dijagnostika servisa u oblaku. *Minimalna nadzora* (Zadana postavka nadzor razine) koristi mjerača performansi prikupljene na operacijskim sustavima glavno računalo za uloga instance (virtualnim strojevima). *Nadzor tekstni* prikuplja dodatne metriku na temelju podataka o performansama unutar instance uloga da biste omogućili bolje analize problema koji se pojavljuju tijekom obrade aplikacije. Da biste saznali kako omogućiti Azure Dijagnostika, potražite u članku [Omogućavanje dijagnostike u Azure](cloud-services-dotnet-diagnostics.md).

Da biste stvorili neki servis u oblaku implementacijama sustava web uloge ili uloge suradnika, morate [stvoriti paket servisa](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Prije početka

- Ako još niste instalirali Azure SDK, kliknite **Instaliranje Azure SDK** za otvaranje [stranice s preuzimanjima Azure](https://azure.microsoft.com/downloads/), a preuzimanje SDK za jezik u kojem želite razviti kod. (Imat ćete priliku to učiniti naknadno.)

- Ako se sve instance uloga Zatraži potvrdu, stvorite certifikate. Servisi u oblaku zahtijevaju .pfx datoteka s privatnim ključem. [Ne možete prenositi certifikata za Azure]() tijekom stvaranje web-mjesta i Implementacija servisa u oblaku.

- Ako planirate implementaciju servisa u oblaku u grupu afinitet, stvorite grupe afinitet. Grupe sustava afinitet možete koristiti za implementaciju servis u oblaku i drugim servisima za Azure na isto mjesto unutar područja. Možete stvoriti grupu afinitet u području **mreža** Azure klasični portala, na stranici **afinitet grupe** .


## <a name="create-and-deploy"></a>Stvorite i implementirajte

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **Novo > virtualnim strojevima**, zatim pomaknite se prema dolje i kliknite **U Oblaku**.

    ![Objavljivanje na servis u oblaku](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)

3. Pri dnu stranice podaci koji se prikazuje, kliknite **Stvori**. 
4. U novi plohu **Servis u Oblaku** , unesite vrijednost za **naziv DNS-a**.
5. Stvorite novu **Grupu resursa** ili odaberite postojeći.
6. Odaberite **mjesto**.
7. Kliknite **paket**. Otvorit će se plohu **Prijenos paketa** . Ispunite obavezna polja.  

     Ako bilo koji od vaša uloga sadrže jednokratan, **uvođenje čak i ako jednu ili više uloga sadrže jednokratan** provjerite je li odabrana.

8. Provjerite je li **implementacije Start** .
9. Kliknite **u redu** koji će se zatvoriti plohu **Prijenos paketa** .
10. Ako nemate sve potvrde da biste dodali, kliknite **Stvori**.

    ![Objavljivanje na servis u oblaku](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Prijenos certifikat

Ako paketa za implementaciju je [konfiguriran za korištenje certifikata](cloud-services-configure-ssl-certificate-portal.md#modify), sada možete prenijeti certifikata.

1. Odaberite **potvrde**i na plohu **Dodaj potvrde** odaberite SSL certifikat .pfx datoteka, a zatim unesite **lozinku** za potvrdu
2. Kliknite **Priloži certifikat**, a zatim **u redu** na plohu **Dodaj certifikata** .
3. Kliknite **Stvori** plohu **Servis u Oblaku** . Kada implementacijskih je dostigne status **spreman** , možete nastaviti na sljedeće korake.

    ![Objavljivanje na servis u oblaku](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)


## <a name="verify-your-deployment-completed-successfully"></a>Provjera uvođenja uspješno dovršena

1. Kliknite oblačić instanca servisa.

    Status treba prikazati servis nije **pokrenut**.

2. U odjeljku **Osnove**kliknite **URL web-mjesta** da biste otvorili servis u oblaku u web-pregledniku.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)


[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Daljnji koraci

* [Općenito konfiguracije servis u oblaku](cloud-services-how-to-configure-portal.md).
* Konfiguriranje [Prilagođeni naziv domene](cloud-services-custom-domain-name-portal.md).
* [Upravljanje servis u oblaku](cloud-services-how-to-manage-portal.md).
* Konfiguriranje [ssl certifikata](cloud-services-configure-ssl-certificate-portal.md).