<properties
    pageTitle="Postavljanje sigurnog ključ za virtualnim strojevima u Azure Voditelj resursa | Microsoft Azure"
    description="Upute za postavljanje sigurnog ključ za korištenje s programa upravitelj resursa Azure virtualnog računala."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Postavljanje sigurnog ključ za virtualnim strojevima u Azure Voditelj resursa

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasični implementacije

U stogu Voditelj resursa Azure tajne/certifikati su katalog modeliran kao resursa koje ste dobili od davatelja resursa od sigurnog ključ. Da biste saznali više o sigurnog ključ Azure, potražite u članku [što je sigurnog ključ Azure?](../key-vault/key-vault-whatis.md)

Bi sigurnog ključ za korištenje s Voditelj resursa Azure virtualnim strojevima, svojstvo *EnabledForDeployment* na sigurnog ključ mora biti postavljeno na true. To možete učiniti u različitim klijentima."

## <a name="use-cli-to-set-up-key-vault"></a>Koristite EŽA da biste postavili sigurnog ključ
Da biste stvorili ključa sigurnog pomoću sučelja naredbenog retka (EŽA), potražite u članku [Upravljanje ključ sigurnog korištenja EŽA](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Za EŽA, morate stvoriti ključa sigurnog prije no što dodijeliti Pravilnik za implementaciju. To možete učiniti pomoću sljedeće naredbe:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Koristite predloške da biste postavili sigurnog ključ
Kada koristite predložak, prvo morate postaviti na `enabledForDeployment` svojstvo `true` ključ sigurnog resursa.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Druge mogućnosti koje možete konfigurirati prilikom stvaranja ključa sigurnog pomoću predložaka potražite u članku [Stvaranje ključa zbirke ključeva](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
