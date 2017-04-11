<properties
    pageTitle="Active Directory Federation Services u Azure | Microsoft Azure"
    description="U ovom dokumentu će Saznajte kako implementirati AD FS u Azure za visoke availablity."
    keywords="Implementacija AD FS servisu azure, uvođenje azure adfs, azure adfs, azure ad fs, uvođenje adfs, uvođenje servisa ad fs adfs u azure, uvođenje adfs u azure, uvođenje AD FS u azure, adfs azure, Uvod u AD FS, Azure AD FS u Azure iaas, ADFS, Premjesti adfs za azure"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>AD FS implementacije u Azure 

AD FS nudi Web jedinstvenu prijavu (SSO) mogućnosti i pojednostavnjeni, zaštićenim identitetu. Vanjski pristup sustavu Azure AD ili O365 korisnicima omogućuje provjeru autentičnosti pomoću vjerodajnica za lokalni i pristup svim izvorima u programu Excel. Zbog toga postaje važno da bi iznimno dostupna infrastruktura za AD FS, da biste bili sigurni pristup resursima oba lokalnog i u oblaku. Implementacija AD FS u Azure može pridonijeti postigli visoke dostupnosti obavezno s minimalnim naporima.
Nekoliko je prednosti implementacije AD FS u Azure, nekoliko ih navedena su u nastavku:

* **Visoke dostupnosti** - s power dostupnosti skupa Azure možete da iznimno dostupna infrastrukture.
* **Easy skaliranje** – potrebna bolje performanse? Jednostavno premještanje jače strojeva tako da samo nekoliko klikova servisu Azure
* **Zalihosti unakrsno-zemlj.** – s Azure zemlj zalihosti koje možete biti sigurni da vaše infrastrukture dostupna iznimno diljem svijeta
* **Easy upravljanje** – mogućnosti iznimno pojednostavljeni upravljanja Azure portalu upravljanje preduvjete infrastrukture je vrlo lako i jednostavan 

## <a name="design-principles"></a>Način dizajna

