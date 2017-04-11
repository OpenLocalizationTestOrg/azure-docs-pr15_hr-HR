<properties
   pageTitle="Stvaranje pristupnika za aplikaciju pomoću portala za | Microsoft Azure"
   description="Saznajte kako stvoriti pristupnik za aplikaciju pomoću portala"
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

# <a name="create-an-application-gateway-by-using-the-portal"></a>Stvaranje pristupnika za aplikaciju pomoću portala

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-create-gateway-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-create-gateway-arm.md)
- [Azure klasični PowerShell](application-gateway-create-gateway.md)
- [Predloška Azure Voditelj resursa](application-gateway-create-gateway-arm-template.md)
- [Azure EŽA](application-gateway-create-gateway-cli.md)

Azure aplikacije pristupnik je sloj 7 opterećenja. Hoće li se nalaze na oblaku ili na lokalnim poslužiteljima, omogućuje prebacivanje, performanse usmjeravanje HTTP zahtjeva između različitih poslužitelja. Pristupnik za aplikacije sadrži brojne aplikacije isporuke kontroleru (ADC) značajke uključujući HTTP opterećenja afinitet utemeljen na kolačića sesije, offload Secure Sockets Layer (SSL), prilagođene stanja probes, podrška za više web-mjesta i mnoge druge. Da biste pronašli popis svih podržane značajke, posjetite [Aplikacije pristupnik za pregled](application-gateway-introduction.md)

## <a name="scenario"></a>Scenarij

U ovom scenariju, Saznajte kako stvoriti pomoću portala za Azure pristupnik za aplikaciju.

Ovaj scenarij će:

- Stvaranje pristupnika za srednje aplikacije s dvije instance.
- Stvaranje virtualne mreže pod nazivom AdatumAppGatewayVNET s rezerviranim blok CIDR 10.0.0.0/16.
- Stvaranje podmreže naziva Appgatewaysubnet koji koristi 10.0.0.0/28 kao njegov CIDR blok.
- Konfiguriranje certifikata za rasterećivanje SSL.

![Primjer scenarija][scenario]

>[AZURE.IMPORTANT] Dodatni konfiguracije pristupnika za aplikaciju, uključujući prilagođene stanja probes, pozadinskog skup adrese i dodatna pravila konfigurirane kada je konfiguriran pristupnik za aplikaciju, a ne tijekom početnog uvođenja.

## <a name="before-you-begin"></a>Prije početka

Azure pristupnika aplikacije potreban je vlastitu podmreže. Prilikom stvaranja virtualne mreže, provjerite je li ostavite dovoljno prostora na adresi imati više podmreže. Nakon implementacije pristupnik za aplikaciju za podmreži pristupnika samo dodatne aplikacije mogu se dodati podmreži.

## <a name="create-the-application-gateway"></a>Stvaranje pristupnika za aplikaciju

### <a name="step-1"></a>Korak 1

Otvorite centar za Azure, kliknite **Novo** > **umrežavanje** > **Aplikacije pristupnika**

![Stvaranje pristupnika za aplikaciju][1]

### <a name="step-2"></a>Korak 2

Zatim popunite osnovne informacije o aplikaciji pristupnika. Po završetku kliknite **u redu**

Podatke koji su potrebni za osnovne postavke je:

- **Ime** - naziv pristupnika aplikacije.
- **Sloju** – to je sloju pristupnik za aplikacije. Dvije razine su dostupni, **WAF** i **standardne**. WAF omogućuje značajku vatrozid aplikaciju za web.
- **Veličina SKU** - tu postavku je veličina pristupnika aplikacije, dostupne su mogućnosti (**mala**, **Srednje**i **Veliko**). *Mali nije dostupna kada je odabran sloj WAF*
- **Instance broj** - broj instanci, ta vrijednost mora biti broj između 2 i 10.
- **Grupa resursa** - grupa resursa za pristupnik za aplikacije može biti postojeću grupu resursa ili novi.
- **Mjesto** - regiji za pristupnik za aplikaciju je na isto mjesto na grupu resursa. *Mjesto je važno kao virtualne mreže i javnu IP mora biti na istom mjestu kao pristupnika*.

![osnovne postavke plohu prikazuje][2]

>[AZURE.NOTE] Instance broj 1 možete odabrati za testiranje. Nije važno je znati da sve instance broj u odjeljku dvije instance ne pokriva u SLA i zbog toga ne preporučuje se. Mali pristupnika su koja će se koristiti za testiranje razvojni i ne namjene.

### <a name="step-3"></a>Korak 3

Kada definirate osnovne postavke, sljedeći je korak da biste definirali virtualne mreže koja će se koristiti. Virtualne mreže nalaze se pristupnik za aplikaciju uravnoteženje opterećenja za aplikacije.

