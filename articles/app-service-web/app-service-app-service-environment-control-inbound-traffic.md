<properties 
    pageTitle="Kako odrediti dolazni promet prema okruženju aplikacije servisa" 
    description="Informirajte se o konfiguriranju pravila sigurnosti mreže da biste odredili dolazni promet prema okruženju aplikacije servisa." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Kako odrediti dolazni promet prema okruženju aplikacije servisa

## <a name="overview"></a>Pregled ##
Servis okruženju aplikacije mogu se kreirati u **svakom** Voditelj resursa Azure virtualne mreže, **ili** klasični implementaciju modela [virtualne mreže][virtualnetwork].  Novi virtualne mreže i novi podmreže može se definirati u vrijeme stvaranja okruženja aplikacije servisa.  Umjesto toga okruženju servisa aplikacija možete stvoriti u postojećih virtualne mreže i postojećih podmreže.  S nedavnih promjena u lipnja 2016, ASEs sada može uvesti u virtualne mreže koje koriste rasponi javno adresa ili razmake RFC1918 adresa (odnosno Privatna adresa).  Dodatne informacije o stvaranju okruženju servisa aplikacija potražite u članku [upute za stvaranje okruženja aplikacije servisa][HowToCreateAnAppServiceEnvironment].

Servis okruženju aplikacije uvijek moraju se stvoriti unutar podmreži jer podmreži granicu mreže koji se može koristiti za zaključavanje dolazni promet iza upstream uređajima i servisima tako da se HTTP i HTTPS promet samo prihvatili s određenim upstream IP adresa.

Ulazni i izlazni mrežni promet na podmreži kontrolira korištenje [mreže sigurnosne grupe][NetworkSecurityGroups]. Kontroliranje dolazni promet zahtijeva stvaranje pravila za sigurnost mreže u sigurnosnoj grupi mreže, a potom dodjelom mrežne sigurnosti grupiranje podmreže koja sadrži okruženje za aplikaciju servisa.

Kada sigurnosne grupe mreže je dodijeljen podmreži, dolazni promet aplikacije u okruženju servisa aplikacija je dopušteno blokiranih ovisno o dozvoli i uskratiti pravila definiranih u sigurnosnoj grupi mreže.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>Mrežni priključci koji se koristi u okruženju aplikacije servisa ##
Prije nego što zaključate dolje ulazni mrežni promet s mreže sigurnosne grupe, je važno je znati skup obaveznih i neobaveznih mrežni priključci koji se koriste okruženju aplikacije servisa.  Slučajno zatvaranja isključivanje promet na neki priključke može uzrokovati gubitak funkcionalnosti u okruženju aplikacije servisa.

Slijedi popis priključaka koristi okruženju aplikacije servisa. Svi priključci su **TCP**, osim ako jasno drugačije:

- 454: **potreban priključak** koristi Azure infrastrukture za Upravljanje projektom i održavanje aplikacije servisa okruženja putem SSL.  Blokiraj promet s tog priključka.  Ovaj priključak uvijek povezane s javnim VIP od programa elika i mala slova.
- 455: **potreban priključak** koristi Azure infrastrukture za Upravljanje projektom i održavanje aplikacije servisa okruženja putem SSL.  Blokiraj promet s tog priključka.  Ovaj priključak uvijek povezane s javnim VIP od programa elika i mala slova.
- 80: zadani je priključak za dolazni promet HTTP aplikacije koji se izvodi u aplikaciju servisa tarife u okruženju aplikacije servisa.  Na je omogućen za ILB elika i mala slova, priključak je povezana s ILB adresu na elika i mala slova.
- 443: zadani priključak za dolazni promet SSL aplikacije koji se izvodi u aplikaciju servisa tarife u okruženju aplikacije servisa.  Na je omogućen za ILB elika i mala slova, priključak je povezana s ILB adresu na elika i mala slova.
- 21: kontrola kanal za FTP.  Ovaj priključak možete sigurno blokirane ako FTP ne koristi.  Na je omogućen za ILB elika i mala slova, priključak može biti povezan sa adresu ILB za programa elika i mala slova.
- 990: kontrola kanal za FTPS.  Ovaj priključak možete sigurno blokirane ako FTPS ne koristi.  Na je omogućen za ILB elika i mala slova, priključak može biti povezan sa adresu ILB za programa elika i mala slova.
- 10001 10020: kanala podataka za FTP.  Kao i kod kanala kontrola priključke možete sigurno blokirane ako FTP ne koristi.  Na je omogućen za ILB elika i mala slova, priključak može povezati s elika i mala slova, ILB adresu.
- 4016: koristi za ispravljanje pogrešaka remote s Visual Studio 2012.  Ovaj priključak možete sigurno blokirane Ako značajku ne koristi.  Na je omogućen za ILB elika i mala slova, priključak je povezana s ILB adresu na elika i mala slova.
- 4018: koristi za ispravljanje pogrešaka udaljenom s Visual Studio 2013.  Ovaj priključak možete sigurno blokirane Ako značajku ne koristi.  Na je omogućen za ILB elika i mala slova, priključak je povezana s ILB adresu na elika i mala slova.
- 4020: koristi za ispravljanje pogrešaka remote s Visual Studio 2015.  Ovaj priključak možete sigurno blokirane Ako značajku ne koristi.  Na je omogućen za ILB elika i mala slova, priključak je povezana s ILB adresu na elika i mala slova.