![Uvođenje dizajna](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Gornji dijagram prikazuje preporučene osnovni topologije da biste pokrenuli implementacija infrastruktura za AD FS u Azure. Načela iza razne komponente topologije navedena su u nastavku:

* **Kontroler / ADFS poslužitelji**: Ako imate manje od 1000 korisnika AD FS uloga možete instalirati samo na vašem kontrolera domena. Ako ne želite da se utjecaja performanse na kontrolera domena ili ako imate više od 1000 korisnika, implementirati AD FS na različitim poslužiteljima.
* **Poslužitelj WAP** – je potrebno za implementaciju Web aplikacije Proxy poslužitelje, tako da korisnici mogu dobiti AD FS kada nisu na mreži tvrtke i.
* **DMZ**: The Web aplikacije Proxy poslužitelji će biti smješten u na DMZ i pristup samo TCP/443 dopuštena između na DMZ i interna podmreže.
* **Učitavanje Balancers**: da biste bili sigurni visoke dostupnosti poslužitelja za AD FS i Proxy aplikacije za Web, preporučujemo korištenje Interna opterećenja za Azure raspoređivača opterećenja i vanjske poslužitelje servisa AD FS Web aplikacije Proxy poslužitelja.
* **Dostupnost skupova**: osigurati redundanciju za implementaciju sustava AD FS, preporučuje se da grupirate dva ili više virtualnim strojevima u dostupnost postavljena za slične radnih opterećenja. Tu konfiguraciju osigurava da tijekom ili planiranog ili neplanirano održavanja događaja, barem jedan virtualnog računala će biti dostupna
* **Računi za pohranu**: preporučuje se da bi se dva računa za pohranu. Nemate račun za jednu za pohranu može dovesti do stvaranja jednu točku pogreške, a može uzrokovati implementacije postaju dostupne u vjerojatno scenarij koji se gdje funkcionira na račun za pohranu. Dva računa za pohranu će vam pomoći povezati jedan račun za pohranu za svaki redak kvara.
* **Mrežni segregation**: Web aplikacije Proxy poslužitelji mora biti implementirano u zasebnu DMZ mrežu. Možete podijeliti virtualne mreža u dva podmreže i implementacija Web aplikacije Proxy poslužitelji Izolirani podmreže. Jednostavno možete konfigurirati mreže sigurnosne postavke grupe za svaku podmreže i dopustiti samo potrebni komunikaciju između dva podmreže. Više pojedinosti ponudit će po implementacije scenarij ispod

##<a name="steps-to-deploy-ad-fs-in-azure"></a>Koraci za implementaciju AD FS servisu Azure

Korake koji se spominju u ovom odjeljku Strukturiranje vodič za implementaciju u nastavku opisane infrastrukturu za AD FS sustava Azure.

### <a name="1-deploying-the-network"></a>1. implementacija s mrežom

Kao što je vidljivo iznad, koje možete ili stvaranje dva podmreže u jednom virtualne mreže jer inače stvaranje dvije potpuno drugačije virtualne mreže (VNet). U ovom se članku će usredotočite se na implementacija virtualne mreže i podijeliti u dvije podmreže. Ovo je trenutno jednostavnije pristup kao dva zasebna VNets je potrebna VNet u VNet pristupnik za komunikaciju.

**1.1 stvaranje virtualne mreže**

![Stvaranje virtualne mreže](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
Na portalu Azure odaberite virtualne mreže, a možete implementirati virtualne mreže i jedan podmreže odmah sa samo jednim klikom. INT podmreže i definirani i spreman je sada za VMs potrebno dodati.
Sljedeći je korak da biste dodali drugu podmreže s mrežom, odnosno podmreže DMZ. Jednostavno stvaranje podmreže DMZ

* Odaberite novostvorenu mrežu
* U svojstvima odaberite podmreže
* U okviru podmreži ploče kliknite gumb Dodaj
* Navedite podmreže ime i adresu prostora podatke da biste stvorili podmreži

![Podmreže](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![Podmreže DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. stvaranje mreže sigurnosne grupe**

Mrežni sigurnosne grupe (NSG) sadrži popis pravila pristupa kontrola popisa (ACL) koja se dopustiti ili zabraniti mrežni promet na vaše VM instance u virtualne mreže. NSGs može biti pridruženi podmreže ili pojedinačne instance VM unutar tog podmreže. Kada je NSG je pridružen podmreži, ACL pravila primjenjuju se na sve instance VM u tom podmreže.
Radi uputama, ne možemo će stvoriti dvije NSGs: jedan svaki internoj mreži i na DMZ. Oni će biti označene NSG_INT i NSG_DMZ redom.

![Stvaranje NSG](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Nakon stvaranja u NSG će biti 0 ulaznog i 0 izlaznog pravila. Kada uloge na poslužiteljima su instalirani i funkcionirati, pravila ulazni i izlazni mogu učiniti prema željenu razinu zaštite.

![Pokretanje NSG](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Nakon stvaranja na NSGs pridružiti NSG_INT podmreže INT i NSG_DMZ s podmreže DMZ. Snimka zaslona na primjer dobiva se ispod:

![Konfiguriranje NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Kliknite na podmreže da biste otvorili ploču za podmreže
* Odaberite podmreže želite pridružiti na NSG 

Nakon konfiguracija na ploči za podmreže trebao izgledati kao ispod:

![Podmreže nakon NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. stvaranje veze na lokalni**

Da biste se uvesti kontrolerom domene (Kontroler) u azure će moramo veze s lokalnim. Azure nudi razne mogućnosti povezivanja povezati infrastruktura za lokalni Azure infrastrukture.

* Točka-mjesta
* Virtualne mreže web-mjesta-na-web-mjesta
* ExpressRoute

Preporučuje se da biste koristili ExpressRoute. ExpressRoute omogućuje stvaranje privatne veze između Azure podatkovnim centrima i infrastrukture lokalno ili u okruženju suautorstvo mjesto. ExpressRoute veze ne otvorite putem javnog Interneta. Oni nude dodatne pouzdanosti, brže brzine, donjem latencies i veću sigurnost od standardne veze putem Interneta.
Premda se preporučuje se da biste koristili ExpressRoute, možete odabrati bilo koji način povezivanja najprikladnije su za tvrtku ili ustanovu. Da biste saznali više o ExpressRoute i razne mogućnosti povezivanja pomoću ExpressRoute, pročitajte [ExpressRoute Tehnički pregled](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. stvoriti račune za pohranu

Da biste zadržali visoke dostupnosti i izbjegli Opširnije na računu jednog prostora za pohranu, možete stvoriti dva računa za pohranu. Strojeva u svakom skupu dostupnost podijeliti u dvije grupe, a zatim dodijelite svake grupe računom zasebnom prostora za pohranu.

![Stvaranje računa za pohranu](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. stvaranje skupova dostupnosti

Za svaku ulogu (Kontroler/AD FS i WAP) stvaranje skupova dostupnost koja će sadržavati 2 strojeva svaki najmanje. To ćete postići veća dostupnost za svaku ulogu. Tijekom stvaranja dostupnost postavlja je ključan odlučite o sljedećem:
* **Kvara domene**: virtualnim strojevima u istu domenu kvara zajedničko korištenje istog power izvora i promjenu fizičke mrežne. Preporučuje se najmanje 2 kvara domene. Zadana vrijednost je 3 i ostavite ga kao što je potrebe ovog implementacije
* **Ažuriranje domene**: strojeva pripadaju isti ažuriranje domene zajedno pokrene tijekom ažuriranje. Želite imati najmanje 2 ažuriranje domene. Zadana je vrijednost 5 i ostavite ga kao što je potrebe ovog implementacije

![Skupovi dostupnosti](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Stvaranje sljedećih skupova dostupnosti

| Postavljanje dostupnosti | Uloga | Kvara domene | Ažuriranje domene |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | KONTROLER/ADFS | 3 | 5 |
| contosowapset | WAP | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4. implementacija virtualnim strojevima
Sljedeći je korak za implementaciju virtualnim strojevima koji će biti smješteno različite uloge u sustavu. Najmanje dva strojeva preporučuje se u svakom skupu dostupnost. Stvaranje šest virtualnim strojevima osnovni implementacije.

| Računalo | Uloga | Podmreže | Postavljanje dostupnosti | Račun za pohranu | IP adresa |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|KONTROLER/ADFS|INT|contosodcset|contososac1|Statički|
|contosodc2|KONTROLER/ADFS|INT|contosodcset|contososac2|Statički|
|contosowap1|WAP|DMZ|contosowapset|contososac1|Statički|
|contosowap2|WAP|DMZ|contosowapset|contososac2|Statički|

Kao što je možda ste primijetili, bez NSG nije naveden. To je zato azure omogućuje korištenje NSG na razini podmreže. Nakon toga možete kontrolirati strojno mrežni promet pomoću pojedinačne NSG pridružene ili podmreži jer inače NIC objekt. Saznajte više o [što je mreža grupe sigurnosti (NSG)](https://aka.ms/Azure/NSG).
Ako sami upravljate DNS-om, preporučuje se statičke IP adrese. Možete koristiti Azure DNS-a i umjesto toga u odjeljku DNS zapisi za domenu upućivati na novu računalima prema svojim certifikatom Azure.
Okno za virtualnog računala trebao izgledati kao ispod nakon dovršetka implementacijskih:

![Virtualnim strojevima implementiran](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. konfiguriranje kontrolerom domene / poslužitelja za AD FS
 Da bi se sve dolazne zahtjev za provjeru autentičnosti, AD FS morat ćete se obratiti kontrolerom domene. Da biste spremili skup putovanja Azure lokalnog Kontroler za provjeru autentičnosti, preporučuje se za implementaciju replike s kontrolerom domene u Azure. Da bi se postići visoke dostupnosti, preporučuje se da biste stvorili dostupnost skup kontrolera najmanje 2 domena.

|Kontroler domene|Uloga|Račun za pohranu|
|:-----:|:-----:|:-----:|
|contosodc1|Replike|contososac1|
|contosodc2|Replike|contososac2|

* Promicanje dva poslužitelja kao replike kontrolera domena s DNS-a
* Konfiguraciju poslužitelja za AD FS instalacijom AD FS uloga pomoću upravitelja poslužitelja.

###<a name="6---deploying-internal-load-balancer-ilb"></a>6. implementacija Interna opterećenja (ILB)

**6.1. Stvaranje na ILB**

Za implementaciju sustava ILB, odaberite Balancers Učitaj u Azure portal, a zatim kliknite Dodavanje (+).
>[AZURE.NOTE] Ako ne vidite **Učitavanja Balancers** na izborniku, kliknite **Pregledaj** u donjem lijevom kutu portal i pomaknite se dok ne ugledate **Balancers Učitaj**.  Zatim kliknite žuta zvjezdica da biste ga dodali na izborniku. Sada odaberite Nova ikona raspoređivača opterećenja da biste otvorili ploču da biste započeli konfiguracije raspoređivača opterećenja.

![Pregled opterećenja](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Naziv**: dati naziv neke odgovarajuću raspoređivača opterećenja
* **Sheme**: jer ovo opterećenja će se nalaziti ispred poslužitelja za AD FS i je namijenjena za interne mrežne veze samo, odaberite "Internog"
* **Virtualne mreže**: Odaberite virtualne mreže na kojima su implementacija AD FS
* **Podmreže**: Odaberite interni podmreže ovdje
* **Dodjela IP adresa**: dinamički

![Interna opterećenja](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
Nakon što kliknete stvaranje i je implementiran u ILB, trebali biste vidjeti ga na popisu balancers opterećenja:

![Učitavanje balancers nakon ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
Sljedeći je korak konfiguriranje skup pozadinskog sustava i probni pozadinskog.

**6.2. konfigurirajte grupe ILB pozadinska aplikacija**

Odaberite novostvorenu ILB na ploči Balancers Učitaj. Otvorit će se ploča s postavkama. 
1.  Odaberite grupe pozadinskog s ploča s postavkama
2.  Na ploči Dodavanje pozadinskog skup, kliknite Dodaj virtualnog računala
3.  Primit ćete s pločom na kojem možete odabrati postavljanje dostupnosti
4.  Odaberite Postavljanje dostupnosti AD FS

![Konfiguriranje grupe ILB pozadinska aplikacija](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6,3. konfiguriranje probni**

Na ploči ILB postavke, odaberite Probes.
1.  Kliknite na Dodaj
2.  Navođenje detalja za probni na. **Naziv**: isprobati b ime. **Protokol**: TCP c. **Priključak**: 443 (HTTPS) d. **Interval**: 5 (Zadana vrijednost) – to je interval na kojoj se ILB isprobati strojeva u pozadinskog e. **Ograničenje praga dobro**: 2 (Zadana vrijednost val) – to je prag neuspjeha uzastopna probni nakon kojeg ILB će deklarirati stroj u pozadinskog ne odgovara, a zaustavite pošaljete promet na nju.

![Konfiguriranje probni ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6,4. Stvaranje pravila za ujednačavanje opterećenja**

Da biste učinkovito saldo promet, trebali biste konfigurirati na ILB pravila za ujednačavanje opterećenja. Da biste stvorili u pravilo za ujednačavanje opterećenja 
1.  Odaberite pravilo s ploča s postavkama u ILB za ujednačavanje opterećenja
2.  Kliknite Dodaj na ploču pravila za ujednačavanje opterećenja
3.  Na ploči pravila za ujednačavanje opterećenja Dodaj u. **Naziv**: Navedite naziv pravila b. **Protokol**: Odaberite TCP c. **Priključak**: 443 d. **Pozadinski priključka**: 443 e. **Skup pozadinskog**: odaberite grupu koju ste stvorili za AD FS skupine starijim f. **Probni**: Odaberite probni ste ranije stvorili za poslužitelje servisa AD FS

![Konfiguriranje ILB opterećenja pravila](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. ažurirajte DNS ILB**

Idite na DNS poslužitelj i stvorite CNAME za na ILB. U CNAME mora biti servisa za vanjski pristup s IP adresom koja pokazuje na IP adresu na ILB. Primjer ako je adresa ILB DIP 10.3.0.8 i servis za vanjski pristup instaliran je fs.contoso.com, stvorite CNAME za fs.contoso.com koja pokazuje na 10.3.0.8.
To će osigurati svu komunikaciju vezane uz fs.contoso.com završetka gore pri u ILB, a komponenta usmjeravanja.

###<a name="7---configuring-the-web-application-proxy-server"></a>7. konfiguriranja Web aplikacije Proxy poslužitelj

**7.1. konfiguriranja Web aplikacije Proxy poslužitelji dosegne poslužitelja za AD FS**

Da biste Web aplikacije Proxy poslužitelji provjerite jesu li moći pristupiti poslužitelja za AD FS iza u ILB, stvorite zapis u na %systemroot%\system32\drivers\etc\hosts za na ILB. Imajte na umu da Razlikovni naziv (DN) treba naziv usluge vanjski pristup, primjerice fs.contoso.com. I unos IP mora biti koji se ILB IP adrese (10.3.0.8 kao u ovom primjeru).

**7,2. instalacije uloga Proxy aplikacije za Web**

Nakon što ste Web aplikacije Proxy poslužitelji provjerite jesu li moći pristupiti poslužitelja za AD FS iza ILB, možete instalirati sljedeće poslužitelje Proxy aplikacije Web. Web-aplikacije Proxy poslužitelji se pridružite domeni. Instalirajte uloga Proxy aplikacije Web na dva Web aplikacije Proxy poslužitelji tako da odaberete ulogu daljinski pristup. Upravitelj poslužitelja vodit će vas da biste dovršili instalaciju WAP.
Dodatne informacije o tome kako implementirati WAP pročitajte [instalirati i konfigurirati web-aplikacije Proxy poslužitelj](https://technet.microsoft.com/library/dn383662.aspx).

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8. implementacija Internet nasuprotne (javni) opterećenja

**8.1. stvaranje Internet nasuprotne (javni) opterećenja**
 
Na portalu Azure odaberite učitavanja balancers, a zatim kliknite na dodavanje. U oknu raspoređivača opterećenja stvaranje unesite sljedeće podatke
1. **Naziv**: naziv raspoređivača opterećenja
2. **Sheme**: javno – tu mogućnost govori Azure ovaj opterećenja ćete javnu adresu.
3. **IP adresa**: Stvorite novu IP adresu (dinamički)

![Dostupnog opterećenja Internet](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Nakon implementacije, raspoređivača opterećenja prikazat će se na popisu balancers Učitaj.

![Popis raspoređivača učitavanja](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8,2. dodijeliti oznaku DNS javnu IP**

Kliknite u stavci raspoređivača opterećenja novostvorenu na ploči balancers učitavanja da biste otvorili ploču za konfiguraciju. Slijedite dolje korake da biste konfigurirali oznaku DNS-a za javno IP:
1.  Kliknite na javnu IP adresu. Otvorit će se na ploči za javnu IP i postavke
2.  Kliknite o konfiguraciji
3.  Navedite natpis DNS-a. To će postati javno DNS oznaku koju možete pristupiti s bilo kojeg mjesta, na primjer contosofs.westus.cloudapp.azure.com. Unos možete dodati na vanjski DNS servisa za vanjski pristup (kao što je fs.contoso.com) koji se pretvara DNS oznaku na vanjskim opterećenja (contosofs.westus.cloudapp.azure.com).

![Konfiguriranje internet nasuprotne opterećenja](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Konfiguriranje internet nasuprotne opterećenja (DNS)](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8,3. konfiguracije grupe pozadinska aplikacija za opterećenja Internet nasuprotne (javnih)** 

Poduzmite iste korake kao stvaranje internog opterećenja, da biste konfigurirali pozadinskog skup za nasuprotne Internet (javni) raspoređivača opterećenja kao dostupnost postaviti za poslužitelje WAP. Na primjer, contosowapset.

![Konfiguriranje pozadinskog skup Internet nasuprotne opterećenja](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8,4. konfigurirajte probni**

Poduzmite iste korake kao konfiguriranje Interna raspoređivača opterećenja da biste konfigurirali probni za skup pozadinskog WAP poslužiteljima.

![Konfiguriranje probni Internet nasuprotne opterećenja](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8.5. Stvaranje pravila za ujednačavanje opterećenja**

Poduzmite iste korake kao ILB da biste konfigurirali ujednačavanja pravila za TCP 443 opterećenja.

![Konfiguriranje ujednačavanje pravila Internet nasuprotne opterećenja](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9. zaštita mreže

**9,1. zaštita Interna podmreže**

Ukupni, potrebno je sljedeća pravila da biste učinkovito sigurne vašoj podmreži za interne (redoslijedom navedene u nastavku)

|Pravila|Opis|Tijek|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| Dopustiti komunikaciju HTTPS iz DMZ | Dolazni |
|DenyInternetOutbound| Pristup Internetu | Izlazni |

![Pravila za pristup INT (ulazni)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[komentar]: <> (![pravila za pristup INT (ulazni)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [komentar]: <> (![INT pravila za pristup (izlazni)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2. zaštita podmreže DMZ**

|Pravila|Opis|Tijek|
|:----|:----|:------:|
|AllowHTTPSFromInternet| Dopustite HTTPS s Interneta u DMZ | Dolazni|
|DenyInternetOutbound|  Sve osim HTTPS Internet blokiran | Izlazni |

![Pravila za pristup ŠIR (ulazni)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[komentar]: <> (![pravila za pristup ŠIR (ulazni)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [komentar]: <> (![ŠIR pravila za pristup (izlazni)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] Ako korisnik klijenta potvrda autentičnosti (Provjera autentičnosti uz klijentski TLS pomoću X509 korisničkih certifikata) potrebno je, a zatim AD FS zahtijeva TCP priključak 49443 biti omogućen za ulazni pristup.

###<a name="10--test-the-ad-fs-sign-in"></a>10. testiranje prijave za AD FS

Najlakši način je da biste testirali AD FS pomoću IdpInitiatedSignon.aspx stranice. Da biste mogli učiniti da je potrebno da biste omogućili IdpInitiatedSignOn svojstava za AD FS. Slijedite korake u nastavku da biste provjerili postavu AD FS
1.  Pokretanje sustava ispod cmdlet poslužitelju za AD FS, pomoću komponente PowerShell postavite ga na omogućeni.
    Postavljanje AdfsProperties - EnableIdPInitiatedSignonPage $true 
2.  Iz bilo kojeg https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx pristup vanjskim računalu  
3.  Trebali biste vidjeti AD FS stranice kao što su ispod:

![Probna stranica za prijavu](./media/active-directory-aadconnect-azure-adfs/test1.png)

Na uspješno prijavu, ona omogućit će vam poruka o uspjehu kao što je prikazano u nastavku:

![Testiranje uspjeh](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Predložak za implementaciju AD FS servisu Azure

Predložak uvodi 6 strojno postavljanje 2 svaki kontrolera domena, AD FS i WAP.

[AD FS u predlošku Azure implementacije](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Možete koristiti postojeće virtualne mreže ili stvaranje nove VNET pri implementaciji ovaj predložak. Različite parametara dostupnih za prilagodbu implementacijskih navedena su u nastavku s opisom korištenje parametra u postupku implementacije. 

| Parametar | Opis |
|:--------|:-----|
|Mjesto| Područje za implementaciju resursa u npr Istok US. |
|StorageAccountType| Vrstu računa za pohranu stvorili|
|VirtualNetworkUsage| Označava stvorit će se novi virtualne mreže ili koristite postojeću|
|VirtualNetworkName| Naziv virtualne mreže za stvaranje, obavezno na postojeći ili novi način korištenja virtualne mreže|
|VirtualNetworkResourceGroupName| Određuje naziv grupe resursa gdje se nalazi postojeće virtualne mreže. Kada koristite postojeće virtualne mreže, to će biti obavezno parametar da bi implementacijskih mogli pronaći ID postojeće virtualne mreže|
|VirtualNetworkAddressRange| Adresa raspon novi VNET, obavezno ako stvaranja novog virtualne mreže|
|InternalSubnetName| Naziv internog podmreže, obavezno na obje mogućnosti korištenja virtualne mreže (novi ili postojeći)|
|InternalSubnetAddressRange| Adresa raspon Interna podmreže koja sadrži kontrolera domena i ADFS poslužitelja, obavezno ako stvaranja novog virtualne mreže.|
|DMZSubnetAddressRange| Adresa raspon podmreže dmz koja sadrži Windows proxy poslužitelje aplikacije, obavezno ako stvaranja novog virtualne mreže.|
|DMZSubnetName| Naziv internog podmreže, obavezno na obje mogućnosti korištenja virtualne mreže (novi ili postojeći). |
|ADDC01NICIPAddress| Interna IP adresa s kontrolerom domene prvi, ovu IP adresu statički dodijelit će se kontroler domene i mora biti valjane ip adresa unutar Interna podmreže|
|ADDC02NICIPAddress| Interna IP adresa s kontrolerom domene drugi, ovu IP adresu statički dodijelit će se kontroler domene i mora biti valjane ip adresa unutar Interna podmreže|
|ADFS01NICIPAddress| Interna IP adresu prvi poslužitelj za ADFS, ovu IP adresu statički dodjeljuju na poslužitelj za ADFS i mora biti valjani ip adresa unutar Interna podmreže|
|ADFS02NICIPAddress| Interna IP adresu drugi poslužitelj za ADFS, ovu IP adresu statički dodjeljuju na poslužitelj za ADFS i mora biti valjani ip adresa unutar Interna podmreže|
|WAP01NICIPAddress| Interna IP adresu prvi poslužitelj WAP, ovu IP adresu statički dodjeljuju WAP poslužitelj i moraju biti valjane ip adresa unutar podmreže DMZ|
|WAP02NICIPAddress| Interna IP adresu drugi poslužitelj WAP, ovu IP adresu statički dodjeljuju WAP poslužitelj i mora biti valjani ip adresa unutar podmreže DMZ|
|ADFSLoadBalancerPrivateIPAddress| Interna IP adresu opterećenja ADFS, ovu IP adresu statički dodijelit će se raspoređivača opterećenja i mora biti valjani ip adresa unutar Interna podmreže|
|ADDCVMNamePrefix| Prefiks naziva virtualnog računala za kontrolera domena|
|ADFSVMNamePrefix| Virtualnog računala prefiks naziva poslužitelja za ADFS|
|WAPVMNamePrefix| Prefiks naziva virtualnog računala WAP poslužitelja|
|ADDCVMSize| Veličina vm kontrolera domene|
|ADFSVMSize| Veličina vm poslužitelja za ADFS|
|WAPVMSize| Veličina vm WAP poslužitelja|
|AdminUserName| Naziv lokalni Administrator virtualnim strojevima|
|AdminPassword| Lozinku lokalnog administratorskog računa virtualnih računala|

## <a name="additional-resources"></a>Dodatni resursi
* [Skupovi dostupnosti](https://aka.ms/Azure/Availability ) 
* [Azure opterećenja](https://aka.ms/Azure/ILB)
* [Interna opterećenja](https://aka.ms/Azure/ILB/Internal)
* [Internet dostupnog raspoređivača opterećenja](https://aka.ms/Azure/ILB/Internet)
* [Računi za pohranu](https://aka.ms/Azure/Storage )
* [Azure virtualne mreže](https://aka.ms/Azure/VNet)
* [AD FS i web-aplikacije proxy poslužitelja](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Daljnji koraci

* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
* [Konfiguriranje i upravljanje vaše AD FS pomoću Azure AD Connect](active-directory-aadconnectfed-whatis.md)
* [Visoke dostupnosti unakrsno geografske AD FS implementacije u Azure s Azure promet Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)




