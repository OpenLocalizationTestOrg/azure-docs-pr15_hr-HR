<properties 
   pageTitle="Hibridno vezu s aplikacijom sloja 2 | Microsoft Azure"
   description="Saznajte kako implementirati virtualne aparata i UDR radi stvaranja okruženja aplikacije s više razina u Azure"
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
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Scenarij virtualne uređaj

Uobičajeni scenarij među veće Azure kupca je potrebno omogućuje dvije tiered aplikacije izložen putem Interneta istovremeni pristup natrag sloju iz podatkovnog centra sustava lokalnog. Ovaj dokument će vas voditi kroz scenarij pomoću korisnički definirane usmjerava (UDR), pristupnik VPN-a i virtualne aparata mreže za implementaciju okruženju dvije sloja koji zadovoljava sljedeće uvjete:

- Web-aplikacije mora biti dostupno putem Interneta javno samo.
- Web-poslužitelju koji se nalaze aplikacije moraju imati mogućnost da biste pristupili pozadinskog poslužitelja aplikacije.
- Sav promet s Interneta u web-aplikaciju mora proći kroz vatrozid uređaj za virtualne. Ovaj virtualni uređaj će se koristiti za internetski promet samo.
- Sav promet na poslužitelju aplikacije mora proći kroz vatrozid uređaj za virtualne. Ovaj virtualni uređaj će se koristiti za pristup podacima s poslužiteljem pozadinske završetka i pristup dolaze iz lokalne mreže putem VPN pristupnika.
- Administratori moraju imati mogućnost upravljanje aparata virtualne vatrozid sa svojim računalima lokalnog, pomoću nekog drugog vatrozida virtualne potražite koristi isključivo za upravljanje svrhama.

To je standardni scenarij DMZ na DMZ i zaštićene mrežom. Takve scenarij možete konstruirana u Azure pomoću NSGs, vatrozid virtualne aparata ili kombinacijom tih dvaju načina. U tablici u nastavku su navedene neke argumente za i protiv između NSGs i virtualne aparata vatrozid.

||Profesionalce|Protiv|
|---|---|---|
|NSG|Nema trošak. <br/>Integrirani u Azure RBAC. <br/>Pravila se može stvoriti u predlošcima OKVIRA.|Složenosti može razlikovati u većim okruženja. |
|Vatrozid|Potpunu kontrolu nad ravnini podataka. <br/>Središnje upravljanje kroz vatrozid konzolu.|Trošak vatrozida potražite. <br/>Nije integriran s Azure RBAC.|

Rješenja u nastavku koristi vatrozid virtualne aparata implementaciju mreže scenarij DMZ/zaštićen.

## <a name="considerations"></a>Razmatranja

Možete implementirati okruženje objašnjena iznad u Azure pomoću različitih značajki dostupnih danas, na sljedeći način.

- **Virtualna mreže (VNet)**. Programa Azure VNet djeluje sličan način za lokalnu mrežu, a možete segmentirajući u jednu ili više podmreže odvajanja promet i odvojenosti opasnosti.
- **Virtualna uređaj**. Nekoliko partnera navedite virtualne aparata u trgovine Windows Azure koje je moguće koristiti za tri vatrozidima prethodno opisan. 
- **Korisnički definirane usmjerava (UDR)**. Usmjeravanje tablica može sadržavati UDRs koristi Azure mreže da biste odredili tijek pakete unutar na VNet. U ovim su tablicama usmjeravanje primjenjuje se na podmreže. Jedna od najnovije značajke na Azure je mogućnost da biste primijenili usmjeravanje tablice GatewaySubnet, pruža proslijediti sav promet ulaze Azure VNet iz hibridnog veze da biste virtualne potražite.
- **Prosljeđivanje IP**. Prema zadanim postavkama modul Azure umrežavanje Proslijedi pakete karticama virtualne mreže sučelja (NIC-ovi) samo ako je paket odredišnu IP adresu udovoljava NIC IP adresa. Stoga na UDR definira paket mora biti poslana na navedeni virtualne potražite, modul Azure umrežavanje želite odbaciti taj paket. Da biste osigurali paket isporučen VM (u ovom slučaju virtualne potražite) koja se ne stvarne odredišta za paket, morate omogućiti IP prosljeđivanje za virtualne uređaj.
- **Mrežni sigurnosnih grupa (NSGs)**. U primjeru u nastavku ne koristio NSGs, ali nije moguće koristiti NSGs primjenjuje se na podmreže i/ili NIC-ovi u rješenje da biste dodatno filtrirali promet i one podmreže i NIC-ovi.


