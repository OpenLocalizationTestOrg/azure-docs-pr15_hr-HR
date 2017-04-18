<properties
   pageTitle="Primjer DMZ – sastavljanje DMZ da biste zaštitili mrežama s vatrozid, UDR i NSG | Microsoft Azure"
   description="Sastavljanje DMZ s vatrozidom, korisnički definirane usmjeravanje (UDR) i mreže sigurnosne grupe (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Primjer 3 – sastavljanje DMZ da biste zaštitili mrežama s vatrozid, UDR i NSG

[Vratite se na stranicu najbolje prakse sigurnost granice][HOME]

U ovom se primjeru će stvoriti na DMZ s vatrozid, četiri poslužiteljima sustava windows, korisnički definirana usmjeravanja, prosljeđivanje IP i mreže sigurnosne grupe. Također će voditi kroz svaki odgovarajući naredbi za dublju razumijevanja svakog koraka. Postoji i promet scenarij sekcije možete unijeti na detaljnije detaljne kako promet nastavlja se kroz slojeve obrane na DMZ. Na kraju, referencama na odjeljak je dovršena Šifra i upute za izradu ovo okruženje za testiranje i isprobajte ih s različitim scenarijima. 

![Dvosmjerni DMZ s NVA, NSG i UDR][1]

## <a name="environment-setup"></a>Postavljanje okruženja
U ovom primjeru prikazuje se na pretplatu koja sadrži sljedeće:

- Tri servise u oblaku: "SecSvc001", "FrontEnd001" i "BackEnd001"
- Virtualne mreže "CorpNetwork" s tri podmreže: "SecNet", "Sučelju" i "Pozadinskog"
- Mrežni virtualne uređaj, u ovom primjeru vatrozid, povezani podmreže SecNet
- Windows Server koja predstavlja aplikacije web-poslužitelj ("IIS01")
- Dva prozora poslužitelje koji predstavljaju aplikacije natrag završili poslužitelje ("AppVM01", "AppVM02")
- Windows server koja predstavlja DNS poslužitelja ("DNS01")

U odjeljku reference ispod postoji skriptu PowerShell koje će izraditi većinu okruženje prethodno opisan. Gradnje VMs i virtualne mreže iako obavljaju skripte primjer ne opisane su detaljno u ovom dokumentu.

Da biste sastavili okruženje:

  1.    Spremanje mreže config xml datoteku u odjeljku reference (ažurirati imena, mjesto i IP adrese tako da odgovaraju određenom scenarij)
  2.    Ažurirajte korisničke varijable u skripti tako da odgovara okruženje skripta će se pokrenuti (pretplate, nazivi servisa itd.)
  3.    Izvršavanje skriptu u ljusci PowerShell

**Napomena**: regija naznačeno u skriptu PowerShell mora odgovarati regija naznačeno u xml datoteci konfiguracije mreže.

Nakon pokretanja skripte uspješno potrebni sljedeći koraci nakon skripte:

1.  Postavljanje pravila vatrozida, to opisana su u odjeljku u nastavku pod naslovom: opis pravila vatrozida.
2.  Po želji u odjeljku reference su dvije skripte da biste postavili web-poslužitelj i poslužitelj aplikacije s jednostavne web-aplikacije da biste omogućili testiranjem tu konfiguraciju DMZ.

Nakon pokretanja skripte uspješno vatrozid pravila potrebno dovršiti, to opisana su u odjeljku pod naslovom: pravila vatrozida.

## <a name="user-defined-routing-udr"></a>Korisnički definirane usmjeravanje (UDR)
Prema zadanim postavkama sljedeće usmjerava sustava definiraju se kao:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Na VNETLocal uvijek je definirani adresu prefix(es) od VNet za tu određenu mrežu (ie ga promijenit će se iz VNet za VNet ovisno o tome kako se svaki određene VNet definiran). Preostali usmjerava sustava statični su i zadane kao gore.

Kao prioriteta, usmjerava obrađuju putem metodu najdulje prefiks Match (LPM), stoga najčešće određene usmjeravanje u tablici primjenjivati na navedeni odredišnu adresu.

Stoga promet (na primjer s poslužiteljem DNS01 10.0.2.4) za lokalne mreže (10.0.0.0/16) želite usmjeriti preko VNet na odredište zbog usmjeravanje 10.0.0.0/16. Drugim riječima, 10.0.2.4, usmjeravanje 10.0.0.0/16 je najčešće određene usmjeravanje iako 10.0.0.0/8 i 0.0.0.0/0 nije moguće primijeniti, no Budući da su manje određene ne utječu tog prometa. Stoga promet na 10.0.2.4 želite imati sljedeći put od lokalnog VNet i jednostavno usmjeriti na odredište.

Ako promet je namijenjen 10.1.1.1, primjerice usmjeravanje 10.0.0.0/16 ne primijenite, ali u 10.0.0.0/8 bio najčešće određene, a to promet bi prekine ("crno holed") jer se sljedeći put je Null. 

Ako niste odredište primijeniti na bilo koju prefiksi Null ili prefiksi VNETLocal, a zatim bi slijedite najmanje određene usmjeravanje 0.0.0.0/0 i moguće usmjeriti s Internetom kao sljedeći put a time i izgleda rub Azure na internet.

Ako postoje dvije identične prefiksi u tablici usmjeravanje, slijedi redoslijed preferenca "izvorišnog" atributa usmjerava na temelju:

1.  "VirtualAppliance" = rutu korisnički definirano ručno dodati u tablicu
2.  "VPNGateway" = dinamičkih usmjeravanje (BGP kada se koristi s hibridnog mreža), dodao dinamički mrežni protokol, te usmjerava mijenjaju tijekom vremena kao protokol za dinamičku automatski prihvaća promjene u peered mreži
3.  "Zadano" = usmjerava sustav, lokalni VNet i statične stavke kao što je prikazano u gornjoj tablici usmjeravanje.

>[AZURE.NOTE] Korisnički definirane usmjeravanje (UDR) sada možete koristiti s ExpressRoute i pristupnika VPN-a da biste nametnuli izlazni i ulazni unakrsno lokaciji promet usmjeriti potražite virtualne mreže (NVA).

#### <a name="creating-the-local-routes"></a>Stvaranje lokalne usmjerava

U ovom primjeru dvije tablice usmjeravanja je potrebno, jedan svaki za podmreže sučelju i pozadinskog. Statički smjerovi prikladna za dani podmreže umetnut svaku tablicu. U ovom se primjeru u svrhu svaku tablicu sadrži tri usmjerava:

1. Lokalni podmreže promet s bez sljedeći parametra definiraju tako da dopušta promet lokalnoj podmreži da biste zaobišli vatrozid
2. Virtualna mrežnog prometa s na sljedeći parametra definiran kao vatrozid, to će zaobići zadana pravila koja omogućuje lokalni VNet promet za usmjeravanje izravno
3. Sve preostale promet (0 na 0) s na sljedeći parametra definiran kao vatrozid

Kada se stvaraju tablice usmjeravanja oni su povezane s njihovim podmreže. Za podmreže sučelju tablicu usmjeravanja, kad stvorili i vezana uz podmreži trebao bi izgledati ovako:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Na primjer, sljedeće naredbe koriste se za Sastavi usmjeravanje, dodavanje korisnički definirane usmjeravanje i povezivanje tablice usmjeravanje podmreži (bilješke; sve stavke ispod počevši od znak za dolar (npr.: $BESubnet) korisnički definirane varijable iz skripte u odjeljku referenca ovog dokumenta):

