<properties
 pageTitle="Klaster HPC paket za Excel i SOA | Microsoft Azure"
 description="Početak rada pokrenut veliki radnih opterećenja programa Excel i SOA na programa paketa HPC klaster servisu Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Početak rada u programu Excel i SOA radnih opterećenja sustavom programa paketa HPC klaster servisu Azure

U ovom se članku objašnjava implementacija paket Microsoft HPC klaster na Azure virtualnih računala pomoću predloška programa Azure brzi početak rada ili po želji skripte za implementaciju Azure PowerShell. Klaster koristi Azure Marketplace VM slike dizajniran za rad u programu Microsoft Excel ili radnih opterećenja vezanima uz servis arhitektura (SOA) s HPC paket. Možete koristiti klaster pokrenuti jednostavne HPC Excel SOA services s lokalnog računala klijenta. Usluge Excel HPC obuhvaćaju rasteretite radne knjige u programu Excel i korisnički definirane funkcije programa Excel ili UDF-ove.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Visoke razine sljedeći dijagram prikazuje klaster HPC paket stvarate.

![Klaster HPC s čvorove izvodi radnih opterećenja programa Excel][scenario]

## <a name="prerequisites"></a>Preduvjeti

*   **Klijentsko računalo** - potrebno je utemeljen na sustavu Windows klijentsko računalo da biste poslali zadataka programa Excel i SOA uzorka klaster. Potreban vam je i na računalu sa sustavom Windows da biste pokrenuli Azure PowerShell klaster implementacijsku skriptu (Ako ste odabrali taj način implementacije) i