![Povezivanje putem protokola IPv6](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

U ovom primjeru prikazuje se na pretplatu koja sadrži sljedeće:

- 2 grupe resursa, ne prikazuje u dijagramu. 
    - **ONPREMRG**. Sadrži sve resurse potrebne za zamjenu za lokalnu mrežu.
    - **AZURERG**. Sadrži sve resurse potrebne za Azure virtualne mrežnom okruženju. 
- VNet koji se naziva **onpremvnet** koristi oponašanje lokalnog podatkovnog centra segmentirajući navedene u nastavku.
    - **onpremsn1**. Podmreže koja sadrži virtualnog računala (VM) radi Ubuntu oponašanje lokalnog poslužitelja.
    - **onpremsn2**. Podmreže koja sadrži VM pokrenut Ubuntu oponašanje računalu na lokalni administrator.
- Postoji jedan vatrozid virtualne potražite pod nazivom **OPFW** na **onpremvnet** koristi da bi se zadržao tunelom za **azurevnet**.
- VNet pod nazivom **azurevnet** segmentirajući navedene u nastavku.
    - **azsn1**. Vanjski vatrozid podmreže koji se koriste isključivo za vanjski vatrozid. Sve internetski promet cirkularnim će ovaj podmreže. U ovom podmreže sadrži samo NIC povezane s vanjski vatrozid.
    - **azsn2**. Sučelje podmreže hosting VM izvodi kao web-poslužitelju koji će se pristupati putem Interneta.
    - **azsn3**. Podmreže pozadinskog hosting VM radi pozadinskog poslužitelja za aplikaciju koja će se pristupati putem web-poslužitelju sučelja.
    - **azsn4**. Upravljanje podmreže koristi isključivo za upravljanje pristup sve aparata virtualne vatrozid. U ovom podmreže sadrži samo NIC za svaki vatrozida potražite virtualne koristi u rješenje.
    - **GatewaySubnet**. Podmreže veze Azure hibridnog potrebne za ExpressRoute i VPN pristupnika možete unijeti veze između Azure VNets i drugim mrežama. 
- Postoje 3 aparata virtualne vatrozid u mreži **azurevnet** . 
    - **AZF1**. Vanjski vatrozid izložen javnog interneta pomoću javnog resursa IP adresa u Azure. Potrebno provjeriti imate predloška iz trgovine ili izravno od dobavljača uređaj, taj dodjeljuje resurse 3 NIC uređaj za virtualne.
    - **AZF2**. Vatrozid za interne određuje promet između **azsn2** i **azsn3**. To je 3 NIC uređaj za virtualne.
    - **AZF3**. Upravljanje vatrozid dostupni administratorima iz podatkovnog centra za lokalni i povezani s podmreži upravljanja koji se koriste za upravljanje sve aparata vatrozid. Traženje 2 NIC predložaka virtualne potražite na tržištu ili ga zatražiti od izravno proizvođaču tog softvera potražite.

## <a name="user-defined-routing-udr"></a>Korisnički definirane usmjeravanje (UDR)

Svaki podmreže u Azure moguće je povezati s UDR tablicu koja se koristi da biste odredili kako promet pokrenuto u tom podmreže usmjeravanja. Ako nema UDRs definiraju, Azure koristi zadani usmjerava da dopušta promet da teče iz jednog podmreže u drugu. Da biste bolje razumjeli UDRs, posjetite [što su korisnički definirana usmjerava i IP prosljeđivanje](./virtual-networks-udr-overview.md#ip-forwarding).

Da biste bili sigurni komunikacije obavlja putem potražite desni vatrozid, obavezne posljednje gore, na temelju potrebnih za stvaranje usmjeravanje u sljedećoj tablici koja sadrži UDRs u **azurevnet**.

### <a name="azgwudr"></a>azgwudr

U ovom scenariju samo promet slijedi lokalnim za Azure će se koristiti za upravljanje u vatrozidima povezivanjem **AZF3**, a taj promet mora proći kroz vatrozid za interne, **AZF2**. Stoga samo jedan usmjeravanje nije potrebno u **GatewaySubnet** kao što je prikazano u nastavku.

|Odredište|Sljedeći put|Objašnjenje|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Dopušta promet na lokalni dosegne Vatrozid za upravljanje **AZF3**|

### <a name="azsn2udr"></a>azsn2udr

|Odredište|Sljedeći put|Objašnjenje|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Dopušta promet na podmreži pozadinskog hosting poslužitelj aplikacije putem **AZF2**|
|0.0.0.0/0|10.0.2.10|Omogućuje sve promet da biste se usmjerena kroz **AZF1**|

### <a name="azsn3udr"></a>azsn3udr

|Odredište|Sljedeći put|Objašnjenje|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Dopušta promet na **azsn2** da teče iz aplikacije server webserver kroz **AZF2**|

Morate stvaranje tablice usmjeravanja za na podmreže u **onpremvnet** oponašanje podatkovnog centra za lokalni.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Odredište|Sljedeći put|Objašnjenje|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Dopušta promet na **onpremsn2** kroz **OPFW**|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Odredište|Sljedeći put|Objašnjenje|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Dopušta promet na sigurnosnu kopiju podmreži u Azure kroz **OPFW**|
|192.168.1.0/24|192.168.2.4|Dopušta promet na **onpremsn1** kroz **OPFW**|

## <a name="ip-forwarding"></a>Prosljeđivanje IP 

UDR i prosljeđivanje IP su značajke koje možete koristiti u kombinaciji dopustili virtualne aparata koja će se koristiti za kontrolu tijek prometa u programa Azure VNet.  Virtualna potražite je ništa više VM koja se pokreće aplikacija korištena za rukovanje mrežni promet na mobitel, kao što su vatrozid ili NAT uređaja.

Ovaj virtualni potražite VM moraju imati mogućnost primanja dolazne prometa čije je adresirana sa sobom. Da biste omogućili VM prima promet adresirana drugih odredišta, morate omogućiti IP prosljeđivanje za na VM. Ovo je programa Azure postavka ne postavku u operacijskom sustavu za goste. Vaš uređaj virtualne i dalje treba pokrenuti neke vrste aplikacija obrađuje dolazne promet pa je komponenta usmjeravanje.

Dodatne informacije o prosljeđivanju IP, posjetite [što su korisnički definirana usmjerava i IP prosljeđivanje](./virtual-networks-udr-overview.md#ip-forwarding).

Na primjer, zamislite da imate sljedeće postavljanje u Azure vnet:

- Podmreže **onpremsn1** sadrži VM pod nazivom **onpremvm1**.
- Podmreže **onpremsn2** sadrži VM pod nazivom **onpremvm2**.
- Virtualna potražite pod nazivom **OPFW** povezan je s **onpremsn1** i **onpremsn2**.
- Korisnički definirane usmjeravanje povezane s **onpremsn1** navodi sve promet na **onpremsn2** mora biti poslana na **OPFW**.

U ovom trenutku **onpremvm1** pokušava uspostaviti vezu s **onpremvm2**, koristit će se u UDR i promet poslat će se **OPFW** kao sljedeći put. Imajte na umu da stvarni paketa odredište je u tijeku promijeniti i dalje dobivate poruku **onpremvm2** je odredište. 

Bez IP prosljeđivanje omogućen za **OPFW**Azure virtualne umrežavanje logike će ispustite pakete, budući da samo omogućuje pakete slati na VM ako je IP adresa u VM odredišta za paket.

S prosljeđivanje IP logike Azure virtualne mreže će proslijediti pakete OPFW, bez promjene izvorne odredišnu adresu. **OPFW** morate učiniti za pakete i određuju što možete raditi s njima.

Za scenarij iznad da biste radili, morate omogućiti IP prosljeđivanja na na NIC-ovi za **OPFW**, **AZF1**, **AZF2**i **AZF3** koji se koriste za usmjeravanje (sve NIC-ovi osim onih koji je povezan s podmreže Upravljanje). 

## <a name="firewall-rules"></a>Pravila vatrozida

Kao što je opisano iznad IP prosljeđivanje samo osigurava pakete bit će poslani virtualne aparata. Vaš uređaj i dalje mora odlučiti što učiniti s tim pakete. U scenariju iznad, morat ćete u vašem aparata stvorite sljedeća pravila:

### <a name="opfw"></a>OPFW

OPFW predstavlja lokalnog uređaju sa sustavom koji sadrži sljedeća pravila:

- **Usmjeravanje**: sav promet 10.0.0.0/16 (**azurevnet**) moraju se poslati putem tunelom **ONPREMAZURE**.
- **Pravila**: dopušta promet za sve Dvosmjeran između **port2** i **ONPREMAZURE**.
 
### <a name="azf1"></a>AZF1

AZF1 predstavlja Azure virtualne uređaj koji sadrži sljedeća pravila:

- **Pravila**: dopušta promet za sve Dvosmjeran između **port1** i **port2**.

### <a name="azf2"></a>AZF2

AZF2 predstavlja Azure virtualne uređaj koji sadrži sljedeća pravila:

- **Usmjeravanje**: sav promet 10.0.0.0/16 (**onpremvnet**) mora biti poslana s IP adresom Azure pristupnika (odnosno 10.0.0.1) do **port1**.
- **Pravila**: dopušta promet za sve Dvosmjeran između **port1** i **port2**.

## <a name="network-security-groups-nsgs"></a>Mrežni sigurnosnih grupa (NSGs)

U ovom scenariju NSGs se ne koriste. Međutim, nije moguće primijeniti NSGs u svakom podmreži da biste ograničili dolazne i odlazne promet. Na primjer, možete primijeniti sljedeća pravila NSG u vanjskim podmreži FW.

**Dolazni**

- Dopusti sve TCP promet s Interneta priključak 80 na bilo kojem VM u podmreži.
- Nemoj dopustiti ostali promet s Interneta.

**Izlazni**
- Nemoj dopustiti sav promet s Internetom.

## <a name="high-level-steps"></a>Visoke razine korake

Da biste implementirali ovom scenariju, više razine sljedećih koraka.

1.  Prijavite se u pretplatu za Azure.
2.  Ako želite implementirati VNet oponašanje lokalne mreže Dodjela resursa koje su dio **ONPREMRG**.
3.  Dodjela resursa koje su dio **AZURERG**.
4.  Dodjela tunelom iz **onpremvnet** za **azurevnet**.
5.  Kada su svi resursi dodjeli, prijavite se na **onpremvm2** i pomoću naredbe ping 10.0.3.101 da biste testirali veze između **onpremsn2** i **azsn3**.
