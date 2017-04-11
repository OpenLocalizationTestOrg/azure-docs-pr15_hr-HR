<properties
    pageTitle="Instalirajte kontroler domene servisa Active Directory za replike u Azure | Microsoft Azure"
    description="Praktični vodič kojima se objašnjava da biste instalirali kontroler domene iz sustava lokalnog servisa Active Directory skupa stabala na Azure virtualnog računala."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Instalirajte kontroler domene servisa Active Directory za replike u Azure virtualne mreže

U ovoj se temi objašnjava instalirajte dodatnu domenu kontrolera (poznat i kao replike prevođenja) za lokalne domene servisa Active Directory na Azure virtualnim strojevima (VMs) u Azure virtualne mreže.

Koje možda će vas zanimati te povezane teme:

-  Po želji možete instalirati novi skup stabala servisa Active Directory na Azure virtualne mreže. Te korake potražite u članku [Instaliranje novog skupa stabala servisa Active Directory na Azure virtualne mreže](../active-directory/active-directory-new-forest-virtual-machine.md).
-  Konceptualni upute o instaliranju Active Directory Domain Services (AD DS) na Azure virtualne mreže potražite u članku [smjernice za implementaciju sustava Windows Server Active Directory virtualnim računalima sustava na Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).


## <a name="scenario-diagram"></a>Scenarij dijagrama

U ovom scenariju vanjski korisnici moraju imati pristup aplikacije koje se izvoditi na poslužiteljima domene pridružili. Na VMs koji se pokreću poslužitelje aplikacije i skupa replike instaliraju Azure virtualne mreže. Virtualne mreže moguće je povezati lokalnu mrežu tako da [web-mjesto VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) vezu, kao što je prikazano na sljedećem su dijagramu ili koristite [ExpressRoute](../expressroute/expressroute-locations-providers.md) za brže vezu.

Poslužitelji aplikacija i na prevođenja uvode se unutar servise u oblaku zasebnom distribuirati računalnim obrada i unutar [dostupnost skupovi](../virtual-machines/virtual-machines-windows-manage-availability.md) za odstupanje poboljšane kvara.
Na prevođenja replicirati međusobno i s lokalnim skupa pomoću replikacije servisa Active Directory. Potrebne su nema alata za sinkronizaciju.

