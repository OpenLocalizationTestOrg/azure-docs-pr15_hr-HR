<properties
    pageTitle="Implementacija MySQL davatelja resursa na hrpu Azure | Microsoft Azure"
    description="Detaljne upute za implementaciju davatelja resursa MySQL snop Azure."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>Implementacija MySQL davatelja resursa u stogu Azure za uporabu WebApps

> [AZURE.NOTE] Sljedeće informacije odnosi se samo na Azure stogu TP1 implementacije.

Koristite ovaj članak da biste pratili detaljne upute za postavljanje davatelja resursa MySQL na hrpu Azure dokaz o pojam (PNA) tako da možete početi [koristiti baze podataka MySQL](azure-stack-mysql-rp-deploy-short.md) u stogu Azure, uključujući korištenje MySQL kao pozadinski WordPress web-mjesta stvoren pomoću [Azure web-aplikacije](azure-stack-webapps-deploy.md).

## <a name="set-up-steps-before-you-deploy"></a>Postavljanje korake prije implementacije

Prije implementacije kod davatelja resursa, morate:

- Sadržavati zadanu sliku Windows Server s .NET 3.5
- Isključivanje poboljšane zaštite Internet Explorer (IE)
- Instalirajte najnoviju verziju Azure PowerShell

### <a name="create-an-image-of-windows-server-including-net-35"></a>Stvaranje slike sustava Windows Server, uključujući .NET 3.5

Ako ste preuzeli bitova Azure stogu nakon 23/2/2016 jer zadane osnovne Windows Server 2012 R2 slike sadrži .NET 3.5 framework u preuzimanje i novijim verzijama, preskočite ovaj korak.

Ako ste preuzeli prije 23/2/2016, morate stvoriti Windows Server 2012 R2 podatkovnog centra VHD sa slikom za .NET 3.5 i skup je kao zadanu sliku iz spremišta platforme slike.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>Isključivanje IE Poboljšana sigurnost i omogućivanje kolačića

Da biste implementirali davatelja resursa, pokrenete u Očisti integrirani skriptiranje okruženje (filtar) kao administrator, tako da morate dopustiti kolačiće i JavaScript u profilu programa Internet Explorer koristite za prijavu na Azure Active Directory za administratora i korisnički znak dodataka.

**Da biste isključili IE Poboljšana sigurnost:**

1. Prijavite se na računalo (PNA) za Azure stogu dokaz pojam kao AzureStack/administrator, a zatim otvorite upravitelj poslužitelja.

2. Isključite **Poboljšana konfiguracija sigurnosti preglednika Internet Explorer** administratorima i korisnicima.

3. Prijavite se u **ClientVM.AzureStack.local** virtualnog računala kao administrator, a zatim otvorite upravitelj poslužitelja.

4. Isključite **Poboljšana konfiguracija sigurnosti preglednika Internet Explorer** administratorima i korisnicima.

**Da biste omogućili kolačiće:**

1. Na zaslonu Start sustava Windows kliknite **sve aplikacije**, Pomagala **Windows**, desnom tipkom miša kliknite **Internet Explorer**, pokažite na **više**, a zatim **Pokreni kao administrator**.

2. Ako se to od vas zatraži, potvrdite **Upotrijebi preporučeni sigurnost**, a zatim **u redu**.

3. U pregledniku Internet Explorer kliknite **Alati () ikonu zupčanika** &gt; **Internetske mogućnosti** &gt; karticu **Privatnost** .

4. Kliknite **Dodatno**, provjerite jesu li oba gumba **Prihvati** , kliknite **u redu**i zatim kliknite **u redu** .

5. Zatvorite Internet Explorer, a zatim ponovno pokrenite Očisti filtar kao administrator.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Instalacija Azure stogu kompatibilne izdanje PowerShell Azure

1. Deinstalirajte postojeće Azure PowerShell s VM vaš klijent.

2. Prijavite se na računalo Azure stogu PNA kao AzureStack/administrator.

3. Putem udaljene radne površine, prijavite se u **ClientVM.AzureStack.local** virtualnog računala kao administrator.

4. Otvorite upravljačku ploču, kliknite **Deinstaliraj program** &gt; kliknite **Azure PowerShell** &gt; kliknite **Deinstaliraj**.

