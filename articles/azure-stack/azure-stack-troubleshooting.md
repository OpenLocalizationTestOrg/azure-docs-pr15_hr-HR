<properties
    pageTitle="Otklanjanje poteškoća s Microsoft Azure snop | Microsoft Azure"
    description="Azure stogu otklanjanje poteškoća."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="microsoft-azure-stack-troubleshooting"></a>Otklanjanje poteškoća s Microsoft Azure stogu

Ovaj dokument sadrži uobičajene informacije o otklanjanju poteškoća za Azure stogu.

Za najbolje mogućnosti rada, provjerite je li okruženje za implementaciju ispunjava li sve [preduvjete](azure-stack-deploy.md) i [pripreme](azure-stack-run-powershell-script.md) prije instalacije. 

Preporuke za otklanjanje problema s koji su opisani u ovom odjeljku potječu iz nekoliko izvora i možda ili ne riješite određeni problem. Ako naiđete na problem ne navedenih, provjerite je li da biste provjerili [Azure stogu MSDN Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) za podršku za daljnje i dodatne informacije.

Primjeri kod služe kao što je, a ne može se zajamčiti očekivane rezultate. U ovom se odjeljku podložni Česti uređivanja i ažuriranja kao što su implementirana poboljšanja proizvoda.

  

## <a name="known-issues"></a>Poznati problemi

 - Možda ćete vidjeti sljedeće koje nisu prekidanje pogreške tijekom implementacije, koja utječu na uspješnost za implementaciju:
     - "Termina 'C:\WinRM\Start-Logging.ps1' nije prepoznata"
     - "Pozivanje EceAction: ne možete indeksirati u null polja" 
     - "InvokeEceAction: ne možete povezati argument parametar"Poruke"jer je bio prazan niz."
 - Možda ćete vidjeti implementacije neće funkcionirati u koraku 60.61.93 poruku o pogrešci "Aplikacijom identifikator"URI"nije pronađena." To je način registriranih u servisu Azure Active Directory.  Ako vam se prikaže Ova pogreška, prijeđite na [ponovno pokrenite instalaciju skripte](azure-stack-rerun-deploy.md) od koraka 60.61.93 implementaciju dok se ne dovrši.
 - Vidjet ćete da resursa **Postavite dostupnost** na tržištu se prikazuje u odjeljku kategorija **virtualMachine ARM** – ovaj izgled je samo kozmetičkih problem.
 - Prilikom stvaranja novog virtualnog računala na portalu u koraku **Osnove** mogućnost za pohranu po zadanom odabrana SSD.  Ta postavka moraju promijeniti podizanje tvrdog diska ili na **veličinu** koraku implementacije VM, nećete vidjeti VM veličine dostupne možete odabrati i nastaviti implementacije. 
 - Prikazat će se prema zadanim postavkama na VM MAS CON01 više nije instaliran AzureRM PowerShell Module (u TP1 to je pod nazivom ClientVM). Ovo je zadano ponašanje, jer nema zamjenski način [instalacije ove moduli](azure-stack-connect-powershell.md)i povezati.  
 - Vidjet ćete da davatelja resursa **Microsoft.Insights** nije automatski registriran za pretplate na klijentu. Želite li da biste vidjeli nadzor podataka za VM uveden kao klijent, pokrenite sljedeću naredbu iz PowerShell (nakon što ste [instalirali i povezivanje](azure-stack-connect-powershell.md) kao klijenta): 
       
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights 

 - Prikazat će se izvesti funkcionalnost na portalu za grupe resursa, no nikakav tekst je prikazati i dostupni za izvoz.      
 - Možete početi implementacije resursa za pohranu veće od dostupnih kvote.  U ovom implementacije ne uspije, a će biti obustavljeno resursi za račun.  Dostupne su dvije mogućnosti olakšava:
     - Administrator servisa možete povećati kvote, iako se promjene će odmah su vidljive i najčešće potrajati i do jedan sat proširiti.
     - Administrator servisa možete stvoriti tarifu dodatak s dodatnim kvota za klijenta možete dodavati s pretplatom.
 - Kada pomoću portala za stvaranje VMs na stogu Azure okruženjima identiteta u "Azure - Kina", nećete vidjeti VM veličine dostupne da biste odabrali u koraku **Veličina** VM implementacije i neće biti moguće da biste nastavili implementacije.
 - Neuspjelo uvođenje portalu možda ćete vidjeti kada se VM zapravo je postavila uspješno.
 - Kad izbrišete neku tarifu, ponudu ili pretplatu, VMs možda neće biti izbrisan.
 - Vidjet ćete proširenja VM na tržištu.
 - Ne možete implementirati VM iz spremljene VM slike.
 - Samoposlužni naići servise koji su uključeni u svoje pretplate.  Kad klijenata pokušaju uvesti te resurse, primit ćete pogrešku.  Primjer: Pretplate klijentu sadrži samo resursa za pohranu.  Klijentu prikazat će se mogućnost da biste stvorili druge resurse kao što su VMs.  U ovom scenariju kada pokuša klijentom za implementaciju VM, pojavit će se poruka koja upućuje na VM nije moguće stvoriti. 
 - Prilikom instalacije TP2, trebali biste aktivirati voditelju OS u VHD Uvjet gdje pokrenuti skriptu postavljanje Azure snop ili primiti pogrešku poruka navodi Windows će uskoro isteći.