![Dijagram bilježaka replike servisa Active Directory domain kontroler Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Stvaranje servisa Active Directory web-mjesta za Azure virtualne mreže

Preporučuje se u stvaranja web-mjesta u servisu Active Directory koji predstavlja mreže područja koja odgovara virtualne mreže. Koji olakšava optimiziranje provjeru autentičnosti, replikacije i ostalih operacija Kontroler mjesto. Sljedeći koraci objašnjavaju kako stvoriti web-mjesta i dodatne pozadinu, u odjeljku [Dodavanje novog web-mjesta](https://technet.microsoft.com/library/cc781496.aspx).

1. Otvorite web-mjesta servisa Active Directory i usluge: **Upravitelj poslužitelja** > **Alati** > **web-mjesta servisa Active Directory i usluge**.
2. Stvaranje web-mjesta za predstavljanje regije koju ste stvorili Azure virtualne mreže: kliknite **web-mjesta** > **Akcija** > **novog web-mjesta** > upišite naziv novog web-mjesta, kao što su Azure NAM Zapad > Odaberite vezu za web-mjesta > **u redu**.
3. Stvaranje podmreži i pridružiti novo web-mjesto: dvokliknite **web-mjesta** > desnom tipkom miša kliknite **podmreže** > **Novi podmreže** > upišite rasponu IP adresa virtualne mreže (kao što su 10.1.0.0/16 u dijagramu scenarij) > odaberite novo web-mjesto Azure > **u redu**.

## <a name="create-an-azure-virtual-network"></a>Stvaranje Azure virtualne mreže

1. [Azure klasični portal](https://manage.windowsazure.com)kliknite **Novo** > **Mrežnim servisima** > **Virtualne mreže** > **Prilagođeno stvaranje** i korištenje sljedeće vrijednosti za dovršavanje čarobnjaka.

    Na ovoj stranici čarobnjaka...  | Navedite sljedeće vrijednosti
    ------------- | -------------
    **Detalji o virtualne mreže**  | <p>Naziv: Upišite naziv virtualne mreže, primjerice WestUSVNet.</p><p>Regija: Odaberite najbliže regiju.</p>
    **Povezivanje s DNS-a i VPN-a**  | <p>DNS poslužitelji: Navedite naziv i IP adrese jedan ili više lokalni DNS poslužitelji.</p><p>Povezivanje: Odaberite **Konfiguriraj VPN-a web-mjesto**.</p><p>Lokalne mreže: odredite novu lokalnu mrežu.</p><p>Ako koristite ExpressRoute umjesto VPN, potražite u članku [Konfiguriranje vezu s ExpressRoute putem davatelja sustava Exchange](../expressroute/expressroute-locations-providers.md).</p>
    **Povezivanje web-mjesto**  | <p>Naziv: Upišite naziv za lokalnu mrežu.</p><p>VPN uređaj IP adresa: odredite javnu IP adresu uređaja na kojem će se povezati s virtualne mreže. Uređaj VPN-a ne može se nalaziti iza na NAT.</p><p>Adresa: Navedite rasponi adresa za lokalne mreže (kao što su 192.168.0.0/16 u dijagramu scenarij).</p>
    **Razmaci adresu virtualne mreže**  | <p>Prostor na adresu: Odredite rasponu IP adresa za VMs koji želite pokrenuti u Azure virtualne mreže (kao što su 10.1.0.0/16 u dijagramu scenarij). Ovaj raspon adresa nije moguće preklopite s rasponi adresa lokalne mreže.</p><p>Podmreže: Navedite ime i adresu podmreže za poslužitelje aplikacije (kao što su Frontend, 10.1.1.0/24) i na skupa (kao što su pozadinskog, 10.1.2.0/24).</p><p>Kliknite **Dodaj podmreže pristupnika**.</p>

2. Nakon toga će konfigurirati pristupnik virtualne mreže da biste stvorili sigurnu vezu VPN-a web-mjesto. Upute potražite u članku [Konfiguriranje virtualne mrežni pristupnik](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .
3. Stvaranje web-mjesto VPN veza između nove virtualne mreže i uređaju sa sustavom lokalnog VPN-a. Upute potražite u članku [Konfiguriranje virtualne mrežni pristupnik](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .


## <a name="create-azure-vms-for-the-dc-roles"></a>Stvaranje Azure VMs Kontroler uloge

Ponovite sljedeće korake da biste stvorili VMs za hostiranje ulogu Kontroler prema potrebi. Trebali biste implementirati najmanje dva skupa virtualne odstupanje kvara i zalihosti. Ako Azure virtualne mreže sadrži najmanje dva skupa na sličan način konfiguriranih (to jest, su oba GCs izvođenja DNS poslužitelj i ne sadrži sve FSMO uloge i tako dalje) postavite VMs koji se pokreću one prevođenja u raspoloživost za odstupanje poboljšane kvara.
Da biste stvorili na VMs pomoću komponente Windows PowerShell umjesto korisničko Sučelje, [koristite Azure](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)potražite u članku Stvaranje i prethodno konfigurirati virtualnim strojevima utemeljen na sustavu Windows.

1. [Azure klasični portal](https://manage.windowsazure.com)kliknite **Novo** > **izračunati** > **virtualnog računala** > **Iz galerije**. Pomoću sljedeće vrijednosti za dovršavanje čarobnjaka. Prihvatite zadane vrijednosti za postavku osim ako je druga vrijednost predložena ili potrebna.

    Na ovoj stranici čarobnjaka...  | Navedite sljedeće vrijednosti
    ------------- | -------------
    **Odaberite sliku**  | Podatkovnog centra sustava Windows Server 2012 R2
    **Konfiguracija virtualnog računala**  | <p>Naziv virtualnog računala: Upišite naziv naljepnice (primjerice AzureDC1).</p><p>Novo korisničko ime: Upišite ime korisnika. Taj korisnik će biti član lokalne grupe administratora na na VM. Trebat će vam taj naziv da biste se prijavili na VM prvi put. Ugrađeni račun pod nazivom Administrator neće funkcionirati.</p><p>Novu lozinku i potvrdite: Upišite lozinku</p>
    **Konfiguracija virtualnog računala**  | <p>Servis u oblaku: Odaberite <b>Stvaranje nove servise u oblaku</b> za prvu VM i odaberite taj isti naziv oblaka servisa prilikom stvaranja više VMs koji će biti smješteno Kontroler ulogu.</p><p>Naziv DNS servisa oblaka: Odredite globalno jedinstveni naziv</p><p>Područje/grupe afinitet/virtualne mreže: Navedite naziv virtualne mreže (kao što je WestUSVNet).</p><p>Račun za pohranu: Odaberite <b>Koristi račun automatski generirani prostora za pohranu</b> za prvu VM, a zatim odaberite taj isti naziv računa za spremište prilikom stvaranja više VMs koji će biti smješteno Kontroler ulogu.</p><p>Postavljanje dostupnosti: Odaberite <b>Stvaranje skupa dostupnost</b>.</p><p>Dostupnost postavite naziv: upišite naziv dostupnosti skupa kada stvorite prvi VM, a zatim odaberite isti naziv prilikom stvaranja više VMs.</p>
    **Konfiguracija virtualnog računala**  | <p>Odaberite <b>Instalacija VM Agent</b> i druge proširenja vam je potrebna.</p>
2. Na disku priložiti svaki VM koji će se pokrenuti Kontroler uloga poslužitelja. Dodatni diskovni potreban je za spremanje baze podataka, zapisnika i SYSVOL AD. Odredite veličinu za disk (kao što su 10 GB) i ostavite **Glavno računalo predmemorije preferenca** postavljen na **ništa**. Upute, potražite u članku [kako priložiti podatkovni Disk na Windows virtualnog računala](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Kada se prvi put prijavite na VM, otvorite **Upravitelj poslužitelja** > **datoteka i servise za pohranu** za stvaranje jedinice na disku pomoću NTFS.
4. Rezerviranje statičke IP adrese za VMs koji će se pokrenuti Kontroler ulogu. Rezervirati statičke IP adrese, preuzmite platformu instalacijskog programa za Microsoft Web i [instalirajte Azure PowerShell](../powershell-install-configure.md) i pokrenite cmdlet skup AzureStaticVNetIP. Ako, na primjer:

    "Get-AzureVM - naziv servisa AzureDC1-naziv AzureDC1 | Postavljanje AzureStaticVNetIP - IPAdresa 10.0.0.4 | Ažuriranje AzureVM

Dodatne informacije o postavljanju statičke IP adrese, potražite u članku [Konfiguriranje statičke internu IP adrese za na VM](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Kliknite pločicu AD DS Azure VMs

Prijavite se na VM pa provjerite jeste li povezivanje preko VPN-a ili ExpressRoute veze web-mjesto s resursima na lokalnu mrežu. Zatim instalirajte AD DS na Azure VMs. Možete koristiti isti postupak koristite da biste instalirali dodatne Kontroler na lokalne mreže (UI, komponente Windows PowerShell ili datoteke odgovora). Kako instalirati AD DS, provjerite jesu li nova jedinica za mjesto AD baze podataka, zapisnika i SYSVOL. Ako vam je potrebna podsjetnik o instalacija AD DS, potražite u članku [Instalacija Active Directory Domain Services (razina 100)](https://technet.microsoft.com/library/hh472162.aspx) ili [ste instalirali replike Windows Server 2012 kontroler domene u postojeću domenu (Level 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Konfigurirajte DNS poslužitelj za virtualne mreže

1. [Azure klasični portal](https://manage.windowsazure.com)kliknite naziv virtualne mreže, a zatim karticu **Konfiguriraj** da biste [ponovno konfigurira IP adrese DNS poslužitelja za virtualne mreže](../virtual-network/virtual-networks-manage-dns-in-vnet.md) da biste koristili statičke IP adrese dodijeljene replike prevođenja umjesto IP adrese sustava lokalnih DNS poslužitelja.

2. Da biste bili sigurni da su sve replike VMs Kontroler na virtualne mreže konfigurirana s za korištenje DNS poslužitelji virtualne mreži, kliknite **virtualnim strojevima**, kliknite stupac statusu za svaki VM, a zatim **ponovno pokrenite**. Pričekajte da se VM prikazuje **izvodi** stanje prije no što pokušate prijavite se u njega.

## <a name="create-vms-for-application-servers"></a>Stvaranje VMs za poslužitelja aplikacije

1. Ponovite sljedeće korake da biste stvorili VMs da biste pokrenuli kao poslužitelji aplikacija. Prihvatite zadane vrijednosti za postavku osim ako je druga vrijednost predložena ili potrebna.

    Na ovoj stranici čarobnjaka...  | Navedite sljedeće vrijednosti
    ------------- | -------------
    **Odaberite sliku**  | Podatkovnog centra sustava Windows Server 2012 R2
    **Konfiguracija virtualnog računala**  | <p>Naziv virtualnog računala: Upišite naziv naljepnice (primjerice AppServer1).</p><p>Novo korisničko ime: Upišite ime korisnika. Taj korisnik će biti član lokalne grupe administratora na na VM. Trebat će vam taj naziv da biste se prijavili na VM prvi put. Ugrađeni račun pod nazivom Administrator neće funkcionirati.</p><p>Novu lozinku i potvrdite: Upišite lozinku</p>
    **Konfiguracija virtualnog računala**  | <p>Servis u oblaku: Odaberite **Stvaranje nove servise u oblaku** za prvu VM i odaberite taj isti naziv oblaka servisa prilikom stvaranja više VMs koji će biti smješteno aplikacije.</p><p>Naziv DNS servis oblaka: Odredite globalno jedinstveni naziv</p><p>Područje/grupe afinitet/virtualne mreže: Navedite naziv virtualne mreže (kao što je WestUSVNet).</p><p>Račun za pohranu: Odaberite **Koristi račun automatski generirani prostora za pohranu** za prvu VM, a zatim odaberite taj isti naziv računa za spremište prilikom stvaranja više VMs koji će biti smješteno aplikacije.</p><p>Postavljanje dostupnosti: Odaberite **Stvaranje skupa dostupnost**.</p><p>Dostupnost postavite naziv: upišite naziv dostupnosti skupa kada stvorite prvi VM, a zatim odaberite isti naziv prilikom stvaranja više VMs.</p>
    **Konfiguracija virtualnog računala**  | <p>Odaberite <b>Instalacija VM Agent</b> i druge proširenja vam je potrebna.</p>

2. Nakon svakog VM je dodijeljen, prijavite se u i pridruživanje domeni. U **Upravitelju poslužitelja**, kliknite **Lokalni poslužitelj** > **radne GRUPE** > **Promijeni...** zatim odaberite **domenu** i upišite naziv domene lokalnog. Unesite vjerodajnice domene korisnika, a zatim ponovo VM da biste dovršili spoja domene.

Da biste stvorili na VMs pomoću komponente Windows PowerShell umjesto korisničko Sučelje, [koristite Azure](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)potražite u članku Stvaranje i prethodno konfigurirati virtualnim strojevima utemeljen na sustavu Windows.

Dodatne informacije o korištenju programa Windows PowerShell potražite u članku [Početak rada s cmdleta Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx) i [Azure Cmdlet referenca](https://msdn.microsoft.com/library/azure/jj554330.aspx).

## <a name="additional-resources"></a>Dodatni resursi

-  [Smjernice za implementaciju sustava Windows Server Active Directory na Azure virtualnim strojevima](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Upute za prijenos postojeće lokalnog Hyper-V kontrolera domena za Azure pomoću komponente PowerShell Azure](http://support.microsoft.com/kb/2904015)
-  [Instaliranje novog skupa stabala Active Directory na Azure virtualne mreže](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure IT Pro IaaS: osnove (01) virtualnog računala](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) stvaranje virtualne mreža i povezivanje s više lokacija](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Cmdleti za upravljanje Azure](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