*   **Azure pretplate** – ako nemate pretplatu na Azure, možete stvoriti na [slobodno računa](https://azure.microsoft.com/pricing/free-trial/) u samo nekoliko minuta.

*   **Kvota jezgri** – možda ćete morati povećati kvote jezgri, osobito ako implementacija nekoliko čvorove klaster s multicore VM veličine. Ako koristite predložak Azure brzi početak rada, kvote jezgri u upravitelju resursa je po regijama Azure. U tom slučaju, bilo bi dobro da biste povećali kvote u određenoj regiji. Potražite u članku [ograničenja Azure pretplatu, kvote, i ograničenja](../azure-subscription-service-limits.md). Da biste povećali ograničenja, [otvorite zahtjev za mrežna Služba podrške](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) bez troškova.

*   Instalira se **licencu za Microsoft Office** – ako pokrenete računalnim čvorove trgovine HPC paket VM slika pomoću verzije programa Microsoft Excel Professional Plus 2013 za procjenu 30 dana programu Microsoft Excel. Nakon izvođenja razdoblja, morate unijeti valjana licenca Microsoft Office da biste aktivirali Excel da biste nastavili da biste pokrenuli radnih opterećenja. Potražite u članku [Aktivacija programa Excel](#excel-activation) u nastavku ovog članka. 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Korak 1. Postavljanje programa paketa HPC klaster servisu Azure

Pokazat ćemo dvije mogućnosti da biste postavili klaster: prvi, pomoću predloška programa Azure brzi početak rada i Azure portal; i druge, pomoću skripte za implementaciju Azure PowerShell.


### <a name="option-1-use-a-quickstart-template"></a>Mogućnost 1. Korištenje predloška za brzi početak rada
Brzo i jednostavno implementacija programa paketa HPC klaster na portalu za Azure pomoću predloška Azure brzi početak rada. Kada otvorite predložak na portalu, dobit ćete jednostavno korisničkog Sučelja koje unosite postavke za svoj klaster. Evo nekoliko koraka. 

>[AZURE.TIP]Ako želite, pomoću [predloška Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) koji stvara slične klaster namijenjenu radnih opterećenja programa Excel. Koraci malo razlikovati od sljedećeg.

1.  Posjetite [stranicu predložak stvoriti klasteru HPC GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Ako želite, pregledajte informacije o predlošku i izvornog koda.

2.  Zatim **Implementiraj za Azure** da biste pokrenuli implementacije s predloškom na portalu za Azure.

    ![Implementacija predloška Azure][github]

3.  Na portalu, slijedite ove korake da biste unijeli parametre HPC klaster predloška.

    na. Na stranici **parametara** unesite ili izmijenite vrijednosti za parametre predložak. (Kliknite ikonu pokraj svake postavke za pomoć). Ogledne vrijednosti prikazane su na sljedećem zaslonu. U ovom se primjeru stvara klaster pod nazivom *hpc01* u domeni *hpc.local* koji se sastoji od glavni čvor i 2 izračunati čvorove. Čvorovi računalnim stvaraju HPC paket VM slike koja obuhvaća Microsoft Excel.

    ![Unesite parametre][parameters]

    >[AZURE.NOTE]Glavni čvor VM automatski se stvara s [najnovijim tržište slika](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC paket 2012 R2 na Windows Server 2012 R2. Trenutno slike temelji se na HPC Service Pack 2012 R2 ažuriranje 3.
    >
    >Izračun VMs čvor se stvaraju iz najnovije slike čvor obitelji odabranog računalnim. Odaberite mogućnost **ComputeNodeWithExcel** najnovije HPC paket računalnim čvor slike koji uključuje probna verzija programa Microsoft Excel Professional Plus 2013. Da biste implementirali klaster za opće SOA sesije ili Excel UDF rasteretite, odaberite mogućnost **ComputeNode** (bez instaliran Excel).

    b. Odaberite pretplatu.

    c. Stvorite grupu resursa za klaster, kao što su *hpc01RG*.

    d. Odaberite mjesto za grupu resursa, kao što su središnje SAD-a.

    e. Na stranici **pravne uvjete** , pregledajte uvjete. Ako se slažete, kliknite **kupite**. Zatim, kada završite postavljanje vrijednosti za predložak, kliknite **Stvori**.

4.  Kada se dovrši implementacijskih (traje oko 30 minuta), izvoz datoteka certifikata klaster iz čvor glavni klaster. U noviji koraku uvoz ovaj javno certifikat na klijentskom računalu omogućuje provjeru autentičnosti poslužiteljsko za sigurno povezivanje HTTP.

    na. Povezivanje s čvor glavni putem udaljene radne površine na portalu Azure.

     ![Povezivanje s glavni čvor][connect]

    b. Koristite standardni postupci u upravitelju certifikat da biste izvezli glavni čvor certifikat (koja se nalazi u odjeljku certifikata: \LocalMachine\My) bez privatni ključ. U ovom se primjeru izvesti *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Izvoz certifikata][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Mogućnost 2. Koristite skriptu HPC paket IaaS implementacije

Implementacijsku skriptu HPC paket IaaS omogućuje drugi način svestrane za implementaciju sustava klaster HPC paket. Stvara klaster u modelu uvođenje klasičnog dok predložak koristi ogledni model implementacije Azure Voditelj resursa. Osim toga, skripta kompatibilan s pretplatom na Azure globalnog ili Azure Kina servisu.

**Dodatni preduvjeti**

* **Azure PowerShell** - [instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) (verzija 0.8.10 ili noviji) na klijentskom računalu.

* **HPC paket IaaS implementacijsku skriptu** – preuzimanje i otpakiravanje najnoviju verziju skripte iz [Microsoftova centra za preuzimanje](https://www.microsoft.com/download/details.aspx?id=44949). Provjerite verziju skriptu tako da pokrenete `New-HPCIaaSCluster.ps1 –Version`. U ovom se članku temelji se na verziju 4.5.0 ili noviju skriptu.

**Stvaranje konfiguracijska datoteka**

 Implementacijsku skriptu HPC paket IaaS kao unos koji opisuje infrastrukture klasteru HPC koristi XML datoteku konfiguracije. Da biste implementirali klaster sastoji se od glavni čvor i 18 računalnim čvorove stvorene iz slike čvor računalnim koja obuhvaća Microsoft Excel, zamijenite vrijednosti okruženju sustava u sljedeće oglednu datoteku za konfiguraciju. Dodatne informacije o konfiguracijska datoteka potražite u članku Manual.rtf datoteke u mapi skripte i [Stvaranje programa klasteru HPC s implementacijsku skriptu HPC paket IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Napomene o konfiguracijska datoteka**

* **VMName** glavni čvor **mora** biti isti kao **naziv servisa**ili SOA poslove se neće pokrenuti.

* Provjerite jesu li **EnableWebPortal** tako da je certifikat glavni čvor generira i izvesti.

* Datoteku određuje skriptu PowerShell poslije konfiguracije PostConfig.ps1 koja se izvršava na glavni čvor. Sljedeće ogledne skripte konfigurira niz za povezivanje Azure prostora za pohranu, uklanja čvor uloga računalnim iz čvor glavni i objedinjuje sve čvorove Internetu prilikom implementacije. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Pokrenuti skriptu**

1.  Otvorite konzole za PowerShell na klijentskom računalu kao administrator.

2.  Promjena direktorija mapa za skripte (E:\IaaSClusterScript u ovom primjeru).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Da biste implementirali klaster HPC paket, pokrenite sljedeću naredbu. U ovom se primjeru pretpostavlja da konfiguracijska datoteka se nalazi u E:\HPCDemoConfig.xml.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

Skripta za implementaciju HPC paket se izvodi neko vrijeme. Jedna od stvari koje ne skriptu je izvoz i preuzmite certifikat klaster i spremite ga u mapi Dokumenti trenutnog korisnika na klijentskom računalu. Skripta stvara poruku koja je slična sljedećoj. U sljedećem koraku uvesti certifikat u spremištu odgovarajuću potvrdu.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Korak 2. Offload radnih knjiga programa Excel i pokretanje korisnički definiranih funkcija iz klijent za lokalni

### <a name="excel-activation"></a>Aktivacija programa Excel

Prilikom korištenja slika ComputeNodeWithExcel VM za proizvodnju radnih opterećenja, morate unesete valjani ključ licence Microsoft Office da biste aktivirali programa Excel na čvorove računalnim. U suprotnom probna verzija programa Excel istječe nakon 30 dana i pokretanje radnih knjiga programa Excel neće uspjeti s COMException (0x800AC472). 

Excel možete rearm drugi 30 dana za vrijeme izvođenja: prijavite se u glavni čvora i clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` na sve Excel izračunati čvorove putem HPC klaster Manager. Možete rearm maksimalno dvaput. Nakon toga unesete valjani ključ licencu za Office.

Za Office Professional Plus 2013 instaliran na slici VM je količinsko izdanje s na generički glasnoću licence ključa (GVLK). Možete aktivirati putem Key Management Service (KMS) / Active Directory-Based aktivacije (AD-BA) ili ključ višestruku aktivaciju (MAK). 

    * Da biste koristili AD/KMS-BA, koristite postojećeg poslužitelja KMS ili postaviti novi pomoću paket licenca za Microsoft Office 2013 glasnoću. (Ako želite, postavite ga na glavni čvor.) Zatim aktiviraj ključ glavno računalo za KMS putem Interneta ili telefona. Zatim clusrun `ospp.vbs` da biste postavili KMS poslužitelja i priključka i aktivacija sustava Office na sve Excel izračunati čvorove. 
    
    * Da biste koristili MAK, prvi clusrun `ospp.vbs` tipku za unos i aktiviranje sve Excel izračunati čvorove putem Interneta ili telefona. 

>[AZURE.NOTE]Maloprodajne ključeve proizvoda za Office Professional Plus 2013 nije moguće koristiti s VM sliku. Ako imate valjan tipke i instalacijskog medija za izdanja sustava Office ili Excel osim ovaj količinsko izdanje sustava Office Professional Plus 2013, možete ih koristiti umjesto toga. Najprije deinstalirajte ovo izdanje količinskog i instalirajte izdanje koje imate. Reinstalled čvor računalnim Excel možete biti zabilježene kao prilagođeni VM sliku da biste koristili implementacije na razini.

### <a name="offload-excel-workbooks"></a>Offload radnih knjiga programa Excel

Slijedite ove korake da biste offload radne knjige programa Excel tako da se izvodi na klasteru HPC paketa u Azure. Da biste to učinili, morate imati Excel 2010 ili 2013 već instaliran na klijentskom računalu.

1. Koristite neku od mogućnosti u koraku 1 za implementaciju sustava klaster HPC paket pomoću programa Excel izračunati čvor slike. Nabavite klaster datoteka certifikata (.cer) i klaster korisničko ime i lozinku.

2. Na klijentskom računalu uvoz certifikata klaster u odjeljku certifikata: \CurrentUser\Root.

3. Provjerite je li instaliran Excel. Stvaranje datoteke Excel.exe.config sljedeće sadržajem u istu mapu kao i Excel.exe na klijentskom računalu. Na taj način da HPC paket 2012 R2 Excel COM dodatka učitava uspješno.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  Postavljanje klijent za slanje poslove klaster HPC paket. Jedan je potpuna [Instalacija HPC Service Pack 2012 R2 ažuriranje 3](http://www.microsoft.com/download/details.aspx?id=49922) preuzmite i instalirajte paket HPC klijenta. Osim toga, preuzmite i instalirajte [uslužni programi HPC Service Pack 2012 R2 ažuriranje 3 klijenta](https://www.microsoft.com/download/details.aspx?id=49923) i na odgovarajuće Visual C++ 2010 za slobodnu distribuciju za vaše računalo ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  U ovom primjeru koristimo Ogledna radna knjiga programa Excel s nazivom ConvertiblePricing_Complete.xlsb. Možete preuzeti [ovdje](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Kopirajte radnu knjigu programa Excel radnu mapu kao što su D:\Excel\Run.

7.  Otvorite radnu knjigu programa Excel. Na vrpci **razvoju** kliknite **COM dodaci** , a zatim potvrdite da HPC paket Excel COM dodatak je uspješno učitan.

    ![Excel dodatak za HPC paket][addin]

8.  Uređivanje makronaredbe VBA HPCControlMacros u programu Excel tako da promijenite komentirane retke kao što je prikazano u sljedeću skriptu. Zamijenite odgovarajuće vrijednosti za vaše okruženje.

    ![Akcija makronaredbe programa Excel za HPC paket][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Kopirajte radnu knjigu programa Excel u imeniku prijenos kao što su D:\Excel\Upload. Taj imenik naveden je u HPC_DependsFiles konstanta u VBA makronaredbe.

10. Da biste pokrenuli radnu knjigu na klasteru u Azure, kliknite gumb **klaster** na radnom listu.

### <a name="run-excel-udfs"></a>Pokretanje korisnički definiranih funkcija programa Excel

Da biste pokrenuli Excel UDF-ove, slijedite prethodne korake 1 do 3 da biste postavili klijentskom računalu. Za UDF-ove programa Excel, ne morate imati instaliranu na računalnim čvorove aplikaciju Excel. Tako, prilikom stvaranja svoj klaster izračunati čvorove, mogli odabrati u običnom izračunati čvor slika umjesto image čvor računalnim s programom Excel.

>[AZURE.NOTE] Nema ograničenja broja 34 znakova u programu Excel 2010 i 2013 klaster poveznik dijaloškog okvira. Pomoću ovog dijaloškog okvira odredite klaster koja se izvršava na UDF-ove. Ako dulja je klaster puni naziv (na primjer, hpcexcelhn01.southeastasia.cloudapp.azure.com), ne stane u dijaloškom okviru. Rješenje se za postavljanje varijable razini računala kao što su *CCP_IAASHN* s vrijednošću naziva dugo klaster. Nakon toga unesite *CCP_IAASHN %* u dijaloškom okviru kao glavni čvor naziva klaster. 

Kada klaster uspješno implementiran, nastaviti sa sljedećim koracima za izvođenje ugrađene uzorka UDF programa Excel. Prilagođene Excel UDF-ove, potražite u članku ove [resurse](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) omogućuje stvaranje na XLL i uvesti ih na klasteru IaaS.

1.  Otvorite novu radnu knjigu u programu Excel. Na vrpci **razvoju** kliknite **Dodaci**. Zatim u dijaloškom okviru kliknite **Pregledaj**, dođite do mape u %CCP_HOME%Bin\XLL32 i odaberite uzorak ClusterUDF32.xll. Ako u ClusterUDF32 ne postoji na klijentskom računalu, kopirajte ga iz mape %CCP_HOME%Bin\XLL32 na glavni čvor.

    ![Odaberite na UDF][udf]

2.  Kliknite **datoteka** > **Mogućnosti** > **Napredno**. U odjeljku **formule**, provjerite **Dopusti korisnički definirane XLL funkcija računalnim klasterima**. Kliknite **Mogućnosti** pa unesite naziv cijelog klaster **klaster glavni čvor naziva**. (Kao što je navedena prethodno okvir za unos ograničeno je na 34 znakova tako dugo klaster naziv možda ne stane u njega. Možete koristiti razini računalu varijabli ime dugo klaster.)

    ![Konfiguriranje na UDF][options]

3.  Da biste pokrenuli UDF izračuna na klaster, kliknite ćeliju koja sadrži vrijednost =XllGetComputerNameC() i pritisnite Enter. Funkcija jednostavno dohvaća naziv čvora računalnim na kojem se izvodi na UDF. Za prvog pokretanja u dijaloškom okviru vjerodajnice traži korisničko ime i lozinku za povezivanje s IaaS klaster.

    ![Pokretanje UDF][run]

    Kada postoje broja ćelija da biste izračunali, pritisnite Alt, Shift i Ctrl + F9 radi izračuna na sve ćelije.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Korak 3. Pokretanje SOA radno opterećenje s klijent za lokalni

Da biste pokrenuli Općenito SOA aplikacije na klasteru HPC paket IaaS, prvi put koristite jedan od načina u koraku 1 za implementaciju klaster. Navedite generičkom izračunati čvor slike u tom slučaju jer nije potrebno programa Excel na čvorove računalnim. Zatim slijedite ove korake.

1. Nakon preuzimanja certifikat klaster, uvezite ga na klijentskom računalu u odjeljku certifikata: \CurrentUser\Root.

2. Instalirajte [HPC paket 2012 R2 ažuriranje 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) i [uslužnih HPC Service Pack 2012 R2 ažuriranje 3 klijenta](https://www.microsoft.com/download/details.aspx?id=49923). Ti alati omogućuju razviti i pokretanje SOA klijentske aplikacije.

3. Preuzmite HelloWorldR2 [uzorak koda](https://www.microsoft.com/download/details.aspx?id=41633). Otvorite na HelloWorldR2.sln u Visual Studio 2010 ili 2012.

4. Najprije sastavljanje EchoService projekta. Nakon toga implementirati servis klaster IaaS na isti način implementacije sustava lokalnog klaster. Detaljne korake potražite u članku Readme.doc u HelloWordR2. Izmijenite i izgradnje na HellWorldR2 i druge projekte kao što je opisano u sljedećem odjeljku da biste generirali SOA klijentske aplikacije koji se izvode na programa Azure IaaS klaster.

### <a name="use-http-binding-with-azure-storage-queue"></a>Korištenje Http povezivanja s reda čekanja Azure prostora za pohranu

Da biste koristili Http povezivanja s red čekanja za Azure prostora za pohranu, promijenite nekoliko uzorak koda.

* Ažurirati naziv klaster.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Po želji, koristite zadanu TransportScheme u SessionStartInfo ili izričito je postavljena na HTTP-a.

```
    info.TransportScheme = TransportScheme.Http;
```

* Korištenje zadane povezivanja na BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Ili skupa izričito pomoću u svojstvo basicHttpBinding.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Po želji, postavite zastavice UseAzureQueue na true u SessionStartInfo. Ako ne postavite, ona će biti postavljena na true po zadanom kada naziv klaster ima nastavke Azure domene, a na TransportScheme HTTP-a.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Korištenje Http povezivanje bez čekanja Azure prostora za pohranu

Da biste koristili Http povezivanje bez red čekanja za Azure prostora za pohranu, izričito postavljanje zastavice UseAzureQueue FALSE u na SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Korištenje NetTcp povezivanja

Da biste koristili NetTcp uvez, konfiguraciju je slična povezivanja programa lokalnog klaster. Morate otvoriti nekoliko krajnje točke na glavni čvor VM. Ako ste koristili HPC paket IaaS implementacijsku skriptu da biste stvorili klaster, na primjer, postavite krajnjih točaka na portalu za Azure klasični na sljedeći način.


1. Zaustavite na VM.

2. Dodavanje TCP priključci 9090, 9087, 9091, 9094 sesije, vi posrednik, vi posrednik tempiranja, a zatim podatkovne usluge, odnosno

    ![Konfiguriranje krajnje točke][endpoint]

3. Započnite s VM.

Klijentska aplikacija SOA zahtijeva nikakve promjene osim mijenjanja glavni ime da biste IaaS klaster puno ime i prezime.

## <a name="next-steps"></a>Daljnji koraci

* Potražite [resurse](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) za dodatne informacije o pokretanju programa Excel radnih opterećenja HPC paketom.

* Potražite u članku [Upravljanje servisima SOA u paket Microsoft HPC](https://technet.microsoft.com/library/ff919412.aspx) dodatne informacije o implementaciji i upravljanje uslugama SOA HPC paketom.

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
