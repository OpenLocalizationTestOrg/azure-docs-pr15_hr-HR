<properties
   pageTitle="Primjer DMZ – Stvaranje jednostavne DMZ s NSGs | Microsoft Azure"
   description="Sastavljanje DMZ sa sigurnosnim grupama s mrežom (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-1--build-a-simple-dmz-with-nsgs"></a>Primjer 1 – Stvaranje jednostavne DMZ s NSGs

[Vratite se na stranicu najbolje prakse sigurnost granice][HOME]

U ovom se primjeru će stvoriti jednostavan DMZ s četiri poslužiteljima sustava windows i mreže sigurnosne grupe. Također će voditi kroz svaki odgovarajuću naredbi za dublju razumijevanja svakog koraka. Postoji i promet scenarij sekcije možete unijeti na detaljnije detaljne kako promet nastavlja se kroz slojeve obrane na DMZ. Na kraju, referencama na odjeljak je dovršena Šifra i upute za izradu ovo okruženje za testiranje i isprobajte ih s različitim scenarijima. 

![Dolazni DMZ s NSG][1]

## <a name="environment-description"></a>Opis okruženje
U ovom primjeru prikazuje se na pretplatu koja sadrži sljedeće:

- Dva servise u oblaku: "FrontEnd001" i "BackEnd001"
- Virtualne mreže "CorpNetwork" s dvije podmreže; "Sučelju" i "Pozadinskog"
- Mrežni sigurnosne grupe koje je primijenjeno na oba podmreže
- Windows Server koja predstavlja aplikacije web-poslužitelj ("IIS01")
- Dva prozora poslužitelje koji predstavljaju aplikacije natrag završili poslužitelje ("AppVM01", "AppVM02")
- Windows server koja predstavlja DNS poslužitelja ("DNS01")

U odjeljku reference ispod postoji skriptu PowerShell koje će izraditi većinu okruženje prethodno opisan. Stvaranje VMs i virtualne mreže, iako obavljaju skripte primjer ne opisana su u detalja u ovom dokumentu. 

Da biste sastavili okruženje;

  1.    Spremanje mreže config xml datoteku u odjeljku reference (ažurirati imena, mjesto i IP adrese tako da odgovaraju određenom scenarij)
  2.    Ažurirajte korisničke varijable u skripti tako da odgovara okruženje skripta će se pokrenuti (pretplate, nazivi servisa itd.)
  3.    Izvršavanje skriptu u ljusci PowerShell

**Napomena**: regija naznačeno u skriptu PowerShell mora odgovarati regija naznačeno u xml datoteci konfiguracije mreže.

Nakon pokretanja skripte uspješno dodatne dodatni koraci potrebni, u odjeljku reference dva skripte da biste postavili web-poslužitelj i poslužitelj aplikacije s jednostavni web-aplikacije da biste omogućili testiranjem tu konfiguraciju DMZ.

U sljedećim se odjeljcima će Unesite detaljan opis mreže sigurnosne grupe i kako funkcioniraju u ovom primjeru tako da prolaska kroz ključa retke skriptu PowerShell.

## <a name="network-security-groups-nsg"></a>Mrežni sigurnosnih grupa (NSG)
U ovom primjeru grupu NSG ugrađena i zatim učitan s šest pravila. 

>[AZURE.TIP] Općenito govoreći, najprije stvorite određene pravila "Dopusti" i više generički pravila "Nemoj dopustiti" zadnji. Dodijeljene prioritet određuje koje su pravila vrednuje prvi. Kada promet nalazi se da biste primijenili na određenom pravilu, vrednuju se daljnje pravila. Pravila NSG možete primijeniti na jedan u smjeru dolazne i odlazne (iz perspektive podmreži).

Deklarativno, sljedeća pravila koja se ugrađenih za dolazni promet:

1.  Dopuštena interne DNS promet (priključak 53)
2.  RDP promet (priključak 3389) s Interneta na bilo kojem VM dopuštena
3.  Dopuštena HTTP promet (priključak 80) s Interneta na web-poslužitelj (IIS01)
4.  Bilo koji promet (svi priključci) iz IIS01 AppVM1 dopuštena
5.  Bilo koji promet (svi priključci) s Interneta na cijeli VNet (obje podmreže) je zabranjen
6.  Sve promet (svi priključci) iz podmreže sučelju podmreže pozadinskog je zabranjen

