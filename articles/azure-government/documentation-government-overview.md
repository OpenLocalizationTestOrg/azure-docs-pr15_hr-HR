<properties
    pageTitle="Azure državne dokumentaciju | Microsoft Azure"
    description="Ovo omogućuje comparision značajki i upute na razvoj aplikacija za državne ustanove Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="08/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-documentation-overview"></a>Pregled dokumentaciju Azure državne ustanove

##  <a name="introduction-to-azure-government-documentation"></a>Uvod u dokumentaciji Azure državne ustanove

Ovo web-mjesto opisuje mogućnosti servisa [Microsoft Azure državne](https://azure.microsoft.com/features/gov/) i sadrži opće smjernice odnosi se na sve kupce. Prije uključivanja posebno regulated podataka u vašoj pretplati državne Azure, upoznajte se s mogućnostima Azure državne i pogledajte timom račun ako imate pitanja.

Pogledajte na [Microsoftovu Azure Vjeruj centar za usklađenost stranicu](http://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx) aktualne informacije na servisima za Azure državne možete pronaći u odjeljku određene accreditations i pravilnicima. Dodatne usluge tvrtke Microsoft može biti dostupna, ali nisu unutar opsega servisa Azure državne prekriveno i su riješili ovaj dokument. Servisa Azure državne možda dopuštaju i koristiti različite dodatni resursi, programe ili servise koje ste dobili od treće strane – ili Microsoft u odjeljku zasebnom uvjete korištenja i izjave o zaštiti privatnosti pravila – koji nisu obuhvaćeni opsega ovog dokumenta. Vi ste odgovorni za pregled uvjeta sve takve "dodatak" ponuda, kao što su trgovine ponuda, da biste bili sigurni da se ne odgovara vašim potrebama o usklađenosti.

Azure državne dostupna entiteti koji upravljaju podatke koje podložni određenih propisa državne i preduvjeti (kao što su NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 i CJIS) koje je potreban korištenje Azure državne radi usklađivanja s pravilnicima. Azure državne kupci podliježe provjere valjanosti kvalificiranosti.

Entiteti s pitanja o ispunjavanja uvjeta za Azure državne trebali proučiti članovima tima za račun.

##  <a name="principles-for-securing-customer-data-in-azure-government"></a>Zaštita korisničkih podataka u Azure državne načela

Azure državne nudi raspon značajke i servise koje možete koristiti da biste sastavili oblaka rješenja potrebama regulated/kontrolirati podataka. Rješenje klijentima usklađene sa se ništa ne više od učinkovitih implementacije mogućnosti Izlaz u-tvorničke Azure državne povezano s vježbe za sigurnost pune podataka.
Kada hostira rješenja u državne Azure, Microsoft upravlja mnoge preduvjeta na razini infrastrukture oblaka.

Sljedeći dijagram prikazuje Azure obrane u dubinske modela. Na primjer, Microsoft pruža infrastrukture u oblaku osnovni DDOS, zajedno s klijenta mogućnosti kao što su aparata sigurnosti za aplikaciju specifične za klijenta mora DDOS.

![Zamjenski tekst](./media/azure-government-Defenseindepth.png)

Ova stranica opisuje foundational načela za osiguravanje servisa i aplikacije, koja omogućuje upute i najbolje prakse za primjenu tih načela; drugim riječima, kako korisnici trebali napraviti pametne korištenje Azure državne da bi odgovarao obaveze i radni zadaci koji su potrebni za rješenje koje obrađuje ITAR podatke.

Overarching načela zaštita korisničkih podataka su:
* Zaštita podataka pomoću šifriranja
* Upravljanje tajne
* Odvajanja da biste ograničili pristup podacima

##  <a name="protecting-customer-data-using-encryption"></a>Zaštita korisničkih podataka pomoću šifriranja

Mitigating rizik i sastanaka pravnih obveza su vožnju rastuće fokusa i važnost šifriranje podataka. Implementacija učinkovitih šifriranje koristiti da biste poboljšali trenutni mreža i aplikacije sigurnosnim mjerama – i smanjenju cjelokupan rizika vaše okruženje oblaka.

### <a name="Overview"></a>Šifriranje na ostale
Šifriranje podataka na ostale primjenjuje zaštitu sadržaja kupca sadrži prostor za pohranu na disku. To se može dogoditi na nekoliko načina:

### <a name="Overview"></a>Šifriranje servis za pohranu

Azure šifriranje servis za pohranu omogućena na račun za razinu pohrane rezultira bloka blob-ova i blob polja stranice koja se automatski šifrirane kada zapisan Azure prostora za pohranu. Prilikom čitanja podataka iz spremišta Azure, ona će se dešifrirati servis za pohranu prije nego što se vraćaju. Ta postavka omogućuje zaštitu podataka bez potrebe za izmjenu ili dodavanje koda za sve programe.

### <a name="Overview"></a>Azure šifriranje
Korištenje Azure Disk šifriranje OS diskova i diskova podatke koristiti tako da je Azure virtualnog računala. Integracija s Azure ključ sigurnog omogućuje kontrolu i olakšava upravljanje ključeva za šifriranje diska.

### <a name="Overview"></a>Klijentsko šifriranje
Šifriranje klijentsko ugrađen u na Java i pohranu .NET klijenta biblioteke koje mogu koristiti Azure ključ sigurnog API-ji, napravite to jednostavne za implementaciju. Da biste dobili pristup tajne u sigurnog Azure ključ za određenim osobama pomoću servisa Azure Active Directory pomoću sigurnog ključ Azure.

### <a name="Overview"></a>Šifriranje na putu

Osnovni šifriranja dostupne za povezivanje za Azure državne podržava protokol prijenosa razinu sigurnosti (TLS) 1.2 i X.509 certifikate. Savezna informacije Processing Standard (FIPS) razine 1 140-2-šifriranja algoritmima pomoću se koriste za infrastrukturu mrežnih veza između podatkovnim centrima državne Azure.  Windows Server 2012 R2 i Windows 8-plus VMs i Azure zajedničke datoteke možete koristiti SMB 3.0 za šifriranje između na VM i zajedničko korištenje datoteka. Korištenje šifriranja klijentsko za šifriranje podataka da bi se prenosi u prostor za pohranu u klijentsku aplikaciju i dešifrirati podatke nakon njega se prenose više mjesta za pohranu.

### <a name="Overview"></a>Najbolje prakse za šifriranje

* IaaS VMs: Korištenje šifriranja Azure Disk. Uključivanje prostora za pohranu servisa šifriranje VHD datoteke koje se koriste za sigurnosno kopiranje tih diskova u Azure prostora za pohranu, ali to samo šifrira upravo pisane podataka. To znači da, ako stvorite na VM, a zatim omogućiti šifriranje servis za pohranu na računu za pohranu koja sadrži datoteku VHD, samo promjene će biti šifrirane, ne izvornu datoteku VHD.
* Šifriranje klijentsko: To je najsigurnija metoda za šifriranje vaše podatke jer je šifrira prije prijenosa, te šifrira podatke na ostale. Međutim, je potreban je dodavanje koda za aplikacije na kojima se koriste za pohranu koji ne želite učiniti. U tom slučaju možete koristiti HTTPs za podatke u prijenosa i šifriranje servis za pohranu za šifriranje podataka na ostale. Šifriranje klijentsko uključuje i više opterećenja na klijentskom računalu – imate račun za to u skalabilnost planove, osobito ako su šifriranje i prijenos velike količine podataka.

Dodatne informacije o mogućnostima šifriranje u Azure potražite u članku [Vodič za sigurnost prostora za pohranu](/storage-security-guide).

##  <a name="protecting-customer-data-by-managing-secrets"></a>Zaštita korisničkih podataka putem upravljanja tajne

Sigurna upravljanja ključem ključan za zaštitu podataka u oblaku. Korisnici trebali biste pokušajte Pojednostavnite upravljanja ključem i kontrolirati tipke koriste oblaka aplikacija i servisa za šifriranje podataka.

### <a name="Overview"></a>Najbolje prakse za upravljanje tajne

* Pomoću tipke sigurnog radi minimiziranja rizika tajne izlaganja programiranih konfiguracijske datoteke, skripte, ili pak u izvornog koda. Azure sigurnog ključ šifrira tipke (kao što su ključeva za šifriranje za šifriranje Azure) i tajne (kao što su lozinke), spremanjem ih u FIPS 140-2 razine 2 provjeriti hardver sigurnost Module (HSMs). Za dodatnu jamstva, moguće je uvesti i generiranje ključeva u te HSMs.
* Kod aplikacije i predloške moraju sadržavati samo URI reference na tajne (što znači stvarni tajne se ne nalaze u kod, konfiguriranje ili izvornog koda spremištima). To sprječava ključa napadima krađe identiteta na unutarnji ili vanjski repos, kao što su harvest robotima u GitHub.
* Koristi funkciju istaknuti RBAC kontrole unutar zbirke ključeva ključ. Ako pouzdanih operator Ostavi tvrtke ili prijenosi u novu grupu unutar tvrtke, trebali biste ih spriječio moći pristupati tajne.  

Dodatne informacije potražite u [Sigurnog ključ za državne ustanove Azure](/azure-government/azure-government-tech-keyvault)

##  <a name="isolation-to-restrict-data-access"></a>Odvajanja da biste ograničili pristup podacima

Odvajanja je pomoću ograničenja, segmente i spremnika za ograničavanje pristupa podacima samo autoriziranih korisnika, servise i aplikacijama. Na primjer, odvojenosti između klijenata je mehanizam za ključne sigurnosti za složene oblaka platforme kao što je Microsoft Azure. Logička odvajanja pomaže u sprečavanju jednog klijenta s ometaju operacije drugi klijent.

### <a name="Overview"></a>Odvajanja okruženje
Okruženje za Azure državne je instanca komponente fizičke koji se razlikuje od ostatka mrežu tvrtke Microsoft. To se postiže prelaziti s jedne fizičke i logičke kontrole koje uključuju sljedeće: zaštita od fizičke barriers pomoću biometrijski uređaji i kamere.  Korištenje određene vjerodajnice i multifactor provjere autentičnosti Microsoftovo osoblje koje je obavezna logičke pristup radnog okruženja.  Sve usluge infrastrukture za Azure državne nalazi se u Sjedinjenim Američkim Državama.

#### <a name="Overview"></a>Odvajanja po klijenta
Kontrola pristupa za Azure primjenjuje mreže i segregation kroz VLAN odvajanja, ACL-a, učitavanje balancers i IP filtara

Korisnici možete dodatno izdvajanja njihove resursi preko pretplate, grupa resursa, virtualne mreže i podmreže.

Dodatne informacije o odvajanja u Microsoft Azure potražite u [odjeljku odvajanja vodič za sigurnost Azure](/azure-security-getting-started/#isolation).

Za dodatne podatke i ažuriranja provjerite pretplatiti na <a href="https://blogs.msdn.microsoft.com/azuregov/">državne Blog o programu Microsoft Azure.</a>
