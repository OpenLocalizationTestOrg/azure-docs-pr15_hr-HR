<properties 
   pageTitle="Otklanjanje poteškoća s mrežom sigurnosnim grupama – Portal | Microsoft Azure"
   description="Saznajte kako otkloniti poteškoće s mrežom sigurnosne grupe u modelu implementaciju upravljanja resursima Azure pomoću portala za Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Otklanjanje poteškoća s mrežom sigurnosne grupe pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal za Azure](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Ako ste konfigurirali mreže sigurnosnih grupa (NSGs) virtualnog računala (VM) i dolazi do problema s povezivanjem VM, ovaj članak sadrži pregled Dijagnostika mogućnosti za NSGs pomoći u rješavanju problema Dodatno.

NSGs omogućuju upravljanje vrstama prometa taj tijek i virtualnim strojevima (VMs). NSGs primjenjuje se na podmreže programa Azure virtualne mreže (VNet), mreže sučelja (NIC) ili oboje. Učinkovita pravila koja se primjenjuju na NIC su prikupljene pravila koja postoji u NSGs primjenjuje na NIC i podmreže je povezano s. Pravila preko tih NSGs ponekad možete međusobno su u sukobu i utjecati na VM mrežne veze.  

Sva pravila učinkovitih sigurnost možete pogledati s NSGs, kao što je primijenjena na vašem VM NIC-ovi. U ovom se članku objašnjava otklanjanje problema s povezivanjem VM pomoću tih pravila u model implementacije Azure Voditelj resursa. Ako niste upoznati s VNet i NSG koncepti, pročitajte u člancima pregled [virtualne mreže](virtual-networks-overview.md) i [mreže sigurnosne grupe](virtual-networks-nsg.md) .

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Pomoću učinkovitih pravila sigurnost da biste otklonili tijek prometa VM

Scenarij u kojem se nalazi iza su primjeri uobičajenih problema s vezom:

VM pod nazivom *VM1* je dio podmreže pod nazivom *Subnet1* unutar VNet pod nazivom *WestUS VNet1*. Pokušaj povezivanja s VM RDP putem TCP priključka 3389 neće uspjeti. NSGs primjenjuju se na NIC *VM1 NIC1* i podmreži *Subnet1*. Promet TCP priključka 3389 je dopušteno NSG pridružene mrežno sučelje *VM1 NIC1*, no pomoću naredbe ping TCP da ne uspije priključak 3389 VM1 korisnika.

Dok je u ovom se primjeru koriste TCP priključak 3389, mogu se sljedeće korake da biste odredili neuspjeha ulazni i izlazni vezu putem bilo kojeg priključka.

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Prikaz učinkovitih sigurnosna pravila za virtualnog računala

Izvršite sljedeće korake za otklanjanje poteškoća s NSGs za na VM:

Potpuni popis pravila učinkovitih sigurnost možete pregledati na NIC, iz VM sam. Možete i dodavanje, izmjena i brisanje NIC i podmreže NSG pravila iz plohu učinkovitih pravila ako imate dozvole za izvođenje te operacije.

1. Prijavite se na portal sustava Azure na https://portal.azure.com.
2. Kliknite **nove servise**, a zatim na popisu koji će se pojaviti kliknite **virtualnim računalima** .
3. Odaberite VM otklanjanje poteškoća s popisa koji će se pojaviti, a pojavljuje se VM plohu s mogućnostima.
4. Kliknite **Dijagnosticiranje & rješavanje problema s** , a zatim odaberite uobičajenih problema. U ovom primjeru **se moguće povezati s moj Windows VM** odabran. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. Koraci se prikazuju u odjeljku problem, kao što je prikazano na sljedećoj slici: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Kliknite *pravila učinkovitih sigurnosne grupe* na popisu preporučena koraka.