Kliknite **Odaberi virtualne mreže** da biste konfigurirali virtualne mreže.

![plohu prikazuje postavke za aplikaciju pristupnika][3]

### <a name="step-4"></a>Korak 4

U plohu *Odaberite virtualne mreže* kliknite **Stvori novo**

Dok ne objašnjenje u ovom scenariju, postojeće virtualne mreže nije sada odabrati.  Ako se koristi postojeće virtualne mreže, važno je znati da virtualne mreže treba prazan podmreže ili podmreži samo aplikacije pristupnika resursa koje želite koristiti.

![Odaberite plohu virtualne mreže][4]

### <a name="step-5"></a>Korak 5

Ispunite podatke o mreži plohu **Stvaranje virtualne mreže** opisan u prethodnom [scenarij](#scenario) opis.

![Stvaranje plohu virtualne mreže s podacima koje ste unijeli][5]

### <a name="step-6"></a>Korak 6

Nakon stvaranja virtualne mreže, sljedeći je korak da biste definirali sučelja IP za pristupnik za aplikacije. Sada je odabir između javni i privatni IP adresa za sučeljem. Odabir ovisi o tome je li aplikacija internet dostupnog ni interne samo. Ovaj scenarij pretpostavlja javnu IP adrese. Da biste odabrali privatne IP adrese, možete kliknuti gumb **Privatno** . Odabran je automatski dodijeljena IP adresa ili kliknete potvrdni okvir za **Odabir određene privatne IP adrese** za jednu Ručni unos.

### <a name="step-7"></a>Korak 7

Kliknite **Odaberi javnu IP adresa**. Ako je dostupan postojeću javnu IP adresu ga možete odabrati sada, u tom slučaju stvorite novu javnu IP adresu. Kliknite **Stvori novi**

![Odaberite javno plohu IP adresa][6]

### <a name="step-8"></a>Korak 8

Zatim dodijelite na javnu IP adresu neslužbeni naziv i kliknite **u redu**

![Stvaranje javnog plohu IP adresa][7]

### <a name="step-9"></a>Korak 9

Posljednja postavka da biste konfigurirali prilikom stvaranja pristupnik za aplikaciju je konfiguracija ga slušatelj.  Ako se koristi **http** ništa se ulijevo da biste konfigurirali i možete je moguće kliknuti **u redu** . Da biste koristili **https** dodatno konfiguracije potreban je.

Da biste koristili **https**, potreban je certifikat. Privatni ključ certifikata je potrebno da .pfx izvoz certifikata i lozinku morate koristiti.

### <a name="step-10"></a>Korak 10

Kliknite **HTTPS**, kliknite ikonu **mape** uz tekstni okvir **Prijenos PFX certifikata** .
Dođite do .pfx datoteka certifikata datotečnom sustavu. Kada je odabrana, dodijelite certifikat neslužbeni naziv, a zatim upišite lozinku za .pfx datoteka.

Kada je dovršena kliknite **u redu** da biste pregledali postavke za pristupnik za aplikacije.

![Odjeljak konfiguracija ga slušatelj na plohu postavke][9]

### <a name="step-11"></a>Korak 11

Pregledajte sažetak stranice, a zatim kliknite **u redu**.  Sada u redu i stvorili pristupnik za aplikacije.

### <a name="step-12"></a>Korak 12

Nakon što pristupnik za aplikaciju, otvorite ga na portalu za nastavak konfiguracije pristupnika za aplikacije.

![Prikaz resursa za pristupnik za aplikaciju][10]

Ove korake stvaranje pristupnika osnovne aplikacije sa zadanim postavkama za ga slušatelj, skup pozadinskog, postavke http pozadinskog sustava i pravila. Možete izmijeniti ove postavke tako da odgovaraju implementaciju sustava kada za dodjelu resursa je uspješan.

## <a name="next-steps"></a>Daljnji koraci

Ovaj scenarij stvara zadani pristupnik aplikacije. Daljnji koraci se mogu konfigurirati pristupnik za aplikaciju dodavanje članova grupe aplikacija, Izmjena postavki i Prilagodba pravila u pristupnika da bi funkcionirao.

Saznajte kako stvoriti prilagođene stanja probes tako što ste posjetili [Stvaranje prilagođenih stanja probni](application-gateway-create-probe-portal.md)

Upute za konfiguriranje SSL rasteretite, a zatim snimite skup dešifriranje SSL izvan web-poslužiteljima web-mjestu [Konfiguriranje SSL Offload](application-gateway-ssl-portal.md)

Saznajte kako se zaštititi aplikacije s [Web aplikacije vatrozid](application-gateway-webapplicationfirewall-overview.md) značajka aplikacije pristupnika.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