1.  Najprije osnovnu tablicu usmjeravanja moraju se stvoriti. U ovom isječak prikazuje stvaranja tablice za podmreže pozadinskog. U skripti odgovarajuće tablice također se stvara za podmreže sučelju.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Nakon stvaranja tablice usmjeravanje određeni korisnički definirane usmjerava mogu dodati. U ovom korištenog sav promet (0.0.0.0/0) će usmjerena kroz virtualne potražite (varijable $VMIP [0] se koristi za prosljeđivanje IP adresa dodijeljena prilikom stvaranja virtualne potražite u prethodnom skripti). U skripti odgovarajuće pravilo stvara se i u tablici u sučelju.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. Iznad stavke usmjeravanje nadjačat će zadani "0.0.0.0/0" smjer, ali i dalje postojeći zadano pravilo 10.0.0.0/16 koji želite da dopušta promet unutar VNet za usmjeravanje izravno na odredište, a ne potražite virtualne mreže. Da biste odgovarajuće takvo ponašanje pravila prati mora biti dodan.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. Sada je odabir izvršiti. S iznad dva usmjerava sav promet će usmjeravanje u vatrozidu za procjenu, čak i promet unutar jedne podmreže. To možda Poželjni, no da dopušta promet unutar podmreže za usmjeravanje lokalno bez involvement iz vatrozid nekog drugog, možete dodati određene pravilo. U ovom usmjeravanje stanja koja destine bilo koje adrese za lokalne podmreže možete samo usmjeravanje postoji izravno (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Na kraju, usmjeravanje tablica stvara i popunjen korisnički definirane usmjerava, tablici mora sada se spojiti podmreži. U skripti na sučelje usmjeravanje tablica i povezana podmreže sučelju. Evo povezivanje skripte za podmreže pozadinskih.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>Prosljeđivanje IP
Značajka suradnika na UDR, je IP prosljeđivanje. To je postavka na virtualne uređaj koji omogućuje primanje promet nisu posebno adresirane na uređaj i prosljeđivanje tog promet na odredište ultimate.

Na primjer, ako promet s AppVM01 čini zahtjeva za poslužitelj DNS01 UDR bi usmjeravanje to u vatrozid. Uz omogućeno prosljeđivanje IP promet za odredište DNS01 (10.0.2.4) bit će prihvatio potražite (10.0.0.4) i zatim proslijediti odredište ultimate (10.0.2.4). Bez IP prosljeđivanje omogućena na vatrozid, promet ne prihvaća se tako da na uređaj iako usmjeravanje tablica ima vatrozid kao sljedeći put. 

>[AZURE.IMPORTANT] Ključne je važnosti da Imajte na umu da biste omogućili IP prosljeđivanja u kombinaciji s korisnički definirana usmjeravanja.

Postavljanje prosljeđivanja IP je jednu naredbu i vrijeme stvaranja VM može ostvariti. Za tijek u ovom se primjeru koda je pri kraju skripte i grupirani s naredbama UDR:

1.  Poziva instancu VM koja je u tom slučaju virtualne uređaj, vatrozida i Omogući prosljeđivanje IP (bilješke; bilo koju stavku u crveno počevši od znak za dolar (npr.: $VMName[0]) je korisnički definirane varijable iz skripte u odjeljku referenca ovog dokumenta. Nula u uglatim zagradama, [0] predstavlja prvi VM u polju VMs, na primjer skripte raditi bez izmjene, prvi VM (VM 0), morate biti vatrozida):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Mrežni sigurnosnih grupa (NSG)
U ovom primjeru NSG grupa je ugrađena i zatim učitati pomoću jedne pravila. Ovu grupu je povezana samo s podmreže sučelju i pozadinskog (ne SecNet). Deklarativno u tijeku je ugrađena sljedeća pravila:

1.  Bilo koji promet (svi priključci) s Interneta na cijelu VNet (sve podmreže) je zabranjen

Iako u ovom se primjeru koriste NSGs, njegova Glavna svrha je kao sekundarni sloj obrane ručno konfiguraciji. Želimo da biste blokirali sve dolazni promet s Interneta da biste u sučelju ili pozadinskog podmreže, promet samo treba tijeka kroz SecNet podmreži omogućili vatrozida (i zatim if odgovarajuće na sučelju ili pozadinskog podmreže). Plus, s pravilima UDR na mjestu, sve promet koji nije pretvorite u sučelju ili pozadinskog podmreže će biti preusmjereni izvan vatrozid (uz pomoć UDR). Vatrozid vidjeti to kao asimetričnim tijek i želite ispustite odlazni promet. Stoga postoje tri slojeva zaštite zaštita podmreže sučelju i pozadinskog; 1) otvaranje krajnje točke na FrontEnd001 i BackEnd001 cloud servise, 2) NSGs odbijanju promet s Interneta, 3) vatrozid ispuštanje asimetričnim promet.

Jednu točku zanimljive o mreži sigurnosne grupe u ovom primjeru je da sadrži samo jedno pravilo prikazano u nastavku, koji je odbiti internetski promet da biste cijelu virtualne mreže koji sadrži podmreže sigurnost. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Međutim, budući da u NSG samo vezana uz podmreže sučelju i pozadinskog, pravilo nije obrađen na promet ulaznog u podmreži sigurnost. Kao rezultat, čak i ako pravilo NSG vas obavještava da ne internetski promet na bilo koje adrese na VNet, jer se NSG nikad povezana podmreže sigurnost, promet će teče podmreže sigurnost.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Pravila vatrozida
Na vatrozid, pravila za prosljeđivanje morat ćete stvoriti. Budući da je vatrozid blokira ili prosljeđivanja sve ulaz, Izlaz i intra VNet promet potrebni su mnoge pravila vatrozida. Osim toga, sav dolazni promet će kliknite sigurnost javnu IP adresu (na različitim priključci), obraditi vatrozida. Preporučenim načinom rada je dijagram logičke tokova prije no što postavite na podmreže i pravila vatrozida da biste izbjegli dorada radi korištenja kasnije. Na slici u nastavku je logički prikaz pravila vatrozida u ovom primjeru:
 
![Logička prikaz pravila vatrozida][2]

>[AZURE.NOTE] Na temelju potražite virtualne mreže koristi priključke za upravljanje razlikovat će se. U ovom primjeru poziva vatrozid NextGen Barracuda koji koristi priključke 22, 801 i 807. Potražite u dokumentaciji dobavljača uređaj da biste pronašli točno priključke za upravljanje uređaja koji koristi.

### <a name="logical-rule-description"></a>Opis logičke pravila
U logičke dijagrama gore, podmreže sigurnost nije prikazan jer vatrozid je samo resursa na tom podmreži i ovaj dijagram koji prikazuje pravila vatrozida i kako ih logično dopustiti ili zabraniti promet tokova, a ne stvarne usmjerena put. Vanjski priključke odabrali za RDP promet koji su na višem ranged priključke (8014 – 8026) i odabranima Pomalo poravnali s zadnje dvije okteta od lokalna IP adresa za lakše čitljivosti (npr. lokalni poslužitelj adresa 10.0.1.4 je povezan s vanjskim priključak 8014), no mogu koristiti sve veći koje nisu sukobljenih priključci.

U ovom primjeru moramo 7 vrste pravila, ove vrste pravila je podrobnije opisan na sljedeći način:

