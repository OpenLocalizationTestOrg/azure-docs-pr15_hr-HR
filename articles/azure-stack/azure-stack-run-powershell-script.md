<properties
    pageTitle="Implementacija Azure stogu PNA | Microsoft Azure"
    description="Saznajte kako pripremiti PNA snop Azure i pokrenuti skriptu PowerShell za implementaciju PNA snop Azure."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Implementacija Azure stogu PNA
Da biste implementirali PNA snop Azure, najprije morate [Priprema računala za implementaciju](#prepare-the-deployment-machine) , a zatim [pokrenite implementacijsku skriptu PowerShell](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Preuzimanje i izdvajanje Microsoft Azure stogu PNA TP2

Prije nego što počnete, provjerite je li vam barem 85 GB prostora.

1. Preuzimanje Azure stogu PNA TP2 sastoji se od zip datoteku koja sadrži sljedeće 12 datoteke, Zbrajanje ~ 20 GB:
    - 1 MicrosoftAzureStackPOC.EXE
2. Pregledajte zaslon licencni ugovor i informacije o čarobnjaku za samoizdvajanje, a zatim kliknite **Dalje**.
3. Pregledajte zaslon Izjava o zaštiti privatnosti i informacije o čarobnjaku za samoizdvajanje, a zatim kliknite **Dalje**.
4. Odaberite odredište za datoteke da biste se izdvajati, kliknite **Dalje**.
    - Zadana vrijednost je: <drive letter>:\<trenutnu mapu > \Microsoft PNA snop Azure
5. Pregledajte zaslon mjesto odredište i informacije o čarobnjaku za samoizdvajanje, a zatim kliknite **Izdvoji** da biste izdvojili CloudBuilder.vhdx (~44.5 GB) i ThirdPartyLicenses.rtf datoteke.

> [AZURE.NOTE] Nakon izdvojiti datoteke, možete izbrisati zip datoteku da biste oslobodili prostor na računalu. Ili možete premjestiti zip datoteku na drugo mjesto tako da, ako morate ponovno implementirate koje ne morate ponovno preuzeti zip datoteke.

## <a name="prepare-the-deployment-machine"></a>Priprema računala za implementaciju

1. Provjerite da možete fizički povezati s računalom za implementaciju ili imaju pristup fizičkoj konzoli (primjerice KVM). Nakon što ponovno pokrenete računalo implementacije u koraku 9 trebat će vam taj pristup.

2. Provjerite je li računalo implementacije ispunjava [minimalni preduvjeti](azure-stack-deploy.md). [Provjera implementacije za Azure stogu Tehnički pretpregled 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) možete koristiti da biste potvrdili svojim potrebama.

3. Prijavite se kao Administrator lokalne na vaše računalo PNA.

4. Kopirajte datoteku CloudBuilder.vhdx C:\CloudBuilder.vhdx.

    > [AZURE.NOTE] Ako odlučite da ne želite koristiti preporučena skriptu da biste pripremili PNA glavnom računalu (korake 5 – korak 7), unesite sve licencni ključ na stranice za aktivaciju. Probnu verziju sustava Windows Server 2016 slika uključen i unos ključa licence uzrokuje isteka poruke upozorenja.

5. Na računalu PNA pokrenite sljedeću skriptu komponente PowerShell da biste preuzeli datoteke za podršku Azure stogu TP2:

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    Ova skripta preuzima Azure stogu TP2 datoteke za podršku na navedenu parametrom $LocalPath mapu.
    
6. Otvorite povećane konzole PowerShell i promijeniti direktorij u koju ste kopirali datoteke.
    - 11 MicrosoftAzureStackPOC-N.BIN (N je 1-11)
7. Desnom tipkom miša kliknite na MicrosoftAzureStackPOC.EXE > Pokreni kao administrator.

8. Pokrenuti skriptu PrepareBootFromVHD.ps1. Ova skripta i datoteke nenadziranu dostupni su s drugim podršku skripte dobiveni uz ovaj Sastavi.
    Postoji pet parametara za ovu skriptu PowerShell:
    - CloudBuilderDiskPath (obavezno) – put do CloudBuilder.vhdx na glavnom RAČUNALU.
    - (Neobavezno) – DriverPath omogućuje dodavanje dodatne upravljačke programe za glavno računalo u virtualne HD.
    - (Neobavezno) – ApplyUnattend navedite taj parametar promjenu da biste automatizirali konfiguriranje operacijskih sustava. Ako navedeni, korisnik mora pružati uslugu AdminPassword konfiguriranje OS-a na pokretanje (zahtijeva pod uvjetom da popratnim datoteke unattend_NoKVM.xml).
    Ako ne koristite taj parametar, generički unattend.xml datoteka koristi se bez dodatne prilagodbe. Morat ćete KVM dovršeno prilagodbe kada je izvršen.
    - AdminPassword (neobavezno) – koristiti samo ako je parametar ApplyUnattend postavljen, potreban je najmanje šest znakova.
    - VHDLanguage (neobavezno) – određuje jezik VHD postaviti "HR-HR".
    Skripta navedenih i sadrži primjer upotrebe, iako najčešće korištenje:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Ako pokrenete tu naredbu točno, morate unijeti AdminPassword naredbenom.

9. Po dovršetku skriptu morate potvrditi na ponovno pokrenite računalo. Ako su drugi korisnici prijavljeni, ta se naredba neće uspjeti. Ako se naredba ne uspije, pokrenite sljedeću naredbu:`Restart-Computer -force` 

10. Glavno računalo izvršen u OS CloudBuilder.vhdx, gdje se implementacije.

> [AZURE.IMPORTANT] Azure stogu potreban je pristup Internetu, izravno ili putem prozirne proxy poslužitelja. Uvođenje TP2 PNA podržava točno jedan NIC za povezivanje s mrežom. Ako imate više NIC-ovi, provjerite je li omogućena samo jedan (i sve ostale su onemogućene) prije pokretanja implementacijsku skriptu u sljedećem odjeljku.

## <a name="run-the-powershell-deployment-script"></a>Pokrenite implementacijsku skriptu PowerShell

1. Prijavite se kao Administrator lokalne na vaše računalo PNA. Koristite vjerodajnice navedene u prethodnim koracima.

2. Otvorite povećane konzole za PowerShell.

3. U komponente PowerShell pokrenite sljedeću naredbu:`cd C:\CloudDeployment\Configuration`

4. Pokretanje naredbe uvođenja:`.\InstallAzureStackPOC.ps1`

5. Kada se zatraži **Enter lozinku** , unesite lozinku, a zatim je potvrdite. To je lozinka za sve virtualnim računalima. Obavezno zabilježite ga.

6. Unesite vjerodajnice za vaš račun za Azure Active Directory. Taj korisnik mora biti globalni administrator u klijentu direktorija.

7. Postupak implementacije može potrajati nekoliko sati, tijekom kojeg sustav automatski izvršen jedanput.

    > [AZURE.IMPORTANT] Ako želite pratiti napredak implementaciju, prijavite se kao azurestack\AzureStackAdmin. Ako se prijaviti kao administrator sustava lokalne kada je računalo priključeno na domenu, nećete vidjeti tijeku implementacije. Ponovno pokrenite implementaciju, umjesto toga prijavite se kao azurestack\AzureStackAdmin da biste provjerili je li pokrenut.

    Nakon uspješnog implementacijskih prikazuje konzole za PowerShell: **DOVRŠENO: Akcija 'Implementacije'**.

    Uvođenje ne uspije, pokušajte [ponovno ga pokrenite](azure-stack-rerun-deploy.md). Ili možete [implementirati](azure-stack-redeploy.md) je od nule.

### <a name="deployment-script-examples"></a>Primjeri implementacije skriptnog jezika

Ako je vaš identitet AAD samo povezan s JEDNOG AAD direktorija:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Ako je vaš identitet AAD povezana s VEĆI od JEDNOG AAD direktorija:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Ako vaše okruženje nema DHCP je omogućen, morate uključiti sljedećih parametara dodatne mogućnosti iznad (primjer korištenje dobili):

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>Neobavezni parametri InstallAzureStackPOC.ps1

| Parametar | Obavezno/neobavezno | Opis |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Neobavezno | Postavlja Azure Active Directory korisničko ime i lozinku. Vjerodajnice za Azure može biti ID tvrtke ili ustanove ili Microsoftov Account. Da biste koristili Microsoftov Account vjerodajnice, preskočite ovaj parametar u cmdlet. Ispuštanje taj parametar traži provjeru autentičnosti Azure skočnog tijekom implementacije. Time se stvara tokeni provjerom autentičnosti i osvježavanje koristi tijekom implementacije. |
| AADDirectoryTenantName | Obavezno | Postavlja direktorija klijenta. Taj parametar koristite da biste odredili određene direktorij gdje račun AAD ima dozvole za upravljanje više direktorija. Puni naziv klijent direktorija AAD u obliku <directoryName>. onmicrosoft.com. |
| AdminPassword | Obavezno | Postavlja lokalni administratorski račun i sve druge korisničke račune na svim računalima virtualne stvoreni kao dio PNA implementacije. Ovu lozinku moraju se podudarati trenutnu lozinku lokalnog administratora na glavnom računalu. |
| AzureEnvironment | Neobavezno | Odaberite okruženje Azure s kojom želite registrirati ovaj implementacije Azure stogu. Mogućnosti obuhvaćaju *Javno Azure*, *Azure - Kina* *Azure - vlada SAD-a*. |
| EnvironmentDNS | Neobavezno | DNS poslužitelj se stvara u sklopu Azure stogu implementacije. Da biste omogućili računala unutar rješenja za razrješavanje imena izvan pečata, navedite postojeće infrastrukture DNS poslužitelj. DNS poslužitelja u oznaku prosljeđuje Nepoznato naziv razlučivost zahtjevi za ovaj poslužitelj. |
| NatIPv4Address | Potrebne za DHCP NAT podrška | Postavlja statičke IP adrese za MAS BGPNAT01. Ovaj parametar koristiti samo ako je na DHCP ne možete dodijeliti valjana IP adresa za pristup Internetu. |
| NatIPv4DefaultGateway | Potrebne za DHCP NAT podrška | Postavlja zadani pristupnik koji se koristi s statičke IP adrese za MAS BGPNAT01. Ovaj parametar koristiti samo ako je na DHCP ne možete dodijeliti valjana IP adresa za pristup Internetu.  |
| NatIPv4Subnet | Potrebne za DHCP NAT podrška | IP podmreže prefiks koji se koristi za DHCP putem podrška za NAT. Ovaj parametar koristiti samo ako je na DHCP ne možete dodijeliti valjana IP adresa za pristup Internetu.  |
| PublicVLan | Neobavezno | Postavlja VLAN ID. Ovaj parametar koristiti samo ako glavno računalo i MAS BGPNAT01 morate konfigurirati VLAN ID-a da biste pristupili fizičke mreže (i Internet). Na primjer,`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Ponovno pokrenite | Neobavezno | Pomoću ovog zastavice implementaciju, ponovno pokrenite.  Koristi se sve prethodne unos. Ponovno unos podataka koje ste prethodno naveli nije podržana jer nekoliko jedinstvene vrijednosti su generirani i koristiti radi implementacije. |
| TimeServer | Neobavezno | Taj parametar koristite ako morate navesti poslužitelj za određeno vrijeme. |

## <a name="next-steps"></a>Daljnji koraci

[Povezivanje s Azure stogu](azure-stack-connect-azure-stack.md)
