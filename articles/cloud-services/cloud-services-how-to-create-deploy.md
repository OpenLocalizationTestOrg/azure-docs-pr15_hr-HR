<properties
    pageTitle="Kako stvoriti i implementirati servis u oblaku | Microsoft Azure"
    description="Saznajte kako stvoriti i implementirati pomoću metode brzo stvaranje servisu Azure servis u oblaku."
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
    ms.date="09/06/2016"
    ms.author="adegeo"/>




# <a name="how-to-create-and-deploy-a-cloud-service"></a>Kako stvoriti i implementirati servis u Oblaku

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-how-to-create-deploy-portal.md)
- [Azure klasični portal](cloud-services-how-to-create-deploy.md)

Azure klasični portal nudi dva načina za stvaranje i implementacija servis u oblaku: **Brzo stvaranje** te **Stvaranje prilagođenih**.

U ovoj se temi objašnjava kako koristiti metodu brzo stvaranje za stvaranje novog servis u oblaku, a zatim da biste prenijeli i implementacija u oblak paketa u Azure pomoću **Prijenos** . Kada koristite ovaj postupak, portala za Azure klasični postaje dostupan praktičan veze za dovršavanje sve zahtjeve tijekom rada. Ako ste spremni za implementaciju servis u oblaku kad ga stvorite, to možete učiniti i u isto vrijeme pomoću **Prilagođenih stvaranje**.

> [AZURE.NOTE] Ako namjeravate objaviti na servis u oblaku s Visual Studio tima Services (VSTS), pomoću brzo stvaranje, a zatim postavljanje VSTS objavljivanje iz **Brzi Start** ili na nadzornoj ploči. Dodatne informacije potražite u članku [Neprekinuti isporuka Azure pomoću Visual Studio tim servisima][TFSTutorialForCloudService], ili potražite u članku pomoć za stranicu za **Brzo pokretanje** .

## <a name="concepts"></a>Koncepti
Tri komponente su potrebna lozinka implementacija aplikacije kao neki servis u oblaku u Azure:

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

- Ako želite uvesti servis u oblaku koji koristi Secure Sockets Layer (SSL) za šifriranje podataka [konfigurirali aplikaciju](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) za SSL.

- Ako želite konfigurirati veze udaljene radne površine na instance uloga [Konfiguriranje uloge](cloud-services-role-enable-remote-desktop.md) za udaljene radne površine.

- Ako želite da biste konfigurirali opširno nadzor za servis u oblaku, omogućite Azure Dijagnostika servisa u oblaku. *Minimalna nadzora* (Zadana postavka nadzor razine) koristi mjerača performansi prikupljene na operacijskim sustavima glavno računalo za uloga instance (virtualnim strojevima). "Opširno nadzor * prikuplja dodatne metriku na temelju podataka o performansama unutar instance uloga da biste omogućili bolje analize problema koji se pojavljuju tijekom obrade aplikacije. Da biste saznali kako omogućiti Azure Dijagnostika, potražite u članku [Omogućavanje dijagnostike u Azure](cloud-services-dotnet-diagnostics.md).