- Vanjski pravila (za dolazni promet):
  1.    Pravila upravljanja Vatrozid: Ovo pravilo za preusmjeravanje aplikacije dopušta promet za prosljeđivanje upravljanje priključaka od potražite virtualne mreže.
  2.    Pravila RDP (za svaki windows server): ta četiri pravila (jedan za svaki poslužitelj) omogućuje upravljanje pojedinačnim poslužitelja putem RDP. To također se objediniti u jedno pravilo ovisno o mogućnostima potražite virtualne mreže koristi.
  3.    Pravila promet aplikacije: Postoje dvije aplikacije promet pravila, prvi za promet web sučelja i druge za pozadinskih promet (npr web-poslužitelj sloju podataka). Konfiguriranje ta pravila će ovise o arhitektura mreže (gdje poslužitelja smještaju) i promet tokova (kojem ga smjeru tokova promet i priključaka koji koriste).
      - Prvo pravilo omogućuje promet stvarni aplikacije dosegne poslužitelj aplikacije. Dok se pravila omogućili za sigurnost, upravljanje, itd., aplikacija su pravila što omogućuje vanjske korisnike ili services da biste pristupili u aplikacijama. U ovom se primjeru postoji jedan web-poslužitelju priključak 80, stoga aplikacije pravila vatrozida za jednu bit ćete preusmjereni dolazni promet za vanjske IP, na web-poslužiteljima interne IP adresu. Sesije preusmjereni promet će biti NAT morali internom poslužitelju.
      - Drugo pravilo promet za aplikaciju je pozadinskih pravilo da biste omogućili web-poslužitelj razgovarati s poslužitelja AppVM01 (ali ne AppVM02) putem bilo koji priključak.
- Interna pravila (promet intra VNet)
  4.    Izlazni Internet pravilo: Ovo pravilo će dopušta promet s mreže za prosljeđivanje odabrane mreže. Tim se pravilom obično je zadano pravilo već na vatrozidu, ali u stanje onemogućenosti. U ovom primjeru želite omogućiti to pravilo.
  5.    Pravilo DNS: Ovo pravilo omogućuje samo DNS (priključak 53) promet za prosljeđivanje na DNS poslužitelj. Za ovaj okruženje Većina promet s u sučelju za pozadinski blokiran, to pravilo posebno omogućuje DNS iz bilo kojeg lokalnoj podmreži.
  6.    Pravilo podmreže podmreže: Ovo pravilo je omogućiti bilo koji poslužitelj na podmreži pozadinskih povezati s bilo kojeg poslužitelja na sučelje podmreži (ali ne obrnuto).
- Fail-Safe pravilo (za promet koji ne zadovoljava nijedna od navedenih):
  7.    Ukinuti sve promet pravilo: To uvijek mora biti konačni pravilo (pomoću prioritet), i kao ako je promet teče ne odgovaraju nijednom prethodnom pravila će ispušteni tim pravilom. To je zadano pravilo te obično je aktiviran bez izmjene obično potrebno.

>[AZURE.TIP] Na drugo pravilo promet za aplikaciju, bilo koji priključak dopuštena za jednostavno ovog primjera u realne scenariju najčešće određeni priključak i rasponi adresa želite koristiti da biste smanjili plošni napada ovog pravila.

<br />

>[AZURE.IMPORTANT] Kada sva pravila za gornje stvaraju, važno je da biste pregledali prioritet svako pravilo da biste bili sigurni promet će dopušten ili zabranjen po želji. U ovom primjeru pravila nalaze se prioritetu. Jednostavno je zaključan iz vatrozida zbog pogrešno naručenih pravila. Barem, provjerite je li upravljanje za vatrozid sam uvijek apsolutne pravilo najveći prioritet.

### <a name="rule-prerequisites"></a>Preduvjeti za pravila
Jedan preduvjeta za virtualnog računala pokrenut vatrozid su javno krajnje točke. Vatrozid za obradu promet za odgovarajući javno krajnje točke mora biti otvoren. Postoje tri vrste promet u ovom primjeru; Promet RDP 1) promet upravljanja kontrolu vatrozid i pravila vatrozida, 2) da biste odredili poslužiteljima sustava windows i promet aplikacije 3). Ovo su tri stupca vrsta promet u gornjem polovicu logičke prikaz pravila vatrozida iznad.

>[AZURE.IMPORTANT] Ključni takeway je zapamtiti **sav** promet prosljeđivala kroz vatrozid. Tako da na udaljenu radnu površinu s poslužiteljem IIS01, čak i ako je u prednji završetka Oblaku i na podmreži sučelja da biste pristupili ovaj poslužitelj smo će morati RDP vatrozidu na priključak 8014, a zatim dopustite Vatrozid za usmjeravanje zahtjev za RDP interno priključak RDP IIS01. Gumb "Povezivanje" Azure portal ne funkcioniraju jer postoji Izravni put RDP za IIS01 (koliko god možete vidjeti na portalu). To znači da sve veze s Interneta bit će potrebno web-mjesto servisa sigurnosti i u okvir za priključak, primjerice secscv001.cloudapp.net:xxxx (referenca gornji dijagram za mapiranje vanjskog priključka i interne IP i priključak).

Krajnje je moguće otvoriti ili u vrijeme stvaranja VM ili objavljivanje Sastavi u primjeru skripte i prikazano u nastavku u ovom isječak koda (bilješke; sve stavke počevši od znak za dolar (npr.: $VMName[$i]) je korisnički definirane varijable iz skripte u odjeljku referenca ovog dokumenta. "$I" u uglatim zagradama, [$i] predstavlja broj polja određene VM u polje VMs):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Iako ne jasno navedene zbog korištenja varijable, ali krajnje točke su **samo** otvoriti na servis u Oblaku sigurnost. Time se omogućuje sav dolazni promet obrađuje (usmjeriti, NAT imali, ispušteno) tako da vatrozid.

Klijent za upravljanje će moraju biti instalirani na Računalu sa sustavom za upravljanje vatrozid i stvaranje konfiguracije potrebne. Da biste vidjeli dokumentaciju iz vatrozida (ili druge NVA) dobavljača upravljanja na uređaju. Ostatak Ova sekcija i u sljedećem odjeljku, stvaranje pravila vatrozida, opisane su Konfiguriranje vatrozida sam putem upravljanja klijent proizvođači (odnosno ne Azure portal ili PowerShell).

