<properties
    pageTitle="Instalacija sustava Active Directory skupa stabala na Azure virtualne mreže | Microsoft Azure"
    description="Praktični vodič kojem se objašnjava kako stvoriti novi skup stabala servisa Active Directory na virtualnog računala (VM) na Azure virtualne mreže."
    services="active-directory, virtual-network"
    keywords="servisa Active directory virtualnog računala, skup instaliranje servisa active directory stabala videozapise servisa azure active directory "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Instaliranje novog skupa stabala Active Directory na Azure virtualne mreže

U ovoj se temi objašnjava da biste stvorili novo okruženje za Active Directory za Windows Server na Azure virtualne mreže na virtualnog računala (VM) [Azure virtualne mreže](../virtual-network/virtual-networks-overview.md). U ovom slučaju Azure virtualne mreže nije povezan s lokalne mreže.

Koje možda će vas zanimati te povezane teme:

- Videozapis koji prikazuje ove korake, potražite [u](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network) članku instaliranje novog skupa stabala servisa Active Directory na Azure virtualne mreže
- Možete [konfigurirati VPN-a web-mjesto](../vpn-gateway/vpn-gateway-site-to-site-create.md) , a zatim na nešto instalirati da skupa stabala novo ili proširivanje lokalnog skupa stabala Azure virtualne mrežu. Te korake potražite u članku [Instalacija replike Active Directory kontroler domene u Azure virtualne mreže](../active-directory/active-directory-install-replica-active-directory-domain-controller.md).
-  Konceptualni upute o instaliranju Active Directory Domain Services (AD DS) na Azure virtualne mreže potražite u članku [smjernice za implementaciju sustava Windows Server Active Directory virtualnim računalima sustava na Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Scenarij dijagrama

U ovom scenariju vanjski korisnici moraju imati pristup aplikacije koje se izvoditi na poslužiteljima domene pridružili. VMs koji se izvode poslužitelji aplikacija VMs koji se izvode kontrolera domena su instalirani i instalirali u drugom vlastite servis u oblaku u Azure virtualne mreže. Također su uvrstiti u raspoloživost za odstupanje poboljšane kvara.

![Active Directory skupa stabala na virtualnim strojevima u Azure virtualne mreže ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Kako se to razlikuju lokalnog?

Ne postoji mnogo razlika između instalacije kontroler domene na Azure i lokalnih. Glavne razlike su navedene u tablici u nastavku.

Da biste konfigurirali...  | Lokalni  | Azure virtualne mreže
------------- | -------------  | ------------
**IP adresa za kontrolerom domene**  | Dodjeljivanje statičke IP adrese na mrežnih prilagodnika svojstava   | Pokrenite cmdlet skup AzureStaticVNetIP da biste dodijelili statičke IP adrese
**Razrješavanje DNS klijenta**  | Postavljanje Preferirani i Zamjenski DNS poslužitelj adrese na mreži svojstva prilagodnika članova domene   | Postavljanje adresu DNS poslužitelja u svojstva virtualnog mreže
**Active Directory baze podataka za pohranu**  | Ako želite promijeniti zadano mjesto za pohranu iz C:\  | Morate promijeniti zadano mjesto za pohranu iz C:\



## <a name="create-an-azure-virtual-network"></a>Stvaranje Azure virtualne mreže

1. Prijavite se na portal Azure klasični.
2. Stvaranje virtualne mreže. Kliknite **mrežama** > **Stvaranje virtualne mreže**. Pomoću vrijednosti u tablici u nastavku da biste dovršili Čarobnjak.

    Na ovoj stranici čarobnjaka...  | Navedite sljedeće vrijednosti
    ------------- | -------------
    **Detalji o virtualne mreže**  | <p>Naziv: Upišite naziv virtualne mreže</p><p>Regija: Odaberite najbliže regiju</p>
    **DNS-a i VPN-a**  | <p>DNS poslužitelj ostavite prazno</p><p>Nemojte odabrati mogućnost VPN-a</p>
    **Razmaci adresu virtualne mreže**  | <p>Naziv podmreže: Unesite naziv vašoj podmreži</p><p>Početni IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Stvaranje VMs da biste pokrenuli kontrolera domena i DNS ulogama poslužitelja

Ponovite sljedeće korake da biste stvorili VMs za hostiranje ulogu Kontroler prema potrebi. Trebali biste implementirati najmanje dva skupa virtualne odstupanje kvara i zalihosti. Ako Azure virtualne mreže sadrži najmanje dva skupa na sličan način konfiguriranih (to jest, su oba GCs izvođenja DNS poslužitelj i ne sadrži sve FSMO uloge i tako dalje) postavite VMs koji se pokreću one prevođenja u raspoloživost za odstupanje poboljšane kvara.

Da biste stvorili na VMs pomoću komponente Windows PowerShell umjesto korisničko Sučelje, [koristite Azure](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)potražite u članku Stvaranje i prethodno konfigurirati virtualnim strojevima utemeljen na sustavu Windows.

1. Na portalu klasični kliknite **Novo** > **izračunati** > **virtualnog računala** > **Iz galerije**. Pomoću sljedeće vrijednosti za dovršavanje čarobnjaka. Prihvatite zadane vrijednosti za postavku osim ako je druga vrijednost predložena ili potrebna.

    Na ovoj stranici čarobnjaka...  | Navedite sljedeće vrijednosti
    ------------- | -------------
    **Odaberite sliku**  | Podatkovnog centra sustava Windows Server 2012 R2
    **Konfiguracija virtualnog računala**  | <p>Naziv virtualnog računala: Upišite naziv naljepnice (primjerice AzureDC1).</p><p>Novo korisničko ime: Upišite ime korisnika. Taj korisnik će biti član lokalne grupe administratora na na VM. Trebat će vam taj naziv da biste se prijavili na VM prvi put. Ugrađeni račun pod nazivom Administrator neće funkcionirati.</p><p>Novu lozinku i potvrdite: Upišite lozinku</p>
    **Konfiguracija virtualnog računala**  | <p>Servis u oblaku: Odaberite <b>Stvaranje nove servise u oblaku</b> za prvu VM i odaberite taj isti naziv oblaka servisa prilikom stvaranja više VMs koji će biti smješteno Kontroler ulogu.</p><p>Naziv DNS servis oblaka: Odredite globalno jedinstveni naziv</p><p>Područje/grupe afinitet/virtualne mreže: Navedite naziv virtualne mreže (kao što je WestUSVNet).</p><p>Račun za pohranu: Odaberite <b>Koristi račun automatski generirani prostora za pohranu</b> za prvu VM, a zatim odaberite taj isti naziv računa za spremište prilikom stvaranja više VMs koji će biti smješteno Kontroler ulogu.</p><p>Postavljanje dostupnosti: Odaberite <b>Stvaranje skupa dostupnost</b>.</p><p>Dostupnost postavite naziv: upišite naziv dostupnosti skupa kada stvorite prvi VM, a zatim odaberite isti naziv prilikom stvaranja više VMs.</p>
    **Konfiguracija virtualnog računala**  | <p>Odaberite <b>Instalacija VM Agent</b> i druge proširenja vam je potrebna.</p>
2. Na disku priložiti svaki VM koji će se pokrenuti Kontroler uloga poslužitelja. Dodatni diskovni potreban je za spremanje baze podataka, zapisnika i SYSVOL AD. Odredite veličinu za disk (kao što su 10 GB) i ostavite **Glavno računalo predmemorije preferenca** postavljen na **ništa**. Upute, potražite u članku [kako priložiti podatkovni Disk na Windows virtualnog računala](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Kada se prvi put prijavite na VM, otvorite **Upravitelj poslužitelja** > **datoteka i servise za pohranu** za stvaranje jedinice na disku pomoću NTFS.
4. Rezerviranje statičke IP adrese za VMs koji će se pokrenuti Kontroler ulogu. Rezervirati statičke IP adrese, preuzmite platformu instalacijskog programa za Microsoft Web i [instalirajte Azure PowerShell](../powershell-install-configure.md) i pokrenite cmdlet skup AzureStaticVNetIP. Ako, na primjer:

    "Get-AzureVM - naziv servisa AzureDC1-naziv AzureDC1 | Postavljanje AzureStaticVNetIP - IPAdresa 10.0.0.4 | Ažuriranje AzureVM

Dodatne informacije o postavljanju statičke IP adrese, potražite u članku [Konfiguriranje statičke interne IP adrese za na VM](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Instalirajte Windows Server Active Directory

Koristite isti Rutina [Instalacija AD DS](https://technet.microsoft.com/library/jj574166.aspx) koristite lokalni (to jest, možete koristiti korisničko Sučelje, datoteke odgovora ili komponente Windows PowerShell). Morate unijeti vjerodajnice administratora da biste instalirali skupa stabala novo. Da biste odredili mjesto za bazu podataka servisa Active Directory, zapisnika i SYSVOL, promijenite zadano mjesto za pohranu iz pogon operacijski sustav na disk dodatnih podataka koju ste priložili na VM.

Po završetku instalacije Kontroler ponovno povezati s VM i prijavite se na kontroler domene. Imajte na umu da biste odredili vjerodajnice domene.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Ponovno postavite DNS poslužitelj za Azure virtualne mreže

1. Ponovno postavljanje forwarder postavke DNS-a na novi Kontroler/DNS poslužitelj.
  1. U upravitelju poslužitelja, kliknite **Alati** > **DNS-a**.
  2. U **Upravitelju DNS-a**, desnom tipkom miša kliknite naziv DNS poslužitelja, a zatim kliknite **Svojstva**.
  3. Na kartici **prosljeđivanja** kliknite IP adresu na forwarder, a zatim **Uređivanje**.  Odaberite IP adresu i pritisnite **Izbriši**.
  4. Kliknite **u redu** da biste zatvorili uređivač i **u redu** da biste zatvorili svojstva DNS poslužitelja.
2. Ažurirajte postavke DNS poslužitelja za virtualne mreže.
  1. Kliknite **Virtualne mreža** > dvokliknite virtualne mreže koju ste stvorili > **Konfiguriraj** > **DNS poslužitelji**, upišite naziv i DIP jedne od VMs koja se pokreće uloga Kontroler/DNS poslužitelja i kliknite **Spremi**.
  2. Odaberite na VM i kliknite **ponovno** da biste pokrenuli VM da biste konfigurirali postavke za razrješavanje DNS-a IP adrese novi DNS poslužitelj.


## <a name="create-vms-for-domain-members"></a>Stvaranje VMs za članove domene

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


## <a name="see-also"></a>Vidi također

-  [Kako instalirati novi skup stabala servisa Active Directory na Azure virtualne mreže](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Smjernice za implementaciju sustava Windows Server Active Directory na Azure virtualnim strojevima](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [Konfiguriranje web-mjesto VPN-a](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Instalirajte replike Active Directory kontroler domene u Azure virtualne mreže](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure IT Pro IaaS: osnove (01) virtualnog računala](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure IT Pro IaaS: (05) stvaranje virtualne mreža i povezivanje s više lokacija](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md)
-  [Kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure Cmdlet Reference](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Postavljanje Azure VM statičke IP adrese](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Kako dodijeliti statičke IP Azure VM](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Instaliranje nove Active Directory skupa stabala](https://technet.microsoft.com/library/jj574166.aspx)
-  [Uvod u Active Directory Domain Services (AD DS) virtualizacije (razina 100)](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
