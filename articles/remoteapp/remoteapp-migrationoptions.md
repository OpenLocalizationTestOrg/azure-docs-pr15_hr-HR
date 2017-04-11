
<properties 
    pageTitle="Mogućnosti migracije iz Azure RemoteApp | Microsoft Azure" 
    description="Dodatne informacije o mogućnostima migracije iz Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Mogućnosti migracije iz Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Ako su zaustavljena pomoću Azure RemoteApp zbog [umirovljenje objave](https://go.microsoft.com/fwlink/?linkid=821148) ili jer ste završili evaluacijsku, morat ćete migrirati iz Azure RemoteApp s drugim servisom aplikacije. Postoje dva načina prikaza različitih migracije: na koja se sama upravljanih (često se nazivaju infrastrukture kao servis [IaaS]) implementaciju ili potpuno upravljanih (često zvane platforma kao Service) ili softver kao servis [PaaS na SaaS] koja nudi. 

Samostalno IaaS je samostalno implementaciju koja se upravlja, kojim upravlja i vlasništvu, izravno implementiran na virtualnim strojevima (VMs) ili fizičke sustave. Na drugoj strani u potpunosti upravljanih PaaS/SaaS koja nudi se više sviđa Azure RemoteApp - partnera nudi sloj servisa pri vrhu remoting rješenje koje rukuje radu i održavanje, dok, kao i Kupac učinite neke upravljanje slike i aplikacije.

Nastavite čitati dodatne informacije, uključujući Primjeri različitih mogućnosti pohrane.    

## <a name="self-managed-iaas-solutions"></a>Samostalno upravljana rješenja (IaaS)

### <a name="rds-on-iaas"></a>**ZAPISI na IaaS** 
Možete implementirati nativni temelji na sesiji udaljene radne površine (u sustavu Windows Server) implementacije pomoću RemoteApp ili stolna računala na lokalni ili u okruženju glavnom računalu (poput na Azure VMs). ZAPISI na IaaS implementacijama su najbolje za već upoznati s klijentima i koje imaju postojeće Stručne osobe s implementacijama ZAPISI. 

> [AZURE.NOTE]
> Potreban vam je količinskog licenciranja s softver jamstvo (SA) za ZAPISI klijentskog pristupa licenci da biste koristili tu mogućnost implementacije.

