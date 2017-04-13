<properties
   pageTitle="Korištenje slika klijenta sustava Windows razvojni i testiranje scenarijima za | Microsoft Azure"
   description="Upute za korištenje prednosti pretplate za Visual Studio implementaciji sustava Windows 7 i 8/10 Azure razvojni i testiranje scenarija"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Klijenti za Windows u Azure scenarije razvojni i testiranje

Koristite Windows 7, Windows 8 ili Windows 10 u Azure razvojni i testiranje scenarijima za navedene imate pretplatu na odgovarajuće Visual Studio (bivši MSDN). U ovom se članku navode uvjete potrebno ispuniti za izvodi klijenta sustava Windows Azure i korištenje slika galerije Azure.


## <a name="subscription-eligibility"></a>Ispunjavanja uvjeta za pretplatu
Aktivni Visual Studio pretplatnike (osobe koje ste nabavili licencu za pretplatu Visual Studio) možete koristiti Windows klijent za razvoj i testiranja. Windows klijent se može koristiti u vlastitim hardver i Azure virtualnim strojevima koji se izvodi u bilo kojoj vrsti Azure pretplate. Windows klijent možda ne biti implementiran na koriste na Azure za korištenje normalni radnog ili koristi osobe koje nisu aktivni pretplatnike Visual Studio.

Praktičan, ne možemo su dostupne određene slike za Windows 10 iz galerije Azure unutar [nudi uvjete razvojni i testiranje](#eligible-offers). Visual Studio pretplatnike unutar bilo koju vrstu ponude možete vidjeti i [odgovarajući način Priprema i stvaranje](virtual-machines-windows-prepare-for-upload-vhd-image.md) 64-bitnom sustavu Windows 7, Windows 8 ili Windows 10 sliku i zatim [Prijenos Azure](virtual-machines-windows-upload-image.md). Korištenje ostat će ograničeni na razvojni i testiranje tako aktivni pretplatnike Visual Studio.


## <a name="eligible-offers"></a>Ponuda za uvjete
U sljedećoj su tablici detaljno ponudu ID-a koji ispunjavaju uvjete za implementaciju sustava Windows 10 putem galerije Azure. Vidljiva ponuda za sljedeće su samo slike za Windows 10. Visual Studio pretplatnike na koji je potrebno za pokretanje sustava Windows klijent ponude za različite vrste potrebno [odgovarajući način Priprema](virtual-machines-windows-prepare-for-upload-vhd-image.md) i stvoriti na 64-bitni sustav Windows 7, Windows 8 ili Windows 10 sliku i [, a zatim prijenos Azure](virtual-machines-windows-upload-image.md).

| Naziv ponude | Broj ponude | Slike dostupne klijenta |
|:-----------|:------------:|:-----------------------:|
| [Razvojni Test pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10 |
| [Pretplatnike Visual Studio Enterprise (mreže Microsoftovih Partnera)](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10 |
| [Visual Studio Professional pretplatnike](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10 |
| [Visual Studio Test Professional pretplatnike](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10 |
| [Visual Studio Premium s MSDN (pogodnost)](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10 |
| [Visual Studio Enterprise pretplatnike](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10 |
| [Pretplatnike Visual Studio Enterprise (BizSpark)](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10 |
| [Razvojni Test Enterprise](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10 |


## <a name="check-your-azure-subscription"></a>Potvrdite svoju pretplatu za Azure
Ako ne znate svoj ID ponuda, možete je dobiti putem portala za Azure ili portala za račun.

Ponudu ID pretplate istaknuta je na "Pretplate" plohu unutar Azure portala:

![Detalji o ID ponudu na portalu za Azure](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

Možete pogledati i ID ponudu ["Pretplate" kartica](http://account.windowsazure.com/Subscriptions) portal za Azure računa:

![Detalji o ID ponudu na portalu račun za Azure](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Daljnji koraci
Sada možete implementirati uz vaše VMs pomoću programa [PowerShell](virtual-machines-windows-ps-create.md), [Predlošci Voditelj resursa](virtual-machines-windows-ps-template.md)ili [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
