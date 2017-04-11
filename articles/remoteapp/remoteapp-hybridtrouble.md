
<properties
    pageTitle="Otklanjanje poteškoća prilikom stvaranja zbirke hibridnog RemoteApp | Microsoft Azure"
    description="Saznajte kako otkloniti poteškoće RemoteApp hibridnog zbirke stvaranja pogreške"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Otklanjanje poteškoća prilikom stvaranja zbirke hibridnog Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Zbirka hibridnog nalazi se u i sprema podatke u Azure oblaka, ali i omogućuje korisnicima pristup podacima i resurse pohranjene u lokalnoj mreži. Korisnici mogu pristupiti aplikacijama po prijave pomoću vjerodajnica službene sinkronizirati ili povezani Azure Active Directory. Možete implementirati hibridnog zbirke koja koristi postojeće Azure virtualne mreže ili možete stvoriti novu virtualne mreže. Preporučujemo stvaranje i korištenje podmreži virtualne mreže s rasponom CIDR dovoljna za očekivani budućeg rasta Azure RemoteApp.

Zbirka još niste stvorili? Upute potražite u članku [Stvaranje hibridnog zbirke](remoteapp-create-hybrid-deployment.md) .

Ako imate problema s stvaranja mjesta ili mislite ako zbirka ne funkcionira onako kako bi trebao, pogledajte sljedeće informacije.

## <a name="your-image-is-invalid"></a>Sliku nije valjan ##
Ako se prikaže poruka kao "GoldImageInvalid" kada čekate Azure Dodjela vaše zbirke, to znači da sliku predložak ne zadovoljava [definirani preduvjeti slike](remoteapp-imagereqs.md). Tako, otvorite pročitajte [preduvjete](remoteapp-imagereqs.md), rješavanje sliku i pokušate stvaraju vašu zbirku.



## <a name="does-your-vnet-have-network-security-groups-defined"></a>Mora li vaš VNET mreže sigurnosnih grupa definirani? ##
Ako imate mrežni sigurnosne grupe definiranih podmreže koristite za svoju zbirku, provjerite jesu li te [URL-ovi i priključaka](remoteapp-ports.md) pristupa u vašoj podmreži.

Dodatni mrežni sigurnosne grupe možete dodati VMs implementiran koje ste u podmreže za kontrola.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>Koristite li sami DNS poslužitelji? Te da su dostupne vašoj podmreži VNET? ##
>[AZURE.NOTE] Morate provjerite jesu li DNS poslužitelji u vašem VNET uvijek prema gore i uvijek možete riješiti virtualnim strojevima smješten u na VNET. Nemojte koristiti Google DNS-a za to.


Za hibridno zbirke koristite vlastitu DNS poslužitelji. Odredite ih u konfiguraciji sheme mreže ili putem portala za upravljanje prilikom stvaranja virtualne mreže. DNS poslužitelji koriste se u redoslijedu su navedeni način prebacivanje (umjesto kružnog).  
Pogledajte [Naziv razlučivost VMs, a uloga instance](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) provjerite jesu li DNS poslužitelji pravilno konfigurirani correcly.

Provjerite je li DNS poslužitelji za vašu zbirku su dostupne i nudi podmreže VNET koji ste naveli za tu zbirku.

Ako, na primjer:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![Definiranje DNS-a](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Koristite programa kontroler domene servisa Active Directory u sklopu zbirke? ##
Trenutno samo jedne domene servisa Active Directory mogu se pridružiti Azure RemoteApp. Zbirka hibridnog podržava samo Azure Active Directory račune koji su sinkronizirani pomoću alata DirSync iz Windows Server Active Directory implementacije; Konkretno, ili sinkronizirali s mogućnošću sinkronizaciju lozinke ili sinkronizirali s vanjskim pristupom Active Directory Federation Services (AD FS) konfiguriran. Morate stvoriti prilagođenu domenu koja odgovara UPN nastavka domene za lokalne domene i postavljanje Integracija direktorija.

Dodatne informacije potražite u članku [Konfiguriranje u servisu Active Directory Azure RemoteApp](remoteapp-ad.md) .

Provjerite je li pojedinosti o domeni navedene su valjani i kontrolerom domene dostupan iz VM stvorene u podmreži koristi za Azure udaljene aplikacije. I provjerite je li vjerodajnice računa servisa navedeni imate dozvole za dodavanje računala navedene domene i Ponuđeni naziv AD može biti riješen od navedenih u na VNET DNS-a.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Što naziv domene nije navedete prilikom stvaranja zbirke web? ##

Naziv domene koje ste stvorili ili dodati mora biti interne domene naziv (ne Azure AD naziva domene) i mora biti u obliku resolvable DNS (contoso.local). Na primjer, imate internog naziva servisa Active Directory (contoso.local) i Active Directory UPN (contoso.com) –, morate koristiti naziv internog prilikom stvaranja zbirke web.