Upute za preuzimanje klijenta i povezivanje s Barracuda u ovom primjeru koriste Ovdje možete pronaći: [Barracuda Pokazuju administrator](https://techlib.barracuda.com/NG61/NGAdmin)

Kada se prijavili na vatrozid, ali prije stvaranja pravila vatrozida, dostupne su dvije klase pripremni objekt koji olakšavaju stvaranje pravila jednostavnije; Objekti mreže i usluge.

U ovom primjeru tri imenovani mreže objekta mora biti definirani (jedan za podmreže sučelju i pozadinskog podmreži i objekt mreže za IP adresu DNS poslužitelja). Stvaranje imenovanog mreže; počevši od Barracuda Pokazuju administratorske klijent nadzorne ploče, idite na karticu konfiguracija, u odjeljku Konfiguriranje radu kliknite skupu pravila, zatim kliknite "Mreža" na izborniku vatrozid objekata, a zatim na izborniku Uređivanje mreža kliknite Novo. Mrežni objekt sada moguće stvoriti, dodavanjem naziv i prefiks:

![Stvaranje objekta sučelju mreže][3]
 
Time stvarate imenovani mreža za podmreže sučelju, slično kao objekt za koji treba kreirati podmreži pozadinskog. Sada na podmreže mogu jednostavnije upućivati naziv pravila vatrozida.

Objekta DNS poslužitelj:

![Stvaranje objekta DNS poslužitelja][4]

Jedan referenca IP adresa će se koristi u pravilu DNS dalje u dokumentu.

Drugi pripremni objekti su objekti servisa. To će predstavljati RDP veze priključke za svaki poslužitelj. Budući da je postojećeg objekta servisa RDP vezana uz zadani je priključak RDP, 3389, nove servise mogu se kreirati da dopušta promet s vanjskim priključke (8014 8026). Novi priključke također treba dodati postojeći servis RDP, no radi lakšeg pokazni, mogu se kreirati pojedinačno pravilo za svaki poslužitelj. Da biste stvorili novo pravilo RDP za poslužitelj; počevši od Barracuda Pokazuju administratorske klijent nadzorne ploče, idite na karticu konfiguracija, u odjeljku Konfiguriranje radu kliknite skupu pravila, a zatim kliknite "Usluge" na izborniku vatrozid objekata, pomaknite se prema dolje na popisu servisa i odaberite servis "RDP". Desnom tipkom miša kliknite i odaberite Kopiraj, a zatim desnom tipkom miša kliknite, a zatim Zalijepi. Sada je objekt servisa RDP Copy1 koje se mogu uređivati. Desnom tipkom miša kliknite RDP Copy1, a zatim odaberite Uredi, a zatim Uređivanje servisa objekt prozora pojavit će se i kako je prikazano ovdje:

![Kopiju zadano RDP pravilo][5]

Vrijednosti se može uređivati za predstavljanje RDP servisa za poslužitelj. AppVM01 iznad zadano pravilo RDP moraju se mijenjati za novi servis naziv, opis i vanjskog RDP priključka koji se koriste u dijagramu slika 8 (Napomena: priključke promijenjeni iz zadanog RDP od 3389 vanjskog priključka koji se koristi za određeni poslužitelj, u slučaju AppVM01 vanjskog priključka je 8025) izmijenjene servis je prikazano u nastavku :

![AppVM01 pravila][6]
 
Da biste stvorili RDP servise za preostale poslužitelje; morate mogu ponoviti postupak AppVM02, DNS01 i IIS01. Stvaranje od tih servisa će postati stvaranje pravila jednostavniji i bila u sljedećem odjeljku.

>[AZURE.NOTE] Servis za RDP za vatrozid nije potrebna dva razloga; 1) prvo vatrozid VM je slika Linux temelji tako da SSH će se koristiti u priključak 22 za upravljanje VM umjesto RDP i 2) priključak 22 i dva druge priključke upravljanje dopušteno u prvo pravilo upravljanja opisan u nastavku da biste omogućili radi upravljanja povezivanje.

### <a name="firewall-rules-creation"></a>Stvaranje pravila vatrozida
Postoje tri vrste pravila vatrozida u ovom primjeru koriste, svi oni imaju distinct ikone:

Pravilo za preusmjeravanje aplikacije: ![Preusmjerite ikona aplikacije][7]

Pravilo za NAT odredište: ![Odredište NAT ikona][8]

Pristupni pravilo: ![Prosljeđivanje ikona][9]

Dodatne informacije o ovim pravilima pronaći ćete na web-mjestu Barracuda.

Za stvaranje sljedećih pravila (ili provjerite postojeće zadana pravila), počevši od Barracuda Pokazuju administratorske klijent nadzorne ploče, idite na karticu konfiguracija pa u radu sa servisom konfiguraciji sekcije skupu pravila. Rešetka naziva, "Glavne pravila" prikazat će postojeća pravila aktivne i neaktivne na tom vatrozidu. U gornjem desnom kutu ovu rešetku mali zeleni "+" gumb, kliknite da biste stvorili novo pravilo (Napomena: vatrozida može biti "zaključani" da bi promjene, ako vidite gumb označena "Zaključavanje", a ne da biste stvorili ili uredili pravila, kliknite ovaj gumb da biste "otključali" Postavljanje pravila i Omogući uređivanje). Ako želite urediti postojeće pravilo, odaberite to pravilo, kliknite desnom tipkom miša i odaberite Uređivanje pravila.

Kada pravila su stvorene i/ili izmijenili, oni moraju biti pomiču vatrozid i zatim aktivirati, ako to ne radi promjene pravila ne primjenjuje se. Automatske i aktivacija postupak je opisan u nastavku opise detalje pravila.

Specifičnosti svako pravilo potrebne da biste dovršili ovaj primjer opisani su na sljedeći način:

- **Pravila upravljanja vatrozid**: ovu aplikaciju preusmjeravanje pravilo dopušta promet za prosljeđivanje priključke za upravljanje sustava na mreži virtualne uređaj, u ovom primjeru vatrozid NextGen Barracuda. Upravljanje priključci su 801 807 te po želji 22. Priključci vanjskih i internih isti su (odnosno nema prijevod priključak). To pravilo postavljanje, upravljanje Dokumentima i PRISTUP je zadano pravilo te po zadanom omogućen (u vatrozid NextGen Barracuda verzija 6.1).

    ![Pravila upravljanja vatrozid][10]

>[AZURE.TIP] Prostor adrese izvora u tim se pravilom može biti bilo koja, ako su rasponi IP adresa za upravljanje poznati, Smanjivanje ovaj doseg i smanjivanje napada površina za priključke za upravljanje.

- **Pravila RDP**: ove odredište NAT pravila omogućuje upravljanje pojedinačnim poslužitelja putem RDP.
Postoje četiri ključna polja potrebne za stvaranje ovo pravilo:
  1.    Izvor – da biste omogućili RDP s bilo kojeg mjesta, referencu "Nešto" koristi u polju izvor.
  2.    Servis – pomoću odgovarajuće objekt servisa ste ranije stvorili, u ovom slučaju "AppVM01 RDP", preusmjeravanje priključke za vanjske poslužitelje lokalna IP adresa i priključaka 3386 (zadani RDP priključak). U ovom određenom pravilu je za pristup RDP AppVM01.
  3.    Odredište – mora biti *Lokalni priključak na vatrozidu*, "DCHP 1 lokalna IP" ili eth0 ako pomoću statičke IP-ovi. Redni broj (eth0, eth1 itd.) mogu se razlikovati ako vaš uređaj mreže ima više lokalne sučelja. Ovo je priključak vatrozida slanja odgovor s (može biti jednaki primanju priključak), stvarni usmjerena odredište je u polju ciljni popis.
  4.    Preusmjeravanje – u ovom se odjeljku upućuje virtualne potražite gdje konačni preusmjerite ovaj promet. Najjednostavniji preusmjeravanje tako da postavite IP i priključak (neobavezno) u polju ciljni popis. Ako se koristi priključak nije odredišnim priključkom na ulazni zahtjev će se koristiti (ie bez prijevod), ako priključak je označen priključak će biti NAT bi zajedno s IP adresa.

    ![Pravila vatrozida RDP][11]

    Ukupno četiri RDP pravila će trebati će biti stvoren: 

  	|   Naziv pravila    | Poslužitelj  |   Servis   |  Ciljni popis  |
  	|----------------|---------|-------------|---------------|
  	| RDP IIS01   | IIS01   | IIS01 RDP   | 10.0.1.4:3389 |
  	| RDP DNS01   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP AppVM01 | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP AppVM02 | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] Suziti opseg polja izvora i servisa će smanjiti plošni napada. Većina ograničeni opseg omogućit će funkcionalnost želite koristiti.

