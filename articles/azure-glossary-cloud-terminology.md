<properties
    pageTitle="Azure Pojmovnik - Azure rječnika | Microsoft Azure"
    description="Pomoću Azure Pojmovnik poznavanje terminologije vezane uz oblak na platformi Azure. U ovom kratkom Azure rječnika nudi definicije uobičajenih uvjeta oblaka za Azure."
    keywords="Azure rječnik, terminologija oblaka, Azure Pojmovnik, terminologija definicije, uvjeti oblaka"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Pojmovnik za Microsoft Azure: rječnik oblaka terminologiju na platforme Azure

Pojmovnik za Microsoft Azure je kratki rječnik oblaka terminologija upravljanja Azure platforme.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Pronalaženje definicija servisa i ostale termine oblaka

* Definicija Azure servisa i njihovih AWS verzija potražite u članku [Microsoft Azure i web-servisa Amazon](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

* Općenito industrijskih oblaka uvjeta potražite u članku [oblaka računalstvo uvjeta](https://azure.microsoft.com/overview/cloud-computing-dictionary/).

Azure Pojmovnik s iznad dvjema referencama nudi do kraja do kraja taksonomije za Azure i industrijskih oblaka.  

## <a name="azure-glossary-list"></a>Azure Pojmovnik popisa


### <a name="account"></a>računa  
Posao ili školu ili osobni Microsoftov račun koji se koristi za pristup i upravljanje Azure pretplate.  
Vidi također [kako Azure pretplate pridružuju Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="availability-set"></a>Postavljanje dostupnosti  
Zbirka virtualnim strojevima upravlja zajedno zalihosti aplikacije i pouzdanosti. Korištenje skupa dostupnost osigurava tijekom događaja planiranog ili neplanirano održavanja barem jedan virtualnog računala dostupna.  
Pogledajte i [Upravljanje dostupnost virtualnim strojevima sa sustavom Windows](./virtual-machines/virtual-machines-windows-manage-availability.md) ili [Upravljanje dostupnost Linux virtualnim strojevima](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="classic-model"></a>Model Azure klasični implementacije  
Jedna od dvije [implementacije modelima](resource-manager-deployment-model.md) koristi za implementaciju resursa u Azure (novog modela je Voditelj resursa za Azure). Neke Azure resurse može uvesti u jedan model ili druge, dok drugi može uvesti u oba modelima. Upute za pojedinačne Azure resurse pojedinosti koje model(s) resursa uvode se sa.


### <a name="cli"></a>Azure sučelje naredbenog retka (EŽA)  
Na [sučelje naredbenog retka](xplat-cli-install.md) koji se mogu koristiti za upravljanje uslugama za Azure iz sustava Windows, OSX i PC-jeva Linux.


### <a name="powershell"></a>Azure PowerShell  
Na [sučelje naredbenog retka](powershell-install-configure.md) za upravljanje uslugama za Azure putem naredbenog retka iz Windows PC-ja. Neke usluge ili značajke usluge može upravljati samo putem komponente PowerShell ili na EŽA. Upute za svakog pojedinačnog Azure resursa pojedinosti koje model(s) resursa uvode se sa.   
Pogledajte i [kako instalirati i konfigurirati Azure PowerShell](powershell-install-configure.md)


### <a name="arm-model"></a>Model implementacije Azure Voditelj resursa  
Jedna od dvije [implementacije modelima](resource-manager-deployment-model.md) koristi za implementaciju resursa u Microsoft Azure (drugi je model klasični implementacije). Neke Azure resurse može uvesti u jedan model ili druge, dok drugi može uvesti u oba modelima. Upute za pojedinačne Azure resurse pojedinosti koje model(s) resursa uvode se sa.


### <a name="fault-domain"></a>kvara domene  
Zbirka virtualnim strojevima u skupu dostupnosti koje možete vjerojatno neće funkcionirati u isto vrijeme. Primjer je grupa računalima s za bicikle koji zajednički izvor i mrežne prekidač za zajedničko korištenje. U Azure, virtualnim strojevima u skupu dostupnost automatski razdjelnik preko više domena kvara.  
Pogledajte i [Upravljanje dostupnost virtualnim strojevima sa sustavom Windows](./virtual-machines/virtual-machines-windows-manage-availability.md) ili [Upravljanje dostupnost Linux virtualnim strojevima](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="geo"></a>zemlj.  
Definirani granica residency podataka koji se obično sadrži dvije ili više područja. Granica možda unutar ili izvan nacionalna Obrubi i regulacije porez utjecati. Svaki zemlj sadrži najmanje jedan regija. Primjeri geos su države Pacifičke i Japan. Naziva se i *Zemljopis*.  
Vidi također [Azure regije](best-practices-availability-paired-regions.md)


### <a name="geo-replication"></a>zemlj replikacijom  
Postupak automatski replikaciju sadržaja kao što su blob-ova, tablice i redova unutar regionalne para.  
Vidi također [Aktivni zemlj. – replikacije baze podataka Azure SQL](./sql-database/sql-database-geo-replication-overview.md)


### <a name="image"></a>Slika  
Datoteka koja sadrži operacijski sustav i konfiguracija aplikacije koje je moguće koristiti za stvaranje bilo koji broj virtualnih računala. U Azure postoje dvije vrste slika: VM slika i slika OS. Sliku VM obuhvaća operacijski sustav i svih disketa priložiti virtualnog računala stvaranja slike. Sliku s operacijskim Sustavom sadrži samo generalizirano operacijski sustav s bez podataka na disku konfiguracijama.  
Pogledajte i [Kretanje i odaberite slika virtualnog računala za Windows Azure pomoću programa PowerShell ili na EŽA](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="limits"></a>ograničenja  
Broj resursa koje je moguće stvoriti ili usporednih performanse koju možete postići. Ograničenja obično povezani su s pretplate, servise i ponude.  
Vidi također [Azure pretplate i ograničenja servisa, kvote, i ograničenjima](azure-subscription-service-limits.md)


### <a name="load-balancer"></a>opterećenja  
Resurs koji distribuira promet računala u mreži. U Azure, raspoređivača opterećenja distribuira promet na virtualnim strojevima koji su definirani u skupu raspoređivača opterećenja. Na [učitati opterećenja](./load-balancer/load-balancer-overview.md) može biti mjesto na Internetu ili može biti interni.  


### <a name="offer"></a>ponude  
Za cijene, kredita i primijeniti na pretplatu Azure povezanih pojmova.  
U odjeljku [Azure nude stranicu s detaljima](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="portal"></a>Portal  
Sigurno web-portal za implementaciju i upravljanje uslugama za Azure.  Postoje dvije portalima: [Azure portal](http://portal.azure.com/) i [Klasični portal](http://manage.windowsazure.com/). Neke usluge dostupne su u oba portalima dok drugi dostupne su samo u jednom ili na drugi. [Grafikon Azure portala dostupnost](https://azure.microsoft.com/features/azure-portal/availability/) popise koji su servisi dostupni koje portalu.  


### <a name="region"></a>regija  
Područje unutar zemlj koji ne ne preko nacionalna Obrubi i sadrži jednu ili više podatkovnim centrima. Cijene, regionalne servise i ponuda vrste ponudit će se na razini regija. Područje obično je u paru s druge regije, što može biti najviše nekoliko stotina milja Odsutan, da biste obrazac regional par. Regionalne parove može se koristiti kao mehanizam za oporavak Izrada i scenariji visoke dostupnosti. Naziva obično kao *mjesto*.  
Vidi također [Azure regije](best-practices-availability-paired-regions.md)


### <a name="resource"></a>resurs  
Stavka koja je dio vašeg rješenja Azure. Svaki Azure servis omogućuje vam za implementaciju različite vrste resursa, kao što su baze podataka ili virtualnim računalima.   
Pogledajte i [Pregled upravitelja resursa za Azure](azure-resource-manager/resource-group-overview.md)


### <a name="resource-group"></a>grupa resursa  
Spremnik u Voditelj resursa koja sadrži povezane resurse za aplikaciju. Grupa resursa možete uključiti sve resurse za aplikaciju ili samo resursi koji su logički grupirane. Možete odlučiti kako želite dodijeliti resurse grupe resursa koji se temelji na što vam najviše odgovara za tvrtku ili ustanovu.  
Pogledajte i [Pregled upravitelja resursa za Azure](azure-resource-manager/resource-group-overview.md)


### <a name="arm-template"></a>Voditelj resursa predloška  
Datoteka JSON deklarativno definira jedan ili više Azure resursa i koji definira međuzavisnosti distribuiranih resursi. Predložak se može koristiti za implementaciju resurse dosljedno i više puta.  
Vidi također [predložaka za izradu Voditelj resursa za Azure](resource-group-authoring-templates.md)


### <a name="resource-provider"></a>davatelja resursa  
Servis za kojeg se dohvaćaju resursa možete uvesti i upravljati njima putem upravitelja resursa. Svaki davatelja resursa nudi postupci za rad s resursima koji su raspoređeni. Davatelji resursa možete pristupiti putem portala za Azure, Azure PowerShell i SDK-ovi nekoliko programiranje.  
Pogledajte i [Pregled upravitelja resursa za Azure](azure-resource-manager/resource-group-overview.md)


### <a name="role"></a>uloga  
Srednje vrijednosti za kontrolu pristupa koji se mogu dodijeliti korisnike, grupe i usluge. Uloge se izvoditi akcije kao što su prilikom stvaranja, upravljanje i materijali Azure resursi.  
Vidi također [RBAC: ugrađene uloge](./active-directory/role-based-access-built-in-roles.md)


### <a name="sla"></a>ugovor o razini usluge (SLA)  
Ugovor koji opisuje tvrtke Microsoft p za vrijeme aktivnosti i povezivanjem. Svaki Azure servis sadrži određene SLA.  
Pogledajte i [ugovore o razini usluge](https://azure.microsoft.com/support/legal/sla/)


### <a name="storage-account"></a>račun za pohranu  
Račun za pohranu koji omogućuje pristup servisima blobova platforme Azure, reda čekanja, tablice i datoteke u Azure prostora za pohranu. Račun za pohranu omogućuje jedinstveni prostor za naziv za objekte Azure pohrane podataka.  
Vidi također [račune za pohranu o Azure](./storage/storage-create-storage-account.md)


### <a name="subscription"></a>pretplate  
Ugovor klijenta s Microsoftom koja omogućuje ih da biste dobili Azure services. Cijene pretplate i povezanih pojmova određena ponuda za pretplatu. Potražite u članku [Microsoft Online pretplate ugovor](https://azure.microsoft.com/support/legal/subscription-agreement/).  
Vidi također [kako Azure pretplate pridružuju Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="tag"></a>oznaka  
Indeksiranje pojam koji omogućuje vam da biste kategorizirali resursa u skladu s potrebama za upravljanje ili naplate. Oznake možete koristiti kad imate složene skup grupa resursa i resurse i trebali biste vizualizacija te imovine na način koji vam najviše odgovara. Na primjer, nije moguće oznaku resursa koji vam poslužiti slične uloga u tvrtki ili ustanovi ili nalaze u istom odjelu.  
Pogledajte i [Korištenje oznake da biste organizirali Azure resursi](resource-group-using-tags.md)


### <a name="update-domain"></a>Ažuriranje domene  
Zbirka virtualnim strojevima u skupu dostupnost koji se ažuriraju u isto vrijeme. Virtualnim strojevima u istom ažuriranje domene ponovno pokrenete zajedno tijekom planiranog održavanja. Azure nikad ne pokreće više ažuriranje domene odjednom. Također se nazivaju nadogradnje domene.  
Pogledajte i [Upravljanje dostupnost virtualnim strojevima sa sustavom Windows](./virtual-machines/virtual-machines-windows-manage-availability.md) ili [Upravljanje dostupnost Linux virtualnim strojevima](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="vm"></a>virtualnog računala  
Implementaciju softvera fizičke računala koja se pokreće operacijski sustav. Više virtualnim strojevima istodobno možete pokrenuti na isti hardver. U Azure, virtualnim strojevima dostupne su u raznih veličina.  
Vidi također [virtualnim strojevima dokumentacija](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="vm-extension"></a>Proširenje virtualnog računala  
Resurs koji implementira ponašanja ili značajki koje omogućuju ili drugim programima rad ili omogućiti mogućnost za interakciju s pokrenuti računalo. Ako, na primjer, možete koristiti proširenje VM pristup za ponovno postavljanje ili izmjena daljinski pristup vrijednosti na Azure virtualnog računala.  
Vidi također [o extensions, virtualnog računala i značajke (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md) ili [o extensions, virtualnog računala i značajke (Linux)](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="vnet"></a>virtualne mreže  
Mreža kojoj se navode veze između Azure resursa koji odvajanja iz drugih Azure klijenata. Može se povezati s drugim mrežama Azure virtualne putem [Azure VPN pristupnika](./vpn-gateway/vpn-gateway-about-vpngateways.md) i lokalne mreže pomoću [više mogućnosti](./vpn-gateway/vpn-gateway-cross-premises-options.md). Potpuno možete kontrolirati na IP adresa blokovi, DNS postavke, sigurnosne pravilnike i usmjeravanje tablice u ovu mrežu.  
Vidi također [Pregled virtualne mreže](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**Vidi također**  
- [Početak rada s Azure](https://azure.microsoft.com/get-started/)
- [Centar za resurse oblaka](https://azure.microsoft.com/resources/)  
- [Azure za svoju aplikaciju za tvrtke](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [Azure u vašem podatkovnim centrom](https://azure.microsoft.com/overview/business-apps-on-azure/) 





