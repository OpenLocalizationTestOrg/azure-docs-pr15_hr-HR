<properties
   pageTitle="Stvaranje prilagođene probni za pristupnik za aplikaciju pomoću portala za | Microsoft Azure"
   description="Saznajte kako stvoriti prilagođene probni za pristupnik za aplikaciju pomoću portala"
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

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Stvaranje prilagođene probni za pristupnik za aplikaciju pomoću portala

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-create-probe-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-create-probe-ps.md)
- [Azure klasični PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Scenarij

Sljedeći scenarij prolaze kroz stvaranje prilagođenih stanja probni u postojeće pristupnik za aplikacije.
Scenarij pretpostavlja da ste već pratili korake da biste [stvorili pristupnik za aplikacije](application-gateway-create-gateway-portal.md).

## <a name="createprobe"></a>Stvaranje na probni

Probes nije konfiguriran u dva koraka putem portala sustava. Prvi je korak da biste stvorili na probni, dalje dodate na probni postavke http pozadinska aplikacija pristupnika.

### <a name="step-1"></a>Korak 1

Dođite do http://portal.azure.com, a zatim odaberite postojeći pristupnik za aplikacije.

![Pregled aplikacija pristupnika][1]

### <a name="step-2"></a>Korak 2

Kliknite **Probes** , a zatim kliknite gumb **Dodaj** da biste dodali novi probni.

![Dodavanje probni plohu podacima ispunjenom tablicom][2]

### <a name="step-3"></a>Korak 3

Ispunite potrebne informacije za na probni i po završetku kliknite **u redu**.

- **Naziv** – to je neslužbeni naziv koji će probni kojemu se može pristupiti na portalu.
- **Glavno računalo** – to je naziv glavnog računala koji se koristi za na probni. Primjenjivo samo kada više web-mjesta je konfiguriran na pristupnik za aplikaciju, u suprotnom koristiti "127.0.0.1". Time se razlikuje od VM naziv glavnog računala.
- **Put** – ostatak cijeli url za prilagođene probni. Valjani put započinje rečenicom "/"
- **Interval (sekunde)** – Učestalost pokretanja na probni da biste provjerili stanje. Ne preporučuje se da biste postavili donjoj od 30 sekundi.
- **Prekoračenje vremena (sekunde)** – količinu vremena na probni čeka prije prekoračenja. Interval vremenskog ograničenja mora biti dovoljno visoku poziv http se može koristiti bilo da bi stranice stanje pozadinskog dostupna.
- **Dobro prag** - broj neuspjelih pokušaja smatraju dobro. Prag 0 znači ako je u stanju provjera ne uspije pozadinske bit će određeni dobro odmah.

> [AZURE.IMPORTANT] Naziv glavnog računala nije naziv poslužitelja. To je naziv virtualnog glavnog računala na poslužitelju aplikacije. Na probni šalje se http://(host name):(port from httpsetting) urlPath

![postavke konfiguracije probni][3]

## <a name="add-probe-to-the-gateway"></a>Dodavanje probni pristupnika

Sad kad je stvoren u probni, vrijeme je da biste ga dodali pristupnika. Postavke probni postavljene su na stranici Postavke http pozadinska aplikacija pristupnika.

### <a name="step-1"></a>Korak 1

Kliknite **Postavke HTTP** pristupnika aplikacije, a zatim trenutne pozadinskog http postavke u prozoru da bi se prikazala plohu konfiguracije.

![prozor https postavke][4]

### <a name="step-2"></a>Korak 2

Na postavke plohu **appGatewayBackEndHttp** kliknite **korištenje prilagođenih probni** i odaberite probni stvorili u odjeljku [Stvaranje na probni](#createprobe) .
Po dovršetku kliknite **u redu** te se postavke primjenjuju.

![plohu appgatewaybackend postavke][5]

Zadani probni provjerava zadani pristup web-aplikaciji. Sad kad je stvorio prilagođene probni pristupnik za aplikaciju koristi prilagođeni put definirani praćenje stanja za pozadinski odabrana. Na temelju kriterija koji je definiran pristupnik za aplikaciju provjerava otvorenu datoteku navedenu u na probni. Ako poziv na glavno računalo: priključak / put do vratite je u redu 200 Http status odgovor, poslužitelj izbaciti iz rotacije, kad zbirka dostigne dobro praga. Probing nastavlja na dobro instancu da biste odredili kada postane dobar ponovno. Kada ponovno dodate instanci resurse poslužitelja dobar promet započinje slijedi da biste ga i probing instanci nastavlja intervalu korisnika navedeni normalno.


## <a name="next-steps"></a>Daljnji koraci

Da biste saznali kako konfigurirati SSL rasteretite s pristupnika Azure aplikacije potražite u članku [Konfiguriranje SSL Offload](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png