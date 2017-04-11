<properties
   pageTitle="Stvaranje pristupnika za aplikacije s web aplikacije vatrozid pomoću portala za | Microsoft Azure"
   description="Saznajte kako stvoriti pristupnik za aplikaciju s vatrozidom web aplikacije pomoću portala"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
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

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Stvaranje pristupnika za aplikaciju s vatrozidom aplikaciju za web pomoću portala

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-web-application-firewall-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-web-application-firewall-powershell.md)

Vatrozid web aplikacije (WAF) u Azure aplikacije pristupnika štiti web-aplikacije od uobičajenih utemeljen na webu napada, kao što je unos SQL, web-mjesta skriptiranja i hijacks sesiju. Web-aplikacije štiti od raznih na OWASP gornji 10 uobičajenih web slabe točke.

Azure aplikacije pristupnik je sloj 7 opterećenja. Hoće li se nalaze na oblaku ili na lokalnim poslužiteljima, omogućuje prebacivanje, performanse usmjeravanje HTTP zahtjeva između različitih poslužitelja.
Aplikacije sadrži brojne aplikacije isporuke kontroleru (ADC) značajke uključujući HTTP opterećenja afinitet utemeljen na kolačića sesije, offload Secure Sockets Layer (SSL), prilagođene stanja probes, podrška za više web-mjesta i mnoge druge.
Da biste pronašli cjelovit popis značajki podržanih, posjetite [Aplikacije pristupnik za pregled](application-gateway-introduction.md)

## <a name="scenarios"></a>Scenariji

U ovom se članku postoje dva scenarija:

