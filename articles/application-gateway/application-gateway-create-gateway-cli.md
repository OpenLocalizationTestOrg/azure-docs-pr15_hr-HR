<properties
   pageTitle="Stvaranje pristupnika za aplikaciju pomoću EŽA Azure u upravitelju resursa | Microsoft Azure"
   description="Saznajte kako stvoriti pristupnik za aplikaciju pomoću EŽA Azure u upravitelju resursa"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Stvaranje pristupnika za aplikaciju pomoću EŽA Azure

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-create-gateway-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-create-gateway-arm.md)
- [Azure klasični PowerShell](application-gateway-create-gateway.md)
- [Predloška Azure Voditelj resursa](application-gateway-create-gateway-arm-template.md)
- [Azure EŽA](application-gateway-create-gateway-cli.md)

Azure aplikacije pristupnik je sloj 7 opterećenja. Hoće li se nalaze na oblaku ili na lokalnim poslužiteljima, omogućuje prebacivanje, performanse usmjeravanje HTTP zahtjeva između različitih poslužitelja. Pristupnik za aplikacije sadrži sljedeće značajke aplikacije isporuke: HTTP učitavanje ujednačavanja, afinitet utemeljen na kolačića sesije i rasterećivanje Secure Sockets Layer (SSL) probes prilagođene stanja i podrške za više web-mjesta.

## <a name="prerequisite-install-the-azure-cli"></a>Preduvjeta: Instalacija Azure EŽA

Da biste izvršili korake navedene u ovom članku, morate [instalirati Azure sučelja naredbenog retka za Mac i Linux, Windows (Azure EŽA)](../xplat-cli-install.md) i morate [prijaviti na Azure](../xplat-cli-connect.md). 

> [AZURE.NOTE] Ako nemate račun za Azure, potreban vam je jedan. Prelazak znak prema gore za [besplatnu probnu verziju u nastavku](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scenarij

U ovom scenariju, Saznajte kako stvoriti pomoću portala za Azure pristupnik za aplikaciju.

Ovaj scenarij će:

- Stvaranje pristupnika za srednje aplikacije s dvije instance.
- Stvaranje virtualne mreže pod nazivom AdatumAppGatewayVNET s rezerviranim blok CIDR 10.0.0.0/16.
- Stvaranje podmreže naziva Appgatewaysubnet koji koristi 10.0.0.0/28 kao njegov CIDR blok.
- Konfiguriranje certifikata za rasterećivanje SSL.

![Primjer scenarija][scenario]

>[AZURE.NOTE] Dodatni konfiguracije pristupnika za aplikaciju, uključujući prilagođene stanja probes, pozadinskog skup adrese i dodatna pravila konfigurirane kada je konfiguriran pristupnik za aplikaciju, a ne tijekom početnog uvođenja.

## <a name="before-you-begin"></a>Prije početka

Azure pristupnika aplikacije potreban je vlastitu podmreže. Prilikom stvaranja virtualne mreže, provjerite je li ostavite dovoljno prostora na adresi imati više podmreže. Nakon implementacije pristupnik za aplikaciju za podmreži pristupnika samo dodatne aplikacije mogu se dodati podmreži.

## <a name="log-in-to-azure"></a>Prijavite se na Azure

Otvorite **naredbeni redak za Microsoft Azure**i prijavite se. 

    azure login

Kada upišete u prethodnom primjeru, navedeni su kod. Dođite do https://aka.ms/devicelogin u pregledniku da biste nastavili postupak prijave.

![Prijava za uređaj s prikazom cmd][1]

U pregledniku, unesite kod koji ste primili. Bit ćete preusmjereni na stranicu za prijavu.

![preglednik da biste unijeli kod][2]

Kada je unesen kod ste se prijavili, zatvorite preglednik da biste nastavili na s scenarija.

![Uspješna prijava][3]

## <a name="switch-to-resource-manager-mode"></a>Prebacivanje u način rada Voditelj resursa

    azure config mode arm

## <a name="create-the-resource-group"></a>Stvaranje grupa resursa

Prije stvaranja pristupnik za aplikaciju za sadrže pristupnika aplikacije stvoren je grupu resursa. Na sljedećoj je slici prikazan naredbu.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Stvaranje virtualne mreže

Nakon stvaranja grupe resursa za pristupnik za aplikaciju stvara se virtualne mreže.  U sljedećem primjeru prostor adrese je kao 10.0.0.0/16 kako je definirano u prethodnom scenarij bilješke.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Stvaranje podmreži

Nakon stvaranja virtualne mreže podmreži dodaje za pristupnik za aplikacije.  Ako namjeravate koristiti pristupnika aplikacije s web-aplikacijama smješten u istom virtualne mreže kao pristupnik za aplikaciju, obavezno ostavite dovoljno prostora za drugi podmreže.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>Stvaranje pristupnika za aplikaciju

Kada se stvaraju virtualne mreže i podmreže, stara requisites za pristupnik za aplikaciju je završite. Uz to su potrebni za sljedeći korak prethodno izvezeni .pfx certifikat i lozinku za potvrdu: IP adrese za pozadinski nisu IP adresa za pozadinskog poslužitelja. Ove vrijednosti može biti privatne IP-ovi u virtualne mreže, javnu IP-ovi ili potpuno kvalificiranih naziva domena za pozadinskog poslužitelja.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Popis parametara koji se može pružati tijekom stvaranja pokrenite sljedeću naredbu: **aplikacije pristupnik za azure mreže stvaranje – pomoć za**.

U ovom se primjeru stvara pristupnik osnovne aplikacije sa zadanim postavkama za ga slušatelj, skup pozadinskog, postavke http pozadinskog sustava i pravila. Također se konfigurira rasterećivanje SSL. Možete izmijeniti ove postavke tako da odgovaraju implementaciju sustava kada za dodjelu resursa je uspješan.
Ako već imate web-aplikacije definirana pomoću u grupu pozadinskog prethodnih koraka nakon stvaranja, opterećenja započinje.

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako stvoriti prilagođene stanja probes tako što ste posjetili [Stvaranje prilagođenih stanja probni](application-gateway-create-probe-portal.md)

Saznajte kako konfigurirati SSL rasteretite, a zatim snimite skup dešifriranje SSL izvan web-poslužiteljima web-mjestu [Konfiguriranje SSL Offload](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png