## <a name="deployment"></a>Uvođenje

### <a name="deployment-failure"></a>Neuspjelo uvođenje
Ako se pojave pogreške prilikom instalacije, stoga Azure instalacijski program omogućuje vam da biste nastavili neuspješne instalacije tako da slijedite [korake za implementaciju, ponovno pokrenite](azure-stack-rerun-deploy.md).

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Na kraju implementacijskih sesiju ljuske PowerShell još uvijek otvoren i ne prikazuje sve izlazne

Takvo ponašanje vjerojatno je samo rezultat zadano ponašanje prozoru za naredbe ljuske PowerShell, kada je odabran. Uvođenje PNA sadrži zapravo je uspjela, ali skripta je pauziran prilikom odabira prozora. Možete provjeriti je to slučaj traženjem riječ "odaberite" u naslovnoj naredbeni prozor.  Pritisnite tipku ESC da biste poništili ga, a poruku dovršetka trebale nalaziti iza nje.

## <a name="templates"></a>Predlošci

### <a name="azure-template-wont-deploy-to-azure-stack"></a>Predloška Azure neće uvesti snop Azure

Provjerite je li:

- Predložak mora pomoću servisa Microsoft Azure koji je već dostupni ili u pretpregledu u stogu Azure.
- API-ji za određeni resurs podržanog lokalnu instancu stogu Azure i birate valjano mjesto ("lokalni" u Azure stogu Tehnički pretpregled (TP) 2, dodavanje veze vanjskih "Istočni sad" ili "Jug Indija" u Azure).
- Pročitajte [Ovaj članak](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) o cmdletima za testiranje AzureRmResourceGroupDeployment koji Uhvatite small razlike u sintaksi Azure Voditelj resursa.