## <a name="outbound-connectivity-and-dns-requirements"></a>Izlazni povezivanje i DNS preduvjeti ##
Za servis okruženju aplikacije ispravan zahtijeva izlaznog pristup različite krajnje točke. U odjeljku "Potrebna veza s mrežom" u članku [Konfiguracija mreže za ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) je se cijeli popis vanjskih krajnje točke koristi se elika i mala slova.

Okruženja aplikacije servisa za potreban je valjan DNS infrastrukture konfigurirana za virtualne mreže.  Ako iz bilo kojeg razloga DNS konfiguraciju promijeniti nakon stvaranja okruženja aplikacije servisa, razvojni inženjeri možete pokrenuti aplikaciju servisa okruženje za obraditi Nova konfiguracija DNS-a.  Pokretanje rolling ponovno okruženje pomoću ikone "Ponovno pokrenite" koja se nalazi pri vrhu plohu upravljanje okruženja aplikacije servisa [Azure portal] [ NewPortal] uzrokovat će okruženje za obraditi Nova konfiguracija DNS-a.

Također preporučuje se da se sve prilagođene DNS poslužitelji na na vnet postavljanje na vrijeme prije stvaranja okruženja aplikacije servisa.  Ako virtualne mreže DNS konfiguraciju Promijeni tijekom stvaranja okruženja aplikacije servisa, koji će rezultirati neuspjeh za postupak stvaranja okruženja aplikacije servisa.  U slične vein, ako postoji prilagođeni DNS poslužitelj na drugoj strani VPN pristupnika i DNS poslužitelj nije dostupan ili nije dostupan, postupak stvaranja okruženja aplikacije servisa također neće uspjeti.

## <a name="creating-a-network-security-group"></a>Stvaranje sigurnosne grupe mreže ##
Sve detalje na kako rade skupine mrežne sigurnosti potražite sljedeće [informacije][NetworkSecurityGroups].  Detalji o ispod za dodir na ističe mreže sigurnosnih grupa s naglaskom na konfiguriranje i Primjena sigurnosne grupe mreže na podmreži koja sadrži okruženju aplikacije servisa.

**Bilješke:** Mrežni sigurnosne grupe mogu se konfigurirati grafički pomoću [Portala za Azure](https://portal.azure.com) ili putem Azure PowerShell.

Sigurnosnim grupama s mreže najprije se stvaraju kao samostalni entitet povezan s pretplatom. Budući da mreže sigurnosne grupe stvaraju se u Azure regiji, provjerite je li sigurnosne grupe mreže nastaje u području isti kao okruženje za aplikaciju servisa.

Sljedeće prikazuje stvaranje sigurnosne grupe mreže:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Nakon stvaranja sigurnosne grupe mreže jedno ili više pravila sigurnosti mreže dodavanja.  Budući da se skup pravila mijenjaju tijekom vremena, preporučuje se razmak out shemi numeriranja koristi za Prioriteti pravila da biste umetnuli dodatna pravila tijekom vremena.

U sljedećem primjeru pokazuje pravilo koje je izričito dopušta pristup priključke za upravljanje potrebne za Azure infrastrukture za upravljanje i imaju okruženja aplikacije servisa.  Imajte na umu da sve upravljanje promet teče putem SSL i klijentskih potvrda osigurani tako da budu čak i ako se otvaraju priključke ne može pristupiti tako da svaki entitet osim Azure upravljanja infrastrukture.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Prilikom zaključavanja dolje pristup priključak 80 i 443 "skrivanje" okruženju aplikacije servisa iza upstream uređaja ili usluge, morat ćete znati upstream IP adresa.  Ako, na primjer, ako koristite vatrozid web aplikacije (WAF), u WAF će imati vlastitu IP adrese (ili adrese) koji koristi kada promet pohranjen na okruženje za servis do aplikacije.  Morat ćete koristiti tu IP adresu u parametru *SourceAddressPrefix* pravila sigurnosti mreže.

U primjeru u nastavku, dolazni promet s određenim upstream IP adresa izričito dopušteno.  Adresa *1.2.3.4* koristi se kao rezervirano mjesto za IP adresu upstream WAF.  Promijenite vrijednost koja odgovara adresi koristi upstream uređaja ili servis.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
Po želji FTP podršku sljedeća pravila mogu se kao predložak dopustiti pristup FTP kontrola priključak i podataka priključke za kanal.  Jer je FTP s praćenjem stanja protokol, možda neće moći FTP promet usmjerili pomoću klasične HTTP/HTTPS vatrozida ili proxy uređaja.  U ovom slučaju morate postaviti *SourceAddressPrefix* na neku drugu vrijednost – na primjer IP adresa raspon razvojni inženjer ili implementacije računala na koje FTP su pokrenuti klijenti. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Bilješke:** raspon priključaka kanala podataka mogu se promijeniti tijekom razdoblja pretpregleda.)