- **Aplikacija promet pravila**: postoje dvije aplikacije promet pravila, prvi za promet web sučelje i druge za pozadinskih promet (npr web-poslužitelj sloju podataka). Ta pravila će ovise o arhitektura mreže (gdje poslužitelja smještaju) i promet tokova (kojem ga smjeru tokova promet i priključaka koji koriste).

    Najprije spominju je pravilo sučelje za web promet:

    ![Pravila vatrozida za Web][12]
 
    Ovo pravilo odredište NAT omogućuje promet stvarni aplikacije dosegne poslužitelj aplikacije. Dok se pravila omogućili za sigurnost, upravljanje, itd., aplikacija su pravila što omogućuje vanjske korisnike ili services da biste pristupili u aplikacijama. U ovom se primjeru postoji jedan web-poslužitelju priključak 80, stoga aplikacije pravila vatrozida za jednu bit ćete preusmjereni dolazni promet za vanjske IP, na web-poslužiteljima interne IP adresu.

    **Napomena**: da postoji priključak nije dodijeljeno u polju ciljnog popisa, stoga ulazni priključak 80 (ili 443 za servis koji je odabran) koristit će se u preusmjeravanja web-poslužitelj. Ako web-poslužitelj priključuje na drugi priključak, primjerice priključak 8080, ciljni popis polja ne može se ažurirati 10.0.1.4:8080 da biste omogućili za preusmjeravanje priključak.

    Sljedeći promet pravila za aplikaciju je pozadinskih pravilo da biste omogućili web-poslužitelj razgovarati s poslužitelja AppVM01 (ali ne AppVM02) putem bilo koji servis za:

    ![Vatrozid AppVM01 pravila][13]

    Ovo pravilo lozinka omogućuje bilo koji poslužitelj IIS na podmreži sučelju dođu u AppVM01 (IP adresa 10.0.2.5) na bilo koji priključak pomoću bilo kojeg protokola za pristup podacima potrebno web-aplikacija.

    U ovom snimku zaslona programa "\<eksplicitnih odredišne\>" koristi u odredišno polje da bi se označavaju 10.0.2.5 kao odredište. To može biti nešto od sljedećeg eksplicitnih kao što je prikazano ili predloška pod nazivom objekta mrežom (kao što je učinjeno u preduvjete za DNS poslužitelj). Ovo je najviše administrator vatrozid kao koji će se koristiti metodu. Da biste dodali 10.0.2.5 kao programa Explict Desitnation, dvokliknite prazan redak u odjeljku \<eksplicitnih odredišne\> , a zatim unesite adresu u prozoru koji se pojavljuje.

    Uz to pravilo proći bez NAT potreban je Budući da je to interni promet da bi se način povezivanja možete postaviti na "Bez SNAT".

    **Napomena**: U izvoru mreža u ovo pravilo nije bilo kojeg resursa na podmreži sučelju ako postoji bit će samo jedno ili poznati određeni broj web-poslužiteljima objekt mrežni resurs se može stvoriti veću preciznost te točno IP adrese umjesto cijelog podmreže sučelju.

>[AZURE.TIP] Ovo pravilo "Sve" koristi servis za lakše primjer aplikacije za postavljanje i korištenje, to će omogućiti ICMPv4 (pomoću naredbe ping) u jednom pravila. No to nije preporučljivo. Priključci i protokoli ("Services") treba narrowed najmanje moguće koji omogućuje aplikacije postupak da biste smanjili plošni napada preko ovu granicu.

<br />

>[AZURE.TIP] Iako to pravilo prikazuje referencu eksplicitnih odredišne koristi dosljedan pristup trebali biste koristiti u cijeloj konfiguraciju vatrozida. Preporučuje se da imenovani mrežni objekt može koristiti u cijeloj za lakše čitljivosti i pružanje podrške. Na EKSPLICITNIH-odredišne koristi ovdje samo za prikaz način zamjenski reference i obično ne preporučuje (posebno u slučaju složene konfiguracije).

- **Izlazni Internet pravilo**: ovaj proći pravilo će dopušta promet iz izvora mreže za prosljeđivanje odabrane mreža odredište. Tim se pravilom pravilo zadani obično već u vatrozid Barracuda NextGen, dok je u stanje onemogućenosti. Desnom tipkom miša na to pravilo možete pristupiti naredbu za aktiviranje pravila. Da biste dodali dva lokalne podmreže stvorenih kao reference u odjeljku pripremni ovog dokumenta izvornog atributa ovo pravilo izmijenjena pravilo što je prikazano ovdje.

    ![Izlazni pravila vatrozida][14]

- **Pravilo DNS**: ovaj proći pravila omogućuje samo DNS (priključak 53) promet za prosljeđivanje na DNS poslužitelj. Ovaj je blokirana Većina promet s u sučelju za pozadinski okruženju, to pravilo posebno omogućuje DNS.

    ![Pravila vatrozida DNS-a][15]

    **Napomena**: U ovom zaslonu uključen snimka način povezivanja. Budući da je ovo pravilo za interne IP za interne promet IP adresa, potreban je bez NATing, to način povezivanja je postavljeno na "Bez SNAT" za to pravilo lozinka.

- **Pravilo podmreže podmreže**: ovaj proći pravilo je pravilo zadani aktiviran i izmijeniti da biste omogućili bilo koji poslužitelj na podmreži pozadinskih povezati s bilo kojeg poslužitelja na podmreži sučelje. Pravilo je sve Interna promet tako da se način povezivanja možete postaviti da ne SNAT.

    ![Vatrozid Intra VNet pravila][16]

    **Napomena**: potvrdni okvir dvosmjerni nije prijavljen (niti je potvrđen okvir u većini pravila), to je značajan ovog pravila u koja će se to pravilo "jedan usmjereno", veze može se inicirati iz podmreže pozadinskih sučelje mreže, ali ne obrnuto. Ako je potvrđen taj potvrdni okvir, to pravilo želite omogućiti dvosmjerni prometa koji iz naših logičke dijagrama ne želi.

- **Odbijanje svih pravila promet**: uvijek trebali biste konačni pravilo (pomoću prioritet), a kao ako je promet teče ne uspije da odgovaraju nijednom prethodni pravila će ispušteni tim pravilom. To je zadano pravilo te obično je aktiviran bez izmjene obično potrebno. 

    ![Vatrozid odbiti pravila][17]

>[AZURE.IMPORTANT] Kada sva pravila za gornje stvaraju, važno je da biste pregledali prioritet svako pravilo da biste bili sigurni promet će dopušten ili zabranjen po želji. U ovom primjeru pravila nisu u redoslijedu njihov izgled među ostalim glavne prosljeđivanje pravila u klijent za upravljanje Barracuda.

## <a name="rule-activation"></a>Aktiviranje pravila
S skupu pravila izmijenio specifikacija dijagram logike u skupu pravila mora biti prenijeli vatrozid i zatim aktiviran.

![Aktiviranje pravila vatrozida][18]
 
U gornjem desnom kutu klijent za upravljanje su klaster gumba. Kliknite gumb "Pošalji promjena" da biste poslali izmjene pravila vatrozida, a zatim kliknite gumb "Aktiviraj".
 
U ovom primjeru okruženje Sastavi s aktivacijom skupu pravila vatrozida je dovršena.

