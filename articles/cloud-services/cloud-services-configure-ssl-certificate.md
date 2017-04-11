<properties 
    pageTitle="Konfiguriranje SSL za servise u oblaku (klasični) | Microsoft Azure" 
    description="Saznajte kako odrediti krajnje HTTPS web uloge te kako prenijeti SSL certifikata radi zaštite vaše aplikacije." 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>Konfiguriranje SSL za aplikaciju u Azure

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-configure-ssl-certificate-portal.md)
- [Azure klasični portal](cloud-services-configure-ssl-certificate.md)

Secure Socket Layer (SSL) šifriranje je najčešće korištenih način zaštiti podataka koji se šalju putem Interneta. Uobičajeni zadatak u članku se opisuje kako odrediti krajnje HTTPS web uloge i kako prenijeti SSL certifikata radi zaštite vaše aplikacije.

> [AZURE.NOTE] Postupci u ovom ćete zadatku primjenjuju se na servise u Oblaku Azure; Aplikacije servisa, potražite u [ovom](../app-service-web/web-sites-configure-ssl-certificate.md) članku.

Ovaj zadatak koristi radnog implementacije. Informacije o korištenju pripremna implementacije navedeni su na kraju ove teme.

U [ovom](cloud-services-how-to-create-deploy.md) se članku ako niste još stvorio neki servis u oblaku.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]


## <a name="step-1-get-an-ssl-certificate"></a>Korak 1: Dobivanje SSL certifikata

Da biste konfigurirali SSL za aplikaciju, najprije morate dobiti SSL certifikat koji je potpisan od certifikat za izdavanje certifikata (CA), pouzdanih treće strane koja se šalje potvrde u tu svrhu. Ako nije već postoji, morate nabaviti od tvrtke koja prodaje SSL certifikata.

Certifikat mora zadovoljiti sljedeće preduvjete za SSL certifikate u Azure:

-   Certifikat mora sadržavati privatni ključ.
-   Certifikat mora se stvara za ključne exchange, moguće je izvesti u datoteku osobne razmjena podataka (.pfx).
-   Potvrde predmetni naziv mora podudarati domenu koja se koristi za pristup servisa u oblaku. Nije moguće nabavite SSL certifikat ustanove za izdavanje potvrda (CA) za domenu cloudapp.net. Morate pribaviti prilagođenog naziva domene koristiti kad pristup servisu. Kada certifikat zatražite od izdavanje certifikata, potvrde predmetni naziv mora odgovarati na prilagođeni naziv domene koristi za pristup aplikaciji. Ako, na primjer, ako prilagođeni naziv domene je **contoso.com** bi zahtjev certifikat od vašeg CA za * **. contoso.com** ili * *www.contoso.com**.
-   Certifikat mora koristiti najmanje 2048-bitni šifriranje.

Testiranje svrhe možete [stvoriti](cloud-services-certs-create.md) i pomoću samopotpisanog certifikata. Samopotpisani certifikat je autentičnost provjerena kroz izdavanje certifikata, a možete koristiti domenu cloudapp.net kao URL web-mjesta. Ako, na primjer, sljedeći zadatak koristi samopotpisani certifikat u kojoj je uobičajeni naziv (CN) korištenog u certifikat **sslexample.cloudapp.net**.

Nakon toga morate uključiti informacije o potvrdi definicija servisa i datoteke za konfiguraciju servisa.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Korak 2: Izmjena datoteke definicije i konfiguracija servisa

Aplikacija mora biti konfiguriran za korištenje certifikat, a krajnje HTTPS mora biti dodan. Zbog toga definicija servisa i datoteke za konfiguraciju servisa potrebno ažurirati.

1.  U razvojno okruženje otvorili datoteku definicije servisa (CSDEF), dodajte section **Certifikati** unutar odjeljka **WebRole** i sadrže sljedeće informacije o certifikat (i srednja certifikati):

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    U odjeljku **potvrde** definira naziv naš certifikata, njezino mjesto i naziv trgovinu gdje se nalazi.
    
    Dozvole (`permisionLevel` atribut) možete postaviti na jedan od sljedećih vrijednosti:

  	| Vrijednost dozvola  | Opis |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Zadano)** Svi uloga procesi mogu pristupiti privatni ključ. |
  	| povećani          | Samo povećane procesi mogu pristupiti privatni ključ.|

