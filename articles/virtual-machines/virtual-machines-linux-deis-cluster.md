<properties
   pageTitle="Implementacija čvor 3 Deis klaster | Microsoft Azure"
   description="U ovom se članku opisuje kako stvoriti čvor 3 Deis klaster na Azure pomoću predloška Azure Voditelj resursa"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>Implementacija čvor 3 Deis klaster

U ovom se članku objašnjavaju dodjele resursa u [Deis](http://deis.io/) klaster na Azure. Pokriva sve korake u stvaranju potrebne certifikati implementacija i skaliranje uzorka **otvorite** aplikaciju na novo dodijeljenu klaster.

Sljedeći dijagram prikazuje arhitektura distribuiranih sustava. Administrator sustava upravlja korištenjem klaster Deis alate kao što su **deis** i **deisctl**. Kroz Azure raspoređivača opterećenja, koji prosljeđuje veze na neki od člana čvorove na klaster uspostaviti su veze. Pristup klijenti implementiran aplikacije kroz raspoređivača opterećenja kao i. U ovom slučaju raspoređivača opterećenja prosljeđuje promet usmjerili na Deis mrežasto tkanje usmjerivač, koji se dodatno routs promet na odgovarajuće Docker spremnika hostirane na klaster.

  ![Dijagram arhitektura distribuiranih Desis klaster](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Da biste pokrenuli kroz sljedeće korake, morat ćete:

 * Aktivnu pretplatu Azure. Ako ga nemate, možete dobiti besplatne trag na [azure.com](https://azure.microsoft.com/).
 * Identifikacijska tvrtke ili obrazovne ustanove za korištenje grupa Azure resursa. Ako imate osobnog računa i prijavite pomoću Microsoftova ID-a, potrebnih za [Stvaranje id rad s osobnu posjetnicu](virtual-machines-windows-create-aad-work-id.md).
 * Bilo – ovisno o operacijskom sustavu klijent – [Azure PowerShell](../powershell-install-configure.md) ili [Azure EŽA za Mac, Linux, i Windows](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). OpenSSL služi za generiranje potrebne certifikata.
 * Klijent brojka kao što su [Tulumu brojka](https://git-scm.com/).
 * Da biste testirali primjer aplikacije i morat ćete DNS poslužitelj. Možete koristiti bilo koji DNS poslužitelji ili usluga koji podržavaju zamjenskih odgovora zapise.
 * Računalo da biste pokrenuli Deis klijentskih alata. Možete koristiti na lokalnom računalu ili virtualnog računala. Te alate možete pokrenuti na gotovo bilo kojeg Linux raspodjele, ali sljedeće upute koristiti Ubuntu.

## <a name="provision-the-cluster"></a>Dodjela resursa za klaster

U ovom odjeljku, koristit ćete predložak [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md) iz spremišta Otvori izvor [azure, brzi početak rada i predloške](https://github.com/Azure/azure-quickstart-templates). Najprije ćete kopirajte dolje predložak. Nakon toga stvorit ćete novu SSH ključa par za provjeru autentičnosti. I nakon toga ćete konfigurirati novi identifikator za koje klaster. I na kraju, koristit ćete Jezgrena skripta ili skriptu PowerShell Dodjela klaster.

1. Kloniraj spremište: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Idite u mapu predložaka:

        cd azure-quickstart-templates\deis-cluster-coreos

3. Stvaranje nove SSH ključa par pomoću ssh-keygen:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. Stvaranje certifikata pomoću iznad privatni ključ:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Idite na [https://discovery.etcd.io/new](https://discovery.etcd.io/new) da biste generirali nove token klaster, koja izgleda na primjer:

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Svaki CoreOS klaster mora imati jedinstveni tokena iz ovu besplatnu uslugu. Dodatne informacije potražite u [dokumentaciji CoreOS](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) .

6. Izmjena **oblaka config.yaml** datoteku da biste zamijenili postojeće token **otkrivanje** novi token:

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Mijenjanje **azuredeploy parameters.json** : otvaranje certifikat koji ste stvorili u koraku 4 u uređivaču teksta. Kopiranje svih između `----BEGIN CERTIFICATE-----` i `-----END CERTIFICATE-----` u parametru **sshKeyData** (morat ćete ukloniti sve znakove novog retka).

8. Izmjena parametar **newStorageAccountName** . Ovo je račun za pohranu za VM OS diskova. Mora biti globalno jedinstveni naziv računa.

9. Izmjena parametar **publicDomainName** . To će postati dio naziva DNS pridružene javnu IP raspoređivača opterećenja. Konačni FQDN će imati oblik _[vrijednosti tog parametra]_. _[region]_. cloudapp.azure.com. Ako, na primjer, ako Navedite naziv kao deishbai32 i grupu resursa je implementiran na područje Zapad SAD-a, zatim konačno FQDN za raspoređivača opterećenja bit će deishbai32.westus.cloudapp.azure.com.

10. Spremite datoteku parametar. A zatim Dodjela resursa klaster pomoću komponente PowerShell Azure:

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  ili Azure EŽA:

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Kada je dodijeljen grupi resursa, vidjet ćete sve resurse u grupi Azure klasični portala. Kao što je prikazano u sljedećim snimku zaslona, grupa resursa sadrži virtualne mreže s tri VMs koji su se pridružili isti skup dostupnost. Grupi sadrži i raspoređivača opterećenja koji ima povezani javnu IP.

  ![Grupa dodijeljenu resursa Azure klasični portala](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Instalacija klijentskih programa

Potrebno je **deisctl** kontrolu na Deis klaster. Iako deisctl automatski se instalira u sve čvorove klaster, je dobro koristiti deisctl na zasebnom administratora računalo. Osim toga, jer su sve čvorove konfiguriran za korištenje samo privatne IP adrese, morate koristiti SSH tuneliranja kroz opterećenja koji ima javnu IP, povezivanje s računala čvor. Slijede upute za postavljanje deisctl na zasebnom Ubuntu fizičke ili virtualnog računala.

1. Instalacija deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. Privatni ključ za ssh agent dodali:

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Konfiguriranje deisctl:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

Predložak definira ulazna pravila NAT mapiranje 2223 za instancu 1, 2224 na instancu 2 i 2225 na instancu 3. To nudi zalihosti pomoću alata za deisctl. Možete provjeriti ta pravila Azure klasični portala:

![Pravila NAT na raspoređivača opterećenja](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] Trenutno predložak podržava samo klastere 3 čvor. To je zbog ograničenja u predlošku Voditelj resursa Azure definicija NAT pravila koja ne podržava sintaksu petlje.

## <a name="install-and-start-the-deis-platform"></a>Instalirajte i pokrenite na Deis platforme

Sada možete koristiti deisctl za instalaciju i pokretanje u Deis platformu:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Pokretanje platforme potrebno neko vrijeme (koliko je god 10 minuta). Osobito, pokretanje Sastavljača servisa može potrajati. I katkad je potrebno pokušaja slijedi: Ako operacija "smrzavanje", pokušajte upisati `ctrl+c` da biste prekinuli izvođenje naredbe i pokušajte ponovno.

Možete koristiti `deisctl list` da biste provjerili je li pokrenut sve servise:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Čestitamo! Sada imate tekući Deis clsuter na Azure! Zatim ćemo uvesti uzorka Idi aplikacije da biste vidjeli klaster u akciji.

## <a name="deploy-and-scale-a-hello-world-application"></a>Implementacija i Skaliraj pozdrav svijeta aplikacije

Sljedeći koraci pokazati kako uvesti u "Pozdrav svijeta" otvorite aplikaciju za klaster. Upute temelje se na [Deis dokumentacije](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Za usmjeravanje mesh ispravno funkcionirao, morate imati zamjenskih A zapis za svoju domenu koja pokazuje na javnu IP od raspoređivača opterećenja. Sljedeće snimka zaslona prikazuje A zapis za registraciju za domenu uzorka na servisu GoDaddy:

    ![Godaddy-A zapis](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Instalacija deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Stvaranje novog ključa SSH, a zatim dodajte javni ključ GitHub (Naravno, možete i ponovno koristiti postojećih ključeva). Da biste stvorili novi par ključa SSH, koristite:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Dodajte id_rsa.pub ili javni ključ na temelju odabira GitHub. To možete učiniti pomoću dodavanje SSH ključa gumba u SSH tipke konfiguracija zaslona:

  ![Github ključ](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Registrirali novog korisnika:

        deis register http://deis.[your domain]
<p />
6. Dodavanje ključa SSH:

        deis keys:add [path to your SSH public key]
  <p />      
7. Stvaranje aplikacije komponente.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.Automatske brojka će pokrenuti Docker slike u komponenti i implementirati, što će potrajati nekoliko minuta. Iz moj ugođaj povremeno korak 10 (Pushing slike privatnim spremište) možda "smrzavanje". Kada se to dogodi, zaustavite proces, uklonite pomoću aplikacije ' deis aplikacije: uništiti – u <application name> ` to remove the application and try again. You can use `deis apps:list "da biste saznali naziv aplikacije. Ako sve radi, trebali biste vidjeti otprilike ovako na kraju izlaze naredbe:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Provjerite je li radi li aplikacija:

        curl -S http://[your application name].[your domain]
  Trebali biste vidjeti:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. Promjena veličine aplikacije 3 instance:

        deis scale cmd=3
<p />
11. Po želji možete koristiti deis informacije da biste pregledali detalje o aplikaciji. Sljedeće izlaze su iz moje implementaciju aplikacije:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Daljnji koraci

U ovom se članku walked kroz sve korake Dodjela novi Deis klaster na Azure pomoću predloška Azure Voditelj resursa. Predložak ne podržava zalihosti u tooling veze, kao i za distribuiranih aplikacije za ujednačavanje opterećenja. Predložak izbjegava i korištenje javnu IP-ovi na člana čvorove sprema dragocjene javnu IP resurse i sadrži više zaštićenim okruženje za aplikacije glavnog računala. Dodatne informacije potražite u sljedećim člancima:

[Pregled Azure Voditelj resursa] [resource-group-overview]  
[Kako koristiti EŽA Azure] [azure-command-line-tools]  
[Korištenje Azure PowerShell s Azure Voditelj resursa] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md