## <a name="traffic-scenarios"></a>Scenariji promet
>[AZURE.IMPORTANT] Ključni takeway je zapamtiti **sav** promet prosljeđivala kroz vatrozid. Tako da na udaljenu radnu površinu s poslužiteljem IIS01, čak i ako je u prednji završetka Oblaku i na podmreži sučelja da biste pristupili ovaj poslužitelj smo će morati RDP vatrozidu na priključak 8014, a zatim dopustite Vatrozid za usmjeravanje zahtjev za RDP interno priključak RDP IIS01. Gumb "Povezivanje" Azure portal ne funkcioniraju jer postoji Izravni put RDP za IIS01 (koliko god možete vidjeti na portalu). To znači da sve veze s Interneta bit će potrebno web-mjesto servisa sigurnosti i u okvir za priključak, primjerice secscv001.cloudapp.net:xxxx.

Sljedeća pravila vatrozida za tih scenarija mora biti na mjestu:

1.  Upravljanje vatrozid
2.  RDP za IIS01
3.  RDP za DNS01
4.  RDP za AppVM01
5.  RDP za AppVM02
6.  Aplikacija promet na webu
7.  Aplikacija promet na AppVM01
8.  Izlazni s Internetom
9.  Sučelju za DNS01
10. Promet Intra podmreže (pozadinskih da biste samo sučelje)
11. Ništa nije dopušteno

Skupu pravila vatrozida za stvarni najvjerojatnije će više pravila uz to, pravila na sve navedene vatrozid dodijelit će se brojevi različitih prioritet od one navedene ovdje. Ovaj popis i pridruženi brojeva su omogućuju važnosti između samo ta pravila eleven i relativan prioritet između njih. Drugim riječima; na stvarni vatrozid na "RDP da biste IIS01" može biti broj pravilo 5, ali dok je ispod pravilo "Upravljanje vatrozidom" i iznad pravilo "RDP da biste DNS01" želite poravnati namjeravate ovaj popis. Na popisu će se i pomoć u na ispod scenariji dopuštanja ispušteno da bude kraće; Npr "FW pravilo 9 (DNS)". Za ispušteno da bude kraće, četiri pravila RDP će biti skupno naziva se i, "pravila RDP" kada je nepovezanih za RDP promet scenarij.

I opoziv jesu na mjestu na mreži sigurnosne grupe za dolazni internetski promet na sučelju i pozadinskog podmreže.

#### <a name="allowed-internet-to-web-server"></a>(Dopušteni) Internet web-poslužitelj
1.  Internetska korisničkog zahtjeva za HTTP stranica iz SecSvc001.CloudApp.Net (Internet nasuprotne servis u Oblaku)
2.  Oblak servisa prolaze prometa Otvori krajnjoj točki priključak 80 sučelje vatrozid na 10.0.0.4:80
3.  Nema NSG dodijeljene podmreže sigurnost, stoga pravila NSG sustava dopušta promet na vatrozid
4.  Promet dodirne interne IP adrese vatrozida (10.0.1.4)
5.  Vatrozid započinje obrada pravila:
  1.    FW pravilo 1 (Upravljanje dokumentima FW) ne primijenite, premještanje na sljedeće pravilo
  2.    Pravila FW 2-5 (RDP pravila) ne primijenite, premještanje na sljedeće pravilo
  3.    FW pravilo 6 (aplikacije: Web) primijenili, promet je dopušteno, vatrozid NATs da 10.0.1.4 (IIS01)
6.  Podmreže sučelju započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (Internet bloka) ne odnosi (ovaj promet je NAT način tako vatrozid, stoga adresu izvorne sada je vatrozid koji je na podmreži sigurnost i vidjeti podmreže sučelju NSG biti "lokalni" promet i stoga je dopušteno), premještanje na sljedeće pravilo
  2.    Zadana pravila NSG dopušta promet podmreže podmreže, promet je dopušteno, Zaustavi obradu NSG pravila
7.  IIS01 priključuje za promet web, primi zahtjev i pokreće obrade zahtjeva
8.  Pokušava IIS01 pokreće sesiju FTP da biste AppVM01 na podmreži pozadinskog
9.  Usmjeravanje UDR na sučelju podmreži čini vatrozida sljedeći put
10. Nema izlaznog pravila na sučelju podmreži promet je dopušteno
11. Vatrozid započinje obrada pravila:
  1.    FW pravilo 1 (Upravljanje dokumentima FW) ne primijenite, premještanje na sljedeće pravilo
  2.    Pravilo FW 2-5 (RDP pravila) ne primijenite, premještanje na sljedeće pravilo
  3.    FW pravilo 6 (aplikacije: Web) ne primijenite, premještanje na sljedeće pravilo
  4.    FW pravilo 7 (aplikacije: pozadinskog) primijenili, promet je dopušteno, vatrozid prosljeđuje promet na 10.0.2.5 (AppVM01)
12. Podmreže pozadinskog započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (Internet bloka) ne primijenite, premještanje na sljedeće pravilo
  2.    Zadana pravila NSG dopušta promet podmreže podmreže, promet je dopušteno, Zaustavi obradu NSG pravila
13.  AppVM01 primi zahtjev i pokreće sesiju i odgovori
14. Usmjeravanje UDR na podmreži pozadinskog čini vatrozida sljedeći put
15. Budući da nema izlaznog NSG pravila na podmreži pozadinskog je dopušteno odgovor
16. Prosljeđuje jer je to je vraćanje promet na uobičajene sesiju vatrozida odgovor povratak na web-poslužitelj (IIS01)
17. Podmreže sučelju započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (Internet bloka) ne primijenite, premještanje na sljedeće pravilo
  2.    Zadana pravila NSG dopušta promet podmreže podmreže, promet je dopušteno, Zaustavi obradu NSG pravila
18. Poslužitelj za IIS prima odgovor, dovršetka transakcija s AppVM01 i dovrši sastavljanje HTTP odgovor, odgovor HTTP slati podnositelja zahtjeva
19. Budući da nema izlaznog NSG pravila na podmreži sučelju je dopušteno odgovora
20. HTTP odgovor dodirne vatrozid i jer je odgovor u sesiju razmjene utvrđene NAT je prihvatio vatrozid
21. Vatrozid pa preusmjerava odgovor natrag na Internet korisnika
22. Jer se ne izlaznog NSG pravila ili UDR preskakanja na podmreži sučelju je dopušteno odgovor i Internet korisnik prima web-stranice koji ste tražili.

#### <a name="allowed-internet-rdp-to-backend"></a>(Dopušteni) Internet RDP da biste pozadinskog
1.  Administrator poslužitelja Interneta zahtjeve RDP sesije AppVM01 putem SecSvc001.CloudApp.Net:8025, pri čemu je 8025 broj priključka korisnik dodijeljen pravila vatrozida "RDP da biste AppVM01"
2.  Servis u oblaku prosljeđuje prometa Otvori krajnje točke na priključak 8025 sučelju vatrozida na 10.0.0.4:8025
3.  Nema NSG dodijeljene podmreže sigurnost, stoga pravila NSG sustava dopušta promet na vatrozid
4.  Vatrozid započinje obrada pravila:
  1.    FW pravilo 1 (Upravljanje dokumentima FW) ne primijenite, premještanje na sljedeće pravilo
  2.    FW pravilo 2 (RDP IIS) ne primijenite, premještanje na sljedeće pravilo
  3.    FW pravilo 3 (RDP DNS01) ne primijenite, premještanje na sljedeće pravilo
  4.    Primjena FW pravilo 4 (RDP AppVM01), promet je dopušteno, vatrozid NATs da 10.0.2.5:3386 (RDP priključak na AppVM01)
