<properties
   pageTitle="Dodavanje i uklanjanje čvorove klaster za servis tkanina samostalne | Microsoft Azure"
   description="Saznajte kako dodati ili ukloniti čvorove klaster tkanina servisa Azure na fizičke ili virtualnog računala sa sustavom Windows Server koja može biti lokalnog ili u bilo kojem oblaka."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Dodavanje i uklanjanje čvorove klaster tkanina servisa za samostalne sustavom Windows Server

Nakon što ste [stvorili svoj klaster samostalne tkanina usluge na poslužitelju Windows](service-fabric-cluster-creation-for-windows-server.md), poslovne potrebe mogu se promijeniti tako da se možda ćete morati dodati ili ukloniti više čvorove za svoj klaster. Ovaj članak sadrži detaljne upute da biste postigli taj cilj.


## <a name="add-nodes-to-your-cluster"></a>Čvorovi dodati svoj klaster

1. Priprema VM/računala za koje želite dodati u svoj klaster slijedeći korake u odjeljku [Priprema strojeva koji zadovoljava preduvjete za implementaciju klaster](service-fabric-cluster-creation-for-windows-server.md#preparemachines) navode.
2. Planiranje koji su kvara domene i nadogradnje domene koju namjeravate VM/računala za dodavanje.
3. Udaljena radna površina (RDP) u VM/stroju koji želite dodati klaster.
4. Kopiranje ili [preuzmite paket samostalne za tkanina servisa za Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) VM/računalu i raspakirajte paketa.
5. Powershell pokrenite kao administrator, a zatim otvorite mjesto raspakiranu datoteku paketa paketa.
6. Pokrenite *AddNode.ps1* Powershell s parametrima s opisom novi čvor da biste dodali. U primjeru u nastavku dodaje novi čvor naziva VM5, s vrstom NodeType0, IP adresa 182.17.34.52 u UD1 i FD1. *ExistingClusterConnectionEndPoint* je krajnje veze za čvor već u postojeće klaster. Za ovu krajnju točku možete odabrati IP adresu *Svi* čvorovi u klasteru.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Uklanjanje čvorove svoj klaster

1. Ovisno o razini Reliablity odabrali za klaster, ne možete ukloniti prvi n (3/5/7/9) čvorove vrste primarni čvor
2. Naredba RemoveNode sustavom razvojni klaster nije podržana.
2. Udaljena radna površina (RDP) u VM/stroju koji želite ukloniti iz skupine.
2. Kopiraj ili [preuzmite paket samostalne za tkanina servisa za Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) i raspakirajte paket VM/računalu.
3. Powershell pokrenite kao administrator, a zatim otvorite mjesto raspakiranu datoteku paketa paketa.
4. Pokrenite *RemoveNode.ps1* PowerShell. U primjeru u nastavku uklanja trenutnog čvora iz skupine. *ExistingClientConnectionEndpoint* je u klijent veze krajnje točke za sve čvor koji će ostati u klasteru. Odaberite web-mjesto IP adresa i u okvir za priključak krajnje točke *bilo koje* **druge čvor** u klasteru. **Drugi čvor** ažurirat će se shodno konfiguracija klaster za čvor uklonjene. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Napominjemo da čak i nakon uklanjanja čvor, ga može prikazivati kao se prema dolje u upitima i SFX. Ovo je poznati oštećenja koja će biti popravljeno u budućem izdanju. 


## <a name="next-steps"></a>Daljnji koraci
- [Konfiguriranje postavki za samostalne Windows klaster](service-fabric-cluster-manifest.md)
- [Sigurne samostalne klaster u sustavu Windows pomoću sigurnost sustava Windows](service-fabric-windows-cluster-windows-security.md)
- [Sigurne samostalne klaster u sustavu Windows pomoću X509 certifikata](service-fabric-windows-cluster-x509-security.md)
- [Stvaranje samostalne klaster tkanina servisa Azure VMs sa sustavom Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)
