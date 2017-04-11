<properties
   pageTitle="Stvaranje samostalne klaster s Azure VMs sa sustavom Windows | Microsoft Azure"
   description="Saznajte kako stvarati i upravljati programa klaster tkanina servisa Azure na Azure virtualnim strojevima sa sustavom Windows Server."
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
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Stvorite tri čvor samostalne servisa tkanina klaster s Azure virtualnim strojevima sa sustavom Windows Server

U ovom se članku opisuje kako stvoriti klaster na utemeljen na sustavu Windows Azure virtualnim strojevima (VMs), pomoću samostalne servisa tkanina instalacijski program za Windows Server. Ovo je posebno slova [Stvaranje i upravljanje klaster sustavom Windows Server](service-fabric-cluster-creation-for-windows-server.md) gdje u VMs su [Azure VMs sa sustavom Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md), no ne stvarate [programa Azure oblaku servisa tkanina klaster](service-fabric-cluster-creation-via-portal.md). Razlika je da klaster servisa tkanina samostalne stvorio sljedeće korake, upravlja vam tijekom na Azure upravlja i nadograditi resursa davatelj usluge tkanina oblaku klastere tkanina servisa.


## <a name="steps-to-create-the-standalone-cluster"></a>Koraci za stvaranje samostalne klaster

1. Prijavite se na portal za Azure i stvorite na novu Windows Server 2012 R2 podatkovnog centra VM u grupu resursa. U članku [Stvaranje VM Windows Azure portalu](../virtual-machines/virtual-machines-windows-hero-tutorial.md) više pojedinosti.
2. Dodavanje nekoliko više Windows Server 2012 R2 podatkovnog centra VMs u istoj grupi resursa. Provjerite je li svaki od na VMs ima isti administrator korisničko ime i lozinku prilikom stvaranja. Jednom stvoreni trebali biste vidjeti sve tri VMs u istom virtualne mreže.
3. Povezivanje sa svim na VMs i isključite Vatrozid za Windows koristi [Upravitelj poslužitelja, lokalni poslužitelj nadzorne ploče](https://technet.microsoft.com/library/jj134147.aspx). Taj se način mogu li mrežni promet komunicirati između strojeva. Dok ste povezani svaki stroj, dobili IP adresu tako da otvorite naredbeni redak i pisati `ipconfig`. Umjesto toga možete vidjeti IP adresu svakom računalu tako da odaberete virtualne mrežnom resursu za grupu resursa na portalu za Azure.
4. Povezali s nekim u VMs i testirati da na dva VMs možete ping uspješno.
5. Povežite se s jednom od VMs i [preuzmite paket servisa tkanina samostalne za Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) u novu mapu na računalu i izdvajanje paketa.
6. Otvorite datoteku *ClusterConfig.Unsecure.MultiMachine.json* u Bloku za pisanje i uređivanje svaki čvora s tri IP adrese strojeva. Promjena naziva klaster pri vrhu, a zatim spremite datoteku.  Djelomično primjera manifest klaster je prikazano u nastavku.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Otvorite [prozor Očisti filtar](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Dođite do mape u kojoj dobivenih instalacijski paket preuzete samostalne i spremili datoteku manifesta klaster. Pokrenite sljedeću naredbu komponente PowerShell.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Trebali biste vidjeti PowerShell pokrenite, povezivanje sa svakom računalu i stvaranje klaster. Nakon minutu, možete provjeriti klaster li radu povezivanjem Explorer tkanina servisa na jednom od računala IP adresa – primjerice pomoću `http://10.7.0.5:19080/Explorer/index.html`. Budući da je to samostalne klaster pomoću Azure VMs da biste zaštitili će moraju [implementirati certifikata za Azure VMs](service-fabric-windows-cluster-x509-security.md) ili postaviti jednu strojeva kao na [Windows Server Active Directory (AD) kontroler za provjeru autentičnosti sustava Windows](service-fabric-windows-cluster-windows-security.md), kao što biste slijedili lokalno.


## <a name="next-steps"></a>Daljnji koraci
- [Stvaranje samostalne servisa tkanina klastere na Windows Server ili Linux](service-fabric-deploy-anywhere.md)
- [Dodavanje i uklanjanje čvorove klaster za servis tkanina samostalni](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Konfiguriranje postavki za samostalne Windows klaster](service-fabric-cluster-manifest.md)
- [Sigurne samostalne klaster u sustavu Windows pomoću sigurnost sustava Windows](service-fabric-windows-cluster-windows-security.md)
- [Sigurne samostalne klaster u sustavu Windows pomoću X509 certifikata](service-fabric-windows-cluster-x509-security.md)