2.  U datoteci definicija servisa, dodajte element **InputEndpoint** unutar odjeljka **krajnje točke** da biste omogućili HTTPS:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  U datoteci definicija servisa, dodajte **Povezivanje** element u odjeljku **web-mjesta** . U ovom se odjeljku dodaje HTTPS povezivanje koji će mapiranje krajnju točku na web-mjesto:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    Dovršite sve potrebne promjene da biste datoteku definicije servisa, ali i dalje trebate da biste dodali informacije o certifikatu konfiguracijska datoteka servisa.

4.  U servis konfiguracijskoj datoteci (CSCFG), ServiceConfiguration.Cloud.cscfg, dodajte **Certifikati** sekcije u odjeljku **uloge** zamjene vrijednost otisak prsta uzorka s certifikat je prikazano u nastavku:

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(U prethodnom primjeru koristi **sha1** za algoritam za otisak prsta. Navedite odgovarajuće vrijednosti za algoritam za otisak prsta potvrde.)

Sad kad ažurirane definicija servisa i datoteke za konfiguraciju servisa, pakiranje implementaciju sustava za prijenos u Azure. Ako koristite **cspack**, nemojte koristiti zastavice **/generateConfigurationFile** , kao što je koji se brišu podaci o certifikatu ste umetnuli.

## <a name="step-3-upload-a-certificate"></a>Korak 3: Prijenos certifikat

Paket za implementaciju ažurirana je da biste koristili certifikat, a krajnje HTTPS dodan. Sada možete prenijeti paket i certifikat za Azure pomoću portala za Azure klasični.

1. Prijavite se na [portal za Azure klasični][]. 
2. Na strani lijevom navigacijskom oknu kliknite **Servise u Oblaku** .
3. Kliknite željeni oblaku.
4. Kliknite karticu **potvrde** .

    ![Kliknite karticu potvrde](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Kliknite gumb **Prenesi** .

    ![Prijenos](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Navedite **datoteke**, **lozinku**, a zatim kliknite **dovrši** (kvačica).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Korak 4: Povezivanje s instancom uloga pomoću HTTPS

Sad kad uvođenja je s radom u Azure, možete se povezati pomoću HTTPS.

1.  Azure klasični portalu odaberite implementaciju sustava, a zatim kliknite vezu u odjeljku **URL web-mjesta**.

    ![Određivanje URL-a web-mjesta][2]

2.  U web-pregledniku izmjena vezu da biste koristili **https** umjesto **http**, a posjetite stranicu.

    >[AZURE.NOTE] Ako koristite samopotpisani certifikat pri pregledavanju krajnje HTTPS pridružen samopotpisani certifikat koji se može se pojaviti pogreška certifikata u pregledniku. Pomoću potpisao pouzdana ustanova za izdavanje certifikata uklanja problem; s aplikacijom, možete zanemariti pogrešku. (Druga mogućnost je da biste dodali samopotpisani certifikat korisnika pouzdana ustanova za izdavanje certifikata spremište.)

    ![SSL primjer web-mjesta][3]

Ako želite koristiti SSL pripremna implementacije umjesto produkcijske implementaciju, najprije morate određivanje URL-a koristi pripremna implementacije. Implementacija servis u oblaku pripremna okruženju bez certifikat ili sve informacije o certifikatu. Kada implementiran, možete odrediti koji se temelji na GUID URL koji će se pojaviti u polje **URL web-mjesta** u Azure klasični portalom. Stvaranje certifikata s uobičajeni je naziv (CN) jednako utemeljen na GUID URL-a (na primjer, **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Pomoću portala za Azure klasični da biste dodali certifikat u postupnu oblaku. Zatim dodajte informacije o certifikatu datotekama CSDEF i CSCFG, repackage aplikacije i ažuriranje postupnu uvođenja da biste koristili novi paket.

## <a name="next-steps"></a>Daljnji koraci

* [Općenito konfiguracije servis u oblaku](cloud-services-how-to-configure.md).
* Saznajte kako [implementirati servis u oblaku](cloud-services-how-to-create-deploy.md).
* Konfiguriranje [Prilagođeni naziv domene](cloud-services-custom-domain-name.md).
* [Upravljanje servis u oblaku](cloud-services-how-to-manage.md).


  [Azure klasični portal]: http://manage.windowsazure.com
  [0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
  [1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
  [2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
  [3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
  [4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