6. Plohu **dohvatite učinkovitih sigurnosna pravila** prikazuje se kao što je prikazano na sljedećoj slici:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    Obratite pozornost na to u sljedećim se odjeljcima slike:

    - **Opseg:** Postavljanje *VM1*, na VM odabran u 3.
    - **Sučelje mreže:** Odabrana je *VM1 NIC1* . Na VM može imati više mreža sučelja (NIC). Svaki NIC može sadržavati jedinstvene učinkovitih sigurnosne pravila. Pri otklanjanju, možda ćete morati prikaz učinkovitih sigurnosna pravila za svaku NIC.
    - **Povezane NSGs:** NSGs primjenjuje se na NIC-a i podmreže NIC-a je povezano s. Na slici u NSG zatvoren NIC-a i podmreže je povezano s. Možete kliknuti da biste izravno izmjena pravila u NSGs nazive NSG.
    - **Kartica VM1 nsg:** Na popisu pravila prikazana na slici namijenjen NSG primjenjuje na NIC. Nekoliko zadana pravila stvaraju Azure prilikom svakog stvaranja programa NSG. Nije moguće ukloniti zadana pravila, ali ih možete nadjačati pomoću pravila viši prioritet. Da biste saznali više o zadana pravila, pročitajte u članku [Pregled NSG](virtual-networks-nsg.md#default-rules) .
    - **ODREDIŠNOM stupcu:** Sadrže neke od pravila tekst u stupcu dok drugi imaju prefiksi adresu. Tekst je naziv zadane oznake koje su primijenjene sigurnosna pravila kada je stavka stvorena. Oznake su identifikatori navedene za sustav koji predstavljaju više prefiksi. Odabir pravila s oznakom, kao što su *AllowInternetOutBound*popis prefiksi u plohu **prefiksi adresu** .
    - **Preuzimanje:** Na popisu pravila može biti duljine. .Csv datoteke pravila za izvanmrežne analizu možete preuzeti tako da kliknete **Preuzimanje** i spremanja datoteke.
    - **AllowRDP** Ulazna pravila: Ovo pravilo omogućuje RDP veze na VM.
7. Kliknite karticu **Subnet1 NSG** da biste vidjeli učinkovitih pravila iz NSG primjenjuje se na podmreži, kao što je prikazano na sljedećoj slici: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Obratite pozornost na to pravilo **ulazni** *denyRDP* . Ulazna pravila koja se primjenjuju na podmreži vrednuju se prije pravila koja se primjenjuju na mrežnom sučelju. Budući da uskrati pravilo primjenjuje na podmreži zahtjev za povezivanje s TCP 3389 ne uspije, jer se nikad ne procjene pravila Dopusti pri NIC-a. 

    Pravilo *denyRDP* je razloga zašto se RDP prekida se veza. Uklonite treba riješiti problem.

    >[AZURE.NOTE]Ako VM pridružene NIC-a nije u stanju izvršavanja ili NSGs još nije bio primijenjen na NIC ili podmreži, prikazuju se nijedno pravilo.

8. Da biste uredili NSG pravila, kliknite *Subnet1 NSG* u odjeljku **Povezane NSGs** .
   Otvorit će se plohu **Subnet1 NSG** . Pravila izravno možete urediti tako da kliknete na **dolazni sigurnosna pravila**.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. Nakon uklanjanja *denyRDP* ulazna pravila u **Subnet1 NSG** i dodavanje pravilo *allowRDP* , na popisu učinkovitih pravila izgleda kao na sljedećoj slici:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Provjerite je li TCP priključak 3389 otvorilo otvaranje veze na VM sustava RDP ili pomoću alata PsPing. Saznajte više o PsPing tako da pročitate [PsPing stranici za preuzimanje](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Prikaz efektivnu sigurnosna pravila za mrežno sučelje

Ako vaš tijek prometa VM utječe za određene NIC, možete vidjeti cijeli popis učinkovitih pravila za NIC iz konteksta sučelja mreže na sljedeći način:

1. Prijavite se na portal sustava Azure na https://portal.azure.com.
2. Kliknite **Dodatne servise**, a zatim na popisu koji će se pojaviti kliknite **sučelje mreže** .
3. Odaberite mrežno sučelje. U sljedećoj je slici NIC pod nazivom *VM1 NIC1* je odabrano.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    Obratite pozornost na to je **opseg** postavljena na mrežnom sučelju odabran. Dodatne informacije o prikazati dodatne informacije potražite u članku koraku 6 odjeljka za **Otklanjanje poteškoća s NSGs za na VM** ovog članka.

    >[AZURE.NOTE] Ako je NSG je uklonjena iz mrežnog sučelja, podmreži NSG je i dalje efektivnu na navedeni NIC. U ovom slučaju izlaz želite prikazati samo pravila iz podmreži NSG. Pravila pojavljuju se samo ako je na VM priložena NIC-a.

4. Možete izravno uređivati pravila za NSGs pridružene na NIC i podmreži. Da biste saznali kako, pročitajte korak 8 odjeljka **Prikaz učinkovitih sigurnosna pravila za virtualnog računala** ovog članka.

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>Prikaz učinkovitih sigurnosna pravila za mrežni sigurnosne grupe (NSG)

Prilikom izmjene pravila NSG, preporučujemo vam da biste pregledali utjecaj pravila dodani na određeni VM. Možete vidjeti cijeli popis učinkovitih sigurnosna pravila za sve mrežne kartice navedeni NSG primijenjen, bez potrebe da biste se prebacili kontekstu s određenom plohu NSG. Da biste otklonili učinkovitih pravila unutar programa NSG, slijedite sljedeće korake:

1. Prijavite se na portal sustava Azure na https://portal.azure.com.
2. Kliknite **nove servise**, a zatim na popisu koji će se pojaviti kliknite **mreže sigurnosne grupe** .
3. Odaberite programa NSG. Na sljedećoj slici programa NSG pod nazivom VM1 nsg nije odabrana.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    Obratite pozornost na sljedećim se odjeljcima prethodnu sliku:

    - **Opseg:** Postavite NSG odabran.
    - **Virtualnog računala:** Kada se NSG primjenjuje podmreži primjenjuje se na sve sučelje mreže priložiti sve VMs povezan s podmreži. Popis prikazuje sve VMs ovaj NSG primjenjuje se na. Možete odabrati sve VM s popisa.

    >[AZURE.NOTE] Ako je NSG primjenjuje samo prazan podmreže VMs nije naveden. Ako je NSG primjenjuje se na NIC koji nije povezan s na VM, te NIC-ovi i neće biti navedene. 
    - **Mrežnog sučelja:** Na VM može imati više mreža sučelja. Možete odabrati mrežno sučelje priložiti odabrane VM.
    - **AssociatedNSGs:** U bilo kojem trenutku u NIC može sadržavati do dva učinkovitih NSGs jedne primijenjene NIC-a i druga za podmreži. Iako opsega je odabran kao VM1 nsg ako NIC-a ima učinkovitih podmreže NSG, izlaz prikazivat će se oba NSGs.
4. Možete izravno uređivanje pravila za NSGs vezane uz NIC ili podmreže. Da biste saznali kako, pročitajte korak 8 odjeljka **Prikaz učinkovitih sigurnosna pravila za virtualnog računala** ovog članka.

Dodatne informacije o prikazati dodatne informacije potražite u članku koraku 6 odjeljka **Prikaz učinkovitih sigurnosna pravila za virtualnog računala** ovog članka.

>[AZURE.NOTE] Iako podmreže i NIC može svaki imati samo jedan NSG primjenjuje se na njih, na NSG mogu se povezati više NIC-ovi i više podmreže.

## <a name="considerations"></a>Razmatranja

Pri otklanjanju poteškoća s povezivanjem Imajte na umu sljedeće:

- Zadana pravila NSG će blokirati ulaznog pristup putem Interneta, a zatim samo dopušta VNet dolazni promet. Želite li dopustiti pristup ulaznog s Interneta, po potrebi treba izričito dodati pravila.
- Ako postoje NSG pravila sigurnosti uzrokuje veza s mrežom na VM uvoza, problem može biti krajnji rok za:
    - Vatrozid radi unutar na VM operacijski sustav
    - Usmjerava konfigurirana za virtualne aparata ili promet na lokalni. Internetski promet može biti preusmjereni na lokalni putem prisilno tuneliranje. RDP/SSH veze sustava s Interneta na VM možda neće funkcionirati uz tu postavku, ovisno o način postupanja s lokalnim mrežnog hardvera u ovom promet. Pročitajte članak [Otklanjanje poteškoća usmjerava](virtual-network-routes-troubleshoot-powershell.md) da biste saznali kako utvrditi probleme usmjeravanje koji impeding tijek prometa i na VM. 
- Ako ste peered VNets, po zadanom, VIRTUAL_NETWORK oznaka automatski se proširiti za uključivanje prefiksi peered VNets. Ove prefiksi možete pogledati na popisu **ExpandedAddressPrefix** za otklanjanje poteškoća vezanih uz VNet peering povezivanje. 
- Učinkovite sigurnosna pravila prikazuju se samo ako je riječ o programa NSG vezane uz na VM NIC i ili podmreže. 
- Ako postoje bez NSGs pridružene NIC-a ili podmreži i imate javnu IP adresa dodijeljena vaše VM, svi priključci bit će otvoren za ulazni i izlazni pristup. Ako je na VM javnu IP adresu, primjene NSGs NIC ili podmreže preporučuje se.