Uvođenje ZAPISI na Azure VMs je lakše nego ikada kada koristite implementacije i zakrpa predlošci (čitati na [Pregled](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) , a zatim [otvorite ih](https://aka.ms/rdautomation)). Možete dobiti isti elastic skaliranja mogućnosti s resursima za model Azure uvođenje klasičnog (ne i resursi modela resursa Azure) unutar Azure RemoteApp pomoću [Automatsko skaliranje skripte](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), iako ima dodatne prilagodbe i konfiguracije. Ako pokrenete ZAPISI na Azure VMs, podrška se odvija pomoću [Azure podržava](https://azure.microsoft.com/support/plans/), iste Stručnjaci za podršku koji podržava Azure RemoteApp. Možete dobiti cijena procjenjuje na temelju postojeće Upotreba tako da se obratite [Podršci za Azure](https://azure.microsoft.com/support/plans/)ili vam može izračunavati sami pomoću programa uskoro da je objavio Kalkulator trošak.  Uz to, N niz VMs (trenutno u privatnu pretpregled) možete dodati vGPU - saznati više o dodavanju vGPU i o tome [analitička poboljšanja ZAPISI u sustavu Windows Server 2016](https://myignite.microsoft.com/videos/2794) u našem Ignite sesiju.   

Imamo korak po korak implementacije vodiče za [Windows Server 2012 R2](http://aka.ms/rdsonazure) i [Windows Server 2016](http://aka.ms/rdsonazure2016) radi jednostavnijeg implementaciju sustava. Pogledajte najnovije vijesti na [blog udaljene radne površine](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) .
 
### <a name="citrix-on-iaas"></a>**Citrix na IaaS** 
Izvorni Citrix implementacije temelji na sesiji XenApp ili XenDesktop može biti implementiran na lokalni ili glavnom računalu okruženja (kao što su na Azure VMs). 

Pogledajte detaljne upute za uvođenje [Citrix XA 7.6 na Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), dodatne informacije. Dodatne informacije o [Citrix na Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), uključujući Kalkulator cijena. Možete pronaći i [obratite Citrix](http://citrix.com/English/contact/index.asp) o mogućnostima s.

## <a name="fully-managed-paassaas-offerings"></a>Potpuno upravljanih ponude (PaaS/SaaS)

### <a name="citrix-cloud"></a>**Citrix oblaka** 
[Postojeće rješenje oblaka Citrix](https://www.citrix.com/products/citrix-cloud/)jednako architecturally Citrix XenApp Express. Citrix nudi [promotivne ponude popust na 50%](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) za postojeće korisnike Azure RemoteApp. 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express (u Tehnički pretpregled)**
[Registrirajte se za svoje Tehnički pregled](http://now.citrix.com/remoteapp), i gledanje [Ignite sesije](https://myignite.microsoft.com/videos/2792) (počevši od 20:30 minuta). XenApp Express jednak architecturally Citrix oblak osim sadrži pojednostavnjeno korisničko Sučelje za upravljanje i druge značajke i mogućnosti koje su slične Azure RemoteApp. 

Saznajte više o [Citrix XenApp Express](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Program davatelja usluge Citrix** 
Program za davatelja usluga Citrix olakšava davateljima usluga za isporuku jednostavnosti virtualne oblaka računalstvo SMBs, daje usluge žele jednostavno, pay-as-you-go modela. Citrix davateljima usluga Povećaj svoje tvrtke Microsoft SPLA i proširivanje njihove ulaganja platformu ZAPISI s bilo kojeg uređaja, bilo kojeg mjesta pristupili, imalo podrška za aplikacije, obogaćenog sučelje, zaštite i povećanu skalabilnost. U nizu, Citrix davateljima usluga privući više pretplatnike, povećajte zadovoljstvo klijenata i smanjiti svoje radno troškove. [Dodatne informacije](http://www.citrix.com/products/service-providers.html) ili [pronaći partnera](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Microsoft hostira davatelj usluga** 
Hostinga partnere obično nude potpuno upravljanih hostira radnu površinu sustava Windows i servis za aplikaciju, koji može obuhvaćati i upravljanje Azure resursa, operacijski sustavi, aplikacije i služba za korištenje partnera je licenciranje ugovori s Microsoft i drugi proizvođači softvera osim servisa davatelja licencni ugovor da biste omogućili reselling od pretplatnički pristup licence (SAL). Sljedeće informacije upute i podaci za kontakt za neki davatelji usluge koje specijalizirani za nije kupci njihove Azure RemoteApp migracije. Pogledajte [trenutni popis hostira davateljima usluga](http://aka.ms/rdsonazurecertified) koje ste dovršili ZAPISI na IaaS učenje put i procjenu.  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en) specializes u ISV-ovi pri prijelazu u oblak i Neovisni "želite li optimizirati svoje trenutne postavu oblaka. ASPEX nudi širok raspon upravljanih, devops i savjetodavnih services.  

Primarno mjesto: ANTWERPEN, Belgija

Operacija regije: zapadnjački Europe

Partnerski status: [Srebrno](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Davatelj usluga za Microsoft Cloud: da

Nude sesiju rješenja utemeljena na web-mjesto RemoteApp i u okvir za radnu površinu: da, oboje

Azure rješenja za migraciju RemoteApp: da, [Saznajte više](https://www.aspex.be/en/azure-remote-apps)

**Kontakt:**

- Telefon: +3232202198
- Pošta:[info@aspex.be](mailto:info@aspex.be)
- Web-mjesto: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (platformu naziv: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) je programa automatizacije platformu za IT tvrtke Pojednostavnite optimiziranje te mjerilo migracije i isporuku remote stolna računala, udaljene aplikacije i infrastrukture u Oblaku Microsoft Azure. 

Platforme MyCloudIT vrijeme uvođenja smanjuje 95%, Azure trošak po 30% i prelazi cijeli IT infrastrukturu njihove klijenta u oblak u samo nekoliko poteza ključa. Partnera možete upravljati klijentima iz jednog globalni nadzorne ploče, servis za krajnje korisnike diljem svijeta uspješnijima nego ikad prije i Povećaj prihod bez dodavanja dodatnih indirektni ili proširenom Azure obuka.  

Primarno mjesto: grada Dallasa, TX, SAD

Operacija regije: diljem svijeta

Partnerski status: [Zlatne](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Davatelj usluga za Microsoft Cloud: da

Nude sesiju rješenja utemeljena na web-mjesto RemoteApp i u okvir za radnu površinu: da, oboje

Azure rješenja za migraciju RemoteApp: da, [Saznajte više](https://mycloudit.com/remote-app-microsoft/)

**Kontakt:**
- Odbio Garoutte, VP razvoja tvrtke

   Telefon: 972-218-0741

   E-pošta:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) specializes u pruža glavnom računalu rješenja za stolna računala, izlaganja cijelog radne površine i aplikacija Neovisni sučelja utemeljena na tehnologije Microsoft globalni klijentu osnovni Azure i vlastite podatkovnim centrima.

Primarno mjesto: London, velika Britanija; Singapur; Houston, TX

Operacija regije: diljem svijeta

Partnerski status: Zlatne

Davatelj usluga za Microsoft Cloud: da

Nude sesiju rješenja utemeljena na web-mjesto RemoteApp i u okvir za radnu površinu: da, oboje

Azure rješenja za migraciju RemoteApp: da, [Saznajte više](http://www.acuutech.com/ara-migration/)

**Kontakt:**

- Ujedinjena Kraljevina:

  5/6 York domu, cesta Langston

  Loughton, Essex IG10 3TQ
  
  Telefon: +44 (0) 20 8502 2155
 
- Singapur:

  #09 02 100 Cecil ulica 
  
  Globus, Singapur 069532
  
  Telefon: + 65 6709 4933
 
- Sjeverna Amerika: 

  3601 S. Sandman St.
  
  Paket 200, Houston, TX 77098
  
  Telefon: +1 713 691 0800

## <a name="need-more-help"></a>Potrebna vam je dodatna pomoć?
I dalje potrebna pomoć za odabir ili da daljnje pitanja? Koristite neku od sljedećih metoda da biste dobili pomoć. 

1.  Slanje e-poštom nam se na [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Obratite se [Azure podržava](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Započnite s otvaranjem programa [Azure podržava velikim slovom](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3.  Nazovite us. [Traži lokalni broj prodaje](https://azure.microsoft.com/overview/sales-number/).
