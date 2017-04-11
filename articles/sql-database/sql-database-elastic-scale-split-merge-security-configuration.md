<properties 
    pageTitle="Konfiguracija sigurnosti Podijeli cirkularnog pisma | Microsoft Azure" 
    description="Postavljanje x409 certifikata za šifriranje" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Konfiguracija sigurnosti Podijeli cirkularnog pisma  

Da biste koristili servisa za podjelu/spajanje, pravilno konfiguracija sigurnosti. Servis je dio značajke Elastic skala s bazom podataka sustava Microsoft Azure SQL. Dodatne informacije potražite u članku [Elastic mjerilo Podijeli i spajanje vodič za servis](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Konfiguriranje certifikata

Certifikati su konfigurirana na dva načina. 

1. [Konfiguriranje SSL certifikata](#To-Configure-the-SSL#Certificate)
2. [Da biste konfigurirali klijentskih potvrda](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Da biste dobili certifikata

Certifikati se mogu dobivaju iz javne ustanovama za izdavanje certifikata (CA) ili [Servis Windows certifikata](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). To su Preferirani načina dobivanja certifikata.

Ako su te mogućnosti dostupne, možete generirati **samopotpisane potvrde**.
 
## <a name="tools-to-generate-certificates"></a>Alati za generiranje certifikata

* [makecert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Pokretanje alata

* Iz za razvojne inženjere naredbeni redak za vizualni Studios, potražite u članku [naredbenog retka za Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) 

    Ako je instaliran, idite na:

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* Početak WDK iz [Windows 8.1: preuzimanje oprema i alati](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>Konfiguriranje SSL certifikata
Šifriranje komunikacije i provjeru autentičnosti poslužitelja potreban je SSL certifikat. Odaberite najčešće primjenjivo tri scenarija ispod i izvršiti sve korake:

### <a name="create-a-new-self-signed-certificate"></a>Stvaranje samopotpisane potvrde

1.    [Stvaranje samopotpisane potvrde](#Create-a-Self-Signed-Certificate)
2.    [Stvaranje datoteke PFX Self-Signed SSL certifikata](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [Prijenos SSL certifikat na servis u Oblaku](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [Ažuriranje SSL certifikata u datoteci konfiguracije servisa](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [Uvoz SSL ustanova za izdavanje certifikata](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Da biste koristili postojeći certifikat iz spremišta certifikata
1. [Izvoz SSL certifikat iz spremišta certifikata](#Export-SSL-Certificate-From-Certificate-Store)
2. [Prijenos SSL certifikat na servis u Oblaku](#Upload-SSL-Certificate-to-Cloud-Service)
3. [Ažuriranje SSL certifikata u datoteci konfiguracije servisa](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Da biste koristili postojeći certifikat u datoteci PFX

1. [Prijenos SSL certifikat na servis u Oblaku](#Upload-SSL-Certificate-to-Cloud-Service)
2. [Ažuriranje SSL certifikata u datoteci konfiguracije servisa](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>Da biste konfigurirali klijentskih potvrda
Da bi se autentičnost zahtjevi za uslugu potrebni su certifikati klijenta. Odaberite najčešće primjenjivo tri scenarija ispod i izvršiti sve korake:

### <a name="turn-off-client-certificates"></a>Isključivanje klijentskih potvrda
1.    [Isključivanje provjere autentičnosti utemeljene na certifikat klijenta](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Problema novog klijenta samopotpisanog certifikata
1.    [Stvaranje samopotpisane ustanova za izdavanje certifikata](#Create-a-Self-Signed-Certification-Authority)
2.    [Prijenos ustanove za Izdavanje certifikata radi servis u Oblaku](#Upload-CA-Certificate-to-Cloud-Service)
3.    [Ažuriranje ustanove za Izdavanje certifikata u datoteci konfiguracije servisa](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Problem klijentskih potvrda](#Issue-Client-Certificates)
5.    [Stvaranje PFX datotekama za klijentskih potvrda](#Create-PFX-files-for-Client-Certificates)
6.    [Uvoz klijentski certifikat](#Import-Client-Certificate)
7.    [Kopiranje Thumbprints certifikata klijenta](#Copy-Client-Certificate-Thumbprints)
8.    [Konfiguriranje dopuštene klijente u konfiguracijskoj datoteci servisa](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Korištenje postojeće klijentskih potvrda
1.    [Pronalaženje CA javni ključ](#Find-CA-Public Key)
2.    [Prijenos ustanove za Izdavanje certifikata radi servis u Oblaku](#Upload-CA-certificate-to-cloud-service)
3.    [Ažuriranje ustanove za Izdavanje certifikata u datoteci konfiguracije servisa](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Kopiranje Thumbprints certifikata klijenta](#Copy-Client-Certificate-Thumbprints)
5.    [Konfiguriranje dopuštene klijente u konfiguracijskoj datoteci servisa](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Konfiguriranje Provjera opoziva certifikata klijenta](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Dopuštene IP adrese

Pristup krajnje točke servisa može biti ograničeno na određene raspone IP adrese.

## <a name="to-configure-encryption-for-the-store"></a>Da biste konfigurirali šifriranje spremišta

Potreban je certifikat za šifriranje vjerodajnica pohranjene u spremištu metapodataka. Odaberite najčešće mjerodavni su tri slučajevi ispod i izvršiti sve korake:

### <a name="use-a-new-self-signed-certificate"></a>Korištenje novog samopotpisanog certifikata

1.     [Stvaranje samopotpisane potvrde](#Create-a-Self-Signed-Certificate)
2.     [Stvaranje datoteke PFX za Self-Signed certifikata za šifriranje](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Prijenos certifikata za šifriranje na servis u Oblaku](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Ažuriranje certifikata za šifriranje u datoteci konfiguracije servisa](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Korištenje postojeći certifikat iz spremišta certifikata

1.     [Izvoz certifikata za šifriranje iz spremišta certifikata](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Prijenos certifikata za šifriranje na servis u Oblaku](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Ažuriranje certifikata za šifriranje u datoteci konfiguracije servisa](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Korištenje postojeći certifikat u datoteci PFX

1.     [Prijenos certifikata za šifriranje na servis u Oblaku](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Ažuriranje certifikata za šifriranje u datoteci konfiguracije servisa](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>Konfiguriranje zadanog

Konfiguriranje zadanog onemogućava pristupa krajnjoj točki HTTP. Ovo je preporučenim postavkama, budući da zahtjevi za te krajnje točke mogu prenositi povjerljive podatke kao što su vjerodajnice za bazu podataka.
Konfiguriranje zadanog omogućuje pristupa krajnjoj točki HTTPS. Ta postavka dodatno mogu biti ograničeni.

### <a name="changing-the-configuration"></a>Promjena konfiguracije

Grupa te pravila za kontrolu pristupa koji se odnose na krajnjoj točki nije konfiguriran u na **<EndpointAcls>** sekciju u **datoteku za konfiguraciju servisa**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Pravila u grupi kontrole programa access nije konfiguriran u na <AccessControl name=""> dio konfiguracijska datoteka servisa. 

Oblik je objašnjeno u dokumentaciji mrežni pristup kontrole popisa.
Na primjer, da biste omogućili samo IP-ovi u rasponu 100.100.0.0 da biste 100.100.255.255 pristupa krajnjoj točki HTTPS, pravila izgledati ovako:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Uskraćivanje sprječavanje servisa

Postoje dvije različite mehanizme podržana za otkrivanje i spriječili napada uskraćivanje servisa:

*    Ograničiti broj istovremeni zahtjevi po udaljenog glavnog računala (po zadanom je isključeno)
*    Ograničavanje stopa pristupa po udaljenog glavnog računala (na po zadanom)

Ove se temelje na značajke dodatno navedenih u dinamičku IP sigurnost u IIS-u. Prilikom promjene tu konfiguraciju čuvajte sljedeće čimbenike:

* Ponašanje proxy poslužitelji i prevođenja mrežnih adresa uređaja nad podacima udaljeno glavno računalo
* Svaki zahtjev za neki resurs u ulozi web smatra (npr. učitavanja skripte, slike, itd)

## <a name="restricting-number-of-concurrent-accesses"></a>Ograničavanje broja Istodobni pristupa

Postavke koje konfiguriranje takvo ponašanje su:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Promijenite DynamicIpRestrictionDenyByConcurrentRequests na true da biste omogućili ovu zaštitu.

## <a name="restricting-rate-of-access"></a>Ograničavanje stopa programa access

Postavke koje konfiguriranje takvo ponašanje su:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>Konfiguriranje odgovor na zahtjev za odbijenih

Sljedeća postavka konfigurira odgovor na zahtjev za odbijenih:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Potražite u dokumentaciji za dinamičku IP sigurnost u IIS drugih podržani vrijednosti.

## <a name="operations-for-configuring-service-certificates"></a>Postupke za konfiguriranje servisa certifikata
U ovoj se temi je samo za referencu. Slijedite korake u konfiguraciji navedene u:

* Konfiguriranje SSL certifikata
* Konfiguriranje klijenta certifikata

## <a name="create-a-self-signed-certificate"></a>Stvaranje samopotpisane potvrde
Izvršavanje:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Da biste prilagodili:

*    -n servisa URL-om. Zamjenski znakovi ("CN = * .cloudapp .net") i podržani su druge nazive ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net").
*    -e s datumom isteka certifikata stvorite jaku lozinku i navedite je kada se to od vas zatraži.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Stvaranje datoteke PFX samopotpisani SSL certifikata

Izvršavanje:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Unesite lozinku, a zatim izvoz certifikata pomoću ove mogućnosti:
* Da, izvezi privatni ključ
* Izvezi sva proširena svojstva

## <a name="export-ssl-certificate-from-certificate-store"></a>Izvoz SSL certifikat iz spremišta certifikata

* Pronalaženje certifikata
* Kliknite Akcije -> sve zadatke -> Izvoz...
* Izvoz certifikat na. PFX datoteka pomoću sljedećih mogućnosti:
    * Da, izvezi privatni ključ
    * Ako je moguće uključiti svi certifikati na putu ustanova * izvoz sva dodatna svojstva

## <a name="upload-ssl-certificate-to-cloud-service"></a>Prijenos SSL certifikata u oblaku

Prijenos certifikat s postojeće ili generira. PFX datoteka s par ključa SSL:

* Unesite lozinku za zaštitu privatni podaci o ključu

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Ažuriranje SSL certifikata u datoteci konfiguracije servisa

Ažurirajte vrijednost otisak prsta sljedeće postavke u konfiguracijskoj datoteci servisa otisak prsta potvrde prenijeli na servis u oblaku:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>Uvoz SSL ustanova za izdavanje certifikata

Slijedite korake u svim računa/stroju koji će komunicirati s uslugom.

* Dvokliknite na. CER datoteke u programu Windows Explorer
* U dijaloškom okviru za potvrdu kliknite Instaliranje certifikata...
* Uvoz certifikata u trgovini izdavanje korijenskih

## <a name="turn-off-client-certificate-based-authentication"></a>Isključivanje provjere autentičnosti utemeljene na certifikat klijenta

Podržana je samo klijent certifikat provjeru autentičnosti utemeljenu na i onemogućivanjem će omogućiti pristup javnim krajnje točke servisa osim ako su drugi načini na mjestu (npr. Microsoft Azure virtualne mreže).

Da biste promijenili te postavke FALSE u konfiguracijskoj datoteci servisa da biste isključili značajku:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Zatim kopirajte otisak prsta isti kao i SSL certifikat u postavci ustanove za Izdavanje certifikata:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Stvaranje samopotpisane ustanova za izdavanje certifikata
Izvođenje sljedeće korake za stvaranje samopotpisane potvrde će poslužiti kao ustanova za izdavanje certifikata:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Da biste ga prilagodili

*    -e s datumom isteka certifikata


## <a name="find-ca-public-key"></a>Pronalaženje CA javni ključ

Svi certifikati klijentskog je morate Izdana ustanova za izdavanje pouzdana servis. Pronađite javni ključ za izdavanje certifikata koja je izdala klijentskog programa potvrde koje namjeravate koristiti za provjeru autentičnosti da bi se prenijeti na servis u oblaku.

Ako datoteku s javnim ključem nije dostupna, ga izvesti iz spremišta certifikata:

* Pronalaženje certifikata
    * Traženje klijentski certifikat izdala isti ustanova za izdavanje certifikata
* Dvokliknite certifikata.
* Odaberite karticu ustanova put u dijaloškom okviru za potvrdu.
* Dvokliknite stavku CA na putu.
* Bilješke svojstva potvrde.
* Zatvorite dijaloški okvir **potvrde** .
* Pronalaženje certifikata
    * Traženje CA zabilježili iznad.
* Kliknite Akcije -> sve zadatke -> Izvoz...
* Izvoz certifikat na. CER pomoću ove mogućnosti:
    * **Ne, izvezi privatni ključ**
    * Ako je moguće uključiti svi certifikati u put certifikata.
    * Izvezi sva proširena svojstva.

## <a name="upload-ca-certificate-to-cloud-service"></a>Prenesite ustanove za Izdavanje certifikata servis u oblaku

Prijenos certifikat s postojeće ili generira. Datoteka CER CA javni ključ.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Ažuriranje CA certifikat u datoteci konfiguracije servisa

Ažurirajte vrijednost otisak prsta sljedeće postavke u konfiguracijskoj datoteci servisa otisak prsta potvrde prenijeli na servis u oblaku:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Isti otisak prsta ažurirati vrijednost sljedeće postavke:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Izdavanje certifikata klijenta

Svaku osobu ovlašten za pristup servisu treba potvrdu klijenta izdavanja za his/hers isključivo pomoću i trebali biste odabrati his/hers vlasnik jaku lozinku radi zaštite privatni ključ. 

Na istom računalu koje generira i spremljene samopotpisani certifikat CA mora izvršiti sljedeće korake:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Prilagodba:

* n - s ID-ove za klijentu koji će biti ovjerena ovog certifikata
* -e s datumom isteka certifikata
* MyID.pvk i MyID.cer s jedinstven naziv za ovaj certifikat klijenta

Ta se naredba će vas za lozinku da biste stvorili i koristi puta. Koristite jaku lozinku.

## <a name="create-pfx-files-for-client-certificates"></a>Stvaranje certifikata PFX datotekama za klijenta

Za svaki generirani klijentski certifikat izvršavanje:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Prilagodba:

    MyID.pvk and MyID.cer with the filename for the client certificate

Unesite lozinku, a zatim izvoz certifikata pomoću ove mogućnosti:

* Da, izvezi privatni ključ
* Izvezi sva proširena svojstva
* Osoba kojima je u tijeku izdan certifikat trebali biste odabrati lozinku za izvoz

## <a name="import-client-certificate"></a>Uvoz klijentski certifikat

Svaku osobu čije potvrdu klijenta sredstava želite uvesti ključne par u strojeva čiji će se koristiti za komunikaciju sa servisom:

* Dvokliknite na. PFX datoteke u programu Windows Explorer
* Uvoz u osobnih spremište potvrde s najmanje ovu mogućnost:
    * Obuhvati sva proširena svojstva provjeriti

## <a name="copy-client-certificate-thumbprints"></a>Kopiranje thumbprints certifikata klijenta
Svaku osobu čije potvrdu klijenta sredstava morate slijediti ove korake da biste dobili otisak prsta his/hers certifikat koji će se dodati konfiguracijska datoteka servisa:
* Pokretanje certmgr.exe
* Odaberite karticu osobne
* Dvokliknite klijentski certifikat koji se koriste za provjeru autentičnosti
* U dijaloškom okviru certifikat koji će se otvoriti, odaberite karticu Detalji
* Provjerite je li prikazuje Prikaži sve
* Odaberite polje pod nazivom otisak prsta na popisu
* Kopirajte vrijednost otisak prsta **Brisanje koji nisu vidljivi Unicode znakova iza posljednje znamenke** Izbriši sve razmake

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>Konfiguriranje dopuštene klijente u konfiguracijskoj datoteci servisa

Ažurirajte vrijednost sljedeće postavke u konfiguracijskoj datoteci servisa s popisom thumbprints klijentskih potvrda dopušten pristup servisu odvojenih zarezom:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Konfiguriranje Provjera opoziva certifikata klijenta

Zadana postavka se obratite se ustanova za izdavanje certifikata za stanje opoziva certifikata klijenta. Da biste uključili provjeru, ako ustanova za izdavanje certifikata koje je izdao klijent potvrde podržava takve provjere, promijenite sljedeće postavke s jednim od vrijednosti definirano u Enumeracije X509RevocationMode:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Stvaranje datoteke PFX za šifriranje samopotpisanog certifikata

Za potvrdu šifriranja izvršavanje:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Prilagodba:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Unesite lozinku, a zatim izvoz certifikata pomoću ove mogućnosti:
*    Da, izvezi privatni ključ
*    Izvezi sva proširena svojstva
*    Kada prijenos certifikata na servis u oblaku ćete lozinku.

## <a name="export-encryption-certificate-from-certificate-store"></a>Izvoz certifikata za šifriranje iz spremišta certifikata

*    Pronalaženje certifikata
*    Kliknite Akcije -> sve zadatke -> Izvoz...
*    Izvoz certifikat na. PFX datoteka pomoću sljedećih mogućnosti: 
  *    Da, izvezi privatni ključ
  *    Ako je moguće uključiti svi certifikati na putu certifikata 
*    Izvezi sva proširena svojstva

## <a name="upload-encryption-certificate-to-cloud-service"></a>Prenesite certifikata za šifriranje servis u oblaku

Prijenos certifikat s postojeće ili generira. Datoteka PFX sa svih parova ključa za šifriranje:

* Unesite lozinku za zaštitu privatni podaci o ključu

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Ažuriranje certifikata za šifriranje u datoteci konfiguracije servisa

Ažurirajte otisak prsta vrijednost od sljedećih postavki u konfiguracijskoj datoteci servisa otisak prsta potvrde prenijeli na servis u oblaku:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Uobičajeni postupci certifikata

* Konfiguriranje SSL certifikata
* Konfiguriranje klijenta certifikata

## <a name="find-certificate"></a>Pronalaženje certifikata

Slijedite ove korake:

1. Pokrenite mmc.exe.
2. Datoteka -> Dodaj/ukloni dodatak...
3. Odaberite **Certifikati**.
4. Kliknite **Dodaj**.
5. Odaberite mjesto spremišta certifikata.
6. Kliknite **Završi**.
7. Kliknite **u redu**.
8. Proširite **certifikata**.
9. Proširite čvor spremište certifikata.
10. Proširite čvor podređeni certifikat.
11. Na popisu odaberite certifikat.

## <a name="export-certificate"></a>Izvoz certifikata
U **čarobnjaku za izvoz certifikata**:

1. Kliknite **Dalje**.
2. Odaberite **da**, **Izvezi privatni ključ**.
3. Kliknite **Dalje**.
4. Odaberite željeni izlazni oblik datoteke.
5. Potvrdite željene mogućnosti.
6. Potvrdite **lozinku**.
7. Unesite jaku lozinku i potvrdite je.
8. Kliknite **Dalje**.
9. Upišite ili kliknite Pregledaj naziv datoteke gdje želite spremiti certifikat (pomoću programa. Nastavak PFX).
10. Kliknite **Dalje**.
11. Kliknite **Završi**.
12. Kliknite **u redu**.

## <a name="import-certificate"></a>Uvoz certifikata

U čarobnjaku za uvoz certifikata:

1. Odaberite mjesto spremišta.

    * Odabir **Trenutnog korisnika** ako samo procesa koji se izvode u odjeljku trenutni korisnik će pristup servisu
    * Ako drugi procesi u računalo će pristup servisu, odaberite **Lokalnog računala**
2. Kliknite **Dalje**.
3. Ako uvozite iz datoteke, provjerite put datoteke.
4. Ako uvoza u. PFX datoteka:
    1.     Unesite lozinku za zaštitu privatni ključ
    2.     Odabir mogućnosti uvoza
5.     Odaberite "Mjesto" potvrde u sljedeće spremište
6.     Kliknite **Pregledaj**.
7.     Odaberite željeni spremište.
8.     Kliknite **Završi**.
       
    * Ako je odabran spremište pouzdana korijenska ustanova za izdavanje certifikata, kliknite **da**.
9.     Kliknite **u redu** u sustavu windows za sve dijaloški okvir.

## <a name="upload-certificate"></a>Prijenos certifikata

[Portal za Azure](https://portal.azure.com/)

1. Odaberite **servise u Oblaku**.
2. Odaberite servis u oblaku.
3. Na izborniku gornji kliknite **Certifikati**.
4. Na donjoj traci kliknite **Prenesi**.
5. Odaberite datoteku certifikata.
6. Ako je na. PFX datoteku, unesite lozinku za privatni ključ.
7. Nakon dovršetka, kopirajte otisak prsta certifikat iz novu stavku na popisu.

## <a name="other-security-considerations"></a>Ostale sigurnosna pitanja vezana uz
 
Postavke SSL opisane u ovom dokumentu Šifrirajte komunikacije između servisa i njegov klijenti kada se koristi krajnju točku HTTPS. Ovo je na važne od vjerodajnice za pristup bazi podataka i potencijalno druge povjerljive podatke sadržani su u komunikacije. Međutim, imajte na umu servis nastavi pojavljivati interne statusa, uključujući vjerodajnice, u interne tablice u bazi podataka sustava Microsoft Azure SQL koju ste naveli za spremište metapodataka u pretplatu na Microsoft Azure. Tu bazu podataka je definirana kao dio sljedeće postavke servisa konfiguracijskoj datoteci (. CSCFG datoteka): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Šifrirane vjerodajnicama pohranjenima u toj bazi podataka. Međutim, kao najbolji način provjerite je li web i tempiranja uloge implementacijama sustava servisa su ažurni i sigurne čim pristup bazi podataka metapodataka i certifikat koji je korišten za šifriranje i dešifriranje pohranjenih vjerodajnica imat. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
