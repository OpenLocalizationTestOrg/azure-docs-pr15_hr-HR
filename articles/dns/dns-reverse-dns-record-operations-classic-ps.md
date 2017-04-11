<properties
   pageTitle="Upravljanje obrnutim DNS zapisa za servisa Azure (klasični) pomoću komponente PowerShell | Microsoft Azure"
   description="Kako upravljati obrnutim DNS zapise ili PTR zapisa za Azure servise pomoću komponente PowerShell u modelu klasični implementacije. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Kako upravljati obrnutim DNS zapise za Azure servisa (klasični) pomoću komponente PowerShell Azure

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Saznajte kako [izvesti sljedeće korake pomoću modela Voditelj resursa](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Provjera valjanosti obrnutim DNS zapisa
Da biste osigurali trećih strana nije moguće stvoriti mapiranje s domenama DNS obrnutim DNS zapise, Azure samo omogućuje stvaranje obrnutim DNS zapisa koje je nešto od sljedećeg istinita:

- Obrnuto DNS FQDN je naziv servisa u Oblaku za koju je navedena ili bilo koji servis u Oblaku naziv unutar iste pretplate – primjerice, obrnutim DNS "contosoapp1.cloudapp.net.".
- Obrnutim DNS FQDN naprijed razrješava ime ili IP servisa u Oblaku koja je navedena ili na neki servis u Oblaku naziv i IP unutar iste pretplate npr je obrnutim DNS "app1.contoso.com." što je CName pseudonim za contosoapp1.cloudapp.net.

Provjera valjanosti izvršavaju samo kada je svojstvo obrnutim DNS-a za servise u Oblaku postavljanje ili izmijenili. Povremeno Ponovna provjera valjanosti se izvodi.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Dodavanje obrnutim DNS postojeće servisima u Oblaku
Postojeći oblaka servis pomoću cmdleta "Postavljanje AzureService" možete dodati obrnutim DNS zapis:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Stvaranje servis u Oblaku s obrnutim DNS-a
Možete dodati nove servise u Oblaku s svojstvo DNS obrnutim navedene pomoću cmdleta "Postavljanje AzureService":

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Prikaz obrnutim DNS za postojeće servise u Oblaku
Konfigurirani vrijednost možete pogledati postojeće oblaka servisa pomoću cmdleta "Get-AzureService":

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Uklanjanje obrnutim DNS iz postojeće servise u Oblaku
Obrnuti DNS svojstvo možete ukloniti iz postojeći oblaka servis pomoću cmdleta "Postavljanje AzureService". To možete učiniti tako da postavite obrnutim DNS svojstva vrijednost blank:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
