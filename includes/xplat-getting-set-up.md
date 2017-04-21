<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Korištenje Azure EŽA

Sljedeći koraci olakšavaju pomoću Azure EŽA jednostavno najnoviju verziju i odgovarajuće pretplate. Ako je potrebno instalirati Azure EŽA i se najprije povezati s računom potražite u članku [sučelje za Azure naredbenog retka (Azure EŽA)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Korak 1: Ažuriranje Azure EŽA verzija

Da biste koristili Azure EŽA izuzetne naredbi s načinom rada za upravljanje servisa, imat ćete novu verziju ako je to moguće. Da biste provjerili je li vaša verzija, upišite `azure --version`. Trebali biste vidjeti otprilike ovako:

    $ azure --version
    0.8.17 (node: 0.10.25)

Ako želite da biste ažurirali verziju preglednika Azure EŽA, potražite u članku [EŽA Azure](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Korak 2: Postavite račun za Azure i pretplate

Kada vaš Azure EŽA ste povezali s računom koji želite koristiti, možda će vam se više pretplata. Ako to učinite, trebali biste provjeriti pretplate dostupna za vaš račun tako da upišete `azure account list`, a zatim odaberite pretplatu koju želite koristiti tako da upišete `azure account set <subscription id or name> true` gdje _id pretplate ili naziv_ je id pretplate ili naziv pretplate koju želite raditi u trenutnoj sesiji. Trebali biste vidjeti otprilike ovako:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Ako još nemate račun za Azure, ali niste pretplaćeni na MSDN pretplatu, možete dobiti besplatne Azure kredita aktiviranjem na [MSDN pretplatnika pogodnosti ovdje](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) –, ili pomoću besplatnog računa. Ili funkcionirat će za Azure pristup.
