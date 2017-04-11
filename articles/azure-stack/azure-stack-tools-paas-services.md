<properties
    pageTitle="Alati i PaaS usluge za Azure stogu | Microsoft Azure"
    description="Saznajte kako započeti rad sa servisima PaaS u stogu Azure."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Alati za Azure stogu

Možete preuzeti alate opisane ispod. Ako želite biti obaviješteni novih servisa i alata, pratite #AzureStack na Twitteru.

## <a name="template-tools"></a>Alati za predložak

### <a name="azure-stack-github-templates"></a>Azure Github snop predložaka
Istražite sve veći skup [Azure stogu GitHub predložaka](https://github.com/Azure/AzureStack-QuickStart-Templates). Baš kao i [Azure](https://github.com/Azure/azure-quickstart-templates)te predloške "Brzo pokretanje" Voditelj resursa Azure usmjerite početak rada u jednostavne sastavnih blokova i primjeri, jeste li spremni za implementaciju u Microsoft Azure stogu Tehnički pretpregled dokaz od pojam okruženju. Uključeni su prvi Primjeri radno opterećenje proizvođača za [ad-koje nisu-slika](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [sql-2014. – koji nisu-slika](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [sharepoint-2013 – koji nisu-kontrole](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), kao što je i sljedeći nekoliko jednostavnih 101 predložaka [101 – jednostavno – windows – vm](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm)kao što su.


### <a name="marketplace-item-packaging-tool-and-sample"></a>Tržište stavke pakiranje alata i primjeri
[Preuzimanje i korištenje alata za pakiranja](http://www.aka.ms/azurestackmarketplaceitem) za stvaranje stavke iz trgovine za vlastite prilagođene predloške da biste dodali u stogu Azure marketplace. Upute za stvaranje stavke marketplace, a zatim ga učiniti dostupnim vaše klijenata pronaći ćete u [stavci za stvaranje trgovine](azure-stack-create-and-publish-marketplace-item.md).

## <a name="developer-tools"></a>Alati za razvojne inženjere


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Koristite Visual Studio i Azure stogu TP2 na MAS CON01 virtualnog računala
Ako želite koristiti Visual Studio na konzoli VM rad s predlošcima stogu Azure, morate instalirati verzije točan potrebni alati. Da biste instalirali podržane verzije za TP2 pomoću sljedećeg postupka.

1. Da biste se prijavili MAS CON01 virtualnog računala s vjerodajnicama azurestack\azurestackadmin pomoću udaljene radne površine.
2. Instalirajte i otvorili instalacijski program platformu za Web.
3. Pronalaženje i instalirajte **Visual Studio zajednice 2015 s Microsoft Azure SDK - 2.9.5**.
4. Koristite **Dodaj/ukloni programe**, deinstalirajte **Microsoft Azure PowerShell (rujan)** koji se instalira u sklopu SDK.
5. Otvorite Web platformu instalacijski program.
6. Pronalaženje i instalirajte **Microsoft Azure PowerShell – Azure stogu Tehnički pretpregled 2**. 
7. Ponovno pokrenite MAS CON01 virtualnog računala.
8. Da biste se prijavili MAS CON01 virtualnog računala s vjerodajnicama azurestack\azurestackadmin pomoću udaljene radne površine.
9. Otvorite Visual Studio, a zatim Provjera valjanosti da možete povezati okruženje za Azure snop, nabavili predloške i tako dalje. 

### <a name="azure-powershell-sdk"></a>Azure PowerShell SDK
Azure PowerShell je modul koji sadrži cmdleti za upravljanje Azure i stoga Azure s komponentom Windows PowerShell. Možete koristiti u Cmdlete da biste stvorili, testirajte, uvođenje i upravljanje rješenja i usluga koji se isporučuje putem platforme Azure stogu.
[Preuzimanje SDK PowerShell Azure](http://aka.ms/azStackPsh) i [instalirajte PowerShell](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure unakrsni platformu naredbeni redak sučelja
Brzo instalirajte sučelja Azure naredbenog retka (Azure EŽA) da biste koristili skup Otvori izvor utemeljen na ljuske naredbi za stvaranje i upravljanje resursima u stogu Microsoft Azure.

[Preuzmite Windows EŽA](http://aka.ms/azstack-windows-cli)

[Preuzimanje Mac EŽA](http://aka.ms/azstack-linux-cli)

[Preuzmite EŽA Linux](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Ako ste na računalu Mac ili Linux, možete pronaći i u EŽA pomoću naredbe`npm install -g azure-cli@0.9.11`</br>
> + Ako nailazite na probleme provjeru valjanosti certifikata, pokrenite naredbu`set NODE_TLS_REJECT_UNAUTHORIZED=0`
