<properties
    pageTitle="Povezivanje SSL certifikata pomoću komponente PowerShell"
    description="Saznajte kako se povezati SSL certifikata na web-aplikaciju pomoću komponente PowerShell."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure aplikacije servisa SSL certifikat povezivanje pomoću komponente PowerShell #

Uz izdanje sustava Microsoft Azure PowerShell verzije 1.1.0 novi cmdlet dodana koji želite omogućiti korisniku povezati postojeći ili novi SSL certifikata u postojeću Web App.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Dodatne informacije o korištenju upravitelja resursa Azure temelji Azure PowerShell cmdleti za upravljanje aplikacijama Web provjerite [Voditelj resursa Azure temelji naredbe ljuske PowerShell za Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Prijenos i vezanje novi SSL certifikat ##

Scenarij: Korisnik želi povezati SSL certifikata na neki od postane web-aplikacije.

Zna naziv grupe resursa koja sadrži web-aplikaciju, naziv aplikacije za web, put datoteke .pfx certifikata na web-mjesto na računalu korisnika, u okvir za lozinku za potvrdu, a prilagođeni naziv glavnog računala, možete koristimo sljedeću naredbu komponente PowerShell da biste stvorili tu SSL povezivanje:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Imajte na umu da prije nego što dodate SSL povezivanja web App, morate imati već konfigurirali naziv glavnog računala (prilagođene domene). Ako nije konfiguriran naziv glavnog računala, a zatim će se pogreška "naziv glavnog računala" ne postoji prilikom pokretanja novo AzureRmWebAppSSLBinding. Naziv glavnog računala možete dodati izravno iz portalu ili pomoću komponente PowerShell Azure. Konfiguriranje glavnog računala prije pokretanja novo AzureRmWebAppSSLBinding može biti sljedeće komponente PowerShell isječka.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
Je važno je znati cmdlet skup AzureRmWebApp prebrisat će hostnames za web-aplikacije. Dakle iznad isječak PowerShell je dodavanje u postojeći popis nazivi glavnog računala za web-aplikacije.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Prijenos i vezanje SSL certifikata ##

Scenarij: Korisnik želi povezati prethodno prenesene SSL certifikat na neki od postane web-aplikacije.

Da će se koristiti popis certifikata koji se već prenijeli određene grupe resursa pomoću sljedeće naredbe

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Imajte na umu da su certifikati lokalni na određeno mjesto i grupa resursa, korisnik morati ponovno prenijeti certifikat ako konfigurirani web-aplikaciji je na neko drugo mjesto, a zatim resurs grupe druge koju koji potrebne certifikata 

Zna naziv grupe resursa koji sadrži web-aplikaciju, naziv aplikacije za web, otisak prsta certifikat i prilagođeni naziv glavnog računala, možete koristimo sljedeću naredbu komponente PowerShell da biste stvorili tu SSL povezivanje:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Brisanje postojeće povezivanje za SSL  ##

Scenarij: Korisnik želite da biste izbrisali postojeću vezu za SSL.

Kada zna naziv grupe resursa koji sadrži web-aplikaciju, naziv aplikacije web i prilagođeni naziv glavnog računala, možete koristimo sljedeću naredbu komponente PowerShell da biste uklonili te povezivanje SSL:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Imajte na umu da ako uklonjene povezivanje SSL je zadnji povezivanja pomoću tog certifikata u tom mjestu, po zadanom certifikat izbrisat će se, ako korisniku želite zadržati certifikata koje možete koristiti mogućnost DeleteCertificate da biste zadržali certifikata

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Reference ###
- [Azure Voditelj resursa koji se temelje naredbe ljuske PowerShell za Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Uvod u okruženje za aplikacije servisa](app-service-app-service-environment-intro.md)
- [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md)