5.  Podmreže pozadinskog započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (Internet bloka) ne odnosi (ovaj promet je NAT način tako vatrozid, stoga adresu izvorne sada je vatrozid koji je na podmreži sigurnost i vidjeti podmreže pozadinskog NSG biti "lokalni" promet i stoga je dopušteno), premještanje na sljedeće pravilo
  2.    Zadana pravila NSG dopušta promet podmreže podmreže, promet je dopušteno, Zaustavi obradu NSG pravila
6.  AppVM01 priključuje za RDP promet i odgovori
7.  Izlazni NSG pravila, zadana pravila primjenjuju se i povrata promet je dopušteno
8.  UDR usmjerava odlazni promet vatrozid kao sljedeći put
9.  Prosljeđuje jer je to je vraćanje promet na uobičajene sesiju vatrozida odgovor vratite na internet korisnika
10. Sesije RDP omogućena
11. AppVM01 traži korisničko ime lozinka

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Dopušteni) Web-poslužitelj DNS pretraživanje na DNS poslužitelj
1.  Web-poslužitelj, IIS01, potrebama podatkovnog sažetka sadržaja na www.data.gov, ali treba riješiti adresu.
2.  Na mrežnoj konfiguraciji za popise VNet DNS01 (10.0.2.4 na podmreži pozadinskog) kao primarni DNS poslužitelj, IIS01 šalje zahtjev DNS DNS01
3.  UDR usmjerava odlazni promet vatrozid kao sljedeći put
4.  Izlazni NSG pravila su povezane s podmreže Frontend, promet je dopušteno
5.  Vatrozid započinje obrada pravila:
  1.    FW pravilo 1 (Upravljanje dokumentima FW) ne primijenite, premještanje na sljedeće pravilo
  2.    Pravilo FW 2-5 (RDP pravila) ne primijenite, premještanje na sljedeće pravilo
  3.    FW pravila 6 i 7 (aplikacija pravila) ne primijenite, premještanje na sljedeće pravilo
  4.    FW pravilo 8 (da biste Internet) ne primijenite, premještanje na sljedeće pravilo
  5.    Primjena FW pravilo 9 (DNS), promet je dopušteno, vatrozid prosljeđuje promet na 10.0.2.4 (DNS01)
6.  Podmreže pozadinskog započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (Internet bloka) ne primijenite, premještanje na sljedeće pravilo
  2.    Zadana pravila NSG dopušta promet podmreže podmreže, promet je dopušteno, Zaustavi obradu NSG pravila
7.  DNS poslužitelj primi zahtjev
8.  DNS poslužitelj ne sadrži adresu predmemorirani i pita korijenski DNS poslužitelj na Internetu
9.  UDR usmjerava odlazni promet vatrozid kao sljedeći put
10. Izlazni NSG pravila na podmreži pozadinskog, promet je dopušteno
11. Vatrozid započinje obrada pravila:
  1.    FW pravilo 1 (Upravljanje dokumentima FW) ne primijenite, premještanje na sljedeće pravilo
  2.    Pravilo FW 2-5 (RDP pravila) ne primijenite, premještanje na sljedeće pravilo
  3.    FW pravila 6 i 7 (aplikacija pravila) ne primijenite, premještanje na sljedeće pravilo
  4.     Primjena FW pravilo 8 (na Internetu), promet je dopušteno, sesija je SNAT out korijenski DNS poslužitelju na Internetu
12. Internet DNS poslužitelj odgovori, budući da je ovu sesiju pokrenuto iz vatrozid, odgovor je prihvatio vatrozid
13. Kao što je utvrđene sesije, vatrozida prosljeđuje odgovor početne poslužitelj DNS01
14. Podmreže pozadinskog započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (Internet bloka) ne primijenite, premještanje na sljedeće pravilo
  2.    Zadana pravila NSG dopušta promet podmreže podmreže, promet je dopušteno, Zaustavi obradu NSG pravila
15. DNS poslužitelj prima predmemorira odgovor i odgovori početni zahtjev natrag na IIS01
16. Usmjeravanje UDR na podmreži pozadinskog čini vatrozida sljedeći put
17. Izlazni NSG pravila postoje na podmreži pozadinskog, promet je dopušteno
18. Ovo je utvrđene sesije na vatrozidu, odgovor prosljeđuje vatrozidom IIS poslužitelj
19. Podmreže sučelju započinje obrada ulazna pravila:
  1.    Postoji nijedno pravilo NSG koji se primjenjuje na ulazni promet s pozadinskom podmreži u sučelju podmreži tako da ništa na NSG pravila primjenjuju
  2.    Dopuštanje promet između podmreže zadano pravilo sustava omogućuje tog prometa tako da je dopušteno promet
20. IIS01 prima odgovor iz DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Dopušteni) Pozadinskog poslužitelja sučelju poslužitelj
1.  Administrator prijavljeni na AppVM02 putem RDP zahtjeve datoteke izravno s poslužitelja IIS01 putem Eksplorer za datoteke
2.  Usmjeravanje UDR na podmreži pozadinskog čini vatrozida sljedeći put
3.  Budući da nema izlaznog NSG pravila na podmreži pozadinskog je dopušteno odgovor
4.  Vatrozid započinje obrada pravila:
  1.    FW pravilo 1 (Upravljanje dokumentima FW) ne primijenite, premještanje na sljedeće pravilo
  2.    Pravilo FW 2-5 (RDP pravila) ne primijenite, premještanje na sljedeće pravilo
  3.    FW pravila 6 i 7 (aplikacija pravila) ne primijenite, premještanje na sljedeće pravilo
  4.    FW pravilo 8 (da biste Internet) ne primijenite, premještanje na sljedeće pravilo
  5.    FW pravilo 9 (DNS) ne primijenite, premještanje na sljedeće pravilo
  6.    Primjena FW pravilo 10 (Intra podmreži), promet je dopušteno, vatrozid prosljeđuje promet 10.0.1.4 (IIS01)
5.  Podmreže sučelju započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (Internet bloka) ne primijenite, premještanje na sljedeće pravilo
  2.    Zadana pravila NSG dopušta promet podmreže podmreže, promet je dopušteno, Zaustavi obradu NSG pravila
6.  Pod pretpostavkom proper provjere autentičnosti i autorizacije, IIS01 prihvati zahtjev i odgovori
7.  Usmjeravanje UDR na sučelju podmreži čini vatrozida sljedeći put
8.  Budući da nema izlaznog NSG pravila na podmreži sučelju je dopušteno odgovora
9.  Budući da je postojećoj sesiji na vatrozidu je dopušteno odgovor i vatrozida vraća odgovor AppVM02
10. Podmreže pozadinskog započinje obrada ulazna pravila:
  1.    NSG pravilo 1 (Internet bloka) ne primijenite, premještanje na sljedeće pravilo
  2.    Zadana pravila NSG dopušta promet podmreže podmreže, promet je dopušteno, Zaustavi obradu NSG pravila
11. AppVM02 prima odgovor

#### <a name="denied-internet-direct-to-web-server"></a>(Zabranjen) Internet izravno na web-poslužitelj
1.  Internet korisnik pokuša pristupiti web-poslužitelj, IIS01, putem servisa FrontEnd001.CloudApp.Net
2.  Budući da su otvorene za HTTP promet krajnje točke, to bi proći kroz servisa u Oblaku i ne može pristupiti poslužitelju
3.  Ako krajnjih točaka su bile otvorene zbog nekog razloga, NSG (Internet bloka) na podmreži sučelju želite blokirati tog prometa
4.  Na kraju, u sučelju podmreže UDR usmjeravanje želite slati sve odlazni promet iz IIS01 vatrozid kao sljedeći put i vatrozida bi ovo vidi kao asimetričnim promet i ispustite izlaznog odgovora stoga postoje najmanje tri nezavisne slojeve obrane između internetske i IIS01 putem njegova servis u oblaku onemogućuje neovlašteno/neprikladan programa access.

