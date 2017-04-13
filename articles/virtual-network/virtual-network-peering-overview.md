
<properties
   pageTitle="Azure virtualne mreže peering | Microsoft Azure"
   description="Saznajte više o VNet peering u Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="narayan" />

# <a name="vnet-peering"></a>VNet peering

Mehanizam koja povezuje dvije virtualne mreže (VNets) u području isti putem mreže Azure backbone VNet peering je. Kada peered, prikazuju se dvije virtualne mreže kao jedan svrhe sve povezivanjem. I dalje upravlja kao zasebna resursa, ali virtualnim strojevima u njima virtualne mogli komunicirati sa međusobno pomoću privatne IP adrese.

Promet između virtualnim strojevima u peered virtualne mreže je usmjerena kroz Azure infrastrukture kao promet usmjeravanja između VMs u istom virtualne mreže. Neke od prednosti korištenja VNet peering obuhvaćaju sljedeće:

- Vezu niske latencije, velikom propusnošću između resursa u različitim virtualne mreže.
- Mogućnost korištenja resursa kao što su aparata mreže i VPN pristupnika kao prijenosa točke u peered VNet.
- Mogućnost povezivanja virtualne mreže koja koristi model upravljanja resursima Azure virtualne mrežom koja koristi klasične implementacije model i omogućivanje punu vezu između resursa u njima virtualne.

Preduvjeti i ključne aspekte VNet peering:

- Dva virtualne mreža koje su peered mora biti u istoj Azure regiji.
- Virtualna mreža koje su peered mora imati koje se ne preklapaju razmake IP adresa.
- VNet peering je između dvije virtualne mreže pa nema izvedenih korištenje odnosa. Ako, na primjer, ako je virtualne mreže odgovora peered s virtualne mreže B, a ako virtualne mreže B je peered s virtualne mreže C, ona ne prevesti na virtualne mreže na koji se peered s virtualne mreže C.
- Peering možete uspostaviti između virtualne mreže u dvije različite pretplate dugo neadministratorskog povlaštene korisnika i pretplata na peering i pretplate povezuju u istom klijentu servisa Active Directory. 
- Peering između virtualne mreže u modelu upravitelj resursa i model klasični implementacije zahtijeva da se VNets mora biti iste pretplate.
- Virtualne mreže koja koristi model implementacije Voditelj resursa koje se mogu peered s drugom virtualne mreže koja koristi ovaj model ili s virtualne mreže koji koristi model klasični implementacije. Međutim, virtualne mreže koje koriste model klasični implementacije nije moguće peered jedni s drugima.
- Iako komunikaciju između virtualnim strojevima u peered virtualne mreže ima bez ograničenja propusnosti dodatne, kapaciteta propusnosti ovisno o veličini VM se i dalje odnosi.


