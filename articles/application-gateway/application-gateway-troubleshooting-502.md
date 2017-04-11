<properties
   pageTitle="Otklanjanje pogrešaka aplikacije pristupnika neispravan pristupnik (502) | Microsoft Azure"
   description="Upute za otklanjanje pogrešaka aplikacije pristupnika 502"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Otklanjanje poteškoća neispravan pristupnik u aplikaciji pristupnika

## <a name="overview"></a>Pregled

Nakon konfiguriranja programa Azure pristupnik za aplikaciju je jedna od pogreške koje korisnici mogu pojaviti "Pogreška poslužitelja: 502 – web-poslužitelj primili koji nisu valjani odgovor tijekom ulozi pristupnika ili proxy poslužitelja". To se može dogoditi zbog sljedećih glavna razloga:

- Skup pozadinske Azure aplikacije pristupnik nije konfiguriran ili je prazan.
- VMs ili instance u skupu skaliranje VM nijedna dobar.
- VMs ili instance postavite skaliranje VM pozadinske ne odgovarate stanja probni zadani.
- Nevaljane ili nepravilno konfiguracija probes prilagođene stanja.
- Zatraži istek vremena ili problema s povezivanjem zahtjevima korisnički.

## <a name="empty-backendaddresspool"></a>Prazan BackendAddressPool

### <a name="cause"></a>Uzrok

Ako je pristupnika aplikacije VMs ni VM skaliranje postavljanje konfiguriran u pozadinskoj adresu, ne možete usmjeriti svaki zahtjev za klijenta i throws neispravan pristupnik pogreške.

### <a name="solution"></a>Rješenja

Provjerite je li na skupna pozadinsku adresa nije prazan. To možete učiniti ili putem komponente PowerShell, EŽA ili portal.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

Izlaz iz prethodnog cmdlet smiju sadržavati skupna adresa pozadinske koje nisu prazne. Slijedi primjer gdje dva grupe vraćaju koji konfigurirane FQDN i IP adrese za pozadinskog VMs. Stanje dodjele resursa u BackendAddressPool mora biti 'uspjela".
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Dobro instance BackendAddressPool

### <a name="cause"></a>Uzrok

Ako su sve instance BackendAddressPool dobro, pristupnika aplikacije bi ne sadrži sve pozadinske za usmjeravanje korisnika zahtjev za. To, može biti slučaj kada pozadinske instanci su dobar, ali ste potrebna aplikacija implementiran.

### <a name="solution"></a>Rješenja

Provjerite je li pojavljivanja su dobar i aplikacije pravilno konfigurirani. Provjerite može odgovoriti na ping iz drugog VM u istoj VNet pozadinsku instance. Ako je konfiguriran za korištenje javne krajnju točku, provjerite je li preglednik zahtjev za web-aplikaciju serviceable.

## <a name="problems-with-default-health-probe"></a>Problemi s probni zadanog stanja

### <a name="cause"></a>Uzrok

502 pogrešaka može biti Česti Pokazatelji stanja probni zadani nije uspio doći do VMs pozadinsku. Kada je dodjeli instance pristupnika aplikacije, automatski konfigurira probni za stanje zadano za svaku BackendAddressPool pomoću svojstva u BackendHttpSetting. Da biste postavili ovaj probni potreban je bez korisničkom unosu. Konkretno, kada je pravilo za ujednačavanje opterećenja konfiguriran, između BackendHttpSetting i BackendAddressPool postala je pridruživanja. Zadani probni je konfiguriran za svaki od tih pridruživanja i aplikacije pristupnika započinje periodičku stanja veze potvrdite svaku instancu u BackendAddressPool pri priključak naveden u elementu BackendHttpSetting. Sljedeća tablica prikazuje pridružene stanja probni zadane vrijednosti.


|Isprobati svojstvo | Vrijednost | Opis|
|---|---|---|
| Isprobati URL-a| http://127.0.0.1/ | Put URL-a |
| Interval | 30 | Isprobati interval u sekundama |
| Prekoračenje vremena  | 30 | Isprobati Prekoračenje vremena u sekundama |
| Dobro praga. | 3 | Isprobati count pokušajte ponovno. Pozadinskom poslužitelju je označen nakon zbroj neuspjeha uzastopnih probni dosegne dobro praga. |

### <a name="solution"></a>Rješenja

