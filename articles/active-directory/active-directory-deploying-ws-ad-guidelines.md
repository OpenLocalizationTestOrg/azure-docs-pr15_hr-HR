<properties
   pageTitle="Smjernice za implementaciju sustava Windows Server Active Directory na Azure virtualnim strojevima | Microsoft Azure"
   description="Ako znate kako implementirati servisa Active Directory Domain Services i servisa Active Directory Federation Services lokalno, Saznajte kako funkcioniraju na Azure virtualnih računala."
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Smjernice za implementaciju Active Directory za Windows Server na virtualnim računalima za Azure

U ovom se članku objašnjava važne razlike između implementacija sustava Windows Server Active Directory Domain Services (AD DS) i Active Directory Federation Services (AD FS) lokalne i implementacije ih na virtualnim računalima sustava Microsoft Azure.

## <a name="scope-and-audience"></a>Opseg i publike

Članak namijenjen je one već iskustvom s Implementacija servisa Active Directory lokalnog. Pokriva razlike između implementaciji servisa Active Directory na Microsoft Azure virtualnim strojevima/Azure virtualne mreže i tradicionalni lokalnim servisom Active Directory implementacijama. Azure virtualnim strojevima i Azure virtualne mreže su dio programa infrastrukture kao-na-usluga (IaaS) koja nudi tvrtkama ili ustanovama odražava računalno resursa u oblak.

