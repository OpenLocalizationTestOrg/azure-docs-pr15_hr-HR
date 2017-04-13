<properties
   pageTitle="Kako upravljati pristup kontrola popisa (ACL) za krajnje točke pomoću komponente PowerShell"
   description="Informirajte se o upravljanju ACL-a pomoću komponente PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Kako upravljati pristup kontrola popisa (ACL) za krajnje točke pomoću komponente PowerShell

Možete stvoriti i upravljati mrežni pristup kontrola popisa (ACL) za krajnje točke retka pomoću Azure PowerShell ili na portalu za upravljanje. U ovoj se temi nalaze postupke za ACL uobičajene zadatke koje možete izvršiti pomoću komponente PowerShell. Na popisu Azure PowerShell cmdleta potražite u članku [Cmdleti za upravljanje Azure](http://go.microsoft.com/fwlink/?LinkId=317721). Dodatne informacije o ACL-a potražite u članku [što je na mreži pristup kontrola popisa (ACL)?](virtual-networks-acl.md). Ako želite upravljati vaše ACL-a pomoću portala za upravljanje potražite u članku [upute za postavljanje točke za virtualnog računala](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Upravljanje mreže ACL-a pomoću ljuske PowerShell za Azure

Cmdleti za Azure PowerShell možete koristiti za stvaranje, uklanjanje i konfiguriranje (skup) mrežni pristup kontrola popisa (ACL). Ne možemo stvaraju nekoliko primjera neki od načina možete konfigurirati ACL pomoću komponente PowerShell.

Da biste dohvatili popis svih cmdleta ljuske PowerShell ACL, upotrijebite nešto od sljedećeg:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Stvaranje mreže ACL pomoću pravila koja dopustiti pristup s udaljenog podmreže

Ilustrira donji primjer način da biste stvorili novi ACL koji sadrži pravila. U ovom ACL zatim primjenjuje se na krajnjoj točki virtualnog računala. Pravila ACL u primjeru u nastavku omogućuje pristup s udaljenog podmreže. Da biste stvorili novi mrežni ACL dopustite pravila za udaljene podmreže, otvorite je Azure Očisti filtar. Kopiranje i lijepljenje skripte ispod, konfiguriranje skriptu pomoću vlastitih vrijednosti i pokrenuti skriptu.

1. Stvorite novi objekt ACL mreže.

        $acl1 = New-AzureAclConfig

1. Postavljanje pravila koja omogućuje pristup s udaljenog podmreže. U primjeru u nastavku postaviti pravila *100* (koji imaju prednost u odnosu pravilo 200 i noviji) dopustili pristup *10.0.0.0/8* udaljene podmreže krajnjoj virtualnog računala. Zamjena vrijednosti konfiguracije potrebama. Naziv "SharePoint ACL config" treba zamijeniti s neslužbeni naziv koji želite pozvati ovo pravilo.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Za dodatne pravila, ponovite cmdlet zamjenjuju vrijednosti konfiguracije potrebama. Ne zaboravite da biste promijenili pravilo broj narudžbi u skladu s vizualnim željeni redoslijed pravila koja želite primijeniti. Manji broj pravilo ima prednost pred veći broj.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Nakon toga možete stvorili krajnju točku (Dodaj) ili postavljanje ACL-a za postojeće krajnju točku (skup). U ovom se primjeru smo će virtualnog računala krajnju točku pod nazivom "web" Dodavanje i ažuriranje krajnju točku virtualnog računala s postavkama ACL.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Nakon toga kombiniranje s cmdletima i pokrenuti skriptu. U ovom primjeru kombinirane cmdleta izgleda ovako:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Uklanjanje mreže ACL pravila koja omogućuje pristup s udaljenog podmreže

Ilustrira donji primjer način da biste uklonili pravilo ACL mreže.  Da biste uklonili pravilo ACL mreže s dopustite pravila za udaljene podmreže, otvorite je Azure Očisti filtar. Kopiranje i lijepljenje skripte ispod, konfiguriranje skriptu pomoću vlastitih vrijednosti i pokrenuti skriptu.

1. Prvi je korak da biste dobili objekt ACL mreže za krajnju točku virtualnog računala. Najprije ćete ukloniti ACL pravilo. U ovom slučaju ne možemo uklanjate ga tako da pravilo ID-a. To će samo uklanjanje ID pravila 0 ACL-a. Uklanjanje ACL objekta iz krajnju točku virtualnog računala.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Nakon toga morate primijeniti objekt mreže ACL krajnjoj virtualnog računala i ažuriranje virtualnog računala.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Uklanjanje mreže ACL krajnju točku virtualnog računala

U određenim slučajevima, možda ćete morati mreže ACL objekt uklonili krajnju točku virtualnog računala. Da biste to učinili, otvorite programa Azure Očisti filtar. Kopiranje i lijepljenje skripte ispod, konfiguriranje skriptu pomoću vlastitih vrijednosti i pokrenuti skriptu.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Daljnji koraci

[Što je na mreži pristup kontrola popisa (ACL)?](virtual-networks-acl.md)
