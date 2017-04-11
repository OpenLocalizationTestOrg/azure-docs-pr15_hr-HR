<properties
   pageTitle="Dodavanje runbooks Azure automatizacije planove za oporavak | Microsoft Azure"
   description="U ovom se članku opisuje kako oporavak Azure web-mjesta sada omogućuje vam da biste proširili tarife za oporavak pomoću Azure Automatizacija da biste dovršili složene zadatke tijekom oporavka za Azure"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans---classic"></a>Dodati runbooks Azure automatizacije planove za oporavak – klasični


Pomoću ovog praktičnog vodiča opisuje kako oporavak web-mjesta Azure integrira Azure automatizaciju omogućuju proširenja za tarife za oporavak. Tarife za oporavak možete orkestrirali oporavak virtualnim računalima sustava zaštićene upravljanjem oporavak Azure web-mjesta za replikaciju sekundarnu oblak i replikacije na Azure scenarije. Pomažu i u mijenjanju za oporavak **dosljedno točne**, **kontrolirani**i **automatiziranog**. Ako se ne uspijeva putem virtualnim strojevima za Azure, Integracija s Automatizacija Azure proširuje tarife za oporavak i daje mogućnost dopušta izvršavanje runbooks, stoga dopuštanja Napredna automatizaciju zadataka.