- Provjerite je li zadano web-mjesto bude konfigurirana, a priključuje pri 127.0.0.1.
- Ako BackendHttpSetting određuje priključak osim 80, zadano mjesto trebali biste ga konfigurirati da biste preslušali na taj priključak.
- Poziv na http://127.0.0.1:port mora vratiti HTTP kod rezultat 200. To bi trebala biti vraćena unutar 30 sec isteka razdoblja.
- Provjerite je li otvoren priključak konfigurirana, a da ne postoje pravila vatrozida ili Azure mreže sigurnosnih grupa koje Blokiraj dolazni ili izlazni promet na priključak konfiguriran.
- Ako se s FQDN ili javnu IP koristi Azure klasični VMs ili servis u Oblaku, provjerite je li odgovarajuće [krajnjoj točki](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) otvoren.
- Ako na VM konfiguriran putem upravitelja za Azure resursa i izvan VNet gdje je implementiran pristupnik za aplikaciju, [Mreže sigurnosne grupe](../virtual-network/virtual-networks-nsg.md) biti konfigurirano tako da omogućuje pristup na željeni priključak.


## <a name="problems-with-custom-health-probe"></a>Problemi s probni prilagođene stanja

### <a name="cause"></a>Uzrok

Prilagođeni stanja probes omogućuju dodatnu fleksibilnost zadano ponašanje probing. Prilikom korištenja prilagođenih probes korisnicima možete konfigurirati interval probni, URL i put da biste testirali i koliko nije uspjelo odgovore da biste prihvatili prije označavanju instancu pozadinske skup kao dobro. Dodat će se sljedeća dodatna svojstva.

|Isprobati svojstvo| Opis|
|---|---|
| Ime | Naziv na probni. Taj naziv se koristi za upućivanje na probni u pozadinskoj HTTP postavke. |
| Protokol | Protokol koji se koristi za slanje u probni. Na probni će koristiti protokol definiranim u postavkama pozadinske HTTP |
| Glavno računalo |  Naziv glavnog računala da biste poslali na probni. Primjenjivo samo kada više web-mjesta je konfiguriran na računala pristupnika. Time se razlikuje od VM naziv glavnog računala.  |
| Put | Relativni put na probni. Valjani put započinje s '/'. Se šalje na probni \<protokol\>://\<glavno računalo\>:\<priključak\>\<put\> |
| Interval | Interval isprobati u sekundama. Ovo je vremenski interval između dva uzastopna probes.|
| Prekoračenje vremena | Isprobati Prekoračenje vremena u sekundama. Na probni je označen kao nije uspjela ako valjani odgovor nije primili unutar tog isteka razdoblja. |
| Dobro praga. | Isprobati count pokušajte ponovno. Pozadinskom poslužitelju je označen nakon zbroj neuspjeha uzastopnih probni dosegne dobro praga. |


### <a name="solution"></a>Rješenja

Provjerite valjanost koji isprobati Prilagođeno stanja ispravno konfigurirano kao prethodnoj tablici. Osim prethodne korake za otklanjanje poteškoća, provjeriti sljedeće:

- Provjerite je li u probni je li pravilno navedena po [vodič](application-gateway-create-probe-ps.md).
- Ako pristupnik za aplikaciju je konfiguriran za jednog web-mjesta, po zadanome domaćin potrebno navesti naziv kao "127.0.0.1", osim ako inače konfiguriran u prilagođene probni.
- Pobrinite se da poziv http://\<glavno računalo\>:\<priključak\>\<put\> vraća HTTP kod rezultat 200.
- Interval, isteka i UnhealtyThreshold provjerite jesu li unutar prihvatljiva raspona.

## <a name="request-time-out"></a>Zatraži istek vremena

### <a name="cause"></a>Uzrok

Primljena zahtjeva za korisnika, pristupnik za aplikacije primjenjuje konfigurirani pravila na zahtjev za i preusmjerava na instancu komponente pozadinske skup. Čeka konfigurirati interval vrijeme na odgovor instancu pozadinske. Prema zadanome, taj interval je **30 sekundi**. Ako aplikacija pristupnika nije moguća iz aplikacije pozadinskih u tom intervalu, zahtjev korisnika vidjeti 502 pogreške.

### <a name="solution"></a>Rješenja

Pristupnik za aplikacije korisnicima omogućuje konfiguriranje tu postavku putem BackendHttpSetting, koji se zatim primijeniti na različite grupe. Različite grupe pozadinske može imati različite BackendHttpSetting i istek vremena dakle zahtjeva konfiguriran.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Daljnji koraci

Ako prethodnih koraka ne riješite problem, otvorite je [podržava karata](https://azure.microsoft.com/support/options/).