![Osnovni peering VNet](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Povezivanje
Nakon što su dvije virtualne mreže peered, virtualnog računala (web-tempiranja uloga) u virtualne mreže izravno možete povezati s drugim virtualnim strojevima u peered virtualne mreže. Ove dvije mreže imaju potpunu IP razinom povezivanje.

Latenciju mreže za trajanjem putovanja između dva virtualnim strojevima peered virtualne mreže je isti kao trajanjem putovanja u lokalnoj mreži virtualne. Propusnost mreže temelji se na propusnost je dopušteno proporcionalno veličini virtualnog računala. Ne postoji neki dodatna ograničenja propusnosti.

Promet između virtualnim strojevima u peered virtualne mreže usmjeravanja izravno kroz Azure pozadinske infrastrukturu i ne pristupnik.

Virtualnim strojevima u virtualne mreže možete pristupiti na interni uravnoteženja (ILB) krajnje točke u peered virtualne mreže. Mrežni sigurnosnih grupa (NSGs) se mogu primijeniti u svakom virtualne mreže da biste blokirali pristup drugim virtualne mreže ili podmreže po želji.

Kada korisnici konfigurirati peering, oni otvaranje i zatvaranje pravila NSG između virtualne mreže. Ako korisnik odabere da biste otvorili punu vezu između peered virtualne mreže (što je zadana mogućnost), zatim mogu koristiti NSGs na određene podmreže ili virtualnim strojevima blokirati ili odbijanje pristupa određene.

Pod uvjetom Azure Interna Razrješavanje DNS naziva za virtualnih računala ne funkcionira preko peered virtualne mreže. Virtualnim računalima imati interne DNS naziva resolvable samo unutar lokalne virtualne mreže. Međutim, korisnici mogu konfigurirati virtualnim strojevima koji se koriste u peered virtualne mreže kao DNS poslužitelji za virtualne mreže.

## <a name="service-chaining"></a>Servis za Ulančavanje
Korisnici mogu konfigurirati korisnički definirane usmjeravanje tablice koje pokažite na virtualnim strojevima u peered virtualne mreže kao "sljedeći put" IP adresu, kao što je prikazano u dijagramu u nastavku ovog članka. To korisnicima omogućuje da postigli servisa Ulančavanje, putem kojega se usmjerili promet s jedne mreže virtualne virtualne uređaj koji se izvodi u peered virtualne mreže pomoću korisnički definiranih usmjeravanje tablice.

Korisnicima možete izrađivati i učinkovito koncentratora i žbica vrsta okruženja kojima koncentrator možete hostirati infrastrukture komponente kao što je uređaj za virtualne mreže. Sve žbica virtualne mreže pa možete ravnopravni uz to, kao i podskup promet na aparata koji rade koncentratora virtualne mreže. Ukratko, VNet peering omogućuje sljedeće mjesto IP adresa u 'korisnički definirane tablice usmjeravanje' biti IP adresu virtualnog računala u peered virtualne mreže.

## <a name="gateways-and-on-premises-connectivity"></a>Povezivanje pristupnika i lokalne
Svaki virtualne mreže, bez obzira na to jesu li peered s drugom virtualne mreže, i dalje možete imati vlastitu pristupnika te ga koristiti za povezivanje s lokalnim. Korisnicima možete konfigurirati [VNet VNet veza](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) pomoću pristupnika, čak i ako su peered virtualne mreže.

Kada niste konfigurirali obje mogućnosti za interconnectivity virtualne mreže, promet između virtualne mreže teče kroz peering konfiguraciju (to jest, do Azure backbone).

Kada su peered virtualne mreže, korisnici možete konfigurirati pristupnik u peered virtualne mreže kao točku prijenosa na lokalni. U ovom slučaju virtualne mreže koja koristi udaljene pristupnik ne mogu imati vlastitu pristupnika. Virtualna mreža može imati samo jedan pristupnik. To može biti lokalni pristupni ili udaljene pristupnika (u peered virtualne mreži), kao što je prikazano na sljedećoj slici.

Prijenosa pristupnik nije podržan u peering odnos između virtualne mreže pomoću modela Voditelj resursa i onih koji koriste model klasični implementacije. Oba virtualne mreže u odnosu peering morate koristiti model implementacije Voditelj resursa za pristupnik prijenosa rad.

Kada su virtualne mreža s kojima zajednički koristite jednu vezu Azure ExpressRoute peered, promet između njih prolaze kroz peering odnosa (to jest, putem mreže Azure backbone). Korisnici i dalje možete koristiti lokalne pristupnika u svakom virtualne mreže za povezivanje s lokalnim elektronička. Osim toga, možete koristiti zajedničke pristupnika i konfiguriranje prijenosa za povezivanje s lokalnim.

![VNet peering prijenosa](./media/virtual-networks-peering-overview/figure02.png)

## <a name="provisioning"></a>Dodjela resursa
VNet peering je povlaštene operacije. Je odvojenoj funkciji u odjeljku naziva VirtualNetworks. Korisnik može biti zadano određena prava da biste autorizirali peering. Korisnik koji ima pristup čitanja i pisanja virtualne mreže automatski nasljeđuje ta prava.

Korisnika ili administrator ili povlaštene korisnike peering mogućnost možete pokrenuti peering operacije na drugi VNet. Ako postoji odgovarajući zahtjev za peering na drugoj strani, a ako su ispunjeni ostale preduvjete, na peering će se uspostaviti.

Potražite u člancima u odjeljku "Sljedeći koraci" da biste saznali više o načinu pokretanja VNet peering između dvije virtualne mreže.

## <a name="limits"></a>Ograničenja
Nema ograničenja broja peerings je dopušteno virtualne mreže. Pogledajte [Azure umrežavanje ograničenja](../azure-subscription-service-limits.md#networking-limits) za dodatne informacije.

## <a name="pricing"></a>Cijene
VNet peering bit će besplatna tijekom razdoblja za pregled. Nakon objavljivanja, pojavit će se nominal trošak na web-mjesto ingress i u okvir za izlazne promet koji se koristi u peering. Dodatne informacije potražite [cijene stranice](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="next-steps"></a>Daljnji koraci
- [Postavljanje peering između virtualne mreže](virtual-networks-create-vnetpeering-arm-portal.md).
- Saznajte više o [NSGs](virtual-networks-nsg.md).
- Informirajte se o [korisnički definirane usmjerava i IP prosljeđivanje](virtual-networks-udr-overview.md).