Ako koju ste ne čuli o Azure Automatizacija još, prijavite se [ovdje](https://azure.microsoft.com/services/automation/) i preuzmite njihove ogledne skripte [ovdje](https://azure.microsoft.com/documentation/scripts/). Pročitajte više o [Oporavak Azure web-mjesta](https://azure.microsoft.com/services/site-recovery/) i kako orkestrirali oporavak za Azure pomoću tarife za oporavak [ovdje](https://azure.microsoft.com/blog/?p=166264).

U ovom vodiču kratki će smo pogledajte kako integrirati Automatizacija Azure runbooks u tarife za oporavak. Radimo će automatizirati jednostavne zadatke koje ranije potrebne ručno intervencije i Saznajte kako pretvoriti oporavak za korak više akcija oporavka jednim klikom. Ne možemo će pogledati kako mogu otkloniti jednostavnu skriptu ako odlazi u redu.

## <a name="protect-the-application-to-azure"></a>Zaštita računala za Azure

Javite nam počinju jednostavne aplikacije koji se sastoji od dva virtualnih računala. Ovdje, imamo HRweb aplikacije od Fabrikam. Fabrikam HRweb sučelju i Fabrikam Hrweb pozadinskog su dvije virtualnim strojevima zaštićeni Azure pomoću oporavak Azure web-mjesta. Da biste zaštitili virtualnim strojevima pomoću oporavak Azure web-mjesta, slijedite korake u nastavku.

1.  Omogući zaštitu za virtualnih računala.

2.  Provjerite je li virtualnim strojevima dovršite početne replikacije i su replikaciju.

3.  Pričekajte dok se početni replikacije dovršava i status replikacije piše zaštićenog.

![](media/site-recovery-runbook-automation/01.png)
---------------------

U ovom ćete praktičnom vodiču ćemo će stvoriti oporavak plan za aplikaciju Fabrikam HRweb prebacivanje aplikacije Azure. Zatim ćemo će integrirati s runbook koji će se stvoriti krajnje točke na nije uspio putem Azure virtualnog računala da bi služio web-stranice na priključak 80.

Najprije ćemo stvoriti plan za oporavak za naše aplikaciju.

## <a name="create-the-recovery-plan"></a>Stvaranje plana za oporavak

Da biste oporavili aplikacije Azure, morate stvoriti plan za oporavak.
Koristite oporavak plan možete odrediti redoslijed oporavak virtualnih računala. Virtualnog računala koja se nalazi u grupi 1 će vratiti i počnite prvi, a zatim će slijedite virtualnog računala u grupi 2.

Stvorite na oporavak Plan koji izgleda kao ispod.

![](media/site-recovery-runbook-automation/12.png)

Da biste pročitali više o tarifama za oporavak, pročitajte dokumentaciju [ovdje](https://msdn.microsoft.com/library/azure/dn788799.aspx "ovdje").

Nakon toga stvaranje potrebne artefakte u automatizaciji Azure.

## <a name="create-the-automation-account-and-its-assets"></a>Stvaranje računa za automatizaciju i njegov resursi

Potreban vam je račun za Azure Automatizacija da biste stvorili runbooks. Ako ste već imate postavljen račun, idite na karticu Azure Automatizacija označena tako da ![](media/site-recovery-runbook-automation/02.png)i stvaranje novog računa.

1.  Računu dajte naziv da biste pronašli s.

2.  Navedite zemljopisnom području na mjesto na koje želite da biste postavili račun.

Preporučuje se da biste postavili račun u području isti kao sigurnog ASR.

![](media/site-recovery-runbook-automation/03.png)

Nakon toga stvorite sljedeći resursi na računu.

### <a name="add-a-subscription-name-as-asset"></a>Dodajte naziv pretplate kao resursa

1.  Dodavanje novih postavki ![](media/site-recovery-runbook-automation/04.png) u resursi za automatizaciju Azure i odaberite da biste![](media/site-recovery-runbook-automation/05.png)

2.  Odaberite vrstu varijable kao **niz**

3.  Odredite naziv varijable kao **AzureSubscriptionName**

    ![](media/site-recovery-runbook-automation/06.png)

4.  Navedite naziv svoje stvarni Azure pretplate kao vrijednost varijable.

    ![](media/site-recovery-runbook-automation/07_1.png)

Možete prepoznati naziv pretplate na stranici postavke vašeg računa na portalu Azure.

### <a name="add-an-azure-login-credential-as-asset"></a>Dodavanje vjerodajnicu za Azure prijava kao resursa

Azure Automatizacija koristi Azure PowerShell da biste se povezali s pretplatom i radi li artefakte. To, morate provjeru autentičnosti pomoću Microsoftova računa ili računa tvrtke ili obrazovne ustanove.
Vjerodajnice računa možete spremiti u sredstvo tako da koristi sigurno na runbook.

1.  Dodavanje novih postavki ![](media/site-recovery-runbook-automation/04.png) u resursi za automatizaciju Azure i odaberite![](media/site-recovery-runbook-automation/09.png)

2.  Odaberite vrstu vjerodajnica kao **Windows PowerShell vjerodajnica**

3.  Navedite naziv kao **AzureCredential**

    ![](media/site-recovery-runbook-automation/10.png)

4.  Navedite korisničko ime i lozinku za prijavu.

Sada i ove postavke dostupne su u svoje imovine.

![](media/site-recovery-runbook-automation/11.png)

Dodatne informacije o povezivanju pretplate PowerShell dobiva [ovdje](../powershell-install-configure.md).

Nakon toga ćete stvoriti na runbook u Automatizacija Azure koje možete dodati krajnje točke za sučelja virtualnog računala nakon prebacivanje.

## <a name="azure-automation-context"></a>Kontekst Azure automatizacije

ASR prosljeđuje kontekst varijabla runbook da biste lakše deterministic skripte za pisanje. Nešto nije argue su predvidljivi nazivi servisa u Oblaku i virtualnog računala, ali će se dogoditi da nije uvijek kutije owing to neke scenarije kao što su onu koju promijenio naziv naziv virtualnog računala zbog nepodržane znakova u Azure. Dakle ti podaci se prenosi tarifu za oporavak ASR kao dio *kontekst*.

U nastavku je primjera izgleda varijabla konteksta.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Sljedeća tablica sadrži naziv i opis za svaku varijablu u kontekstu.

**Naziv varijable** | **Opis**
---|---
RecoveryPlanName | Naziv plana pokrenut. Omogućuje da iskoristite akcija na temelju ime koristeći istu skripte
FailoverType | Određuje je li u prebacivanje je testirati, planiranog ili neplanirano.
FailoverDirection | Odredite hoće li se oporavak primarni ili sekundarni
GroupID | Prepoznavanje broj grupe unutar plan za oporavak kada je pokrenut plan
VmMap | Polja sve virtualnim strojevima u grupi
VMMap ključ | Jedinstveni ključ (GUID) za svaki VM. Gdje je to primjenjivo je isti kao VMM ID virtualnog računala.
Naziv uloge | Naziv VM Azure koji je obnavljaju
CloudServiceName | Azure servis u Oblaku naziv pod kojim se stvara virtualnog računala.


Da biste odredili tipku VmMap u kontekstu može se na stranici Svojstva VM u ASR i pogledajte svojstvo VM GUID.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Autor runbook programa automatizacije

Sada stvorite runbook da biste otvorili priključak 80 na pristupnom virtualnog računala.

1.  Stvaranje nove runbook u račun za automatizaciju Azure s nazivom **OpenPort80**

    ![](media/site-recovery-runbook-automation/14.png)

2.  Prijeđite u prikaz autora u kompilacije i unesite način skice.

3.  Najprije odrediti varijabla da biste upotrijebili kontekst plan za oporavak

    ```
        param (
            [Object]$RecoveryPlanContext
        )

    ```

4.  Sljedeće povezivanje s pretplatom s nazivom vjerodajnice i pretplate

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    Imajte na umu da koristite Azure resursi – **AzureCredential** i **AzureSubscriptionName** ovdje.

5.  Sada možete odrediti Detalji o krajnjoj točki i GUID virtualnog računala za koje želite da biste otkrili krajnju točku. U ovom slučaju sučelja virtualnog računala.

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    Određuje protokol Azure krajnje točke, Lokalni priključak na VM i njegova mapiranih javno priključka. Ove varijable su parametri potrebnih Azure naredbe koje dodavanje krajnje točke VMs. Na VMGUID sadrži GUID virtualnog računala morate raditi na.

6.  Skripta sada se izdvajanje kontekstu za navedeni GUID VM i stvaranje krajnje točke na virtualnog računala koje se pozivaju ga.

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. Kada se to završi, kliknite Objavi ![](media/site-recovery-runbook-automation/20.png) da biste omogućili skriptu da bi bio dostupan za izvršavanje.

Dovršavanje skripte navedeni su dolje za referencu

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Dodajte skriptu tarifu za oporavak

Kad je spremna skriptu, možete ga dodati oporavak tarifu koju ste prethodno stvorili.

1.  U oporavak plan koji ste stvorili, odaberite da biste dodali skriptu iza grupi 2. ![](media/site-recovery-runbook-automation/15.png)

2.  Navedite naziv skripte. To je samo neslužbeni naziv Ova skripta za prikazivanje unutar plan za oporavak.

3.  U prebacivanje Azure skriptu – odaberite naziv računa za automatizaciju Azure.

4.  U Azure Runbooks odaberite runbook ste autor.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Primarni strani skripti

Kada se izvršava prebacivanje na Azure, možete izvršiti skripte primarni strani. Te skripte će se pokrenuti na poslužitelju VMM tijekom prebacivanje.
Primarni strani skripte samo dostupne su samo za prije zatvaranja i objaviti faze zatvaranja. To je jer smo očekivali primarni web-mjesto bude obično dostupna kad je Izrada teži.
Tijekom neplanirano prebacivanje, samo ako se odlučite za tu mogućnost za operacije primarni web-mjesta će pokušati pokreću skripte primarni strani. Ako nisu dostupna ili vremensko ograničenje, na prebacivanje će i dalje da biste oporavili virtualnih računala.
Primarni strani skripte nisu dostupni su za VMware/fizičke/Hyper-v web-mjesta bez VMM zaštićene za Azure - tijekom prebacivanje za Azure.
Međutim, kad ste failback iz Azure na lokalni, primarni strani skripte (Runbooks) moguće je koristiti za svih ciljeva osim VMware.

## <a name="test-the-recovery-plan"></a>Testiranje plan za oporavak

Kada dodate na runbook tarifu možete započeti test prebacivanje i pogledajte ga na djelu. Uvijek preporučuje se da biste pokrenuli test prebacivanje da biste testirali aplikacije i oporavak plan da biste bili sigurni da postoje pogreške.

1.  Odaberite plan za oporavak i pokretanje test prebacivanje.

2.  Tijekom izvođenja plan možete vidjeti hoće li se na runbook sadrži izvršava nije putem ili njezin status.

    ![](media/site-recovery-runbook-automation/17.png)

3.  Na stranici Azure automatizaciju zadataka za na runbook možete vidjeti i status izvođenja detaljne runbook.

    ![](media/site-recovery-runbook-automation/18.png)

4.  Kada se dovrši za prebacivanje, osim rezultat izvođenja runbook, vidjet ćete li izvršavanja uspjelo ili nije potražite na stranici Azure virtualnog računala i pogledate krajnjih točaka.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Ogledne skripte

Dok mi smo napustili Automatizacija nešto obično koristi zadatak dodavanje krajnje Azure virtualnog računala pomoću ovog praktičnog vodiča, možete napraviti brojnih drugih naprednih automatizaciju zadataka pomoću Azure automatizaciju. Microsoft i zajednice Azure automatizaciju omogućuju runbooks uzorka, što vam može pomoći početak rada prilikom stvaranja vlastitog rješenja i runbooks utility koje možete koristiti kao sastavni blokovi za veće automatizaciju zadataka. Početak korištenja ih iz galerije i stvaranje naprednih jednim klikom oporavak tarife za oporavak Azure web-mjesta pomoću aplikacije.

## <a name="additional-resources"></a>Dodatni resursi

[Pregled Azure automatizacije] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Pregled Azure automatizacije")

[Ogledne skripte Azure automatizacije] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Ogledne skripte Azure automatizacije")
