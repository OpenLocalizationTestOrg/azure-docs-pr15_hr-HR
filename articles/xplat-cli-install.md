<properties
    pageTitle="Instalacija Azure sučelja naredbenog retka | Microsoft Azure"
    description="Instalacija na Azure sučelje naredbenog retka (EŽA) za Mac, Linux i Windows da biste počeli koristiti servise za Azure"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Instalacija Azure EŽA

> [AZURE.SELECTOR]
- [PowerShell](powershell-install-configure.md)
- [Azure EŽA](xplat-cli-install.md)

Brzo instalirajte sučelja Azure naredbenog retka (Azure EŽA) da biste koristili skup Otvori izvor utemeljen na ljuske naredbi za stvaranje i upravljanje resursa u Microsoft Azure. Imate nekoliko mogućnosti da biste instalirali te alate za različite platforme na vašem računalu: 

* **paket npm** – pokretanje npm (paket upravitelja JavaScript) da biste instalirali najnoviju Azure EŽA paket na Linux raspodjele ili OS. Potrebni node.js i npm na vašem računalu.
* **Instalacijski program** – preuzimanje pojavila se jednostavno instalacije na Mac ili Windows installer.
* **Spremnik docker** - početak rada s najnovijim EŽA u spremniku Docker jeste li spremni za pokretanje. Potreban je Docker glavnog računala na vašem računalu.
    
Dodatne mogućnosti i pozadine, potražite u članku spremište projekta na [GitHub](https://github.com/azure/azure-xplat-cli). 

Kada EŽA Azure instaliran, [povežite ga s pretplatom Azure](xplat-cli-connect.md) i pokretanje naredbe **azure** s sučelje naredbenog retka (tulumu, Terminal, naredbeni redak i tako dalje) da biste radili s Azure resurse.



## <a name="option-1-install-an-npm-package"></a>Mogućnost 1: Instalacija paketa npm

Da biste instalirali na EŽA iz paketa npm, provjerite je li ste preuzeli i instalirali [najnovija Node.js i npm](https://nodejs.org/en/download/package-manager/). Izvedite **npm instalacija** da biste instalirali paket azure eža: 

    npm install -g azure-cli

Na Linux distribucija, bilo bi dobro da koristite **sudo** da biste uspješno pokrenuti naredbu __npm__ na sljedeći način:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Ako je potrebno instalirati ili ažuriranje Node.js i npm na Linux raspodjele ili OS, preporučujemo da instalirate najnoviju verziju Node.js LTS (4.x). Ako koristite stariju verziju, mogla bi vam se pogreške prilikom instalacije. 

Ako biste radije, preuzmite najnoviju Linux [tar datoteke] [ linux-installer] paketa npm lokalno. Zatim instalirajte paket preuzete npm na sljedeći način (na Linux distribucija možda ćete morati koristiti **sudo**):

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Mogućnost 2: Koristi instalacijski program

Ako koristite računalo Mac i Windows, za preuzimanje dostupne su sljedeće instalaciju EŽA:

* [Instalacijski program za Mac OS X][mac-installer]

* [Windows MSI][windows-installer] 

>[AZURE.TIP]U sustavu Windows, možete preuzeti i [Installer platformu Web](https://go.microsoft.com/?linkid=9828653) da biste instalirali na EŽA. Ovaj instalacijski program omogućuje vam da biste instalirali dodatne SDK Azure i alati naredbenog retka nakon instalacije na EŽA. 


## <a name="option-3-use-a-docker-container"></a>Mogućnost 3: Korištenje spremniku Docker

Ako ste postavili računalo kao [Docker](https://docs.docker.com/engine/understanding-docker/) glavno računalo, možete pokrenuti najnovije Azure EŽA u spremniku Docker. (Na Linux distribucija možda ćete morati koristiti **sudo**), pokrenite sljedeću naredbu:

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Pokretanje naredbe EŽA Azure
Nakon instalacije EŽA Azure pokrenite naredbu **azure** putem naredbenog retka korisničkog sučelja (tulumu, Terminal, naredbeni redak i tako dalje). Na primjer, da biste pokrenuli naredbi pomoć, upišite sljedeće:

```
azure help
```
> [AZURE.NOTE]Na nekim distribucija Linux možda o pogrešci slična `/usr/bin/env: ‘node’: No such file or directory`. Ta se pogreška dolazi iz nedavne instalacija Node.js instaliraju pri /usr/bin/nodejs. Da biste riješili problem, stvorite simboličke veze da biste /usr/bin/node tako da pokrenete sljedeću naredbu:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Da biste vidjeli verziju Azure EŽA ste instalirali, upišite sljedeće:

```
azure --version
```

Sada ste spremni! Da biste pristupili sve naredbe EŽA za rad s resursima vlastite [povezati pretplatu za Azure EŽA Azure](xplat-cli-connect.md).

>[AZURE.NOTE] Prilikom prvog korištenja Azure EŽA, vidjet ćete poruku koja vas pita želite li Dopusti Microsoftu prikupljanje informacija o korištenju. Sudjelovanje je dobrovoljno. Ako odaberete da biste sudjelovali, možete isključiti u bilo kojem trenutku tako da pokrenete `azure telemetry --disable`. Da biste omogućili sudjelovanje u bilo kojem trenutku, pokrenite `azure telemetry --enable`.


## <a name="update-the-cli"></a>Ažuriranje na EŽA

Microsoft često izdaje ažurirane verzije EŽA Azure. Ponovno instalirajte EŽA pomoću instalacijski program za vaš operacijski sustav, ili pokrenite najnovije spremnik Docker. Ili, ako imate najnoviju Node.js i npm instaliran, ažurirajte upisivanjem sljedeće (na Linux distribucija možda ćete morati koristiti **sudo**).

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Omogućivanje dovršetka kartica

Kartica obavljanjem EŽA naredbe nije podržana za Mac i Linux.

Da biste omogućili u zsh, pokrenite:

```
echo '. <(azure --completion)' >> .zshrc
```

Da biste omogućili u tulumu, pokrenite:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Daljnji koraci 

* [Povezivanje s EŽA u pretplatu za Azure](xplat-cli-connect.md) za stvaranje i upravljanje Azure resursi.

* Dodatne informacije o EŽA Azure, preuzmite izvornog koda, prijavljivanja problema ili sudjelovanje u projekt, posjetite [GitHub spremište EŽA Azure](https://github.com/azure/azure-xplat-cli).

* Ako imate pitanja o korištenju Azure EŽA ili Azure, posjetite [Forume za Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Ako želite, možete pokušati i sustavom Python [Azure EŽA 2.0 pretpregled](https://github.com/azure/azure-cli).

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