Pomoću tih pravila vezana uz svaki podmreže ako HTTP zahtjev je ulaznog s Interneta na web-poslužitelj, i pravila 3 (Omogući) i 5 (Uskrati) primjenjivati, no Budući da se pravilo 3 ima viši prioritet samo ga želite primijeniti, a zatim pravilo 5 bi isporučuju u Reproduciraj. Stoga HTTP zahtjev želite omogućiti na web-poslužitelj. Ako taj isti promet je pokušaju pristupiti poslužitelju DNS01, pravilo 5 (Uskrati) bio prvi da biste primijenili i promet bi nije dopuštena za prosljeđivanje na poslužitelj. Pravilo 6 (Uskrati) blokira podmreže Frontend iz razgovor podmreže pozadinskog (osim dopuštene promet u pravilima 1 i 4), to štiti mreže pozadinskog u slučaju da je napadač kompromisa web-aplikaciju sučelju, napadač će je ograničen pristup s pozadinskom "zaštićeni" mrežom (samo za resurse koji se prikazuje na poslužitelju AppVM01).

Postoji zadani izlazni pravilo koje dopušta promet odgovor s Internetom. U ovom primjeru smo si omogućivanja odlazni promet i ne izmjena izlaznog pravila. Da biste zaključali dolje promet u oba smjera, korisnički definirane usmjeravanje potreban je, to je istražili u "Primjer 3" ispod.

Svako pravilo govori detaljno na sljedeći način (**bilješke**: bilo koju stavku u ispod popisa u počevši od znak za dolar (npr.: $NSGName) je korisnički definirane varijable iz skripte u odjeljku referenca ovog dokumenta):

1. Najprije sigurnosne grupe mreže moraju se napisati na čuvanje pravila:

        New-AzureNetworkSecurityGroup -Name $NSGName `
            -Location $DeploymentLocation `
            -Label "Security group for $VNetName subnets in $DeploymentLocation"
 
2.  Prvo pravilo u ovom primjeru će dopušta promet DNS između sve mreže interne DNS poslužitelju na podmreži pozadinskog. Pravilo ima nekoliko važnih parametara:
  - "Upišite" označava u kojem ga smjeru prometa to pravilo primjenjuje se; Ovo je iz perspektive podmreži ili virtualnog računala (ovisno o tome gdje je vezana ovaj NSG). Stoga ako je vrsta "Ulazni" i promet unose podmreži, pravilo primjenjivati i promet ostavite podmreži bi neće utjecati na ovo pravilo.
  - "Prioritet" postavlja redoslijed kojim će se računati tijek prometa. Na donjoj broj veći prioritet. Čim se pravilo primjenjuje tijek određene prometa, daljnje pravila se obrađuju. Stoga ako pravilo prioritet 1 dopušta promet, i pravila s prioritetom 2 onemogućava promet, i obje se pravila primjenjuju se na promet, a zatim promet želite omogućiti tijeka (jer pravilo 1 imala veći prioritet potrebno efekt i pravila za daljnje zatvorene).
  - "Akcije" označava promet utječe ovo pravilo blokirane ili dopuštene.

            Get-AzureNetworkSecurityGroup -Name $NSGName | `
                Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
                -Type Inbound -Priority 100 -Action Allow `
                -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
                -DestinationAddressPrefix $VMIP[4] `
                -DestinationPortRange '53' `
                -Protocol *