Stoga Azure predložaka već u [spremištu GitHub](http://aka.ms/AzureStackGitHub/) možete koristiti i da vam pomoći da započnete.

## <a name="virtual-machines"></a>Virtualnim strojevima

### <a name="after-starting-my-microsoft-azure-stack-poc-host-all-my-tenants-vms-are-gone-from-hyper-v-manager-and-come-back-automatically-after-waiting-a-bit"></a>Nakon pokretanja glavno računalo za Moje Microsoft Azure stogu PNA sve moje klijenata VMs su nestaje s Upravitelj Hyper-V i vratite se automatski nakon čekanje malo?

Kao što je sustav natrag dolazi podsustav prostora za pohranu i RPs moraju odrediti dosljednost. Vrijeme potrebno ovisi o hardver i specifikacija koristi, ali može biti neko vrijeme nakon ponovnog pokretanja računala koje hostira za klijent VMs vratite i prepoznati.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Li izbrišete neke virtualnim strojevima, ali i dalje vidjeti VHD datoteke na disku. Takvo ponašanje očekuje?

Da, to je ponašanje koje očekuje. Budući da je bio dizajniran ovako:

- Kada izbrišete na VM, VHDs ne brišu se. Diskova su zasebnom resursa u grupu resursa.
- Kada se briše s računom za pohranu, brisanje vidljiv odmah kroz Azure Voditelj resursa (portal PowerShell), ali diskova možda sadrži i dalje čuvaju u prostor za pohranu dok se ne pokreće smeća.

Ako se prikaže "zadnji redak" VHDs, važno je znati ako su dio mape za pohranu račun koji je izbrisan. Ako račun za pohranu je izbrisana, uobičajeno je oni postoje li i dalje.

Dodatne informacije o konfiguriranju praga zadržavanja u [upravljanje računima za pohranu](azure-stack-manage-storage-accounts.md).

## <a name="installation-steps"></a>Korake za instalaciju
Sljedeće informacije o snopu Azure korake za instalaciju može biti korisno za rješavanje problema:  

| Indeks | Ime | Opis|
| ----- | ----- | -----|
|0.11 | (DEP) Provjerite valjanost fizičke računalima | Provjera valjanosti hardvera i konfiguracije OS na fizičke čvorove. |
| 0.12 | (DEP) Konfiguriranje umrežavanje fizičke strojeva za PNA | Konfiguriranje sučelja i parametri virtualne mreže. |
| 0.14 | (DEP) Implementacija domene | Implementacija servisa Active Directory na virtualnog računala. |
| 0,15 | (DEP) Konfiguriranje poslužitelja domene | Konfiguriranje poslužitelja domene sa sigurnosnim grupama itd. |
| 0,16 | (DEP) Konfiguriranje fizičke računala | Konfiguriranje umrežavanje, pridruži domeni i administratori za lokalne instalacije. |
| 0.18 | (KINI) Konfiguriranje klaster prostora za pohranu | Stvaranje klaster prostora za pohranu, stvorite poslužitelj grupe aplikacija i datoteka za pohranu. |
| 0.19 | (CPI) Postavljanje tkanina infrastrukture | Postavljanje preduvjeti za implementaciju tkanina. |
| 0.21 | (NET) Postavljanje BGP i NAT | Instalira BGP i NAT - potrebno samo za jedan čvor. |
| 0.22 | (NET) Konfiguriranje NAT i poslužitelja za vrijeme | Sinkronizira poslužitelja za vrijeme i konfigurira NAT stavke. |
| 40.41 | (CPI) Stvaranje VMs goste | Stvaranje upravljanje VMs. |
| 40.42 | (FBI) Postavljanje PowerShell JEA | Postavljanje JEA ljuske PowerShell za sve uloge. |
| 40.43 | (FBI) Postavljanje Azure stogu ustanova za izdavanje certifikata | Instalira Azure stogu ustanove za izdavanje certifikata. |
| 40.44 | (FBI) Konfiguriranje Azure stogu ustanove za izdavanje certifikata | Konfigurira Azure stogu ustanove za izdavanje certifikata. |
| 40.45 | (NET) Postavljanje NC na VMs | Instalira NC na VMs goste |
| 40.46 | (NET) Konfiguriranje NC na VMs | Konfiguriranje NC na VMs goste |
| 40.47 | (NET) Konfiguriranje VMs goste | Konfiguriranje upravljanja VMs s NC ACL-a. |
| 60.61.81 | (FBI) Implementacija Azure stogu tkanina Nazovi usluge – FabricRing preduvjeta | Stvara VIPs za FabricRing |
| 60.61.82 | (FBI) Implementirali servise za Azure stogu tkanina Nazovi - implementacija tkanina Nazovi klaster | Instalira i konfigurira Azure stogu tkanina Nazovi klaster. |
| 60.61.83 | (FBI) Implementacija Extensions Administrator davateljima resursa | Instaliranje proširenja za administratore za davatelje usluga resursa |
| 60.61.84 | (ACS) Postavljanje Azure dosljedan prostora za pohranu u razini čvora. | Instalira i konfigurira Azure dosljedan prostora za pohranu u razini čvora. |
| 60.61.85 | (ACS) Postavljanje Azure dosljedan prostora za pohranu na razini klaster. | Instalira i konfigurira Azure dosljedan prostora za pohranu na razini klaster. |
| 60.61.86 | (FBI) Implementacija Azure stogu tkanina Nazovi kontroler usluge – preduvjeta | Preduvjeti za InfraServiceController |
| 60.61.87 | (FBI) Implementacija Azure stogu tkanina Nazovi kontroler usluge – preduvjeta | Preduvjeti za CPI |
| 60.61.88 | (FBI) Implementacija Azure stogu tkanina Nazovi kontroler usluge – preduvjeta | Preduvjeti za ASAppGateway |
| 60.61.89 | (FBI) Implementacija Azure stogu tkanina Nazovi kontroler usluge – preduvjeta | Preduvjeti za pohranu kontroler |
| 60.61.90 | (FBI) Implementacija Azure stogu tkanina Nazovi kontroler usluge – preduvjeta | Preduvjeti za HealthMonitoring |
| 60.61.91 | (FBI) Implementacija Azure stogu tkanina Nazovi kontroler usluge – preduvjeta | Preduvjeti za ECE |
| 60.61.92 | (FBI) Implementacija Azure stogu tkanina Nazovi kontroler usluge – preduvjeta | Preduvjeti za PMM |
| 60.61.93 | (Katal) Stvaranje upravitelji AzureStack servisa | Stvaranje Azure grafikonu aplikacija i servisa upravitelji u AAD. |
| 60.61.94 | (NET) Postavljanje GW VMs | Instalira GW na VMs za goste. |
| 60.61.95 | (NET) Konfiguriranje GW VMs | Konfigurira GW VMs za goste. |
| 60.61.96 | (NET) Implementacija IDN-ovi na glavno računalo | Implementacija IDN-ovi na infrastrukture domaćini |
| 60.61.97 | (NET) Konfiguriranje IDN-ovi | Konfiguriranje uloge IDN-ovi |
| 60.61.98 | (FBI) Postavljanje WSUS VMs | Instalira WSUS poslužitelj na VMs za goste. |
| 60.61.99 | (FBI) Konfiguriranje WSUS VMs | Konfigurira WSUS poslužitelja VMs za goste. |
| 60.61.100 | (FBI) Postavljanje VMs Azure SQL | Instalira Azure SQL server na VMs goste |
| 60.61.101 | (Katal) Preduvjeti za je VMs za postavljanje. | Postavlja preduvjeti za Microsoft Azure snop na VMs za goste. |
| 60.61.102 | (Katal) Instalacijski program nije VMs | Instalira Microsoft Azure snop na VMs za goste. |
| 60.120.121 | (FBI) Implementacija davatelji resursa i kontrolera | Instalira davatelja resursa i kontrolera |
| 60.120.121 | (FBI) Implementacija davatelji resursa i kontrolera | Instalira davatelja resursa i kontrolera |
| 60.120.121 | (FBI) Implementacija davatelji resursa i kontrolera | Instalira davatelja resursa i kontrolera |
| 60.120.121 | (FBI) Implementacija davatelji resursa i kontrolera | Instalira davatelja resursa i kontrolera |
| 60.120.121 | (FBI) Implementacija davatelji resursa i kontrolera | Instalira davatelja resursa i kontrolera |
| 60.120.121 | (FBI) Implementacija davatelji resursa i kontrolera | Instalira davatelja resursa i kontrolera |
| 60.120.121 | (FBI) Implementacija davatelji resursa i kontrolera | Instalira davatelja resursa i kontrolera |
| 60.120.121 | (FBI) Implementacija davatelji resursa i kontrolera | Instalira davatelja resursa i kontrolera |
| 60.120.122 | (FBI) Konfiguracija kontrolera | Konfigurira kontrolera |
| 60.120.123 | (Katal) Konfiguriranje WAS VMs | Konfigurira Microsoft Azure stogu VMs za goste. |
| 60.120.124 | (Katal) Konfiguracija AAD Azure stogu. | Konfigurira Azure snop sa Azure AD. |
| 60.120.125 | (Katal) Instalacija ADFS | Instalira Active Directory Federation Services (ADFS) |
| 60.120.126 | (Katal) Instalacija ADFS-grafikonu | Instalira Azure stogu grafikonu |
| 60.120.127 | (Katal) Konfiguriranje ADFS | Konfigurira Active Directory Federation Services (ADFS) |
| 60.140.141 | (FBI) Konfiguriranje SRP | Konfigurira davatelja resursa za pohranu |
| 60.140.142 | (ACS) Konfiguriranje Azure dosljedan prostora za pohranu. | Konfigurira Azure dosljedan prostora za pohranu. |
| 60.140.143 | (FBI) Stvaranje računa za pohranu | Stvorite sve račune za pohranu koriste različite davatelje usluga. |
| 60.140.144 | (FBI) Registrirajte se za SRP korištenje | Korištenje Registrirajte se za davatelja usluga za pohranu. |
| 60.140.145 | (CPI) Migriranje stvoren VMs, domaćini i klaster CPI | Prenosi objekte stvorene VMs, domaćini i klaster u CPI |
| 60.140.146 | (FBI) Konfiguriranje programa Windows Defender | Konfigurira programa Windows Defender |
| 60.160.161 | (PONEDJELJAK) Konfiguriranje nadzora Agent | Konfigurira nadzor Agent |
| 60.160.162 | (FBI) NRP preduvjeta | Instalira NRP preduvjeti |
| 60.160.163 | (FBI) NRP implementacije | Instalira NRP  |
| 60.160.164 | (FBI) Konfiguriranje NRP | Konfigurira NRP |
| 60.160.165 | (FBI) PZK preduvjeta | Instalira PZK preduvjeti |
| 60.160.166 | (FBI) Uvođenje PZK | Instalira PZK |
| 60.160.167 | (FBI) Konfiguriranje PZK | Konfigurira PZK |
| 60.160.168 | (FBI) FRP preduvjeta | Instalira FRP preduvjeti |
| 60.160.169 | (FBI) FRP implementacije | Instalira FRP  |
| 60.160.170 | (FBI) Konfiguriranje FRP | Konfigurira FRP |
| 60.160.174 | (FBI) URP pripremni | Instalira URP preduvjeti |
| 60.160.175 | (FBI) URP implementacije | Instalira URP  |
| 60.160.176 | (FBI) Konfiguriranje URP | Konfigurira URP |
| 60.160.171 | (FBI) HRP preduvjeta | Instalira HRP preduvjeti |
| 60.160.172 | (FBI) HRP implementacije | Instalira HRP  |
| 60.160.173 | (FBI) Konfiguriranje HRP | Konfigurira HRP |
| 60.160.177 | (KV) KeyVault preduvjeta | Instalira KeyVault preduvjeti |
| 60.160.178 | (KV) KeyVault implementacije | Instalira KeyVault  |
| 60.160.179 | (KV) Konfiguriranje KeyVault | Konfigurira KeyVault | 
| 60.190.191 | (FBI) Konfiguriranje galerije | Konfiguriranje galerije |
| 60.190.192 | (FBI) Konfiguriranje servisa za Nazovi tkanina | Konfiguriranje servisa za Nazovi tkanina |
| 60.221 | (FBI) Postavljanje konzole VMs | Instalira konzole poslužitelj na VMs za goste. |
| 60.222 | (FBI) Postavljanje konzole VMs | Premještanje DVM sadržaja na konzolu VM. |
| 251 | Priprema za buduće glavno računalo izvršen | Postavljanje pravila za ponovno pokretanje |


## <a name="next-steps"></a>Daljnji koraci

[Najčešća pitanja](azure-stack-FAQ.md)
