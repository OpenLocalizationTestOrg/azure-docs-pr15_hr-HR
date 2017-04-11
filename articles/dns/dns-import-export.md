<properties
   pageTitle="Uvoz i izvoz u datoteku zone domene za Azure DNS-a pomoću EŽA | Microsoft Azure"
   description="Upute za uvoz i izvoz datoteka zone DNS-a za Azure DNS-a pomoću EŽA Azure"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Uvoz i izvoz u datoteku zone DNS-a pomoću EŽA Azure


U ovom se članku će vas voditi kroz upute za uvoz i izvoz datoteka zone DNS-a za Azure DNS-a pomoću EŽA Azure.

## <a name="introduction-to-dns-zone-migration"></a>Uvod u migraciju zone DNS-a

Datoteka zone DNS-a je tekstna datoteka koja sadrži detalje o svakoj upravljanje pravima na informacije (DNS-Domain Name System) zapis u zoni. Slijedi standardni oblik upućivanje prikladna za prijenos DNS zapise između sustava DNS-a. Datoteka zone je brzi, pouzdanog i praktičan način na koji želite prenijeti DNS zone pojavljivanje ili iščezavanje Azure DNS.

Azure DNS podržava uvoz i izvoz datoteka zone pomoću Azure sučelja naredbenog retka (EŽA). Azure EŽA je više platformi alat naredbenog retka koji se koristi za upravljanje servisa Azure. Nije dostupan za Windows, Mac i Linux platforme [Azure preuzimanja stranice](https://azure.microsoft.com/downloads/). Podrška za različite platforme je osobito važno za uvoz i izvoz datoteka zone jer najčešće naziv poslužitelja softver, [POVEZIVANJE](https://www.isc.org/downloads/bind/), obično se izvršava na Linux.

## <a name="obtain-your-existing-dns-zone-file"></a>Nabavite postojeću datoteku zone DNS-a

Prije uvoza datoteke zone DNS-a u Azure DNS-a, morat ćete dobiti kopiju datoteke zone. Izvor datoteke ovisi o kojoj trenutno hostira DNS zone.

- Ako se DNS zone hostira servis partnera (primjerice registrara domena, namjenski davatelja usluge hostinga DNS ili zamjenski oblaka davatelja), servis mora sadržavati mogućnost da biste preuzeli datoteku zone DNS-a.

- Ako DNS zone nalazi se na Windows DNS, zadana mapa za datoteke zone je **%systemroot%\system32\dns**. Na kartici **Općenito** konzole za upravljanje DNS-a servisa također prikazuje cijeli put do svaku datoteku zone.

- Ako se DNS zone hostira pomoću VEZANJA, mjesto datoteke zone za svaku zonu naveden je u datoteci konfiguracije VEZANJA **named.conf**.

**Rad s datotekama zone sa servisu GoDaddy**<BR>
Zone datoteke preuzete iz GoDaddy s oblikovanjem malo nestandardne. Morate riješiti prije uvoza datoteke zone u Azure DNS-a. DNS naziva u RData svaki DNS zapisa koji su navedeni kao potpuno kvalificiranih naziva, ali ne moraju na prekidanje "." To znači da su protumačiti drugi sustavi DNS relativni nazive. Uredite datoteku zone da biste dodali na prekidanje "." da biste njihova imena prije ih uvezete u Azure DNS.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Datoteka zone DNS-a uvesti Azure DNS-a


Uvoz datoteke zone će stvoriti novu zonu u Azure DNS ako nešto još ne postoji. Ako već postoji u zonu, skupove zapisa u datoteku zone morate spojiti s postojećeg zapisa skupovima.

### <a name="merge-behavior"></a>Spajanje ponašanje

- Po zadanom su spaja postojeće i nove skupove zapisa. Poništi dvostruko su jednake zapisi unutar spojeni skup zapisa.

- Osim toga, navođenjem na `--force` mogućnosti uvoza zamijenit će postojeće skupove zapisa s novi skupovi zapisa. Postojeći zapis skupova koje ne postoji odgovarajući zapis u datoteku uvezene zone će se ukloniti.

- Kada se spajaju skupove zapisa, koristi se vrijeme trajanja (TTL) skupova već postojeća zapisa. Kada `--force` je koristi, koristi se TTL novi skup zapisa.

- Početak parametara za izdavanje certifikata (SOA) (osim `host`) uvijek uzimaju iz datoteke uvezene zone, bez obzira na to želite li `--force` koristi. Isto tako, za naziv poslužitelja zapis postavljeno na vrh zone na TTL uvijek koristi se iz datoteke uvezene zone.

- Uvezene CNAME zapis neće zamijeniti postojeći zapis CNAME s istim nazivom osim ako se `--force` parametar.

- Kada je sukoba između CNAME zapis i drugim zapisom isti naziv, ali druge vrste (bez obzira na to koja je postojeći ili novi), zadržava se postojeći zapis. To ne ovisi o korištenje `--force`.

### <a name="additional-information-about-importing"></a>Dodatne informacije o uvozu

Sljedeće bilješke sadrže dodatne tehničke pojedinosti o zoni postupku uvoza.

- Na `$TTL` uputa nije obavezan, a zatim ga podržava. Kada `$TTL` uputa dobiva, zapisa bez izričitog TTL će biti uvezeni postavljene na zadani TTL vrijednost od 3600 sekundi. Kada dva zapisa u isti skup zapisa odredite različite TTLs, koristi se nižu vrijednost.

- Na `$ORIGIN` uputa nije obavezan, a zatim ga podržava. Kada `$ORIGIN` postavljena, zadana vrijednost koja se koristi se naziv zone kao što je navedeno u naredbenom retku (plus na prekidanje ".").

- Na `$INCLUDE` i `$GENERATE` upute poslužitelja nisu podržane.

- Podržani su ove vrste zapisa: A, AAAA, CNAME, MX, NS, SOA, SRV i TXT.

- Zapis SOA je automatski stvorio Azure DNS stvaranja zone. Kada uvezete datoteku zone, sve parametre SOA uzimaju se iz u zonu datoteke *osim* na `host` parametar. Taj parametar koristi vrijednost nudi Azure DNS-a. To je jer je taj parametar mora se odnositi na primarni nazivni poslužitelj nudi Azure DNS-a.

- Zapis poslužitelja naziva postavljeno na vrh zone i automatski se stvara prema Azure DNS stvaranja zone. Uvozi samo TTL ovaj skup zapisa. Ove zapisi sadrže nazive poslužitelja naziva nudi Azure DNS-a. Bilježenje podataka neće se prebrisati prema vrijednosti koji se nalazi u datoteci uvezene zone.

- Tijekom javno pretpregleda Azure DNS podržava samo jedan niz TXT zapisa. Multistring TXT zapisi će se spajaju i odbacuju se decimale do 255 znakova.

### <a name="cli-format-and-values"></a>Oblikovanje EŽA i vrijednosti.


Oblik s naredbom za Azure EŽA da biste uvezli DNS zone je:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Vrijednosti:

- `<resource group>`je naziv grupe resursa u zonu u Azure DNS-a.
- `<zone name>`je naziv zone.
- `<zone file name>`je put i naziv zone datoteka za uvoz.

Ako u zonu s tim nazivom ne postoji u grupi resursa, stvorit će se za vas. Ako već postoji u zonu, uvezene skupove zapisa će se spojiti s postojećeg zapisa skupovima. Da biste prebrisali postojeće skupove zapisa, koristite na `--force` mogućnost.

Da biste provjerili oblik datoteke zone bez zapravo uvoza, koristite na `--parse-only` mogućnost.

### <a name="step-1-import-a-zone-file"></a>Korak 1. Uvoz datoteke zone

Da biste uvezli datoteku zone za zone **contoso.com**.

1. Prijavite se u pretplatu za Azure pomoću EŽA Azure.

        azure login

2. Odaberite pretplatu u koju želite stvoriti novi DNS zone.

        azure account set <subscription name>

3. Azure DNS je Azure samo za resursima usluga, pa EŽA Azure mora biti prebačena u način Voditelj resursa.

        azure config mode arm

4. Prije korištenja servisa Azure DNS-a, morate se registrirati pretplate da biste koristili davatelja resursa Microsoft.Network. (To je jednokratni postupak za svaku pretplatu.)

        azure provider register Microsoft.Network

5. Ako ne postoji već, morate stvoriti grupu resursa Voditelj resursa.

        azure group create myresourcegroup westeurope

6. Da biste uvezli zone **contoso.com** iz datoteke **contoso.com.txt** u DNS zone u grupu resursa **myresourcegroup**, pokrenite naredbu `azure network dns zone import`.<BR>Ta naredba će učitajte datoteku zone i analizirati ga. Naredba će izvršavanje niza naredbi na servisu Azure DNS zone za stvaranje i svih zapisa postavlja u zoni. Naredba i izvješćivanje o tijeku u prozoru konzole uz sve pogreške i upozorenja. Budući da skupove zapisa stvaraju se u nizu, može potrajati nekoliko minuta da biste uvezli datoteku velike zone.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Korak 2. Provjerite je li u zonu

Da biste potvrdili DNS zone kada uvezete datoteku, možete koristiti bilo koju od sljedećih načina:

- Pomoću sljedeće naredbe Azure EŽA može prikazati zapise.

        azure network dns record-set list myresourcegroup contoso.com

- Sastavite popis zapisa pomoću cmdleta komponente PowerShell `Get-AzureRmDnsRecordSet`.

- Možete koristiti `nslookup` za provjeru razrješenje naziva za zapise. Budući da u zonu još nije dodijeljeno, morat ćete izričito odredite točne Azure DNS poslužitelji naziva. Donji primjer pokazuje kako dohvatiti nazive poslužitelja naziv dodijeljen zoni. IT također prikazuje kako postaviti upit "www" zapis pomoću `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Korak 3. Ažuriranje DNS delegiranje

Nakon što ste potvrdili da zoni uvezena pravilno, morat ćete ažurirati DNS delegiranje da upućuju na poslužitelje naziva Azure DNS-a. Dodatne informacije potražite u članku [Ažuriranje delegiranje DNS-a](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Izvoz u datoteku zone DNS-a iz Azure DNS-a

Oblik s naredbom za Azure EŽA da biste uvezli DNS zone je:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Vrijednosti:

- `<resource group>`je naziv grupe resursa u zonu u Azure DNS-a.
- `<zone name>`je naziv zone.
- `<zone file name>`je put i naziv zone datoteke koju želite izvesti.

Kao pri uvozu zone koje najprije morate prijaviti, odaberite pretplatu i konfiguriranje Azure EŽA za korištenje načina upravljanja resursima.

### <a name="to-export-a-zone-file"></a>Da biste izvezli datoteku zone


1. Prijavite se u pretplatu za Azure pomoću EŽA Azure.

        azure login

2. Odaberite pretplatu u koju želite stvoriti novi DNS zone.

        azure account set <subscription name>

3. Azure DNS je Azure samo za resursima servisa. Azure EŽA mora biti prebačena u način Voditelj resursa.

        azure config mode arm

4. Da biste izvezli u postojeću Azure DNS zone **contoso.com** u grupu resursa **myresourcegroup** datoteke **contoso.com.txt** (iz trenutne mape), pokrenite `azure network dns zone export`. Ta se naredba podići servisa Azure DNS-a da biste numeriranje skupove zapisa u zoni i Izvezi rezultate u datoteku zone VEZANJA kompatibilnog.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

