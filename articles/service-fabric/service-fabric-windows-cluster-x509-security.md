<properties
   pageTitle="Povezivanje sigurne privatne klaster | Microsoft Azure"
   description="U ovom se članku opisuje kako sigurne komunikacije unutar samostalne ili privatne klaster dobro između klijenata i klaster."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Sigurne samostalne klaster u sustavu Windows pomoću X.509 certifikate

U ovom se članku opisuje sigurnu komunikaciju između različitih čvorove svoj klaster Windows samostalni kao i kako provjeriti autentičnost klijente za povezivanje s ovom klasteru pomoću X.509 certifikati. Na taj način da samo ovlašteni korisnici možete pristupiti klaster, distribuiranih aplikacije i obavljati zadatke upravljanja.  Sigurnosni certifikat mora biti omogućen na klaster stvaranja klaster.  

Dodatne informacije o sigurnosti klaster kao što je sigurnost čvor čvor, klijent čvor sigurnost i kontrola pristupa na temelju uloga potražite u članku [scenariji klaster sigurnost](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Koji se certifikati hoćete li trebati?

Da biste započeli, [preuzmite paket klaster samostalne](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) na jedan od čvorove u svoj klaster. U paketu preuzete tražit će **ClusterConfig.X509.MultiMachine.json** datoteke. Otvorite datoteku, a zatim pročitajte odjeljak za **Sigurnost** u odjeljku **Svojstva** :

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

U ovom se odjeljku opisuju potvrde koje su vam potrebne za osiguravanje svoj klaster samostalne sustava Windows. Da biste omogućili certifikat temeljenu na postavite vrijednosti **ClusterCredentialType** i **ServerCredentialType** na *X509*.

>[AZURE.NOTE] [Otisak prsta](https://en.wikipedia.org/wiki/Public_key_fingerprint) je primarni identitet certifikat. Saznajte [kako dohvatiti otisak prsta potvrdu](https://msdn.microsoft.com/library/ms734695.aspx) da biste saznali otisak prsta certifikata koji ste stvorili.

U sljedećoj su tablici navedeni certifikate koji ćete na klaster postavki:

|**Postavka CertificateInformation**|**Opis**|
|-----------------------|--------------------------|
|ClusterCertificate|Za sigurnu komunikaciju između čvorove na klaster potreban je ovog certifikata. Dvije različite certifikata, primarni i sekundarni možete koristiti za nadogradnju. U odjeljku **otisak prsta** i koje sekundarni u varijable **ThumbprintSecondary** postavite otisak prsta primarni certifikata.|
|ServerCertificate|Ova potvrda dodjeljuje klijentu prilikom pokušaja povezivanja u ovoj grupi. Pogodnost, možete odabrati korištenje istog certifikata za *ClusterCertificate* i *ServerCertificate*. Dva certifikati drugi poslužitelj, primarni i sekundarni možete koristiti za nadogradnju. U odjeljku **otisak prsta** i koje sekundarni u varijable **ThumbprintSecondary** postavite otisak prsta primarni certifikata. |
|ClientCertificateThumbprints|To je skup certifikate koji želite instalirati na čija je autentičnost provjerena klijente. Možete odrediti broj drugi klijent certifikati instalirana na računalima koji želite dopustiti pristup klaster. Postavite otisak prsta svaki certifikat u **CertificateThumbprint** varijabli. Ako **IsAdmin** postavite na *true*, zatim klijenta s ovog certifikata na njemu instalirano možete učiniti administrator aktivnosti upravljanja na klaster. Ako je **IsAdmin** *false*, klijenta s ovog certifikata možete izvršiti samo akcije dopušteno korisnička prava pristupa, obično samo za čitanje. Dodatne informacije o ulogama čitati [kontrole pristupa (RBAC) na temelju uloga](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|Postavljanje uobičajenih naziv prve klijentski certifikat za **CertificateCommonName**. **CertificateIssuerThumbprint** je otisak prsta za izdavač ove potvrde. Čitanje [radi s potvrdama](https://msdn.microsoft.com/library/ms731899.aspx) dodatne informacije o uobičajena imena i izdavač.|
|HttpApplicationGatewayCertificate|Neobavezni certifikat koji mogu biti navedeni želite li secure Http pristupnika za aplikacije. Provjerite je li postavljen u nodeTypes reverseProxyEndpointPort ako koristite taj certifikat.|

Evo primjera klaster konfiguracije gdje je dodijeljen certifikati klaster, poslužitelja i klijenta.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Dobiti na X.509 certifikata
Sigurnost komunikacije unutar klaster, najprije morat ćete dobiti X.509 certifikate za vaše čvorove klaster. Osim toga, da biste ograničili vezu s ovom klasteru ovlaštenim strojeva/korisnicima, morat ćete nabaviti i instalirati certifikati klijentskom računalu.

Za klastere pokrenute radnih opterećenja proizvodnje, koristite [Za izdavanje certifikata (CA)](https://en.wikipedia.org/wiki/Certificate_authority) prijavljeni certifikat x.509 sigurnost klaster. Pojedinosti o stjecanje ove potvrde, idite na [Kako: Dobivanje certifikata](http://msdn.microsoft.com/library/aa702761.aspx).

Za klastere koje koristite za testiranje svrhe, možete odabrati pomoću samopotpisanog certifikata.

## <a name="optional-create-a-self-signed-certificate"></a>Neobavezno: Stvaranje samopotpisane potvrde
Jedan od načina za stvaranje samopotpisanog certifikata koji se mogu zaštiti pravilno je koristiti skripte *CertSetup.ps1* u mapi servisa tkanina SDK u imeniku *C:\Programske datoteke\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*. Uredite tu datoteku, a ta postavka omogućuje stvaranje certifikata odgovarajuću naziva.

Sada potvrdi Izvoz u datoteku PFX zaštićene lozinkom. Najprije morate dobiti otisak prsta potvrde. Pokrenite aplikaciju certmgr.exe. Dođite do mape u **Lokalnom Computer\Personal** i pronaći certifikat koji ste upravo stvorili. Dvaput kliknite potvrdu da biste ga otvorili, odaberite karticu *Detalji* i pomaknite se prema dolje do polje *otisak prsta* . Kopirajte vrijednost otisak prsta u naredbu komponente PowerShell ispod, uklanjanja razmaci.  Promijenite vrijednost *$pswd* prikladnosti sigurne lozinke da biste ga zaštititi i pokrenuli u PowerShell:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Da biste vidjeli detalje certifikata koji je instaliran na računalu možete pokrenite sljedeću naredbu komponente PowerShell:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Osim toga, ako imate pretplatu na Azure, slijedite u odjeljku [Dodavanje certifikata da sigurnog ključ](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>Instalirajte potvrde
Nakon što dodate certifikate, instalirajte ih na čvorove klaster. Vaš čvorove moraju imati najnoviju komponente Windows PowerShell 3.x instalirana na njima. Morat ćete ponovite korake na svakom čvor za klaster i poslužitelja potvrde i sve sekundarne certifikata.

1. Kopiranje datoteke .pfx čvor.
2. Otvorite prozor PowerShell kao administrator, a zatim unesite sljedeće naredbe. Zamijenite *$pswd* lozinku koju ste koristili za stvaranje ovog certifikata. Zamijenite *$PfxFilePath* cijeli put .pfx kopirali čvor.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Sljedeće morate postaviti kontrola pristupa na ovom certifikatu tako da se servis tkanina procesa koji se izvodi u odjeljku mrežni servis račun, možete ga koristiti tako da pokrenete sljedeću skriptu. Navedite otisak prsta certifikat i "MREŽNI servis" za račun servisa. Možete provjerite jesu li ACL-a na certifikatu ispravni pomoću alata za certmgr.exe i pogledate upravljanje privatni ključevi u certifikata.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Povezivanje ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - StoreLocation CurrentUser - Nazivtrgovine Moje
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Povezivanje ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```
