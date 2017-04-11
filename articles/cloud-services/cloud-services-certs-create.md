<properties 
    pageTitle="Cloud servisa i upravljanje certifikati | Microsoft Azure" 
    description="Saznajte kako stvoriti i pomoću certifikata Microsoft Azure" 
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Pregled certifikata za servise u Oblaku Azure
Certifikati se koriste u Azure za servise u oblaku ([Certifikati servisa](#what-are-service-certificates)) i provjera autentičnosti s upravljanjem API-JA ([Upravljanje certifikata](#what-are-management-certificates) prilikom korištenja Azure klasični portal i ne ARM). Ova tema sadrži Općenito pregled obje vrste certifikata kako [stvoriti](#create) i [implementirati](#deploy) im Azure.

Potvrde koje se koriste u Azure su x.509 certifikate v3 ili možete potpiše drugi pouzdanih certifikat, a može biti samopotpisani. Samopotpisani certifikat je potpisan vlastitu autor i zbog toga, ne pouzdani prema zadanim postavkama. Većini preglednika možete zanemariti to. Samopotpisane potvrde trebali biste koristiti samo sebi prilikom razvoja i testiranja servise u oblaku. 

Certifikati koristi Azure mogu sadržavati privatnog ili javni ključ. Certifikati su otisak prsta koja omogućuje znači radi prepoznavanja budu jednoznačni način. U ovom otisak prsta se koristi u Azure [konfiguracijska datoteka](cloud-services-configure-ssl-certificate.md) da biste odredili koji certifikat trebali biste koristiti servise u oblaku. 

## <a name="what-are-service-certificates"></a>Što su certifikati servis?
Servis certifikati su priložene servise u oblaku i omogućite sigurnu komunikaciju i iz usluge. Na primjer, ako implementiran web uloge, bi želite navesti certifikat koji možete provjeriti autentičnost prikazanog HTTPS krajnje točke. Certifikati servisa definirano u definicije servisa automatski se implementiran na virtualnog računala sa sustavom instance komponente vaša uloga. 

Možete prenijeti certifikati servisa Azure klasični portal ili pomoću portala za Azure klasični ili pomoću upravljanja API servisa. Servis certifikati su povezan sa servisom cloud određene i dodijeljene implementacije u datoteku definicije servisa.

Servis certifikata njome se upravlja zasebno iz servisa, a možda upravlja različite osobe. Na primjer, razvojni inženjer može prenijeti paket servisa koji se odnosi na certifikat koji je IT Voditelj koje ste prethodno prenijeli Azure. IT Voditelj možete upravljati i obnoviti taj certifikat promjene konfiguracije servisa bez obzira na to Prenesi novi paket servisa. Ovo je moguće jer logički naziv certifikata i spremište naziv i mjesto u određeni su klauzulom datoteku definicije servisa dok otisak prsta certifikat je naveden u konfiguracijskoj datoteci servisa. Da biste ažurirali certifikata, samo je potrebno prenijeti novi certifikat i promijenite vrijednost otisak prsta u konfiguracijskoj datoteci servisa.

## <a name="what-are-management-certificates"></a>Što su upravljanje certifikata?
Upravljanje certifikati omogućuju provjeru s API upravljanje usluge nudi Azure klasični. Mnoge programima i alatima (primjerice Visual Studio ili Azure SDK) će koristiti ove potvrde da biste automatizirali konfiguracija i implementaciju različite servise za Azure. To nije zaista vezani uz servise u oblaku. 

>[AZURE.WARNING] pazi! Ove vrste certifikata Dopusti svima koji potvrđuje s njima da biste upravljali pretplate na kojima su povezane. 

### <a name="limitations"></a>Ograničenja
Postoji ograničenje od 100 certifikati upravljanje po pretplati. Postoji i ograničenje od 100 upravljanje certifikati za sve pretplate u odjeljku administrator određene servisa korisničkog ID-a Ako je korisnički ID za administratorski račun već koristi da biste dodali 100 upravljanje potvrde i nema potrebe za više certifikata, možete dodati zajednički administrator da biste dodali Dodatni certifikati. 

Prije dodavanja više od 100 certifikata potražite u članku ponovne postojeći certifikat. Korištenje dodatnih administratora dodaje potencijalno nepotrebne složenosti proces upravljanja certifikata.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Stvaranje samopotpisane potvrde
Možete koristiti bilo koji alat za stvaranje samopotpisane potvrde pod uvjetom da se drži ovih postavki:

* X.509 certifikat.
* Sadrži privatni ključ.
* Stvara za ključne exchange (.pfx datoteka).
* Predmetni naziv mora podudarati domenu koja se koristi za pristup servisa u oblaku. 
    > Ne dobije SSL certifikata u cloudapp.net (ili bilo koju Azure povezane) domene; potvrde predmetni naziv mora odgovarati na prilagođeni naziv domene koristi za pristup aplikaciji. Na primjer, **contoso.net**, ne **contoso.cloudapp.net**.
* Najmanji od 2048-bitni šifriranje.
* **Servis certifikata samo**: klijentsko certifikat mora se nalaziti u spremištu *osobnih* certifikata.

Postoje dva jednostavna načina za stvaranje certifikata u sustavu Windows, u `makecert.exe` utility ili IIS.

### <a name="makecertexe"></a>Makecert.exe

Uslužni zastarjela i više navedenih u nastavku. Pročitajte [Ovaj članak MSDN](https://msdn.microsoft.com/library/windows/desktop/aa386968) dodatne informacije.

### <a name="powershell"></a>PowerShell

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Ako želite koristiti certifikat s IP adresom umjesto domene pomoću IP adresa u parametru - DnsName.


Ako želite koristiti taj [certifikat portal za upravljanje](../azure-api-management-certs.md)ga izvesti **.cer** datoteka:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet Information Services (IIS)

Postoje broj stranica na Internetu koji pokrivaju kako to učiniti s IIS-om. [Ovdje](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) je odličan jedan mogu pronaći mislim da ga dobro objašnjava. 

### <a name="java"></a>Java
Java omogućuju [Stvaranje certifikata](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[U ovom](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) se članku opisuje kako stvoriti potvrde s SSH.

## <a name="next-steps"></a>Daljnji koraci

[Prijenos certifikat servisa Azure klasični portal](cloud-services-configure-ssl-certificate.md) (ili [portal za Azure](cloud-services-configure-ssl-certificate-portal.md)).

Prenesite [Upravljanje API certifikat](../azure-api-management-certs.md) Azure klasični portalu.

>[AZURE.NOTE] Portal za Azure korištenja upravljanja certifikata za pristup s API-JA, ali umjesto koristi korisničke račune.