3.  Tim se pravilom omogućuje RDP promet da teče s Interneta priključak RDP na bilo kojem poslužitelju na neki podmreži u na VNET. Tim se pravilom koristi dvije posebne vrste prefiksi adresa; "VIRTUAL_NETWORK" i "INTERNET". To je jednostavan način adresa kategorije veće prefiksi adresu.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
            -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '3389' `
            -Protocol *

4.  Ovo pravilo dopušta unutarnje internetski promet pogoditi iz prvog web-poslužitelj. To ne mijenja ponašanje usmjeravanje; samo se omogućuje promet namijenjene prikazivanju na IIS01 za prosljeđivanje. Stoga ako promet s Interneta imali na web-poslužitelju kao odredište ovo pravilo će ga omogućite i Zaustavi obradu dodatno pravila. (U pravilu pri prioritet 140 sve druge ulaznog internetski promet blokiran). Ako koristite samo obradu HTTP promet, to pravilo nije dodatno ograničiti Dopusti samo odredišni priključak 80.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
            -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] `
            -DestinationPortRange '*' `
            -Protocol *

5.  Tim se pravilom dopušta promet za prosljeđivanje s poslužitelja IIS01 AppVM01 poslužitelja i noviji blokovi pravila sve sučelju da biste promet pozadinskog. Da biste poboljšali ovo pravilo ako priključak je poznato koje treba dodati. Na primjer, ako IIS poslužitelj je odlazak samo SQL Server na AppVM01, odredišni priključak raspon treba promijeniti iz "*" (sve) da biste tako što omogućuje manje plošni ulaznog napada na AppVM01 treba web-aplikaciji ikada biti ugrožena 1433 (SQL priključak).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
            -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
 
6.  Tim se pravilom onemogućava promet s Interneta na sve poslužitelje na mreži. U kombinaciji s pravilom pri prioritet 110 i 120 omogućuje samo ulaznog internetski promet za vatrozid i priključke RDP druge poslužitelje i blokira sve ostalo. 

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $VNetName VNet from the Internet" `
            -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '*' `
            -Protocol *
 
7.  Konačni pravilo onemogućava promet iz podmreže sučelju podmreže pozadinskog. Budući da je to pravilo ulazne samo, obrnutom promet je dopušteno (s pozadinskom u sučelju).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
            -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix `
            -DestinationPortRange '*' `
            -Protocol * 

## <a name="traffic-scenarios"></a>Scenariji promet
#### <a name="allowed-web-to-web-server"></a>(*Dopušteni*) Web na web-poslužitelj
1.  Internetska korisničkog zahtjeva za HTTP stranica iz FrontEnd001.CloudApp.Net (Internet nasuprotne servis u Oblaku)
2.  Cloud servisa prolaze prometa Otvori krajnjoj točki priključak 80 pri IIS01 (web-poslužitelj)
3.  Frontend podmreže započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (DNS) ne primijenite, premještanje na sljedeće pravilo
  2.    NSG pravilo 2 (RDP) ne primijenite, premještanje na sljedeće pravilo
  3.    Primjena NSG pravilo 3 (Internet da biste IIS01), promet je dopušteno, zaustavite pravilo obrada
4.  Promet dodirne internu IP adresu web-poslužitelja IIS01 (10.0.1.5)
5.  IIS01 priključuje za promet web, primi zahtjev i pokreće obrade zahtjeva
6.  IIS01 traže SQL Server na AppVM01 podatke
7.  Nema izlaznog pravila na sučelju podmreži promet je dopušteno
8.  Podmreže pozadinskog započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (DNS) ne primijenite, premještanje na sljedeće pravilo
  2.    NSG pravilo 2 (RDP) ne primijenite, premještanje na sljedeće pravilo
  3.    NSG pravilo 3 (Internet na vatrozid) ne primijenite, premještanje na sljedeće pravilo
  4.    4 NSG pravilo Primijeni (IIS01 za AppVM01), promet je dopušteno, Zaustavi obradu pravila
9.  AppVM01 prima SQL upita i odgovori
10. Jer nema izlaznog pravila na podmreži pozadinskog je dopušteno odgovora
11. Frontend podmreže započinje obrada ulazna pravila:
  1.    Postoji nijedno pravilo NSG koji se primjenjuje na ulazni promet s pozadinskom podmreži u sučelju podmreži tako da ništa na NSG pravila primjenjuju
  2.    Dopuštanje promet između podmreže zadano pravilo sustava omogućuje tog prometa tako da je dopušteno promet.
12. Prima odgovor SQL IIS server dovršava HTTP odgovor i šalje podnositelja zahtjeva
13. Jer se dopuštena izlaznog pravila na podmreži sučelju odgovor, a Internet dobiva web-stranice koji ste tražili.

#### <a name="allowed-rdp-to-backend"></a>(*Dopušteni*) RDP da biste pozadinskog
1.  Administrator poslužitelja Interneta zahtjeve RDP sesiju AppVM01 na BackEnd001.CloudApp.Net:xxxxx pri čemu je xxxxx broj slučajno dodijeljene priključak za RDP za AppVM01 (dodijeljene priključak nalazi se na Portal za Azure ili PowerShell)
2.  Podmreže pozadinskog započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (DNS) ne primijenite, premještanje na sljedeće pravilo
  2.    Primjena NSG pravilo 2 (RDP), promet je dopušteno, zaustavite pravilo obrada
3.  Bez izlaznog pravila primjenjuju zadane pravila i povrata promet je dopušteno
4.  Sesije RDP omogućena
5.  AppVM01 traži korisničko ime lozinka

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(*Dopušteni*) Web-poslužitelj DNS pretraživanje na DNS poslužitelj
1.  Web-poslužitelj, IIS01, potrebama podatkovnog sažetka sadržaja na www.data.gov, ali treba riješiti adresu.
2.  Na mrežnoj konfiguraciji za popise VNet DNS01 (10.0.2.4 na podmreži pozadinskog) kao primarni DNS poslužitelj, IIS01 šalje zahtjev DNS DNS01
3.  Nema izlaznog pravila na sučelju podmreži promet je dopušteno
4.  Podmreže pozadinskog započinje obrada ulazna pravila:
  1.     Primjena NSG pravilo 1 (DNS), promet je dopušteno, zaustavite pravilo obrada
5.  DNS poslužitelj primi zahtjev
6.  DNS poslužitelj ne sadrži adresu predmemorirani i pita korijenski DNS poslužitelj na Internetu
7.  Nema izlaznog pravila na podmreži pozadinskog promet je dopušteno
8.  Internet DNS poslužitelj odgovori, jer interno pokrenuo ovu sesiju, odgovor je dopušteno
9.  DNS poslužitelj predmemorira odgovor i odgovori na zahtjev za početne natrag na IIS01
10. Nema izlaznog pravila na podmreži pozadinskog promet je dopušteno
11. Frontend podmreže započinje obrada ulazna pravila:
  1.    Postoji nijedno pravilo NSG koji se primjenjuje na ulazni promet s pozadinskom podmreži u sučelju podmreži tako da ništa na NSG pravila primjenjuju
  2.    Dopuštanje promet između podmreže zadano pravilo sustava omogućuje tog prometa tako da je dopušteno promet
12. IIS01 prima odgovor iz DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Dopušteni*) Web-poslužitelj datoteka programa access na AppVM01
1.  IIS01 Traži datoteke na AppVM01
2.  Nema izlaznog pravila na sučelju podmreži promet je dopušteno
3.  Podmreže pozadinskog započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (DNS) ne primijenite, premještanje na sljedeće pravilo
  2.    NSG pravilo 2 (RDP) ne primijenite, premještanje na sljedeće pravilo
  3.    NSG pravilo 3 (Internet da biste IIS01) ne primijenite, premještanje na sljedeće pravilo
  4.    4 NSG pravilo Primijeni (IIS01 za AppVM01), promet je dopušteno, Zaustavi obradu pravila
4.  AppVM01 primi zahtjev i Odgovori s datotekom (Ako je dozvolu pristupa)
5.  Budući da nema izlaznog pravila na podmreži pozadinskog odgovor dopuštena
6.  Frontend podmreže započinje obrada ulazna pravila:
  1.    Postoji nijedno pravilo NSG koji se primjenjuje na ulazni promet s pozadinskom podmreži u sučelju podmreži tako da ništa na NSG pravila primjenjuju
  2.    Dopuštanje promet između podmreže zadano pravilo sustava omogućuje tog prometa tako da je dopušteno promet.
7.  Poslužitelj za IIS primi datoteku

#### <a name="denied-web-to-backend-server"></a>(*Zabranjen*) Internet da biste pozadinskog poslužitelja
1.  Internet korisnik pokuša za pristup datoteci na AppVM01 putem usluge BackEnd001.CloudApp.Net
2.  Budući da su otvorene za zajedničko korištenje datoteka krajnje točke, to bi proći servisa u Oblaku i ne može pristupiti poslužitelju
3.  Ako krajnjih točaka su bile otvorene zbog nekog razloga, NSG pravilo 5 (Internet da biste VNet) želite blokirati tog prometa

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(*Zabranjen*) Traženje DNS web na DNS poslužitelj
1.  Internet korisnik pokuša potražiti interne DNS zapis na DNS01 putem usluge BackEnd001.CloudApp.Net
2.  Budući da su otvorene za DNS krajnje točke, to bi proći servisa u Oblaku i ne može pristupiti poslužitelju
3.  Ako krajnjih točaka su bile otvorene zbog nekog razloga, NSG pravilo 5 (Internet da biste VNet) želite blokirati tog prometa (Napomena: taj pravilo 1 (DNS) za bi Primjena dva razloga, najprije adresu izvorne s Internetom, to pravilo primjenjuje se samo na lokalni VNet kao izvor, i to je pravilo Dopusti tako da nikada ne bi odbiti promet)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Zabranjen*) Web Access SQL kroz vatrozid
1.  Internet korisnik zatraži SQL podatke iz FrontEnd001.CloudApp.Net (Internet nasuprotne servis u Oblaku)
2.  Budući da su otvorene za SQL krajnje točke, to bi proći servisa u Oblaku i ne može doći do vatrozid
3.  Ako krajnje točke su bile otvorene zbog nekog razloga, podmreže sučelju započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (DNS) ne primijenite, premještanje na sljedeće pravilo
  2.    NSG pravilo 2 (RDP) ne primijenite, premještanje na sljedeće pravilo
  3.    Primjena NSG pravilo 3 (Internet da biste IIS01), promet je dopušteno, zaustavite pravilo obrada
4.  Promet dodirne interne IP adresu na IIS01 (10.0.1.5)
5.  IIS01 nije slušanje na priključak 1433, tako da nema odgovor na zahtjev

## <a name="conclusion"></a>Zaključak
To je razmjerno jednostavne i jednostavan način izoliranja podmreže pozadinskih iz ulaznog promet.

Dodatne primjere i pregled mreže sigurnosnih ograničenja nalazi se [ovdje][HOME].

## <a name="references"></a>Reference
### <a name="main-script-and-network-config"></a>Glavni skripte i konfiguracija mreže
Spremite cijelog skripte u datoteci skriptu PowerShell. Spremite Config mreže u datoteku pod nazivom "NetworkConf1.xml".
Korisnički definirane varijable izmijeniti prema potrebi. Pokrenuti skriptu, a zatim pratite upute postavljanje pravila vatrozida koje se nalaze u gornjem dijelu primjera 1.

#### <a name="full-script"></a>Potpuna skripte
Ova skripta će, ovisno o korisnički definirane varijable;

1.  Povezivanje s Azure pretplate
2.  Stvaranje novog računa za pohranu
3.  Stvaranje nove VNet i dva podmreže kako je definirano u mreži konfiguracijskoj datoteci
4.  Sastavljanje 4 windows server VMs
5.  Konfiguriranje NSG uključujući:
  - Stvaranje na NSG
  - Popunjavanje pomoću pravila
  - Povezivanje s NSG odgovarajuće podmreže

Ovu skriptu PowerShell mora izvesti na lokalno povezani s Internetom PC-JU ili poslužitelju.

>[AZURE.IMPORTANT] Kada se Ova skripta, možda postoje upozorenja ili drugih pružaju koji pop u ljusci PowerShell. Samo poruke o pogreškama u crvenoj boji su uzrok problem.


    <# 
     .SYNOPSIS
      Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.5
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
      
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 0
        #       - The AppVM1 Server must be VM 1
        #       - The DNS server must be VM 3
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 1 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 2 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 3 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #   

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                Remove-AzureEndpoint -Name "PowerShell" | `
                New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Note: A Remote Desktop endpoint is automatically created when each VM is created.
            $i++
        }
        # Add HTTP Endpoint for IIS01
        Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
      

#### <a name="network-config-file"></a>Konfiguracijska datoteka u mreži
Spremanje xml datoteke s ažuriranim mjesto, a zatim dodati vezu na tu datoteku da biste varijabla $NetworkConfigFile u skripti iznad.
    
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Ogledne skripte aplikacije
Ako želite instalirati aplikaciju uzorka za ovu i još primjera DMZ nešto je dodijeljen pomoću sljedeće veze: [Skriptu aplikaciju uzorka][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Dolazni DMZ s NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