U prvom scenariju Doznajte kako [dodati aplikacije Vatrozid za web u postojeće pristupnik za aplikaciju](#add-web-application-firewall-to-an-existing-application-gateway).

Drugi scenarij saznat ćete da biste [stvorili pristupnik za aplikaciju s vatrozidom aplikaciju za web](#create-an-application-gateway-with-web-application-firewall)

![Primjer scenarija][scenario]

>[AZURE.NOTE] Dodatni konfiguracije pristupnika za aplikaciju, uključujući prilagođene stanja probes, pozadinskog skup adrese i dodatna pravila konfigurirane kada je konfiguriran pristupnik za aplikaciju, a ne tijekom početnog uvođenja.

## <a name="before-you-begin"></a>Prije početka

Azure pristupnika aplikacije potreban je vlastitu podmreže. Prilikom stvaranja virtualne mreže, provjerite je li ostavite dovoljno prostora na adresi imati više podmreže. Nakon implementacije pristupnik za aplikaciju za podmreži pristupnika samo dodatne aplikacije mogu se dodati podmreži.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Dodavanje vatrozid aplikaciju za web u postojeće pristupnik za aplikaciju

Ovaj scenarij ažurira postojeći pristupnika aplikacije za podršku web aplikacije vatrozid u načinu rada za sprječavanje.

### <a name="step-1"></a>Korak 1

Otvorite centar za Azure, odaberite postojeći pristupnik za aplikacije.

![Stvaranje pristupnika za aplikaciju][1]

### <a name="step-2"></a>Korak 2

Kliknite **Konfiguracija** i ažuriranje postavki pristupnika aplikacije. Po završetku kliknite **Spremi**

Postavke da biste ažurirali postojeće pristupnika aplikacije za podršku web aplikacije vatrozid su:

- **Sloju** – sloju odabrana mora biti **WAF** za podršku web aplikacije vatrozid
- **Veličina SKU** - tu postavku je veličina pristupnika aplikacije s vatrozidom aplikaciju za web, dostupne su mogućnosti (**Srednje velike** i **Veliko**).
- **Stanje vatrozida** - Ova postavka onemogućuje ili omogućuje vatrozid aplikaciju za web.
- **Vatrozid način** - Ova postavka je kako web aplikacije vatrozid bavi zlonamjerni promet. Način za **otkrivanje** samo zapisnik događaja, gdje načinom rada za **Sprječavanje** zapisnike događaja i zaustavlja zlonamjerni promet.

![osnovne postavke plohu prikazuje][2]

>[AZURE.NOTE] Da biste pogledali web-aplikacije vatrozid dnevnika, mora biti omogućen Dijagnostika i odabranim ApplicationGatewayFirewallLog. Instance broj 1 možete odabrati za testiranje. Nije važno je znati da sve instance broj u odjeljku dvije instance ne pokriva u SLA i zbog toga ne preporučuje se. Mali pristupnika nisu dostupne kada se koristi vatrozid aplikaciju za web.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Stvaranje pristupnika za aplikaciju s vatrozidom aplikaciju za web

Ovaj scenarij će:

- Stvaranje pristupnika aplikacije Vatrozid za srednje web aplikacije s dvije instance.
- Stvaranje virtualne mreže pod nazivom AdatumAppGatewayVNET s rezerviranim blok CIDR 10.0.0.0/16.
- Stvaranje podmreže naziva Appgatewaysubnet koji koristi 10.0.0.0/28 kao njegov CIDR blok.
- Konfiguriranje certifikata za rasterećivanje SSL.

### <a name="step-1"></a>Korak 1

Otvorite centar za Azure, kliknite **Novo** > **umrežavanje** > **Aplikacije pristupnika**

![Stvaranje pristupnika za aplikaciju][1-1]

### <a name="step-2"></a>Korak 2

Zatim popunite osnovne informacije o aplikaciji pristupnika. Obavezno odaberite **WAF** kao u sloju. Po završetku kliknite **u redu**

Podatke koji su potrebni za osnovne postavke je:

- **Ime** - naziv pristupnika aplikacije.
- **Sloju** – sloju pristupnika aplikacije dostupne su mogućnosti (**standardni** i **WAF**). Vatrozid web aplikacije dostupna je samo u sloju WAF.
- **Veličina SKU** - tu postavku je veličina pristupnika aplikacije, dostupne su mogućnosti (**Srednje velike** i **Veliko**).
- **Instance broj** - broj instanci, ta vrijednost mora biti broj između **2** i **10**.
- **Grupa resursa** - grupa resursa za pristupnik za aplikacije može biti postojeću grupu resursa ili novi.
- **Mjesto** - regiji za pristupnik za aplikaciju je na isto mjesto na grupu resursa. *Mjesto je važno kao virtualne mreže i javnu IP mora biti na istom mjestu kao pristupnika*.

![osnovne postavke plohu prikazuje][2-2]

>[AZURE.NOTE] Instance broj 1 možete odabrati za testiranje. Nije važno je znati da sve instance broj u odjeljku dvije instance ne pokriva u SLA i zbog toga ne preporučuje se. Mali pristupnika nisu podržani za scenarije vatrozid aplikaciju za web.

### <a name="step-3"></a>Korak 3

Kada definirate osnovne postavke, sljedeći je korak da biste definirali virtualne mreže koja će se koristiti. Virtualne mreže nalaze se pristupnik za aplikaciju uravnoteženje opterećenja za aplikacije.

Kliknite **Odaberi virtualne mreže** da biste konfigurirali virtualne mreže.

![plohu prikazuje postavke za aplikaciju pristupnika][3]

### <a name="step-4"></a>Korak 4

U plohu **Odaberite virtualne mreže** kliknite **Stvori novo**

Dok ne objašnjenje u ovom scenariju, postojeće virtualne mreže nije sada odabrati.  Ako se koristi postojeće virtualne mreže, važno je znati da virtualne mreže treba prazan podmreže ili podmreži samo aplikacije pristupnika resursa koje želite koristiti.

![Odaberite plohu virtualne mreže][4]

### <a name="step-5"></a>Korak 5

Ispunite podatke o mreži plohu **Stvaranje virtualne mreže** opisan u prethodnom [scenarij](#scenario) opis.

![Stvaranje plohu virtualne mreže s podacima koje ste unijeli][5]

### <a name="step-6"></a>Korak 6

Nakon stvaranja virtualne mreže, sljedeći je korak da biste definirali sučelja IP za pristupnik za aplikacije. Sada je odabir između javni i privatni IP adresa za sučeljem. Odabir ovisi o tome je li aplikacija internet dostupnog ni interne samo. Ovaj scenarij pretpostavlja javnu IP adrese. Da biste odabrali privatne IP adrese, možete kliknuti gumb **Privatno** . Odabran je automatski dodijeljena IP adresa ili kliknete potvrdni okvir za **Odabir određene privatne IP adrese** za jednu Ručni unos.

Kliknite **Odaberi javnu IP adresa**. Ako je dostupan postojeću javnu IP adresu ga možete odabrati sada, u tom slučaju stvorite novu javnu IP adresu. Kliknite **Stvori novi**

![Odaberite javno plohu IP adresa][6]

### <a name="step-7"></a>Korak 7

Zatim dodijelite na javnu IP adresu neslužbeni naziv i kliknite **u redu**

![Stvaranje javnog plohu IP adresa][7]

### <a name="step-8"></a>Korak 8

Nakon toga postavite ga slušatelj konfiguracije.  Ako se koristi **http** ništa ostane da biste konfigurirali i možete je moguće kliknuti **u redu** . Da biste koristili **https** dodatno konfiguracije potreban je.

Da biste koristili **https**, potreban je certifikat. Privatni ključ certifikata je potrebno da bi se na .pfx izvoz certifikat mora se pružati i lozinku za datoteku.

Kliknite **HTTPS**, kliknite ikonu **mape** uz tekstni okvir **Prijenos PFX certifikata** .
Dođite do .pfx datoteka certifikata datotečnom sustavu. Kada je odabrana, dodijelite certifikat neslužbeni naziv, a zatim upišite lozinku za .pfx datoteka.

Kada je dovršena kliknite **u redu** da biste pregledali postavke za pristupnik za aplikacije.

![Odjeljak konfiguracija ga slušatelj na plohu postavke][8]

### <a name="step-9"></a>Korak 9

Konfiguriranje postavki određene **WAF** .

- Na ili **Stanje vatrozida** - Ova postavka isključuje WAF.
- **Način vatrozid** - Ova postavka određuje WAF akcije vodi na zlonamjerni promet. Ako je odabran **otkrivanje** , prijavljen je samo promet.  Ako je odabran **Sprječavanje** , promet je prijavljen i zaustavljena s 403 Unauthorized.

![postavke vatrozida za web-aplikacije][9]

### <a name="step-10"></a>Korak 10

Pregledajte sažetak stranice, a zatim kliknite **u redu**.  Sada u redu i stvorili pristupnik za aplikacije.

### <a name="step-12"></a>Korak 12

Nakon što pristupnik za aplikaciju, otvorite ga na portalu za nastavak konfiguracije pristupnika za aplikacije.

![Prikaz resursa za pristupnik za aplikaciju][10]

Ove korake stvaranje pristupnika osnovne aplikacije sa zadanim postavkama za ga slušatelj, skup pozadinskog, postavke http pozadinskog sustava i pravila. Možete izmijeniti postavke da bi odgovarao implementaciju sustava kada za dodjelu resursa ne uspije

## <a name="next-steps"></a>Daljnji koraci

Upute za konfiguriranje dijagnostičkog zapisnika za zapisivanje događaja koji su otkrivena ili spriječio s vatrozidom aplikaciju za Web tako što ste posjetili [Dijagnostika pristupnika aplikacije](application-gateway-diagnostics.md)

Saznajte kako stvoriti prilagođene stanja probes tako što ste posjetili [Stvaranje prilagođenih stanja probni](application-gateway-create-probe-portal.md)

Saznajte kako konfigurirati SSL rasteretite, a zatim snimite skup dešifriranje SSL izvan web-poslužiteljima web-mjestu [Konfiguriranje SSL Offload](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png