One koje niste upoznati s programom AD implementaciju, potražite u članku [Vodič za implementaciju AD DS](https://technet.microsoft.com/library/cc753963) ili [planirate implementaciju sustava AD FS](https://technet.microsoft.com/library/dn151324.aspx) po potrebi.

U ovom se članku pretpostavlja se da je poznato sljedeće koncepata čitač:

- Windows Server AD DS implementacije i upravljanja
- Uvođenje i konfiguriranje DNS-a za podršku infrastruktura za Windows Server AD DS
- Windows Server AD FS implementacije i upravljanja
- Implementacija, konfiguriranje i upravljanje potrebe za oslanjanjem proizvođača aplikacije (web-mjesta i web-servise) koje mogu zauzeti Windows Server AD FS tokena
- Koncepte Općenito virtualnog računala, kao što su upute za konfiguriranje virtualnog računala, virtualne diskova i virtualne mreže

U ovom se članku ističe preduvjeti za implementaciju scenarija hibridnog u kojoj Windows Server AD DS ili AD FS su djelomično distribuiranih lokalnog i djelomično implementiran na Azure virtualnim strojevima. Dokument najprije pokriva ključne razlike između sa sustavom Windows Server AD DS i AD FS na Azure virtualnim strojevima nasuprot lokalnog i važne odluke koje utječu na dizajn i implementaciju. Ostatak papira objašnjava smjernice za svaku točaka odluka detaljno, i da biste primijenili smjernice različitim scenarijima implementacije.

U ovom se članku govori konfiguracije [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), koji je utemeljen na OSTALE servis koji omogućuje upravljanje identitetom i mogućnosti za kontrolu pristupa za oblak aplikacije. Azure Active Directory (Azure AD) i Windows Server AD DS su, međutim, međusobnoj kombinaciji služe nudi identiteta i pristup rješenje upravljanja za hibridno današnjeg IT okruženjima i moderan aplikacije. Da biste razumjeli razlike i odnose između Windows Server AD DS i Azure AD, imajte na umu sljedeće:

1. Windows Server AD DS može izvoditi u oblaku na Azure virtualnim strojevima prilikom korištenja Azure širenje vaše lokalne podatkovnog centra u oblak.
2. Azure AD možete koristiti da bi se dobilo vaše korisnike jedinstvenu prijavu aplikacijama softver-kao-na-Service (SaaS). Microsoft Office 365 koristi ove tehnologije, na primjer, a aplikacija na web-mjesto Azure ili u okvir za druge platforme oblaka možete koristiti.
3. Azure AD (njegov kontrola servis programa Access) možete koristiti da biste korisnicima koji se prijavite pomoću identiteta s Facebooka, Google, Microsoft i drugih davatelja identiteta aplikacije koje se nalaze u oblaku ili na lokalnim poslužiteljima.

Dodatne informacije o te razlike potražite u članku [Azure identitet](fundamentals-identity.md).

## <a name="related-resources"></a>Povezani resursi

Možete preuzeti i Pokreni [Azure virtualnog računala spremnosti za procjenu](https://www.microsoft.com/download/details.aspx?id=40898). Na vrednovanje automatski se pregledati lokalnog okruženja i generirati Prilagođeno izvješće prema uputama u ovoj se temi pomoći prenesite okruženje Azure.

Preporučujemo da se najprije pregledate vodiče, vodiče i videozapise koje naslovnice u sljedećim temama:

- [Konfiguriranje samo oblak virtualne mreže na portalu za Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [Konfiguriranje web-mjesto VPN-a na portalu za Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [Instaliranje novog skupa stabala servisa Active Directory na Azure virtualne mreže](active-directory-new-forest-virtual-machine.md)
- [Kliknite pločicu kontroler domene servisa Active Directory za replike Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure IT Pro IaaS: osnove (01) virtualnog računala](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure IT Pro IaaS: (05) stvaranje virtualne mreže i povezivanje više lokacija](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Uvod

Osnovna preduvjeti za implementaciju Active Directory za Windows Server na virtualnim računalima za Azure razlikuju vrlo malo iz implementacija u lokalni virtualnim strojevima (i, u određenoj mjeri fizičke strojeva). Ako, na primjer, slučaju Windows Server AD DS, ako kontrolera domena (prevođenja) koji implementacije na Azure virtualnim strojevima replike u postojeći lokalnog tvrtke domene/skup stabala, a zatim Azure implementacije možete uglavnom tretirati na isti način kao što je može tretirati bilo koji drugi dodatne Windows Server Active Directory web-mjesta. To je podmreže mora biti definirano u Windows Server AD DS, web-mjesta stvorili, a zatim podmreže povezani s web-mjesta i s drugih web-mjesta pomoću odgovarajuće veze web-mjesta. Postoje, međutim, neke razlike koje su zajedničke sve Azure implementacije i neke koji razlikuju se ovisno o scenariju određene implementacije. Dvije temeljne razlike su strukturirane ispod:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azure virtualnim strojevima možda će biti potrebno povezivanje s lokalnim korporacijskom mrežom.

Povezivanje Azure virtualnim strojevima natrag na lokalni korporacijskom mrežom zahtijeva Azure virtualne mreže, koji obuhvaća web-mjesto ili web-mjesta točke virtualne privatne mreže (VPN-a) komponentu možete jednostavno povezati Azure virtualnim strojevima i lokalnog računala. Komponenta za VPN-a može omogućiti lokalne domene člana računalima pristupali čije kontrolera domena nalaze se isključivo na Azure virtualnim računalima sustava Windows Server Active Directory domene. Je važno je napomenuti da, no da ne uspijete VPN-om, o provjeri autentičnosti i ostalih operacija koje ovise o Windows Server Active Directory također neće uspjeti. Dok se korisnici mogu se prijavite se pomoću postojećih predmemoriranih vjerodajnica, sve--ravnopravnih članova ili pokušaja provjera autentičnosti poslužitelja klijenta za koju karticama još nije izdati ili prekorače zastarjele neće uspjeti.

Pogledajte [Virtualne mreže](http://azure.microsoft.com/documentation/services/virtual-network/) za Pokaz videozapisa i popis priručnici, uključujući [Konfiguriranje VPN-a web-mjesto na portalu za Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] Možete i implementirati Active Directory za Windows Server Azure virtualne mreži koji imaju povezivanje s lokalne mreže. Smjernice u ovoj temi, međutim, pretpostavlja se koristi Azure virtualne mreže jer pruža IP adresama koje su ključne za Windows Server.

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statičke IP adrese mora biti konfigurirana Azure PowerShell.

Dinamični adrese su dodijeljeno po zadanom, no pomoću cmdleta skup AzureStaticVNetIP dodijeliti statičke IP adrese. Koji se postavlja statičke IP adresu koja će održati putem servisa automatskog popravka i ponovno pokrenite/VM zatvaranja. Dodatne informacije potražite u članku [statičke interne IP adrese za virtualnim računalima](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Uvjeti i definicije

Slijedi popis koji nisu iscrpan izraza za različite Azure tehnologije koje će se pozivati u ovom članku.

- **Azure virtualnim strojevima**: nudi IaaS u servisu Azure koji omogućuje klijentima za implementaciju VMs izvodi gotovo bilo kojeg Najčešći lokalnog poslužitelja radno opterećenje.

- **Azure virtualne mreže**: umrežavanje Azure koji omogućuje korisnicima u stvaranju i upravljanje virtualne mrežama u Azure i sigurno povezivanje vlastitom lokalnog mrežne infrastrukture putem virtualne privatne mreže (VPN-a).

- **Virtualna IP adresa**: mjesto na Internetu IP adresu koja je povezana s određene kartice sučelja računala ili mreže. Servisi u oblaku dodjeljuju virtualna IP adresa za primanje mrežnog prometa koji se preusmjerava na VM Azure. Virtualna IP adresa je svojstvo servis u oblaku – koje može sadržavati neke Azure virtualnih računala. Imajte na umu da Azure virtualne mreže mogu sadržavati jedan ili više-servise u oblaku. Virtualna IP adrese navedite nativne mogućnosti za ujednačavanje opterećenja.

- **Dinamični IP adresa**: to je IP adresa koji se interne. Ga mora biti konfigurirana kao statičke IP adrese (pomoću cmdleta skup AzureStaticVNetIP) za VMs koji hostira uloge poslužitelja Kontroler/DNS.

- **Servis automatskog popravka**: postupak kojim Azure automatski vraća servisa izvodi stanje ponovno kada utvrdi da servis nije uspjela. Servis automatskog popravka jedan je od aspekte Azure koji podržava dostupnosti i otpornost. Dok je vjerojatno, rezultat praćenja servisa automatskog popravka incident za Kontroler sustavom na VM je slična neplanirano ponovno pokrenite računalo, ali ima nekoliko popratne pojave:

 - Virtualna mrežnog prilagodnika u na VM će se promijeniti
 - Promijenit će se MAC adresu virtualne mrežnog prilagodnika
 - ID procesor/procesora u VM će se promijeniti
 - Konfiguracija IP virtualne mrežnog prilagodnika neće se promijeniti kao na VM je priložena virtualne mreže i s VM IP adresa je statične.

 Nijedan od ovih ponašanja utjecati na Windows Server Active Directory jer je bez zavisnost o MAC adresu ili procesor/procesora ID-JA, a sve implementacijama Active Directory za Windows Server na Azure preporučuje se da biste pokrenuli na Azure virtualne mreže kao što je vidljivo iznad.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Je li sigurno virtualizacije Windows Server Active Directory kontrolera domena?

Implementacija sustava Windows Server Active Directory skupa na Azure virtualnim strojevima podložni smjernice za isti kao radi prevođenja lokalnog u virtualnog računala. Radi prevođenja virtualiziranom je sigurnih praksa pod uvjetom da se iza smjernice za sigurnosno kopiranje i vraćanje skupa. Dodatne informacije o ograničenjima i upute za izvođenje virtualiziranom prevođenja potražite u članku [Pokretanje kontrolera Hyper-V](https://technet.microsoft.com/library/dd363553).

Hypervisors podijelite ili trivialize tehnologije koje mogu uzrokovati probleme za mnoge raspodijeljeno sustava, uključujući Windows Server Active Directory. Na primjer, fizičke poslužitelju, možete Kloniraj na disku ili nepodržane metode vratiti stanje poslužitelja, uključujući korištenje SANs i tako dalje, ali to činite fizičke poslužitelj je mnogo teže od vraćanje virtualnog računala snimke u na hypervisor. Azure nudi funkcije koje se može uzrokovati isti neželjenog uvjet. Ako, na primjer, ne kopirajte VHD datoteka skupa umjesto izvođenje redovito sigurnosno kopiranje jer vraćanju može uzrokovati slično situaciji pomoću značajke brze snimke vraćanja.

Takve vraćanja na staro stanje predstavljanje USN mjehurićima koji može dovesti do trajno divergent stanja između skupa. Koje bi mogle probleme kao što su:

- Lingering objekata
- Nedosljedno lozinki
- Vrijednosti koje nisu usklađene atributa
- Shema nepodudarnosti ako sheme matrica je vraćen

Dodatne informacije o kako su utjecati prevođenja potražite u članku [USN i vraćanje USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Počevši od Windows Server 2012, [Dodatne zaštitu ugrađeni u AD DS](https://technet.microsoft.com/library/hh831734.aspx). Ove zaštitu zaštitite kontrolera virtualiziranom domena na ispunjavaju prethodno navedene probleme, pod uvjetom da podlozi platformu hypervisor podržava VM GenerationID. Azure podržava VM-GenerationID, što znači da kontrolera domena sa sustavom Windows Server 2012 ili noviji na Azure virtualnim računalima imati dodatne zaštitu.

> [AZURE.NOTE] Trebali biste isključiti i ponovno pokrenite VM koji se izvodi ulogu kontroler domene u Azure unutar operacijski sustav goste umjesto korištenja mogućnosti **Isključi** Azure klasični portalu. Danas, pomoću portala za klasični isključiti na VM uzrokuje VM da biste se deallocated. Deallocated VM ima prednost ne povećavanja naknade, ali je i vraća VM GenerationID koji je neželjenog za na Kontroler. Kada je ponovno VM GenerationID, invocationID AD DS baze podataka i ponovno postavljanje, skup RID se odbacuju i SYSVOL je označena kao koje nisu mjerodavne. Dodatne informacije potražite u članku [Uvod u značajke virtualizacije Active Directory Domain Services (AD DS)](https://technet.microsoft.com/library/hh831734.aspx) i [Sigurno Virtualizing DFSR](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Zašto implementacija Windows Server AD DS virtualnim računalima sustava na Azure?

Mnoge Windows Server AD DS implementacije scenarija dobro prikladniji za implementaciju kao VMs na Azure. Recimo da imate tvrtke u Europi koje su potrebne za provjeru autentičnosti korisnika na udaljenom mjestu u Azija. Tvrtke ne prethodno postavila Windows Server Active Directory prevođenja u Azija zbog trošak za implementaciju ih i ograničeno stručna znanja da biste upravljali nakon implementaciju poslužitelja. Kao rezultat zahtjeva provjeru autentičnosti iz Azija su kvalificiranom prevođenja u Europi ili lošije rezultate. U tom slučaju možete implementirati Kontroler na VM koje ste naveli morate pokrenuti unutar Azure podatkovnog centra u Azija. Taj Kontroler priložite Azure virtualne mreže koji je priključen izravno na udaljenom mjestu će poboljšati performanse provjere autentičnosti.

Azure je dobro prikladniji kao zamjenu za web-mjesta za oporavak (DR) u suprotnom skup Izrada. Relativno najniža-trošak hosting mali broj kontrolera domena i jedan virtualne mreže Azure predstavlja alternativu privlačne.

Na kraju, trebali biste implementaciju aplikacija za mrežu na Azure, kao što je SharePoint, koji zahtijeva Windows Server Active Directory, ali je ne ovisnosti na lokalnu mrežu ili tvrtke sustava Windows Server Active Directory. U ovom slučaju poslužiteljski preduvjeti uvođenje Izolirani skupa stabala na Azure da bi odgovarao SharePoint je optimalne. Ponovno implementacija mrežne aplikacije potrebna je veza s lokalne mreže i imenika Active podržano je i.

> [AZURE.NOTE] Budući da u njoj nalaze veze layer 3, komponentu VPN-a u kojoj se navode veze između Azure virtualne mreže i lokalne mreže možete omogućiti člana poslužitelje koji se izvode lokalnog odražava skupa koji se pokreću kao Azure virtualnim strojevima na Azure virtualne mreže. No ako nije dostupna u VPN komunikaciju između lokalnog računala i kontrolera domena utemeljen na Azure neće funkcionirati, rezultirajući u provjeru autentičnosti i razne pogreške.  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Contrasts između kontrolera za domena Active Directory za Windows Server na virtualnim strojevima sa sustavom Azure nasuprot lokalne implementacije

- Bilo koji Windows Server Active Directory implementacije scenariju koja sadrži više od jedne VM je potrebno koristiti Azure virtualne mreže dosljednost IP adresa. Imajte na umu da se ovaj vodič pretpostavlja da prevođenja su pokrenuti na Azure virtualne mreže.

- S lokalnim prevođenja, preporučuje se statičke IP adrese. Statičke IP adrese moguće je konfigurirati pomoću Azure PowerShell. Dodatne informacije potražite u članku [statičke interne IP adrese za VMs](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) . Ako imate nadzor sustava ili druga rješenja provjeru statičke IP adrese način konfiguriranja unutar goste operacijski sustav, svojstva prilagodnika mreže na VM možete dodijeliti iste statičke IP adrese. No imajte na umu da se mrežnog prilagodnika bit će zanemarene ako na VM prolazi kroz servisa automatskog popravka ili isključiti na portalu klasični i sadrži deallocated na njegovu adresu. U tom slučaju statičke IP adrese unutar za goste morat ćete se ponovno postaviti.

- Implementacija VMs na virtualne mreže ne impliciraju (ili obavezan) povezivanje natrag na lokalnu mrežu; virtualne mreže samo omogućuje tu mogućnost. Morate stvoriti virtualne mreže za privatne komunikaciju između Azure i lokalne mreže. Morate uvesti VPN krajnjoj točki lokalne mreže. Azure otvorenom VPN-om za lokalnu mrežu. Dodatne informacije potražite u članku [Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md) i [Konfiguriranje web-mjesto VPN-a na portalu za Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] Mogućnost da biste [stvorili točku web VPN-a](../vpn-gateway/vpn-gateway-point-to-site-create.md) je moguće izravno povezati pojedinačna računala utemeljen na sustavu Windows Azure virtualne mreže.

- Bez obzira na to stvarate li virtualne mreže ili ne, Azure naknade za izlazne promet, ali ne ingress. Različite mogućnosti dizajna za Windows Server Active Directory može utjecati na koliko izlazne promet je generirao implementacije. Ako, na primjer, implementacija kontroler domene samo za čitanje (RODC) ograničenjima izlazne promet jer ne replicirati izlaznog. No odluka za implementaciju sustava RODC mora biti weighed na temelju potreba za izvođenje operacije pisanja izvoditi na kontroler domene i [kompatibilnost](https://technet.microsoft.com/library/cc755190) aplikacija i servisa na web-mjestu imaju RODCs. Dodatne informacije o promet troškova potražite u članku [Azure cijene pri kratak](http://azure.microsoft.com/pricing/).

- Dok su potpuni kontrolirati putem koje poslužitelj resursa za korištenje VMs informacije o lokalnom, kao što su RAM-a, disk je veličina i tako dalje, na Azure morate odabrati s popisa veličina konfiguriranog poslužitelja. Za Kontroler, na disku podatke potrebne osim disk operacijski sustav da bi se spremanje baze podataka sustava Windows Server Active Directory.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Možete implementirati Windows Server AD FS na Azure virtualnih računala?

Da, možete implementirati Windows Server AD FS na Azure virtualnim strojevima i primijeniti na [Praktični savjeti za implementaciju AD FS](https://technet.microsoft.com/library/dn151324.aspx) lokalnog ravnomjerno AD FS implementacije na Azure. No neke najbolje prakse kao što su uravnoteženje opterećenja i visoke dostupnosti potrebni tehnologije osim što AD FS nudi sam. Potrebno je navesti po podlozi infrastrukture. Pogledajmo pregledajte neke od tih najbolje prakse i potražite u članku kako ih možete postići pomoću Azure VMs i Azure virtualne mreže.

1. **Nikad ne izlažu sigurnosnih tokena (STS) poslužitelji izravno s Internetom.**

    To je važno jer se STS problemi sigurnosnih tokena. Zbog toga STS poslužitelje kao što je poslužitelja za AD FS treba je tretirati s jednaku razinu zaštite kao kontroler domene. Ako je STS ugrožena, zlonamjerni korisnici imaju mogućnost problema pristupna tokena potencijalno koja sadrži zahtjevima njihove odabiru potrebe za oslanjanjem proizvođača aplikacije i poslužitelji STS u pouzdanim tvrtkama ili ustanovama.

2. **Implementacija servisa Active Directory kontrolera domena za sve domene korisnika u mrežu na isti kao poslužitelja za AD FS.**

    AD FS poslužitelji koristiti Active Directory Domain Services za provjeru autentičnosti korisnika. Preporučuje se za implementaciju kontrolera domena na istoj mreži kao poslužitelja za AD FS. To omogućuje poslovanje za slučaj da veza između Azure mreže i lokalne mreže prekine i omogućuje donjem Latencija i poboljšane performanse za prijave.

3. **Implementacija više čvorove AD FS visoke dostupnosti i za ujednačavanje opterećenja.**

    U većini slučajeva pogreške aplikacije koja omogućuje AD FS nije prihvatljiva jer aplikacije koji zahtijevaju sigurnosnih tokena često su zaštita njihove privatnosti ovise od ključne važnosti. Zbog toga i jer u kritične putanje za pristup zaštita njihove privatnosti ovise ključnih aplikacija sada nalazi AD FS, servisa AD FS mora biti vrlo dostupne putem više proxy poslužitelja za AD FS i poslužitelja za AD FS. Da biste postigli raspodjele zahtjeva za učitavanje balancers uvode obično ispred proxy poslužitelja za AD FS i poslužitelja za AD FS.

4. **Implementacija čvorove Proxy Web aplikacije za pristup Internetu.**

    Kada korisnici moraju imati pristup aplikacijama koje su zaštićeni servisom AD FS, servisa AD FS mora biti dostupan putem Interneta. To se postiže uvođenjem Web Proxy aplikacije servisa. Preporučuje implementacije više čvor u svrhu visoke dostupnosti i uravnoteženje opterećenja.

5. **Ograničiti pristup čvorove Proxy aplikacije Web internoj mreži resursi.**

    Da biste vanjskim korisnicima omogućili pristup AD FS putem Interneta, morate uvesti čvorove Proxy aplikacije Web (ili Proxy za AD FS u starijim verzijama sustava Windows Server). Čvorovi proxy poslužitelja web-aplikacije izravno izložen putem Interneta. Nisu potrebne da biste se pridružili za domenu, a oni samo potreban je pristup poslužitelja za AD FS putem TCP priključci 443 i 80. Preporučuje se blokirana je komunikacija sa svim ostalim računalima (osobito kontrolera domena).

    To je obično postići lokalnog pomoću programa DMZ. Vatrozidima pomoću načina whitelist operacije da biste ograničili promet iz s DMZ lokalne mreže (to jest, samo je dopušteno promet na navedeni IP adresa i putem navedeni priključci i sve druge promet blokiran).

Na sljedeći dijagram prikazuje tradicionalni lokalne implementacije AD FS.

![Dijagram tradicionalni lokalnu implementaciju Active Directory Federation Services](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

No Budući da Azure ne nudi nativni, mogućnost punu vatrozid druge mogućnosti potrebno će se koristiti za ograničavanje promet. Sljedeća tablica prikazuje svake mogućnosti i prednosti i nedostatke.

| Mogućnost | Prednosti | Nedostatak |
| ------ | --------- | ------------ |
| [Azure mreže ACL-a](virtual-networks-acl.md) | Manji skup i jednostavniji Početna konfiguracija | Dodatni mrežna konfiguracija ACL potreban sve nove VMs dodaju za implementaciju |
| [Barracuda Pokazuju vatrozid](https://www.barracuda.com/products/ngfirewall) | Način Whitelist postupak i zahtijeva bez mrežna konfiguracija ACL-a | Povećana trošak i složenosti za početnog postavljanja |

Više razine korake za implementaciju AD FS u ovom slučaju su na sljedeći način:

1. Stvaranje virtualne mreže s povezivanjem više lokacija pomoću VPN-a ili [ExpressRoute](http://azure.microsoft.com/services/expressroute/).

2. Implementacija kontrolera domena na virtualne mreže. Ovaj korak nije obavezno, ali preporučuje.

3. Implementacija domene pridruženo poslužitelja za AD FS na virtualne mreže.

4. Stvaranje programa [Interna učitavanja raspoređen skup](http://azure.microsoft.com/blog/internal-load-balancing/) sadrži poslužitelja za AD FS i koristi novi privatne IP adrese unutar virtualne mreže (dinamičku IP adresu).

  1. Ažuriranje DNS-a da biste stvorili FQDN tako da pokazuje na privatne IP adrese (dinamički) skupa Interna rasporediti opterećenje.

5. Stvaranje servis u oblaku (ili zasebnom virtualne mreže) za čvorove Proxy aplikacije za Web.

6. Implementacija čvorove Proxy aplikacije za Web u oblaku ili virtualne mreže

  1. Stvaranje skupa vanjskih rasporediti opterećenje koja obuhvaća čvorove Proxy aplikacije za Web.

  2. Ažurirajte naziv vanjskog DNS (FQDN) tako da pokazuje na cloud servisa javnu IP adresu (virtualne IP adresa).

  3. Konfiguriranje servisa AD FS proxy poslužitelja za korištenje FQDN koji odgovara skup Interna rasporediti opterećenje za poslužitelje servisa AD FS.

  4. Ažurirajte utemeljene na zahtjevima web-mjesta da biste koristili vanjske FQDN davatelju zahtjevima.

7. Ograničiti pristup između Web Proxy aplikacije u bilo kojem računalu u AD FS virtualne mreže.

Da biste ograničili promet, skup uravnoteženja za Azure Interna opterećenja potrebno je konfigurirati za samo promet za TCP priključci 80 i 443, a ostali promet za interne dinamičke IP adrese skupa rasporediti opterećenje se prekine.

![Dijagram ACL-ADFS mreže a putem TCP 443 + 80 je dopušteno.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Promet na poslužitelje servisa AD FS će biti zakonskim samo sljedećih izvora:

- Na Azure Interna opterećenja.
- IP adrese administrator lokalne mreže.

> [AZURE.WARNING] Dizajn morate biste čvorove Proxy aplikacije Web onemogućili da druge VMs u Azure virtualne mreže ili bilo kojeg mjesta na lokalnu mrežu. Koje se mogu izvršiti konfiguriranjem pravila vatrozida potražite lokalnog za usmjeravanje Express veze ili uređaj VPN-a za web-mjesto VPN veze.

Nedostatak da biste tu mogućnost je potrebno da biste konfigurirali mrežu ACL-a za više uređaja, uključujući interne opterećenja, poslužitelja za AD FS i još neki poslužitelji koji se dodaju virtualne mreže. Ako je bilo kojeg uređaja dodan implementacijskih niste konfigurirali mreže ACL-a da biste ograničili promet, cijeli implementacije može biti rizik. Ako ikad promijenite IP adrese čvorove Proxy aplikacije za Web, mreže ACL-a mora ponovno pokrenuti (što znači u proxy poslužitelji treba biti konfigurirana tako da pomoću [statičke IP adrese dinamički](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![ADFS na Azure s mrežom ACL-a.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Druga je mogućnost da biste koristili [Vatrozid Pokazuju Barracuda](https://www.barracuda.com/products/ngfirewall) uređaj da biste odredili promet između proxy poslužitelje servisa AD FS i poslužitelja za AD FS. Ova mogućnost uspostavu najbolje prakse za sigurnost i visoke dostupnosti i zahtijeva manje administracije nakon početnog postavljanja jer Barracuda Pokazuju vatrozida potražite nudi whitelist način administracije vatrozid i moguće je instalirati izravno na Azure virtualne mreže. Koji nema potrebe da biste konfigurirali mreže ACL-a uvijek se implementacijskih dodaje novi poslužitelj. No tu mogućnost dodaje početnog uvođenja složenosti i cijene.

U ovom slučaju dvije virtualne mreže uvode se umjesto jednog. Ne možemo ćete nazovite ih VNet1 i VNet2. VNet1 sadrži na proxy poslužitelji a VNet2 na STSs i mrežnu vezu natrag u poslovnoj mreži. VNet1 stoga je fizički (koriste Premda gotovo) Izolirani iz VNet2 i, shodno tome, poslovnoj mreži. VNet1 pa je povezano s VNet2 pomoću posebnih tunneling tehnologije poznati kao prijenosa neovisno mreže arhitektura (TINA). Tunelom TINA je pridružen svakom virtualne mreže pomoću Barracuda Pokazuju vatrozid – jedan Barracuda za svaku od virtualne mreže.  Za visoku dostupnost, preporučuje se implementacije dva Barracudas na svakom virtualne mreže; jedan Aktivno, u pasivni. Oni nude iznimno obogaćenog firewalling mogućnosti koje omogućuju nam oponašanje operacija tradicionalni lokalnog DMZ u Azure.

![ADFS na Azure s vatrozidom.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Dodatne informacije potražite u članku [AD FS: proširivanje zahtjevima umu lokalnog sučelja aplikacije s Internetom](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Zamjena za AD FS implementacije ako je SSO za Office 365 samostalno cilja

Postoji drugi zamjenski implementacije AD FS sasvim ako je vaš cilj samo da biste omogućili prijave za Office 365. U tom slučaju možete jednostavno implementacija DirSync lozinku sinkronizaciju lokalnog sustava i postigli isti rezultat završetka s minimalnim implementacije složenosti jer takvog ne zahtijeva AD FS ili Azure.

U sljedećoj su tablici uspoređuje funkcioniranje procesa prijave sa ili bez implementacija AD FS.

| Office 365 jednom prijave pomoću servisa AD FS i DirSync | Office 365 isti prijave pomoću DirSync + sinkronizaciju lozinke |
| ------------- | ------------- |
| 1. korisnik prijavljuje s korporacijskom mrežom i provjere autentičnosti u sustavu Windows Server Active Directory. | 1. korisnik prijavljuje s korporacijskom mrežom i provjere autentičnosti u sustavu Windows Server Active Directory. |
| 2. se korisnik pokuša pristupiti sustavu Office 365 (sam @contoso.com). | 2. se korisnik pokuša pristupiti sustavu Office 365 (sam @contoso.com). |
| 3. office 365 preusmjerava korisnika Azure AD. | 3. office 365 preusmjerava korisnika Azure AD. |
| 4. jer Azure AD nije moguće provjeriti autentičnost korisnika i razumije postoji pouzdanost AD FS lokalnog sustava, preusmjerava korisnika za AD FS. | 4. azure AD ne možete izravno prihvatite Kerberos karte, a ne pouzdani odnos postoji pa ga zahtjeve korisnik unijeti vjerodajnice. |
| 5. korisniku šalje Kerberos karata STS za AD FS. | 5. korisnik unese istu lozinku za lokalni i Azure AD ovjerava ih protiv korisničko ime i lozinku koja je sinkronizirati tako da DirSync. |
| 6. AD FS pretvorbe karata Kerberos za potrebne tokena oblik/zahtjevima i preusmjerava korisnika Azure AD. | 6. azure AD preusmjerava korisnika u Office 365. |
| 7. korisnik potvrđuje da biste Azure AD (drugi transformaciju događa). |  7. korisnika mogu se prijaviti Office 365 i OWA pomoću token Azure AD. |
| 8. azure AD preusmjerava korisnika u Office 365. |  |
| 9. korisnik tihu prijavljen Office 365. |  |

U sustavu Office 365 pomoću DirSync s scenarij sinkronizaciju lozinke (bez AD FS), jedinstvenu prijavu zamijenjen je "isti prijave" mjesto "jednako" jednostavno znači da korisnici morate ponovno unesite iste vjerodajnice lokalnog prilikom pristupanja sustavu Office 365. Imajte na umu da se ove podatke možete pamti preglednik korisnika pomažu smanjenju sljedeće upute.

### <a name="additional-food-for-thought"></a>Dodatni misli za hranu

- Ako pokrenete proxy za AD FS na Azure virtualnog računala, potrebna je veza s poslužitelja za AD FS. Ako su lokalni, preporučuje se pod utjecajem povezivanje VPN-a web-mjesto koje ste dobili od virtualne mreže da biste omogućili čvorove Proxy Web aplikacije možete komunicirati s njihovim poslužitelja za AD FS.

- Ako implementacija poslužitelj za ADFS na Azure virtualnog računala potrebna je veza s kontrolera domena u sustavu Windows Server Active Directory, atribut trgovine i konfiguracijske baze podataka, a mogu zahtijevati programa ExpressRoute ili web-mjesto VPN vezu između Azure virtualne mreže i lokalne mreže.

- Troškovi primjenjuju se na sve promet iz Azure virtualnim strojevima (izlazne promet). Ako je trošak vožnju faktor, preporučuje se da implementacija čvorove Proxy Web aplikacije na Azure, ostavite u AD FS poslužitelje lokalnog. Ako na Azure virtualnim strojevima kao i uvode se poslužitelja za AD FS, dodatne troškove će biti nastale za provjeru autentičnosti lokalne korisnike. Izlazne promet uključuje trošak bez obzira na to je li se traversing na ExpressRoute ili VPN veza web-mjesto.

- Ako odlučite da biste koristili mogućnosti za visoke dostupnosti AD FS poslužitelja za ujednačavanje opterećenja za Azure, izvorni poslužitelj, imajte na umu da opterećenja nudi probes koji se koriste za utvrđivanje stanja virtualnim strojevima unutar servisa u oblaku. U slučaju Azure virtualnim strojevima (umjesto weba ili tempiranja ulogama), prilagođene probni mora se koristiti jer agent koji odgovara probes zadani ne postoji Azure virtualnim računalima. Da biste postigli jednostavnosti, možda koristite prilagođene probni TCP – to potreban je samo da TCP (TCP SYN segmenta šalje i odgovorili s TCP SYN ACK segmenta) biti uspješno uspostaviti vezu da biste odredili stanje virtualnog računala. Možete konfigurirati prilagođene probni da biste koristili neki TCP priključak na koji su virtualnim strojevima aktivno listening.

> [AZURE.NOTE] Strojeva koje je potrebno da biste otkrili isti skup priključci izravno na Internetu (na primjer, priključak 80 i 443) ne možete zajednički koristiti iste servis u oblaku. Dakle, preporučuje se stvorite namjenski oblaka služba za Windows Server AD FS poslužitelja da biste izbjegli potencijalne preklapa između priključke za aplikaciju i Windows Server AD FS.

## <a name="deployment-scenarios"></a>Scenariji za implementaciju

U sljedećoj sekciji navode commonplace implementaciju scenariji da biste privukli pažnju na važne stavke koje morate uzeti u obzir. Svaki scenarij sadrži veze na dodatne informacije o odluke i čimbenici koje treba uzeti u obzir.

1. [AD DS: Implementacija AD DS-umu aplikacijom bez zahtjeva za povezivanje s korporacijskom mrežom](#BKMK_CloudOnly)

    Ako, na primjer, na mjesto na Internetu sustava SharePoint servis je implementiran na Azure virtualnog računala. Aplikacija ima bez ovisnosti o resursima korporacijskom mrežom. Aplikacije potreban Windows Server AD DS, ali ne zahtijeva tvrtke AD DS Windows Server.

2. [AD FS: Proširivanje zahtjevima umu lokalnog sučelja aplikacije na Internetu](#BKMK_CloudOnlyFed)

    Na primjer, zahtjevima umu aplikacije koji je uspješno distribuiranih lokalnog i koristilo tvrtke osoba mora postalo dostupno putem Interneta. Aplikacija potrebno je moguće pristupiti izravno putem Interneta i poslovni partneri korištenju vlastite tvrtke identiteta i postojeće korisnike tvrtke.

3. [AD DS: Implementaciju aplikacija za Windows Server AD DS-umu koje je potrebna je veza s korporacijskom mrežom](#BKMK_HybridExt)

    Ako, na primjer, LDAP umu aplikacije koja podržava Windows integriranu provjeru autentičnosti i koristi Windows Server AD DS kao spremište podataka o konfiguraciji i korisnički profil je implementiran na Azure virtualnog računala. Je poželjno za aplikaciju za korištenje postojeće tvrtke Windows Server AD DS i davanje jedinstvenu prijavu. Aplikacija je znati zahtjevima.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Implementacija AD DS-umu aplikacijom bez zahtjeva za povezivanje s korporacijskom mrežom

![Samo oblak AD DS implementaciju](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**Slika 1**

#### <a name="description"></a>Opis

SharePoint je implementiran na Azure virtualnog računala i aplikaciju ima bez ovisnosti korporacijskom mrežom resursa. Aplikacije potreban Windows Server AD DS ali ne *ne* zahtijevaju tvrtke Windows Server AD DS. Nema Kerberos pridruženim trusts potrebni su ili jer su korisnici samostalnih dodijeljenu pomoću aplikacije u Windows Server AD DS domenu koja se nalazi u oblaku na Azure virtualnih računala.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scenarij pitanja i kako tehnologije područja primijenite scenarija

- [Mrežna topologija](#BKMK_NetworkTopology): Stvaranje Azure virtualne mreže bez više lokacija povezivanja (poznat i kao web-mjesto s povezivanjem).

- [Konfiguraciju implementacije Kontroler](#BKMK_DeploymentConfig): implementacija novi kontroler u novu, jednu domenu, Windows Server Active Directory skupa stabala. To se primjenjuje uz Windows DNS poslužitelj.

- [Topologija web-mjesta za Windows Server Active Directory](#BKMK_ADSiteTopology): korištenje zadano mjesto Windows Server Active Directory (sva računala bit će u zadanom ime-fonetski web-mjesta –).

- [IP adresiranja i DNS- a](#BKMK_IPAddressDNS):

 - Postavljanje statičke IP adrese za kontroler domene pomoću cmdleta ljuske PowerShell za postavljanje AzureStaticVNetIP Azure.
 - Instaliranje i konfiguriranje DNS poslužitelja za Windows na controller(s) domene na Azure.
 - Konfiguriranje svojstava virtualne mreže s imenom i IP adrese VM koji hostira uloge poslužitelja Kontroler i DNS- a.

- [Globalni katalog](#BKMK_GC): prvi Kontroler u u skup stabala mora biti globalni katalog poslužitelj. Dodatne skupa trebali biste konfigurirati kao GCs jer u jednu domenu skupa-stabala globalni katalog ne zahtijeva napravili dodatne iz kontroler domene.

- [Položaj Windows Server AD DS baze podataka i SYSVOL](#BKMK_PlaceDB): dodavanje podatkovni disk skupa koji se izvodi kao Azure VMs da bi se spremanje baze podataka sustava Windows Server Active Directory, zapisnika i SYSVOL.

- [Sigurnosno kopiranje i vraćanje](#BKMK_BUR): odredite mjesto na koje želite pohraniti sigurnosne kopije stanja sustava. Ako je potrebno, dodajte drugi disk podataka VM Kontroler za spremanje sigurnosne kopije.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: proširivanje zahtjevima umu lokalnog sučelja aplikacije na Internetu

![Vanjski pristup s povezivanjem više lokacija](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**Slika 2**

#### <a name="description"></a>Opis

Zahtjevima umu aplikacije koji je uspješno distribuiranih lokalnog i tvrtke korisnicima mora biti dostupno izravno putem Interneta. Aplikacija služi kao u sučelju web s bazom podataka SQL u kojoj se pohranjuju podaci. SQL Server koristi aplikacija i nalaze se na mreži tvrtke. Dva STSs Windows Server AD FS i raspoređivača opterećenja su distribuiranih lokalnog za pristup tvrtke korisnicima. Aplikacija sada se mora uz moguć izravno putem Interneta i poslovni partneri korištenju vlastite tvrtke identiteta i postojeće korisnike tvrtke.

U trud da biste pojednostavnili i zadovoljavaju potrebe implementaciji i konfiguraciji taj novi zahtjev je navedena je dvije dodatne web frontends i dva na Azure virtualnim strojevima biti instaliran Windows Server AD FS proxy poslužitelji. Sve četiri VMs će biti izložen izravno putem Interneta i objavit ćemo zajedno veza s lokalne mreže pomoću Azure mrežnu virtualne mogućnost VPN-a za web-mjesto.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scenarij pitanja i kako tehnologije područja primijenite scenarija

- [Mrežna topologija](#BKMK_NetworkTopology): Stvaranje Azure virtualne mreže i [Konfiguriranje povezivanja s više lokacija](../vpn-gateway/vpn-gateway-site-to-site-create.md).

 > [AZURE.NOTE] Za svaku certifikati Windows Server AD FS provjerite da URL definirani unutar predložak certifikata i konačne certifikati mogu pristupiti tako da na Windows Server AD FS instanci koje su pokrenute na Azure. To može zahtijevati više lokacija veza sa sustavom dijelove infrastruktura za PKI. Za primjer ako krajnje točke CRL-utemeljen na LDAP i hostira isključivo lokalnog, zatim više lokacija povezivanje bit će potrebno. Ako nije poželjno, možda će biti potrebno pomoću certifikata izdala CA čije CRL-Ova je dostupan putem Interneta.

- [Konfiguriranje usluge oblaka](#BKMK_CloudSvcConfig): imate dvije servise u oblaku u redoslijedu omogućuje dvije uravnoteženja virtualne IP adrese. Virtualna IP adresa prvi servisa u oblaku, bit će usmjereni na dva Windows Server AD FS proxy VMs na priključke 80 i 443. Windows Server AD FS proxy VMs će se tako da upućuju na IP adresu na lokalni-opterećenja prednju STSs za Windows Server AD FS. Virtualna IP adresa drugog servisa u oblaku, preusmjeravat će se dva VMs ponovno pokrenuti sučelju web na priključke 80 i 443. Konfiguriranje prilagođenih probni da biste bili sigurni raspoređivača opterećenja samo usmjerava promet na funkcionalnu Windows Server AD FS proxy i web-sučelju VMs.

- [Konfiguraciju vanjskog poslužitelja](#BKMK_FedSrvConfig): Konfiguriranje Windows Server AD FS kao vanjskom poslužitelju (STS) radi generiranja sigurnosnih tokena za skup stabala Windows Server Active Directory stvorene u oblak. Postavljanje vanjskog pristupa zahtjevima davatelja pouzdanost odnose različite partneri želite prihvatiti identitete iz i konfiguriranje potrebe za oslanjanjem strana pouzdani odnosi s različitih aplikacija želite generirati tokeni za.

    U većini slučajeva, Windows Server AD FS proxy poslužitelji su u implementirana na mjesto na Internetu kapaciteta sigurnosnih razloga dok njihove verzija vanjski pristup za Windows Server AD FS ostaju Izolirani iz izravno s Internetom. Bez obzira na to scenariju implementaciju, morate konfigurirati servis u oblaku s virtualna IP adresa koje vam ponuditi javno prikazanog IP adresa i priključka koji je moći učitavanja saldo preko dvije instance Windows Server AD FS STS ili proxy instance.

- [Konfiguriranje visoke dostupnosti Windows Server AD FS](#BKMK_ADFSHighAvail): preporučuje se da uvođenje farmu Windows Server AD FS s najmanje dva poslužitelja za prebacivanje i uravnoteženje opterećenja. Možda razmislite o korištenju Windows interne baze podataka (RID) za Windows Server AD FS konfiguracije podatke, a koriste Interna učitavanja ujednačavanje mogućnost u Azure za raspodjelu zahtjevi svim poslužiteljima u farmi.

Dodatne informacije potražite u članku [Vodič za implementaciju AD DS](https://technet.microsoft.com/library/cc753963).


### <a name="BKMK_HybridExt"></a>3. AD DS: Implementaciju aplikacija za Windows Server AD DS-umu koje je potrebna je veza s korporacijskom mrežom

![Uvođenje AD DS-lokacija](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**Slika 3**

#### <a name="description"></a>Opis

LDAP umu aplikacija je implementiran na Azure virtualnog računala. Podržava Windows integriranu provjeru autentičnosti i koristi Windows Server AD DS kao spremište za konfiguraciju i korisničke podatke profila. Cilj je za aplikaciju za korištenje na postojeće tvrtke Windows Server AD DS i davanje jedinstvenu prijavu. Aplikacija je znati zahtjevima. Korisnicima je potrebna za pristup aplikaciji izravno s Interneta. Da biste optimizirali za performanse i cijena, ona navedena je da se dva kontrolera dodatnu domenu koje su dio korporacijsku domenu implementirati uz aplikaciju na Azure.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scenarij pitanja i kako tehnologije područja primijenite scenarija

- [Mrežna topologija](#BKMK_NetworkTopology): Stvaranje Azure virtualne mreže s [lokacija povezivanje](../vpn-gateway/vpn-gateway-site-to-site-create.md).

- [Način instalacije](#BKMK_InstallMethod): implementacija replike prevođenja iz tvrtke domene Active Directory za Windows Server. Za replike Kontroler, instalirajte Windows Server AD DS na na VM i po želji koristite značajku instalirati iz medijske sadržaje (IFM) da biste smanjili količinu podataka koje je potrebno je replicirati na novi Kontroler tijekom instalacije. Praktični vodič, potražite u članku [Instalacija kontroler domene servisa Active Directory replike na Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Čak i ako koristite IFM, možda će učinkovitije sastavljanje u virtualne Kontroler lokalnog i prijelaz na cijeli virtualne na tvrdom disku (VHD) u oblak umjesto replikaciju Windows Server AD DS tijekom instalacije. Za sigurnost, preporučuje se da izbrišete na VHD iz lokalne mreže kada kopirate Azure.

- [Topologija web-mjesta za Windows Server Active Directory](#BKMK_ADSiteTopology): Stvaranje nove web-mjesta Azure Active Directory web-mjestima i servisima. Stvaranje objekta podmreže Windows Server Active Directory za predstavljanje Azure virtualne mreže i dodavanje podmreži web-mjesto. Stvorite novu vezu web-mjesta koja sadrži novog Azure web-mjesta i web-mjesta u kojoj se nalazi da biste mogli upravljati i optimizirati promet Windows Server Active Directory i iz Azure Azure virtualne mreže VPN krajnjoj točki.

- [IP adresiranja i DNS- a](#BKMK_IPAddressDNS):

 - Postavljanje statičke IP adrese za kontroler domene pomoću cmdleta ljuske PowerShell za postavljanje AzureStaticVNetIP Azure.
 - Instaliranje i konfiguriranje DNS poslužitelja za Windows na controller(s) domene na Azure.
 - Konfiguriranje svojstava virtualne mreže s imenom i IP adrese VM koji hostira uloge poslužitelja Kontroler i DNS- a.

- [Zemlj raspodijeljenih skupa](#BKMK_DistributedDCs): Konfiguriranje dodatne virtualne mreže prema potrebi. Ako vaša topologija web-mjesta servisa Active Directory zahtijeva prevođenja u geographies koji odgovaraju za različite regije Azure nego što želite da biste stvorili web-mjesta servisa Active Directory sukladno tome.

- [Samo za čitanje skupa](#BKMK_RODC): možda implementacija sustava RODC Azure web-mjestu, ovisno o potrebama za izvođenje pisanje operacije protiv kontroler domene i kompatibilnost aplikacija i servisa sustava u web-mjesta s RODCs. Dodatne informacije o kompatibilnosti aplikacija potražite u članku [Vodič za kompatibilnost aplikacija za kontrolera domene samo za čitanje](https://technet.microsoft.com/library/cc755190).

- [Globalni katalog](#BKMK_GC): GCs su vam potrebne da biste servisne zahtjeve za prijavu u multidomain šuma. Ako ne implementirali GC Azure web-mjestu, će plaćati izlazne promet troškove zahtjeva za provjeru autentičnosti prouzročiti upita GCs druga web-mjesta. Da biste minimizirali tu promet, možete omogućiti Univerzalna grupa članstvo predmemoriju za web-mjesto Azure Active Directory web-mjestima i servisima.

    Ako pokrenete GC, Konfiguriranje veze web-mjesta i troškove veze web-mjesta tako da GC Azure web-mjestu nije Preferirani kao izvor Kontroler tako da druge GCs koje su potrebne za replikaciju iste particije djelomične domene.

- [Položaj Windows Server AD DS baze podataka i SYSVOL](#BKMK_PlaceDB): dodavanje podatkovni disk skupa koji se izvode na Azure VMs da bi se spremanje baze podataka sustava Windows Server Active Directory, zapisnika i SYSVOL.

- [Sigurnosno kopiranje i vraćanje](#BKMK_BUR): odredite mjesto na koje želite pohraniti sigurnosne kopije stanja sustava. Ako je potrebno, dodajte drugi disk podataka VM Kontroler za spremanje sigurnosne kopije.

## <a name="deployment-decisions-and-factors"></a>Uvođenje odluka i čimbenika

U ovoj su tablici navedene područja tehnologije Windows Server Active Directory koji su utjecati u prethodnim scenarijima i odgovarajuće odluke treba uzeti u obzir s vezama na više detalja u nastavku. Nekim područjima tehnologije možda ne vrijedi za svaki scenarij implementaciju, a nekim područjima tehnologije možda više ključnih za implementaciju scenarij od druga područja tehnologije.

Na primjer, ako implementacija replike Kontroler virtualne mreži, a vaše skupa stabala sadrži samo jednu domenu, zatim odabirom implementacija poslužitelja za globalni katalog u tom slučaju neće biti ključna za implementaciju scenarij jer će stvoriti sve dodatne replikacije preduvjeti. S druge strane, ako se skup stabala ima nekoliko domena, zatim odluka za implementaciju globalni katalog virtualnih mreži može utjecati na prikaz raspoložive propusnosti, performanse, provjere autentičnosti, direktorija pretraživanja i tako dalje.

| Područje tehnologija za Windows Server Active Directory | Odluka | Čimbenici |
| ---- | ---- | ---- |
| [Mrežna topologija](#BKMK_NetworkTopology) | Stvorite virtualne mreže? | <li>Preduvjeti za pristup Corp resursi</li> <li>Provjera autentičnosti</li> <li>Upravljanje računima</li> |
| [Konfiguraciju Kontroler implementacije](#BKMK_DeploymentConfig) | <li>Implementacija zasebnom skupa stabala bez sve trusts?</li> <li>Implementacija u novi skup stabala s vanjskim pristupom?</li> <li>Implementacija skupa stabala za novo pouzdano skupa stabala Windows Server Active Directory ili Kerberos?</li> <li>Proširivanje skupa stabala Corp uvođenjem replike Kontroler?</li> <li>Proširivanje skupa stabala Corp uvođenjem novu domenu podređeni ili stabla domene?</li> | <li>Sigurnost</li> <li>Usklađenost</li> <li>Trošak</li> <li>Stabilnosti i kvara odstupanje</li> <li>Kompatibilnost aplikacija</li> |
| [Topologija web-mjesta za Windows Server Active Directory](#BKMK_ADSiteTopology) | Kako vam konfigurirati podmreže, web-mjesta i veze web-mjesta s Azure virtualne mreže optimiziranje promet i minimiziranja trošak? | <li>Definicija podmreže i web-mjesta</li> <li>Svojstva veze web-mjesta i promjena obavijesti</li> <li>Sažimanje replikacije</li> |
| [IP adresiranja i DNS- a](#BKMK_IPAddressDNS) | Upute za konfiguriranje IP adrese i razlučivanje naziva? | <li>Pomoću cmdleta za korištenje skupa AzureStaticVNetIP da biste dodijelili statičke IP adrese</li> <li>Instalirajte Windows Server DNS poslužitelj i konfiguriranje svojstava virtualne mreže s imenom i IP adrese VM koji hostira uloge poslužitelja Kontroler i DNS- a</li> |
| [Dovršen distributed za zemlj.](#BKMK_DistributedDCs) | Kako je replicirati na prevođenja zasebnom virtualne mrežama? | Ako vaša topologija web-mjesta servisa Active Directory zahtijeva prevođenja u geographies koja odgovara različite regije Azure, nego što želite da biste stvorili web-mjesta servisa Active Directory sukladno tome. [Konfiguracija virtualne mreže virtualne mreže povezivanje](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) za replikaciju između kontrolera domena na zasebnom virtualne mreže. |
| [Samo za čitanje skupa](#BKMK_RODC) | Koristite samo za čitanje ili pisanje prevođenja? | <li>Filtriranje HBI/PII atributa</li> <li>Filtriranje tajne</li> <li>Ograničenje odlazni promet</li> |
| [Globalni katalog](#BKMK_GC) | Instalirati globalni katalog? | <li>Za skup stabala jednu domenu, provjerite sve GCs skupa</li> <li>Za multidomain skup stabala GCs su potrebni za provjeru autentičnosti</li> |
| [Način instalacije](#BKMK_InstallMethod) | Kako se instalira Kontroler u Azure? | Oba: <li>Instalacija AD DS pomoću komponente Windows PowerShell ili Dcpromo</li> <li>Premještanje VHD na lokalni virtualne Kontroler</li> |
| [Položaj Windows Server AD DS baze podataka i SYSVOL](#BKMK_PlaceDB) | Gdje želite spremiti Windows Server AD DS baze podataka, zapisnika i SYSVOL? | Promijenite Dcpromo.exe zadane vrijednosti. Ove ključnih servisa Active Directory datoteke *mora* nalaziti na diskova Azure podataka umjesto diskova operacijski sustav koji implementira zapisivanju. |
| [Sigurnosno kopiranje i vraćanje](#BKMK_BUR) | Kako se zaštitili i oporavak podataka? | Stvaranje sigurnosne kopije stanja sustava |
| [Konfiguraciju vanjskog poslužitelja](#BKMK_FedSrvConfig) | <li>Implementacija u novi skup stabala s vanjskim pristupom u oblaku?</li> <li>Uvođenje servisa AD FS lokalnog i izlaganje proxy poslužitelja u oblak?</li> | <li>Sigurnost</li> <li>Usklađenost</li> <li>Trošak</li> <li>Pristup aplikacijama po poslovni partneri</li> |
| [Konfiguriranje usluge oblaka](#BKMK_CloudSvcConfig) | Servis u oblaku implicitno je implementirana prvi put stvorite virtualnog računala. Potrebne da biste implementirali servise dodatne oblak? | <li>Na VM ili VMs zahtijeva Izravni izlaganje s Internetom?</li> <li> Servis zahtijeva ujednačavanje opterećenja?</li> |
| [Vanjski pristup poslužiteljski preduvjeti za javne i privatne IP adresama (dinamičke IP nasuprot virtualne IP)](#BKMK_FedReqVIPDIP) | <li>Instancu sustava Windows Server AD FS mora može otvoriti izravno s Internetom?</li> <li>Zahtijeva li aplikacija uvodi u oblaku vlastitu mjesto na Internetu IP adresa i priključaka?</li> | Stvaranje jednog-servis u oblaku za svaki virtualne IP adresu koja je potrebno za implementaciju sustava |
| [Konfiguriranje komponente Windows Server AD FS visoke dostupnosti](#BKMK_ADFSHighAvail) | <li>Koliko čvorove Moje farmi poslužitelja Windows Server AD FS?</li> <li>Koliko čvorove za implementaciju u Windows Server AD FS proxy farme?</li> | Odstupanje stabilnosti i isječci |

### <a name="BKMK_NetworkTopology"></a>Mrežna topologija

Da bi se ispunjava dosljednost IP adresa i preduvjeti DNS-a sustava Windows Server AD DS, je potrebno najprije stvorite [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md) i priložite virtualnih računala. Prilikom njegova stvaranja mora odlučiti hoće li po želji proširivanje povezivanje s lokalnim korporacijskom mrežom, koji se proziran povezuje Azure virtualnim strojevima lokalnog računala – to je postići pomoću klasične VPN tehnologije i potreban je VPN krajnje izložiti na rubu poslovnoj mreži. To je VPN-om pokreće iz Azure korporacijskom mrežom, ne obratno.

Imajte na umu da dodatne naplaćuje kada proširivanje virtualne mreže za lokalnu mrežu izvan standardne troškove koji se odnose na svakom VM. Konkretno, postoje naknade za vrijeme procesora pristupnika Azure virtualne mreže i promet izlazne generira svaki VM koja vas obavještava s lokalnog računala preko VPN-om. Dodatne informacije o mrežni promet troškova potražite u članku [Azure cijene pri kratak](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>Konfiguraciju Kontroler implementacije

Način konfiguriranja kontroler domene ovisi o zahtjevima za uslugu koju želite pokrenuti na Azure. Ako, na primjer, možda implementirati u novi skup stabala, Izolirani iz vlastite tvrtke skupa stabala, za testiranje neki dokaz-od-koncept, novu aplikaciju ili neke druge projekta kratki termina koje je potrebno imeničkim servisima, ali nisu specifične pristup interne tvrtke resurse.

Kao u korist Izolirani skupa stabala Kontroler replicirati s lokalnog prevođenja, rezultira manje izlazni mrežni promet koje generira sustav, izravno smanjenju troškova. Dodatne informacije o mrežni promet troškova potražite u članku [Azure cijene pri kratak](http://azure.microsoft.com/pricing/).

Kao drugi primjer, pretpostavimo da ste izjave o zaštiti privatnosti zahtjevi za uslugu, ali servis ovisi o pristup vašem Interna Windows Server Active Directory. Ako vam dopušteno podataka glavno računalo za servis u oblaku, možda Implementirajte novu domenu podređeni za interne skup stabala na Azure. U tom slučaju možete implementirati Kontroler novi podređeni domene (bez globalni katalog da bi se pomoć problemi privatnosti adresa). Scenarij uz replike implementacije Kontroler zahtijeva virtualne mreže za povezivanje s vaše lokalne skupa.

Ako stvorite skup stabala novo, odaberite želite li koristiti [smatra pouzdanima servisa Active Directory](https://technet.microsoft.com/library/cc771397) ili [vanjski pristup smatra pouzdanima](https://technet.microsoft.com/library/dd807036). Saldo preduvjeti diktirali kompatibilnosti, sigurnost, sukladnost, trošak i otpornost. Na primjer, da biste iskoristili [selektivno provjere autentičnosti](https://technet.microsoft.com/library/cc755844) možete uvesti skup stabala za novo na Azure i stvoriti Windows Server Active Directory pouzdanosti između skupa stabala lokalnog i skup stabala oblaka. Ako aplikacija nije zahtjevima umu, no možda implementirati vanjski pristup trusts umjesto trusts skupa stabala servisa Active Directory. Drugi faktor je trošak replicirati više podataka produženjem vaše lokalne Windows Server Active Directory u oblak ili generiranje više odlazni promet kao rezultat provjerom autentičnosti i učitavanja upita.

Preduvjeti za dostupnosti i kvara odstupanje utječe i na temelju odabira. Ako, na primjer, ako prekinuta veza aplikacije korištenje Kerberos pouzdanost ili na vanjski pristup vjerovati su sve vjerojatno potpuno do oštećenja osim ako ste implementiran dovoljno infrastrukture na Azure. Konfiguracija zamjenski implementacije kao što su replike skupa (snimanje ili RODCs) povećali vjerojatnost od moći tolerate kvarove vezu.

### <a name="BKMK_ADSiteTopology"></a>Topologija web-mjesta za Windows Server Active Directory

Trebate pravilno definirati web-mjesta i veze web-mjesta da biste mogli Optimiziraj promet i minimiziranja trošak. Web-mjesta, veze web-mjesta i podmreže utjecati na topologije replikacije između prevođenja i tijek promet za provjeru autentičnosti. Razmotrite sljedeće naknada promet i implementirati i konfigurirati prevođenja prema potrebama scenariju implementacije:

- Postoji nominal naknadu po satu pristupnika sam:

 - Se može pokrenuti i zaustaviti kao svojim potrebama

 - Ako zaustavite, Azure VMs su Izolirani s korporacijskom mrežom

- Dolazni promet je besplatno

- Izlazni promet se naplaćuje prema [Azure cijene pri kratak](http://azure.microsoft.com/pricing/). Svojstva web-mjesta vezu između lokalnog web-mjesta i web-mjesta oblaka možete optimizirati na sljedeći način:

 - Ako koristite više mreža virtualne, konfigurirati veze web-mjesta i njihove troškove pravilno da biste spriječili da Windows Server AD DS iznad jedne koji mu može poslati iste razine servisa bez troškova čimbenika Azure web-mjesta. Preporučujemo i onemogućivanje most sve web-mjesta mogućnosti povezivanja (BASL) (koji je omogućen po zadanom). Na taj način replicirati da se međusobno samo izravno vezom web-mjesta. Dovršen transitively povezanog web-mjesta više ne mogu za replikaciju izravno s drugom, ali morate je replicirati putem uobičajenih web-mjesta ili web-mjesta. Ako privremene web-mjesta više nije dostupan zbog nekog razloga replikacije između skupa transitively povezanog web-mjesta neće se pojaviti čak i ako je dostupna veza između web-mjesta. Na kraju, pri čemu je sekcije ponašanja korištenje replikacije ostaju poželjno, stvaranje web-mjesta bridges veza koje sadrže odgovarajuće veze web-mjesta i web-mjesta, kao što su lokalnoj mreži tvrtke web-mjesta.

 - [Konfiguriranje web-mjesta vezu troškove](https://technet.microsoft.com/library/cc794882) pravilno da biste izbjegli neočekivane promet. Ako, na primjer, ako je omogućena postavka **Pokušajte sljedeće najbliže web-mjesta** , provjerite jesu li virtualne mreže web-mjesta ne dalje najbliže povećavanjem trošak povezan objekta veza web-mjesta koja se povezuje Azure web-mjesta natrag u poslovnoj mreži.

 - Konfigurirajte web-mjesta vezu [intervalima](https://technet.microsoft.com/library/cc794878) i [rasporede](https://technet.microsoft.com/library/cc816906) prema dosljednost preduvjeti i rata objekt promijeni. Poravnanje replikacije raspored s Latencija odstupanje. Dovršen replicirati samo zadnji status vrijednosti, da bi se smanjuje intervala ponavljanja možete spremiti troškove postoji li dovoljno objekt promjena stopa.

- Ako minimiziranje troškove je prioritet, provjerite je li zakazanog replikacije i obavijest o promjeni nije omogućen. Ovo je Zadana konfiguracija kad replikaciju između web-mjesta. Nije važno ako implementacije sustava RODC na virtualne mreže jer u RODC će replicirati izlaznog promjene. No ako pokrenete snimanje Kontroler, provjerite je li veza web-mjesta nije konfiguriran tako da s nepotrebne učestalost ponavljanja ažuriranja. Ako pokrenete poslužiteljem globalni katalog (GC), provjerite je li svaki drugi web-mjesta koja sadrži GC replicira particije domenu izvor Kontroler u web-mjesta koja je povezana s veza web-mjesta ili web-mjesta veze koje ste donjem trošak od GC Azure web-mjestu.


- Moguće je i dalje dodatno smanjili mrežni promet generira replikacije između web-mjesta tako da promijenite algoritam replikacije spajanja. Algoritam sažimanja upravlja algoritam REG_DWORD registra stavka HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator sažimanja. Zadana vrijednost je 3, koji korelaciji algoritam Xpress po datumu. Možete promijeniti vrijednost 2, koji se mijenja algoritam MSZip. U većini slučajeva, to će se povećati spajanja, ali to čini štetu procesora. Dodatne informacije potražite u odjeljku [Topologija replikacije kako Active Directory](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP adresiranja i DNS- a

Azure virtualnim strojevima su po zadanom dodijeliti "DHCP leased adrese". Budući da se dinamički adrese Azure virtualne mreže zadržava s virtualnog računala za vijek virtualnog računala, preduvjeti za Windows Server AD DS su ispunjeni.

Zbog toga kada koristite dinamičke adresu na Azure, zapravo pomoću statičke IP adrese jer je usmjeravati za razdoblje u Zakup i razdoblje u Zakup jednak je vijek servisa u oblaku.

Međutim, dinamični adresa je deallocated ako na VM je isključen. Da biste spriječili IP adresu koja se deallocated, možete [koristiti skup AzureStaticVNetIP da biste dodijelili statičke IP adrese](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Za razlučivanje naziva uvesti vlastitu (ili korištenje postojeće) DNS poslužitelja infrastrukture; Pod uvjetom Azure DNS zadovoljavaju potrebe razlučivost napredne naziv Windows Server AD DS. Na primjer, ga ne podržava dinamičke SRV zapisa itd. Razlučivanje naziva je stavka ključnih konfiguracija skupa i domene pridruženo klijente. Dovršen mora imati sposobnost Registriranje zapisa resursa i ispravljanje zapisa resursa druge Kontroler.
Razloga kvara odstupanje i performanse je optimalnih instalirajte Windows Server DNS servis na prevođenja sustavom Azure. Zatim konfigurirati svojstva Azure virtualne mreže s imenom i IP adresu DNS poslužitelja. Pri pokretanju druge VMs virtualne mreži, njihove postavke za klijent Razrješavanje DNS će konfigurirati DNS poslužitelj kao dio dinamički Dodjela IP adresa.

> [AZURE.NOTE] Ne možete uključiti lokalnog računala za Windows Server AD DS servisa Active Directory domene koje se hostira na Azure izravno putem Interneta. Priključke za Active Directory i operacija domene pridruživanja prikaz je nepraktično da biste izravno izložiti potrebne priključci i u praksi, cijeli Kontroler s Internetom.

VMs registrirati svoje ime DNS automatski prilikom pokretanja ili ako se promjena naziva.

Dodatne informacije o ovom primjeru i drugi primjer u kojem se pokazuje kako Dodjela prvi VM i instalirati AD DS na njemu potražite u članku [Instaliranje novog skupa stabala servisa Active Directory na Microsoft Azure](active-directory-new-forest-virtual-machine.md). Dodatne informacije o korištenju programa Windows PowerShell potražite u članku [Instaliranje modula Azure PowerShell](../powershell-install-configure.md) i [Cmdleti za upravljanje Azure](https://msdn.microsoft.com/library/azure/jj152841).

### <a name="BKMK_DistributedDCs"></a>Dovršen distributed za zemlj.

Azure nudi prednosti kada hosting više skupa na različitim virtualne mreže:

- Kvara odstupanje više web-mjesta

- Fizička Blizina podružnicama (donjem Latencija)

Dodatne informacije o konfiguriranju direktnu komunikaciju između virtualne mreže, potražite u članku [Konfiguracija virtualne mreže virtualne mrežne veze](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Samo za čitanje skupa

Morate odabrati želite li uvesti samo za čitanje ili pisanje skupa. Možda inclined za implementaciju RODCs jer nećete imati fizičke kontrolu nad ih, ali su namijenjene RODCs biti implementirano u mjesta gdje je njihova fizičke sigurnost rizik, kao što su podružnicama.

Azure izlaganje fizičke sigurnosni rizik granu office, ali RODCs mogu i dalje dokazuje biti troškova učinkovitijih jer su značajke pružaju dobro prikladniji te okruženja u kojima se koriste Premda je za vrlo razloga. Ako, na primjer, RODCs imate bez replikacije izlazni i moći selektivno popunjavanje tajne (lozinke). Negativna strana, nedostatak te tajne možda će biti potrebno odlazni promet na zahtjev za provjeru valjanosti kao korisnik i potvrđuje računala. No tajne mogu selektivno prethodno i predmemorirani.

RODCs Navedite prednost dodatnih u i oko HBI i PII opasnosti jer možete dodati atributi koji sadrže osjetljive podatke u RODC filtrirani skup atributa (DI). Na DI je moguće prilagoditi skup atributa koji su replicirati na RODCs. Možete koristiti u DI kao na zaštite u slučaju da nisu dopušteni ili želite pohraniti PII ili HBI u Azure. Dodatne informacije potražite u članku [RODC filtrirani atribut postavljen [(https://technet.microsoft.com/library/cc753459)].

Provjerite je li aplikacija će biti kompatibilan s RODCs namjeravate koristiti. Mnoge aplikacije Windows Server Active Directory omogućen rad s RODCs, ali neke aplikacije možete izvršiti inefficiently ili neće uspjeti ako nemaju pristup snimanje Kontroler. Dodatne informacije potražite u članku [Vodič za kompatibilnost aplikacija prevođenja samo za čitanje](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Globalni katalog

Morate odabrati želite li da biste instalirali globalni katalog (GC). U skup stabala u jednu domenu, trebali biste konfigurirati svih kontrolera kao globalni katalog poslužiteljima. Ga neće povećati troškove jer će biti bez dodatnih replikacije promet.

U multidomain skupa stabala GCs su potrebne da biste proširili članstva u grupi univerzalno tijekom provjere autentičnosti. Ako ne implementirali GC, radnih opterećenja na virtualne mreže koji provjeru autentičnosti Kontroler na Azure generirat će neizravno izlaznu provjeru autentičnosti promet upit GCs lokalnog tijekom pokušaja svaki provjere autentičnosti.

Troškove vezane uz GCs su predvidljivi manje jer oni hostira svaki domene (u dijela). Ako povećavaju hostira servis za mjesto na Internetu i potvrđuje korisnika u odnosu na Windows Server AD DS, troškovi može biti potpuno nepredvidljive. Da biste smanjili GC upita izvan web-mjesta oblaka tijekom provjere autentičnosti, možete [omogućili univerzalni članstvo u grupi predmemoriranje](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Način instalacije

Morat ćete odabrati kako instalirati na skupa na virtualne mreže:

- Promicanje novog skupa. Dodatne informacije potražite u članku [Instaliranje novog skupa stabala servisa Active Directory na Azure virtualne mreže](active-directory-new-forest-virtual-machine.md).

- Premještanje VHD na lokalni virtualne Kontroler u oblak. U ovom slučaju, morate biti sigurni da Kontroler virtualne lokalnog "Premjesti," ne "kopirane" ili "kloniranu".

Korištenje samo Azure virtualnim strojevima skupa (umjesto Azure "web" ili "tempiranja" uloga VMs). Oni su durable i potreban je rok trajanja stanja na Kontroler. Azure virtualnim strojevima namijenjena je radnih opterećenja kao što je dovršen.

Nemojte koristiti SYSPREP za implementaciju ili Kloniraj skupa. Mogućnost Kloniraj prevođenja je samo dostupne početak u sustavu Windows Server 2012. Postupka kloniranja značajka zahtijeva podršku za VMGenerationID u podlozi hypervisor. Hyper-V u sustavu Windows Server 2012 i Azure virtualne mrežama oba podržava VMGenerationID, kao proizvođači virtualizacije drugih proizvođača.

### <a name="BKMK_PlaceDB"></a>Položaj Windows Server AD DS baze podataka i SYSVOL

Odaberite da biste pronašli Windows Server AD DS baze podataka, zapisnika i SYSVOL. Mora biti uveden na podacima Azure diskova.

> [AZURE.NOTE] Azure diskova podaci su ograničeni na 1 TB.

Podaci diskovnih pogona učinite ne predmemorije zapisivanje prema zadanim postavkama. Diskovnih pogona podataka koje su priložene na VM koristiti putem pisanja predmemoriju. Pisanje-putem predmemorije čini li pisanja nastoji durable Azure prostora za pohranu prije dovršetka transakcija iz perspektive na VM operacijski sustav. Pruža rok trajanja štetu malo sporije zapisivanja.

To je važno za Windows Server AD DS jer pisanje zaostajete diska-predmemoriranje poništava pretpostavke načinio kontroler domene. Windows Server AD DS pokušava da biste onemogućili zapisivanju, ali je na disku IO sustava za proslavu ga. Onemogućivanje zapisivanju nije uspjelo, u određenim uvjetima, umjesto toga USN vraćanje rezultira lingering objekte i drugih problema.

Kao Najbolji postupci za virtualne prevođenja, učinite sljedeće:

- Postavljanje postavka Preferirani predmemorije glavnog računala na disku Azure podataka za NIŠTA. To sprječava probleme s zapisivanju za AD DS operacije.

- Spremanje baze podataka, zapisnika i SYSVOL na neki istom podatkovni disk ili diskova zasebne podatke. To je obično zaseban disk s diska koji se koriste za sam operacijski sustav. Ključni takeaway je da Windows Server AD DS bazu podataka i SYSVOL moraju neće biti pohranjena na disku vrstu Azure operacijski sustav. Postupak instalacije AD DS po zadanom se instalira te komponente u mapi % systemroot %, koji se preporučuje za Azure.

### <a name="BKMK_BUR"></a>Sigurnosno kopiranje i vraćanje

Imajte na umu što je i nije podržana za sigurnosno kopiranje i vraćanje na Kontroler Općenito i točnije, one izvodi u na VM. Potražite u članku [sigurnosnog kopiranja i vraćanja zahtjevi za Virtualiziranom skupa](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Stvara sustava sigurnosne kopije stanja pomoću samo softver za sigurnosno kopiranje koje je izričito upoznat sigurnosne preduvjete za Windows Server AD DS, kao što je Windows Server sigurnosne kopije.

Kopirajte ili Kloniraj VHD datoteka skupa umjesto izvođenje redovito sigurnosno kopiranje. Vraćanje ikad treba obavezno, to pomoću kloniranu ili kopirate VHDs bez Windows Server 2012 i podržanim hypervisor predstavljanje USN mjehurića.

### <a name="BKMK_FedSrvConfig"></a>Konfiguraciju vanjskog poslužitelja

Konfiguracija sustava Windows Server AD FS vanjskim poslužiteljima (STSs) ovisi o djelomice li aplikacije koje želite uvesti na Azure moraju imati pristup resursima na lokalnu mrežu.

Ako aplikacija ne ispunjava sljedeće kriterije, nije moguće uvesti aplikacije u odvajanja iz lokalne mreže.

- Prihvaćaju SAML sigurnosnih tokena

- Su exposable s Internetom

- Ne pristupaju lokalnog resursa

U ovom slučaju, potrebno je konfigurirati Windows Server AD FS STSs na sljedeći način:

1. Konfiguriranje Izolirani skupa stabala jedne domene na Azure.

2. Omogućuje pridruženi pristup u skup stabala konfiguriranjem farmu poslužitelja vanjski pristup za ADFS u sustavu Windows Server.

3. Konfiguriranje Windows Server AD FS (vanjski pristup poslužitelja i vanjskim proxy poslužitelja) u skup stabala lokalnog.

4. Uspostavljanje vanjski pristup pouzdanih odnosa između lokalnog i Azure instance programa Windows Server AD FS.

S druge strane, ako aplikacije potreban pristup lokalnog resursa, koje nije moguće konfigurirati ADFS u sustavu Windows Server aplikaciju na Azure na sljedeći način:

1. Konfiguriranje povezivanja s između lokalne mreže i Azure.

2. Konfiguriranje farme poslužitelja za vanjski pristup Windows Server AD FS u skup stabala lokalnog.

3. Konfiguriranje farmu poslužitelja proxy vanjski pristup za ADFS u sustavu Windows Server na Azure.

Tu konfiguraciju ima prednost smanjivanje izlaganje lokalnog resursa, slično konfiguriranju Windows Server AD FS s aplikacijama u mreži opseg.

Imajte na umu da u svakom slučaju, možete uspostaviti odnose trusts s više davatelji identiteta, potrebi Suradnja tvrtke tvrtke.

### <a name="BKMK_CloudSvcConfig"></a>Konfiguriranje usluge oblaka

Servisi u oblaku su potrebni ako želite da biste otkrili VM izravno s Internetom ili da bi se prikazale aplikacije uravnoteženja mjesto na Internetu. Ovo je moguće jer se svaki servis u oblaku nudi jednom konfigurirati virtualne IP adresom.

### <a name="BKMK_FedReqVIPDIP"></a>Vanjski pristup poslužiteljski preduvjeti za javne i privatne IP adresama (dinamičke IP nasuprot virtualne IP)

Svaki Azure virtualnog računala prima dinamičku IP adresu. Dinamičku IP adresu je dostupna samo u sklopu Azure Privatna adresa. U većini slučajeva, međutim, bit će potrebno da biste konfigurirali virtualna IP adresa za implementacijama sustava Windows Server AD FS. Virtualna IP je potrebno da biste otkrili Windows Server AD FS krajnje točke na Internet i koristit će vanjski partneri i klijenti za provjeru autentičnosti i daljnje upravljanje. Virtualna IP adresa je svojstvo servis u oblaku koja sadrži jedan ili više Azure virtualnih računala. Ako aplikaciju zahtjevima umu implementiran na Azure i Windows Server AD FS su mjesto na Internetu i najčešće priključke za zajedničko korištenje, svaki potrebno virtualna IP adresa vlastitog i stoga će biti potrebno da biste stvorili jedan servis u oblaku za aplikaciju i druge za Windows Server AD FS.

Definicija uvjeta virtualna IP adresa i dinamičku IP adresu, potražite u članku [Uvjeti i definicije](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Konfiguriranje komponente Windows Server AD FS visoke dostupnosti

Dok je moguće da biste implementirali servise vanjski pristup za ADFS u sustavu Windows Server samostalni, preporučuje se za implementaciju farmu s najmanje dva čvorove za AD FS STS i proxy poslužitelja za radnog okruženja.

Potražite u članku [AD FS 2.0 implementacije topologije pitanja vezana uz](https://technet.microsoft.com/library/gg982489) u na [AD FS 2.0 dizajn vodič](https://technet.microsoft.com/library/dd807036) odluči koje konfiguracija mogućnosti implementacije najbolje odgovara vašim potrebama određeni.

> [AZURE.NOTE] Da biste dobili za krajnje točke Windows Server AD FS na Azure za ujednačavanje opterećenja konfiguriranje sve članove u farmi ADFS u sustavu Windows Server u istoj servis u oblaku, a pomoću ujednačavanje opterećenja mogućnost Azure za HTTP (zadano 80) i HTTPS priključaka (zadano 443). Dodatne informacije potražite u članku [probni Azure raspoređivača opterećenja](https://msdn.microsoft.com/library/azure/jj151530).
Windows Server mreže učitavanja opterećenja (NLB) nije podržana na Azure.
