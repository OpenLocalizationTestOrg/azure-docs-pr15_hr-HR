<properties
   pageTitle="Postavljanje PowerShell da biste stvorili na VM za Marketplace | Microsoft Azure"
   description="Upute za postavljanje Azure PowerShell i korištenje kao neobavezno proces tijeka da biste stvorili VM slike želite uvesti, a prodaja na trgovine Windows Azure"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Postavljanje Azure PowerShell za stvaranje ponude za Azure Marketplace
Detaljne informacije o postavljanju PowerShell u Azure potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). Jednostavan pristup je koristiti metodu certifikat koji preuzimanja i uvozi certifikat koji je potreban za provjeru autentičnosti. Da biste dobili potrebne certifikata, koristite cmdlet **Get-AzurePublishSettingsFile** . Kada se od vas zatraži, spremite datoteku. Da biste uvezli certifikat u sesiju ljuske PowerShell, koristite cmdlet **Uvoz AzurePublishSettingsFile** .

Da biste konfigurirali i uobičajene postavke pretplate Microsoft Azure za pohranu za sesiju ljuske PowerShell, pomoću cmdleta **Skup AzureSubscription** i **Odaberite AzureSubscription** :

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Prva naredba zadani račun za pohranu pridružuje pretplate (nužan za neki VM postupci za dodjelu resursa).  Drugi čini pretplata trenutni jedan (prepoznaje druge Cmdlete).

## <a name="see-also"></a>Vidi također
- [Početak rada: kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md)
- [Stvaranje virtualnog računala slika za Marketplace](marketplace-publishing-vm-image-creation.md)