5. [Preuzmite najnovije Azure PowerShell koji podržava stogu Azure](http://aka.ms/azstackpsh) i instalirajte ga.

    Nakon što instalirate PowerShell, možete pokrenuti potvrdu skriptu PowerShell da biste provjerili možete li se povezati s vašoj instanci Azure snop (prikazivati na web-stranicu za prijavu).

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>Povezati davatelja implementacije resursa PowerShell

1. Povezivanje udaljene radne površine Azure stogu PNA clientVm.AzureStack.Local i prijavite se kao azurestack\\azurestackuser.

2. [Preuzimanje binarne datoteke za to MySQL](http://aka.ms/masmysqlrp) datoteku i Izdvoji D:\\MySQLRP.

3. Pokretanje sustava D:\\MySQLRP\\Bootstrap.cmd datoteke kao administrator (azurestack\administrator).

    Bootstrap.ps1 datoteka se otvara u Očisti filtar.

4. Kada windows Očisti filtar dovrši učitavanje, kliknite gumb "reprodukciju" ili pritisnite F5.

    Učitavanje će dvije glavne kartice, svaka sadrži skripte i datoteke koje su potrebne za implementaciju davatelja MySQL resursa.

## <a name="prepare-prerequisites"></a>Priprema preduvjeti

Kliknite karticu **Priprema preduvjeti** za:

- Stvaranje potrebnih certifikata
- Preuzmite MySQL binarne datoteke na snop Azure
- Prijenos artefakte s računom za pohranu na hrpu Azure
- Objavljivanje galeriju stavki

### <a name="create-the-required-certificates"></a>Stvaranje potrebnih certifikata
Ova skripta **Novo SslCert.ps1** dodaje na \_. AzureStack.local.pfx SSL certifikata u D:\\MySQLRP\\preduvjeti\\BlobStorage\\spremnik mape. Potvrda secures komunikaciju između davatelja resursa i lokalnu instancu programa upravitelj resursa Azure.

1. Na kartici glavna **Priprema preduvjeti** kliknite karticu **Novo SslCert.ps1** , a zatim ga pokrenuti.

2. U naredbeni redak koji će se pojaviti upišite lozinku PFX koji štiti privatni ključ i **zabilježite lozinku**. Morat ćete ga kasnije.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>Preuzmite MySQL binarne datoteke na snop Azure

1. Odaberite karticu **Preuzimanje MySqlServer.ps1** , a zatim ga pokrenuti.
2. Kada se to od vas zatraži, kliknite **da** u dijaloškom okviru potvrda da biste prihvatili EULA.

    Ta se naredba dodaje dva zip datoteke u mapu D:\MySql\Prerequisites\BlobStorage\Container.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Prijenos svih artefakte s računom za pohranu na hrpu Azure

1. Kliknite karticu **Prijenos Microsoft.MySql RP.ps1** , a zatim ga pokrenuti.

2. U dijaloškom okviru Windows PowerShell vjerodajnica zahtjev upišite administratorske vjerodajnice servisa Azure stogu.

3. Kada se to od vas zatraži Azure Active Directory klijentu ID-a, upišite svoje Azure Active Directory klijentu potpuno kvalificirani naziv domene:, na primjer, microsoftazurestack.onmicrosoft.com.

    Skočni prozor traži vjerodajnice.

    > [AZURE.TIP] Ako se ne pojavi skočni prozor, koje ili niste isključili IE Poboljšana sigurnost da biste omogućili JavaScript na računalu i korisnika ili niste prihvatili kolačića u preglednika Internet Explorer. Potražite u članku [Postavljanje korake prije implementacije](#set-up-steps-before-you-deploy).

4. Upišite Azure stogu servisa administratorske vjerodajnice, a zatim kliknite **Prijava**.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Objavljivanje stavki galeriju za kasnije stvaranje resursa

Odaberite karticu **Objavljivanje GalleryPackages.ps1** , a zatim ga pokrenuti. Ova skripta dodaje dvije stavke trgovine portalu PNA snop Azure marketplace koje možete koristiti za implementaciju resurse za bazu podataka kao stavke trgovine.

## <a name="deploy-the-mysql-resource-provider-vm"></a>Implementacija davatelja MySQL resursa VM

Sad kad ste pripremili stoga PNA Azure s potrebne potvrde i stavkama trgovine, možete implementirati davatelj usluga SQL Server resursa. Kliknite karticu i **Implementacija MySQL davatelja** :

   - Unesite vrijednosti u JSON datoteku koja referencira postupak implementacije
   - Implementacija davatelja resursa
   - Ažuriranje lokalni DNS-a
   - Registrirajte se prilagodnik za davatelja resursa SQL Server

### <a name="provide-values-in-the-json-file"></a>Unesite vrijednosti u datoteci JSON

Kliknite **Microsoft.MySqlprovider.Parameters.JSON**. Datoteka sadrži parametre koje predloška Azure Voditelj resursa treba pravilno implementacija snop Azure.

1. Popunite **prazne** parametara u datoteci JSON:

    - Provjerite je li navedite **adminusername** i **adminpassword** za VM za davatelja MySQL resursa.

    - Provjerite je li za parametar **SetupPfxPassword** koje ste napravili obratite pozornost na korak [Priprema prequisites](#prepare-prerequisites) unesete lozinku.

    - Provjerite je li pružaju parametri **basicAuthUserName** i **basicAuthPassword** . **Zabilježite od ovih vrijednosti.** Morat ćete ih kasnije da biste se registrirali davatelja resursa.

2. Kliknite **Spremi**.

### <a name="deploy-the-resource-provider"></a>Implementacija davatelja resursa

1. Kliknite karticu **uvođenja Microsoft.Mysql provider.PS1** i pokrenuti skriptu.
2. U Azure Active Directory kada se to od vas zatraži, upišite svoje ime klijenta.
3. U skočnom prozoru unos administratorske vjerodajnice servisa Azure stogu.

Puni implementacije, može proći između 15 i 45 minuta na nekim vrlo koriste POCs Azure stogu.

### <a name="update-the-local-dns"></a>Ažuriranje lokalni DNS-a

1. Kliknite karticu **Register Microsoft.MySQL fqdn.ps1** i pokrenuti skriptu.
2. Upit za Azure Active Directory klijentu ID za unos vašeg Azure Active Directory klijentu potpuno kvalificirani naziv domene:, na primjer, **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>Registrirajte se davatelj usluga za SQL to resursa

1. Kliknite karticu **Register Microsoft.My provider.ps1** i pokrenuti skriptu.

2. Kad tražiti vjerodajnice, slijedite navedene kao parametar **basicAuthUserName** i **basicAuthPassword** .

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Provjera uvođenja pomoću portala za stog Azure

1. Odjava iz sustava ClientVM i ponovno se prijavite kao **AzureStack\User**.

2. Na radnoj površini, kliknite **Azure stogu PNA Portal** i prijavite se na portal kao administratora servisa.

3. Provjerite je li uspješno uvođenje. Kliknite **Pregledaj** &gt; **Grupe resursa**, kliknite grupu resursa koji ste koristili (Zadana vrijednost je **MySQLRP**), i provjerite je li to essentials dio plohu (gornjem dijelu) glasi **implementacije uspješno**.


4. Provjerite je li uspješno registracije. Kliknite **Pregledaj** &gt; **davatelji resursa**, a zatim potražite **MySQL lokalno**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Stvaranje prve baze podataka MySQL da biste testirali implementaciju sustava

1. Prijavite se na portal za Azure stogu PNA kao administrator servisa.

2. Kliknite na **+** gumb &gt; **Prilagođena** &gt; **& baze podataka MySQL poslužitelja**.

3. Ispunite obrazac Detalji o bazi podataka.

    **Zabilježite "naziv poslužitelja" unesete.** Niz veze za bazu podataka sadrži "naziv poslužitelja" kao dio korisničko ime:, na primjer, ** "user@ <ServerName>"**. Morat ćete kada se povežete s bazom podataka za unos korisničkog imena u ovom obliku:, na primjer, kada implementacija MySQL web-mjesta pomoću davatelja resursa Azure Web-mjesta


## <a name="next-steps"></a>Daljnji koraci

Pokušajte drugih [PaaS servise](azure-stack-tools-paas-services.md) kao što je [davatelj usluga za SQL Server resursa](azure-stack-sql-rp-deploy-short.md) i [davatelja resursa web-aplikacije](azure-stack-webapps-deploy.md).