#### <a name="denied-internet-to-backend-server"></a>(Zabranjen) Internet da biste pozadinskog poslužitelja
1.  Internet korisnik pokuša za pristup datoteci na AppVM01 putem usluge BackEnd001.CloudApp.Net
2.  Budući da su otvorene za zajedničko korištenje datoteka krajnje točke, to bi proći servisa u Oblaku i ne može pristupiti poslužitelju
3.  Ako krajnjih točaka su bile otvorene zbog nekog razloga, NSG (Internet bloka) želite blokirati tog prometa
4.  Na kraju, usmjeravanje UDR želite slati sve odlazni promet iz AppVM01 vatrozid kao sljedeći put i vatrozida bi ovo vidi kao asimetričnim promet i ispustite izlaznog odgovor stoga postoje najmanje tri neovisno slojeve obrane između internetske i AppVM01 putem njegova servis u oblaku onemogućuje neovlašteno/neprikladan programa access.

#### <a name="denied-frontend-server-to-backend-server"></a>(Zabranjen) Poslužitelj sučelju pozadinskog poslužitelja
1.  Pretpostavimo IIS01 je ugrožena i sustavom pokušaja skeniranje podmreže poslužitelje pozadinskom kodu.
2.  Smjer UDR podmreže sučelju želite poslati sve odlazni promet iz IIS01 vatrozid kao sljedeći put. Ovo nije nešto što možete izmijeniti tako da compromised VM.
3.  Vatrozid želite obrađivati promet, ako je zahtjev za AppVM01 ili na DNS poslužitelj za DNS pretraživanja koje promet potencijalno imati dopuštenje vatrozidom (zbog FW pravila 7, 9). Svi ostali promet želite blokirati FW pravilo 11 (Uskrati svima).
4.  Ako napredne prijetnju otkrivanje je omogućena na vatrozida (koja nije obuhvaćeno ovaj dokument, potražite u dokumentaciji dobavljača za vaš uređaj navedena mreža naprednih mogućnosti prijetnju), čak i promet koju želite dopustiti pravila osnovni prosljeđivanje koji se spominju u ovom dokumentu moći ako nalazi promet poznati potpisa ili znakova koje označite zastavicom pravilo napredne prijetnju.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Zabranjen) Traženje Internet DNS na DNS poslužitelj
1.  Internet korisnik pokuša potražiti interne DNS zapis na DNS01 putem usluge BackEnd001.CloudApp.Net 
2.  Budući da su otvorene za DNS promet krajnje točke, to bi proći kroz servisa u Oblaku i ne može pristupiti poslužitelju
3.  Ako krajnjih točaka su bile otvorene zbog nekog razloga, pravilo NSG (Internet bloka) na podmreži sučelju želite blokirati tog prometa
4.  Na kraju, smjer UDR podmreže pozadinskog želite slati sve odlazni promet iz DNS01 vatrozid kao sljedeći put i vatrozida bi ovo vidi kao asimetričnim promet i ispustite izlaznog odgovor stoga postoje najmanje tri neovisno slojeve obrane između internetske i DNS01 putem njegova servis u oblaku onemogućuje neovlašteno/neodgovarajuće programa access.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Zabranjen) Internet u access SQL kroz vatrozid
1.  Internet korisnik zatraži SQL podatke iz SecSvc001.CloudApp.Net (Internet nasuprotne servis u Oblaku)
2.  Budući da su otvorene za SQL krajnje točke, to bi proći servisa u Oblaku i ne može doći do vatrozid
3.  Ako SQL krajnje točke su bile otvorene zbog nekog razloga, vatrozid želite započeti obrada pravila:
  1.    FW pravilo 1 (Upravljanje dokumentima FW) ne primijenite, premještanje na sljedeće pravilo
  2.    Pravila FW 2-5 (RDP pravila) ne primijenite, premještanje na sljedeće pravilo
  3.    FW pravilo 6 i 7 (aplikaciju pravila) ne primijenite, premještanje na sljedeće pravilo
  4.    FW pravilo 8 (da biste Internet) ne primijenite, premještanje na sljedeće pravilo
  5.    FW pravilo 9 (DNS) ne primijenite, premještanje na sljedeće pravilo
  6.    FW pravilo 10 (Intra podmreži) ne primijenite, premještanje na sljedeće pravilo
  7.    Primjena pravila 11 FW (Uskrati svima), promet je obrada blokiranih, zaustavite pravila


## <a name="references"></a>Reference
### <a name="main-script-and-network-config"></a>Glavni skripte i konfiguracija mreže
Spremite cijelog skripte u datoteci skriptu PowerShell. Spremite Config mreže u datoteku pod nazivom "NetworkConf2.xml".
Korisnički definirane varijable izmijeniti prema potrebi. Pokrenuti skriptu, a zatim pratite vatrozid pravilo postavljanje upute iznad.

#### <a name="full-script"></a>Potpuna skripte
Ova skripta će, ovisno o korisnički definirane varijable:

1.  Povezivanje s Azure pretplate
2.  Stvaranje novog računa za pohranu
3.  Stvaranje nove VNet i tri podmreže kako je definirano u mreži konfiguracijskoj datoteci
4.  Sastavljanje pet virtualnim strojevima; Vatrozid za 1 i 4 windows server VMs
5.  Konfiguriranje UDR uključujući:
  1.    Stvaranje dva novih usmjeravanje tablica
  2.    Dodavanje smjerova s tablicama
  3.    Povezivanje tablice s odgovarajućim podmreže
6.  Omogućivanje IP prosljeđivanja na na NVA
7.  Konfiguriranje NSG uključujući:
  1.    Stvaranje na NSG
  2.    Dodavanje pravila
  3.    Povezivanje s NSG odgovarajuće podmreže

Ovu skriptu PowerShell mora izvesti na lokalno povezani s Internetom PC-JU ili poslužitelju.

>[AZURE.IMPORTANT] Kada se Ova skripta, možda postoje upozorenja ili drugih pružaju koji pop u ljusci PowerShell. Samo poruke o pogreškama u crvenoj boji su uzrok problem.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"
    
    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #
    
      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
    

#### <a name="network-config-file"></a>Konfiguracijska datoteka u mreži
Spremanje xml datoteke s ažuriranim mjesto, a zatim dodati vezu na tu datoteku da biste varijabla $NetworkConfigFile u skripti iznad.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Ogledne skripte aplikacije
Ako želite instalirati aplikaciju uzorka za ovu i još primjera DMZ nešto je dodijeljen pomoću sljedeće veze: [Skriptu aplikaciju uzorka][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Dvosmjerni DMZ s NVA, NSG i UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logička prikaz pravila vatrozida"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Stvaranje objekta sučelju mreže"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Stvaranje objekta DNS poslužitelja"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopiju zadano RDP pravilo"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 pravila"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Ikona aplikacije preusmjeravanje"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Ikona NAT odredište"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Ikona lozinka"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Pravila upravljanja vatrozid"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Pravila vatrozida RDP"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Pravila vatrozida za Web"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Vatrozid AppVM01 pravila"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Izlazni pravila vatrozida"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Pravila vatrozida DNS-a"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Vatrozid Intra VNet pravila"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Vatrozid odbiti pravila"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Aktiviranje pravila vatrozida"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