Ako daljinsko uklanjanje programskih pogrešaka s Visual Studio koristi se sljedeća pravila Demonstracija dopustiti pristup.  Postoji zasebna pravila za svaki podržanu verziju programa Visual Studio jer u svakoj verziji koristi drugi priključak za daljinsko uklanjanje programskih pogrešaka.  Kao i kod pristup FTP udaljeni promet za ispravljanje pogrešaka možda neće imati pravilno putem klasične WAF ili uređajem.  *SourceAddressPrefix* se umjesto toga možete postaviti na rasponu IP adresa za razvojne inženjere računala koji se izvode Visual Studio.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Dodjeljivanje sigurnosne grupe mreže podmreži ##
Sigurnosne grupe mreže sadrži pravila za zadane sigurnosne koja onemogućava se pristup svim vanjskim promet.  Rezultat kombiniranje pravila sigurnosti mreže prethodno opisan i blokiranje dolazni promet zadano pravilo sigurnost je taj samo promet s izvorna adresa raspona pridružene akciju *Dopusti* će moći poslati promet aplikacije u okruženju aplikacije servisa.

Nakon sigurnosne grupe mreže je popunjen sigurnosna pravila, treba dodijeliti podmreže koja sadrži okruženje za aplikaciju servisa.  Naredba dodjele reference i naziv virtualne mreže gdje se nalazi u okruženju aplikacije servisa, kao i naziv podmreže gdje je stvoren u okruženju aplikacije servisa.  

U sljedećem primjeru pokazuje mreže sigurnosne grupe dodijeljene podmreže i virtualne mreže:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Nakon uspješnog dodjeljivanjem mreže sigurnosne grupe (dodjeljivanjem je dugoročnih postupaka i može potrajati nekoliko minuta), samo dolazni promet *Dopusti* pravila podudaranja uspješno doći do aplikacija u okruženju servisa aplikacija.

U sljedećem primjeru dovršenosti prikazuje kako ukloniti i stoga dis-pridružiti mreže sigurnosne grupe iz podmreži:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Posebne napomene za eksplicitnih IP SSL ##
Ako aplikaciju konfiguriran eksplicitnih IP-SSL adresa (primjenjivo ASEs koji imaju samo javne VIP), umjesto korištenja zadane IP adrese servisa okruženja aplikacije HTTP i HTTPS promet tokova u podmreži drugi skup priključke osim priključke 80 i 443.

Pojedinačne par priključke koristi svake adrese IP SSL pronaći ćete u portala korisničkog sučelja u okruženju servisa aplikacija pojedinosti UX plohu.  Odaberite "sve postavke"--> "IP adrese".  Plohu "IP adrese" prikazuje tablicu sve izričito konfigurirani SSL za IP adrese servisa okruženju aplikacije koji se nalazi uz par posebno priključka koji se koristi za usmjeravanje HTTP i HTTPS promet pridružene svake adrese IP SSL.  To je ovaj priključak par koje je potrebno koristiti za parametre DestinationPortRange prilikom konfiguriranja pravila u sigurnosnoj grupi mreže.

Kada aplikacije na programa elika i mala slova konfiguriran za korištenje IP SSL, vanjski klijenti nećete vidjeti i ne morate brinuti o mapiranju par posebno priključak.  Promet aplikacijama obično će tijek konfigurirani adresu IP SSL.  Prijevod za uparivanje posebno priključak automatski će se dogoditi interno tijekom konačni leg usmjeravanje prometa u podmreže koji sadrži na elika i mala slova. 

## <a name="getting-started"></a>Početak rada

Da biste započeli aplikacije servisa okruženja, potražite u članku [Uvod u okruženje za aplikaciju servisa][IntroToAppServiceEnvironment]

Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Detalje oko aplikacije sigurno povezivanje s pozadinskom resursa iz servisa okruženja aplikacije potražite u članku [sigurno povezivanje s pozadinskom resursa iz okruženja aplikacije servisa][SecurelyConnecttoBackend]

Dodatne informacije o platforme Azure aplikacije servisa potražite u članku [Aplikacije servisa za Azure][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