Da biste stvorili neki servis u oblaku implementacijama sustava web uloge ili uloge suradnika, morate [stvoriti paket servisa](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Prije početka

- Ako još niste instalirali Azure SDK, kliknite **Instaliranje Azure SDK** za otvaranje [stranice s preuzimanjima Azure](https://azure.microsoft.com/downloads/), a preuzimanje SDK za jezik u kojem želite razviti kod. (Imat ćete priliku to učiniti naknadno.)

- Ako se sve instance uloga Zatraži potvrdu, stvorite certifikate. Servisi u oblaku zahtijevaju .pfx datoteka s privatnim ključem. Možete [prenijeti certifikata za Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) dok stvarate i Implementacija servisa u oblaku.

- Ako planirate implementaciju servisa u oblaku u grupu afinitet, stvorite grupe afinitet. Grupe sustava afinitet možete koristiti za implementaciju servis u oblaku i drugim servisima za Azure na isto mjesto unutar područja. Možete stvoriti grupu afinitet u području **mreža** Azure klasični portala, na stranici **afinitet grupe** .


## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Kako: Stvaranje pomoću brzo stvaranje servis u oblaku

1. [Azure klasični portal](http://manage.windowsazure.com/)kliknite **Novo**>**izračunati**>**U Oblaku**>**Brzo stvaranje**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)

2. U **URL-a**unesite naziv poddomene za korištenje u javnog URL-a za pristup oblaku u implementacijama proizvodnje. Oblik URL-a za implementacije radni je: http://*myURL*. cloudapp.net.

3. U **regiji ili afinitet grupe**, odaberite regiji ili afinitet grupu koju želite uvesti servisa u oblaku. Ako želite uvesti servis u oblaku na isto mjesto kao drugih servisa za Azure unutar područja, odaberite grupu afinitet.

4. Kliknite **Stvori servis u Oblaku**.

    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)

    Možete nadzirati statusu postupka u područja za poruku pri dnu prozora.

    Otvorit će se u područje za **Servise u Oblaku** i sa servisom cloud novog prikaza. Kada se status promijeni u stvoreno, stvaranje servisa oblaka uspješno dovršena.

    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)


## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Kako: prijenos certifikat za servise u oblaku

1. [Azure klasični portal](http://manage.windowsazure.com/)kliknite **Servise u Oblaku**, kliknite naziv servisa u oblaku, a zatim **Certifikati**.

    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)


2. Kliknite **Prijenos certifikat** ili **Prijenos**.

3. U **datoteci**, koristite **Pregledaj** da biste odabrali certifikat (.pfx datoteka).

4. U okvir **Lozinka**unesite privatni ključ za potvrdu.

5. Kliknite **u redu** (kvačica).

    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)

    Možete gledati tijeku prenosi u područja za poruku u nastavku. Kada prijenos završi, certifikat se dodaje tablici. U područje za poruku, kliknite u redu da biste zatvorili poruku.

    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Kako: implementacija servis u oblaku

1. [Azure klasični portal](http://manage.windowsazure.com/)kliknite **Servise u Oblaku**, kliknite naziv servisa u oblaku, a zatim **nadzorne ploče**.

2. Kliknite **Prenesi novi radni implementacije** ili **Prijenos**.

3. **Natpis za implementaciju**, unesite naziv nove implementacije – na primjer, MyCloudServicev4.

3. U **paket**, koristite **Pregledaj** da biste odabrali datoteku paketa servisa (.cspkg) da biste koristili.

4. U **konfiguraciji**, koristite **Pregledaj** da biste odabrali datoteku Konfiguriranje servisa (.cscfg) da biste koristili.

5. Ako servis u oblaku neće sadržavati sve uloge s samo jednu instancu, uključite potvrdni okvir **Implementacija čak i ako jednu ili više uloga sadrže jednokratan** da biste omogućili nastavak implementacije.

    Azure možete samo jamči postotak 99.95 pristup servisu oblaka tijekom održavanja i servisa ažuriranja Ako svaku uloga ima barem dvije instance. Ako je potrebno, možete dodati dodatne uloga instanci na stranici **Skaliranje** nakon implementacije u oblaku. Dodatne informacije potražite u članku [Ugovore o razini usluge](https://azure.microsoft.com/support/legal/sla/).

6. Kliknite **u redu** (kvačica) da biste započeli uvođenje servisa oblaka.

    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)

    Možete nadzirati status implementacije u područja za poruku. Kliknite u redu da biste sakrili poruku.

    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Provjera uvođenja uspješno dovršena

1. Kliknite **nadzorna ploča**.

    Status treba prikazati servis nije **pokrenut**.

2. U odjeljku **brzi pogled**kliknite URL web-mjesta da biste otvorili servis u oblaku u web-pregledniku.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


[TFSTutorialForCloudService]: cloud-services-continuous-delivery-use-vso.md
 
## <a name="next-steps"></a>Daljnji koraci

* [Općenito konfiguracije servis u oblaku](cloud-services-how-to-configure.md).
* Konfiguriranje [Prilagođeni naziv domene](cloud-services-custom-domain-name.md).
* [Upravljanje servis u oblaku](cloud-services-how-to-manage.md).
* Konfiguriranje [ssl certifikata](cloud-services-configure-ssl-certificate.md).