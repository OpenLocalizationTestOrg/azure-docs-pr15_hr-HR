<properties 
    pageTitle="Upravljanje računima servisa Azure Media sa servisom PowerShell" 
    description="Informirajte se o upravljanju računa servisa Azure Media Services pomoću cmdleta ljuske PowerShell." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Upravljanje računima servisa Azure Media sa servisom PowerShell

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [OSTALE](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Da biste mogli stvoriti račun servisa Azure Media Services, morate imati račun za Azure. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure besplatnu probnu verziju</a>.

##<a name="overview"></a>Pregled 

U članku se navode Azure PowerShell cmdleti za Azure Media Services (AMS) u framework Azure Voditelj resursa. S cmdletima postoji u **Microsoft.Azure.Commands.Media** naziva.

## <a name="versions"></a>Verzija

**ApiVersion**: "2015-10-01"
               

## <a name="new-azurermmediaservice"></a>Novi AzureRmMediaService

Stvara servis za medijske sadržaje.

### <a name="syntax"></a>Sintaksa

Postavljanje parametara: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Postavljanje parametara: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parametri

**-ResourceGroupName &lt;niza&gt;**

Određuje naziv resursa grupe kojoj pripada taj servis za medijske sadržaje.

Pseudonima | Ništa
---|---
Obavezan?   |  TRUE
Položaj?   |  0
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova?  |FALSE

**-AccountName &lt;niza&gt;**

Određuje naziv servisa za medijske sadržaje.

Pseudonima |Ime
---|---
Obavezan? |TRUE
Položaj? |1
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |FALSE
Prihvaćanje zamjenskih znakova? |FALSE

**-Mjesto &lt;niza&gt;**

Određuje mjesto resursa pružanja usluge za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj? |2
Zadana vrijednost  |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**-StorageAccountId &lt;niza&gt;**

Navodi račun primarni prostora za pohranu koji je povezan sa servisom za medijske sadržaje.

- Novi prostor za pohranu račun (stvorena pomoću upravitelja resursa API-JA) podržano samo.

- Račun za pohranu mora postojati i ima na istom mjestu sa servisom za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj? |3
Zadana vrijednost  |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Parametar postavljen naziv |StorageAccountIdParamSet
Prihvaćanje zamjenskih znakova?|FALSE

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Određuje račune za pohranu koji je povezan sa servisom za medijske sadržaje.

- Novi prostor za pohranu račun (stvorena pomoću upravitelja resursa API-JA) podržano samo.

- Račun za pohranu mora postojati i ima na istom mjestu sa servisom za medijske sadržaje.

- Samo jedan račun za pohranu možete navesti primarnim.

Pseudonima |Ništa
---|---
Obavezan?  |TRUE
Položaj?  |3
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Parametar postavljen naziv |StorageAccountsParamSet
Prihvaćanje zamjenskih znakova? |FALSE

**-Oznake &lt;Hashtable&gt;**

Navodi tablice raspršivanje oznake koje su vezane uz servis za medijske sadržaje.

- Primjer:@{"tag1"="value1";"tag2"=:value2"}

Pseudonima |Ništa
---|---
Obavezan?  |FALSE
Položaj?  |pod nazivom
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |FALSE
Prihvaćanje zamjenskih znakova? |FALSE

**&lt;CommandParameters&gt;**

Ovaj cmdlet podržava uobičajene parametre:-ispravljanje pogrešaka, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - opširno, - WarningAction, i - WarningVariable.

### <a name="inputs"></a>Unosa

Unos vrsta je vrste objekata koje možete pipe cmdletu.

### <a name="outputs"></a>Izlaze

Vrsta izlaza je vrsta objekata koji cmdlet emits.

## <a name="set-azurermmediaservice"></a>Postavljanje AzureRmMediaService

Ažurira servis za medijske sadržaje.

### <a name="syntax"></a>Sintaksa

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parametri

**-ResourceGroupName &lt;niza&gt;**

Određuje naziv resursa grupe kojoj pripada taj servis za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan?  |TRUE
Položaj?  |0
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**-AccountName &lt;niza&gt;**

Određuje naziv servisa za medijske sadržaje.

Pseudonima |Ime
---|---
Obavezan? |TRUE
Položaj? |1
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Određuje račune za pohranu koji je povezan sa servisom za medijske sadržaje.

- Novi prostor za pohranu račun (stvorena pomoću upravitelja resursa API-JA) podržano samo.

- Račun za pohranu mora postojati i ima na istom mjestu sa servisom za medijske sadržaje.

- Samo jedan račun za pohranu možete navesti primarnim.

Pseudonima |Ništa
---|---
Obavezan? |FALSE
Položaj? |Pod nazivom
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Parametar postavljen naziv |StorageAccountsParamSet
Prihvaćanje zamjenskih znakova? |FALSE

**-Oznake &lt;Hashtable&gt;**

Navodi tablice raspršivanje oznake koje su vezane uz taj servis za medijske sadržaje.

- Oznake koje su vezane uz servis media zamjenjuje vrijednost navedena koje je korisnik odabrao.

Pseudonima |Ništa
---|---
Obavezan? |FALSE
Položaj?  |Pod nazivom
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**&lt;CommandParameters&gt;**

Ovaj cmdlet podržava uobičajene parametre:-ispravljanje pogrešaka, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - opširno, - WarningAction, i - WarningVariable.

### <a name="inputs"></a>Unosa

Unos vrsta je vrste objekata koje možete pipe cmdletu.

### <a name="outputs"></a>Izlaze

Vrsta izlaza je vrsta objekata koji cmdlet emits.

## <a name="remove-azurermmediaservice"></a>Uklanjanje AzureRmMediaService

Uklanja servis za medijske sadržaje.

### <a name="syntax"></a>Sintaksa

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametri

**-ResourceGroupName &lt;niza&gt;**

Određuje naziv resursa grupe kojoj pripada taj servis za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj? |0
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**-AccountName &lt;niza&gt;**

Određuje naziv servisa za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj? |2
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal?  |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**&lt;CommandParameters&gt;**

Ovaj cmdlet podržava uobičajene parametre:-ispravljanje pogrešaka, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - opširno, - WarningAction, i - WarningVariable.

### <a name="inputs"></a>Unosa

Unos vrsta je vrste objekata koje možete pipe cmdletu.

### <a name="outputs"></a>Izlaze

Vrsta izlaza je vrsta objekata koji cmdlet emits.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService

Dohvaća sve media services u grupu resursa ili servis za medijske sadržaje s navedenim nazivom.

### <a name="syntax"></a>Sintaksa

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametri

**-ResourceGroupName &lt;niza&gt;**

Određuje naziv resursa grupe kojoj pripada taj servis za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj?  |0
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Parametar postavljen naziv |ResourceGroupParameterSet, AccountNameParameterSet
Prihvaćanje zamjenskih znakova?   FALSE

**-AccountName &lt;niza&gt;**

Određuje naziv servisa za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj?  |1
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Parametar postavljen naziv  |AccountNameParameterSet
Prihvaćanje zamjenskih znakova? |FALSE

**&lt;CommandParameters&gt;**

Ovaj cmdlet podržava uobičajene parametre:-ispravljanje pogrešaka, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - opširno, - WarningAction, i - WarningVariable.

### <a name="inputs"></a>Unosa

Unos vrsta je vrste objekata koje možete pipe cmdletu.

### <a name="outputs"></a>Izlaze

Vrsta izlaza je vrsta objekata koji cmdlet emits.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys

Dohvaća tipke pružanja usluge za medijske sadržaje.

### <a name="syntax"></a>Sintaksa

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametri

**-ResourceGroupName &lt;niza&gt;**

Određuje naziv resursa grupe kojoj pripada taj servis za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj?  |0
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**-AccountName &lt;niza&gt;**

Određuje naziv servisa za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj? |1
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**&lt;CommandParameters&gt;**

Ovaj cmdlet podržava uobičajene parametre:-ispravljanje pogrešaka, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - opširno, - WarningAction, i - WarningVariable.

### <a name="inputs"></a>Unosa

Unos vrsta je vrste objekata koje možete pipe cmdletu.

### <a name="outputs"></a>Izlaze

Vrsta izlaza je vrsta objekata koji cmdlet emits.

## <a name="set-azurermmediaservicekey"></a>Postavljanje AzureRmMediaServiceKey

Obnavlja primarni ili sekundarni ključ pružanja usluge za medijske sadržaje.

### <a name="syntax"></a>Sintaksa

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parametri

**-ResourceGroupName &lt;niza&gt;**

Određuje naziv resursa grupe kojoj pripada taj servis za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan?  |TRUE
Položaj?  |0
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal?  |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**-AccountName &lt;niza&gt;**

Određuje naziv servisa za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj?  |1
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal?   |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**-KeyType &lt;KeyType&gt;**

Određuje vrsta ključa pružanja usluge za medijske sadržaje.

- Primarno ili sekundarni

Pseudonima |Ništa
---|---
Obavezan?  |TRUE
Položaj?  |2
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |FALSE
Prihvaćanje zamjenskih znakova? |FALSE

**&lt;CommandParameters&gt;**

Ovaj cmdlet podržava uobičajene parametre:-ispravljanje pogrešaka, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - opširno, - WarningAction, i - WarningVariable.

### <a name="inputs"></a>Unosa

Unos vrsta je vrste objekata koje možete pipe cmdletu.

### <a name="outputs"></a>Izlaze

Vrsta izlaza je vrsta objekata koji cmdlet emits.

## <a name="sync-azurermmediaservicestoragekeys"></a>Sinkroniziranje AzureRmMediaServiceStorageKeys

Sinkronizira tipke račun za pohranu za pohranu račun povezan sa servisom za medijske sadržaje.

### <a name="syntax"></a>Sintaksa

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametri

**-ResourceGroupName &lt;niza&gt;**

Određuje naziv resursa grupe kojoj pripada taj servis za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj? |0
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**-AccountName &lt;niza&gt;**

Određuje naziv servisa za medijske sadržaje.

Pseudonima |Ništa
---|---
Obavezan? |TRUE
Položaj? |1
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**-StorageAccountId &lt;niza&gt;**

Navodi prostora za pohranu račun povezan sa servisom za medijske sadržaje.

Pseudonima |ID-a
---|---
Obavezan? |TRUE
Položaj?  |2
Zadana vrijednost |Ništa
Prihvaćanje unosa kanal? |      TRUE(ByPropertyName)
Prihvaćanje zamjenskih znakova? |FALSE

**&lt;CommandParameters&gt;**

Ovaj cmdlet podržava uobičajene parametre:-ispravljanje pogrešaka, - ErrorAction, - ErrorVariable, - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - opširno, - WarningAction, i - WarningVariable.

### <a name="inputs"></a>Unosa

Unos vrsta je vrste objekata koje možete pipe cmdletu.

### <a name="outputs"></a>Izlaze

Vrsta izlaza je vrsta objekata koji cmdlet emits.

## <a name="next-step"></a>Sljedeći korak 

Pogledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
