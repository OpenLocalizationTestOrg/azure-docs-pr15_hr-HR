<properties
    pageTitle="Upravitelj promet - promet usmjeravanje metode | Microsoft Azure"
    description="Članci pomoći će vam razumijevanje načina usmjeravanja različite promet koristi Upravitelj promet"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Metode promet usmjeravanje prometa Manager

Azure promet Upravitelj podržava tri načina promet usmjeravanja da biste saznali kako mrežni promet usmjerili na različite krajnje točke servisa. Upravitelj promet primjenjuje metodu promet usmjeravanje svaki upit DNS primi. Način promet usmjeravanje određuje koje krajnje točke vratiti u odgovoru DNS-a.

Podrška za Azure Voditelj resursa za Upravitelj promet koristi različitu terminologiju od model klasični implementacije. U sljedećoj su tablici prikazane su razlike između Voditelj resursa i klasični uvjete:

| Voditelj resursa termina | Klasični termina |
|-----------------------|--------------|
| Metodu promet usmjeravanja | Način za ujednačavanje opterećenja |
| Način prioritet | Prebacivanje način |
| Ponderirani način | Kružno način |
| Način performansi | Način performansi |

Na temelju povratnih informacija od klijenata, ne možemo promijeniti terminologiju za poboljšanje preglednosti i smanjiti uobičajenih misunderstandings. Nema razlike u funkcijama.

Dostupne u upravitelju promet su tri načina za usmjeravanje prometa:

- **Prioritet:** Odaberite "Prioriteta" kada želite koristiti krajnja točka za primarni servisa za sve promet i navedite sigurnosnih kopija u slučaju da primarni ili sigurnosne kopije krajnje točke nisu dostupne.
- **Težinski faktor:** Odaberite "težinski faktor" kada želite distribuirati promet u skupu rezultata krajnje točke, ili ravnomjerno ili prema debljine koji definiraju.
- **Performanse:** Odaberite "Performanse" kada imate krajnje točke u različitim zemljopisna mjesta, a želite krajnjim korisnicima omogućuje korištenje "najbliže" krajnjoj točki pomoću najniže latenciju mreže.

Svi profili promet Upravitelj obuhvaćaju praćenje stanja krajnjoj točki i prebacivanje automatsko krajnjoj točki. Dodatne informacije potražite u članku [Upravitelj promet krajnjoj točki praćenja](traffic-manager-monitoring.md). Jedan profil promet Manager možete koristiti samo jedan promet metodu usmjeravanja. Možete odabrati način za usmjeravanje različite promet za svoj profil u bilo kojem trenutku. Promjene će se primijeniti unutar jedne minute pa je ne nedostupnost nastali. Promet usmjeravanje metode možete kombinirati pomoću ugniježđene promet upravitelj profila. Ugnježđivanje omogućuje sofisticirane i prilagodljivo promet usmjeravanje konfiguracije koji zadovoljavaju potrebe veće, složene aplikacije. Dodatne informacije potražite u članku [ugniježđene promet upravitelj profila](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Način promet usmjeravanje prioritet

Često tvrtkom ili ustanovom želi omogućavaju pouzdanosti uslugama uvođenjem jedne ili više usluga sigurnosnu kopiju u slučaju da funkcionira njihove primarne servisa. "Prioriteta" promet usmjeravanje način omogućuje Azure korisnici jednostavno implementaciju ovaj uzorak prebacivanje.

![Azure promet Upravitelj 'Prioritet' metodu promet usmjeravanja][1]

Upravitelj promet profila s popisom prioritet krajnje točke servisa. Prema zadanim postavkama, promet Upravitelj šalje sav promet primarni krajnjoj (najveći prioritet). Ako se primarni krajnje točke nije dostupna, promet Upravitelj preusmjerava promet drugi krajnjoj točki. Ako su oba primarnih i sekundarnih krajnjih točaka dostupni, promet odlazak treće i tako dalje. Dostupnost krajnje točke temelji se na konfigurirani status (omogućiti ili onemogućiti) i nadzor tijeku krajnjoj točki.

### <a name="configuring-endpoints"></a>Konfiguriranje krajnje točke

Pomoću upravitelja Azure resursa, morate konfigurirati prioritet krajnjoj točki izričito korištenjem svojstvo "prioritet" za svaku krajnju točku. Ovo svojstvo je vrijednost između 1 i 1000. Niže vrijednosti predstavljaju viši prioritet. Krajnje točke ne možete zajednički koristiti prioritet vrijednosti. Postavite svojstvo nije obavezno. Kada je izostavljen, koristi se zadani prioritet temelji se na redoslijedu krajnjoj točki.

Sa sučeljem klasični prioritet krajnjoj točki konfiguriran implicitno. Prioritet temelji se na redoslijed krajnjih točaka u kojem se prikazuju u definiciji profila.

## <a name="weighted-traffic-routing-method"></a>Ponderirani promet usmjeravanje način

"Ponderiranog promet usmjeravanje način omogućuje vam razmještavanje promet ili koristite unaprijed definirane razine.

![Azure promet Upravitelj "ponderiranog promet usmjeravanje način][2]

U način za ponderiranog promet usmjeravanje dodijeliti debljinu svaki krajnje točke u konfiguraciji promet upravitelj profila. Težina je cijeli broj od 1 do 1000. Taj parametar nije obavezno. Ako se ispusti, promet upravitelji koristi zadani debljine "1".

Za svaki upit DNS primili promet Upravitelj slučajno odabire dostupne krajnjoj točki. Vrijednost vjerojatnosti odabiru krajnje temelji se na debljine dodijelili sve dostupne za krajnje točke. Korištenje iste Debljina preko svih krajnje točke rezultira s raspodjelom prometa čak i. Korištenje više ili niže debljine na određene krajnje točke uzrokuje te krajnje točke se vraćaju više ili manje često u odgovore DNS-a.

Ponderirani metoda omogućuje neke korisne scenarije:

- Nadogradnja postupne aplikacije: dodjelu postotka promet usmjerili na krajnju točku i postupno povećati promet tijekom vremena na 100%.
- Aplikacija migracije na Azure: Stvaranje profila s Azure i vanjske krajnje točke. Prilagodite debljine krajnje točke odabere novi krajnje točke.
- Oblak bursting dodatne kapaciteta: brzo Proširi lokalne implementacije u oblaku postavljanjem iza promet upravitelj profila. Kada vam je potrebna dodatna kapacitet u oblak, možete dodati ili omogućivanje više krajnje točke i odredite koji dio promet vodi do svaki krajnjoj točki.

Na novom portalu Azure podržava konfiguraciju ponderiranog promet usmjeravanja. Na portalu klasični ne može konfigurirati debljine. Možete konfigurirati i debljine pomoću upravitelja resursa i klasične verzije Azure PowerShell, EŽA i REST API-ji.

Je važno je znati DNS odgovore predmemoriraju klijenti i DNS poslužitelji rekurzivne koje klijente koristiti za razrješavanje DNS naziva. U ovom predmemoriranje možete imati utjecaj na distribucija ponderiranog promet. Kada je prevelik broj klijenata i rekurzivna DNS poslužitelji, promet raspodjele radi prema očekivanjima. Međutim, kad je broj klijente ili rekurzivna DNS poslužitelji small, predmemoriranja možete znatno skew raspodjele promet.

Uobičajena slučaja korištenja obuhvaćaju sljedeće:

- Razvoj i testiranja okruženja
- Aplikaciju komunikacije
- Aplikacija usmjerenih pri usko korisnika – osnovu dijelite uobičajenih infrastrukture rekurzivna DNS-a (na primjer, zaposlenici tvrtke povezujete putem proxy poslužitelj)

Efekti predmemoriranja ove DNS su uobičajeni na sve DNS utemeljen promet usmjeravanje sustave, ne samo Azure promet Upravitelj. U nekim slučajevima izričito čišćenje predmemorije DNS pružati zaobilazno rješenje. U drugim slučajevima neke druge metode promet usmjeravanje pogodni više.

## <a name="performance-traffic-routing-method"></a>Način promet usmjeravanje performansi

Implementacija krajnjih točaka u dva ili više mjesta diljem svijeta možete poboljšati odziv mnoge aplikacije usmjeravanje prometa na željeno mjesto 'najbližeg' vam je. Metodu promet usmjeravanje 'Performanse' sadrži tu mogućnost.

![Azure promet Upravitelj "Performanse" metodu promet usmjeravanja][3]

"Najbliže krajnje točke nije nužno najbliže mjeri se geografske udaljenost. Umjesto toga način za promet usmjeravanje 'Performanse' određuje najbliže krajnje točke po mjerenje latenciju mreže. Upravitelj promet održava latencije tablicu programa Internet da biste pratili round-trip vrijeme između rasponi IP adresa i svaki Azure podatkovnog centra.

Upravitelj promet traži IP adresu izvora dolaznih zahtjeva za DNS u tablici Latencija Internet. Upravitelj promet odabire krajnje dostupne u Azure podatkovnom centru koji ima najmanje kašnjenje za tu rasponu IP adresa, a zatim vraća te krajnje točke u odgovoru DNS-a.

Kao što je opisano u [Kako funkcionira Upravitelj promet](traffic-manager-how-traffic-manager-works.md), promet Upravitelj DNS upite ne primaju izravno s klijentima. Umjesto toga DNS upite potjecati iz DNS servis rekurzivna klijente su konfigurirana za korištenje. Stoga se s IP adresom koristiti da biste odredili krajnju točku "najbliže nije IP adresa klijenta, ali je IP adresu DNS servis rekurzivna. U praksi IP adresa je dobro proxy poslužitelja za klijentski program.

Upravitelj promet redovito ažurira tablicu Latencija Internet da bi se promjene u globalni internetske i nova Azure područja. Međutim, performanse aplikacije razlikuje se ovisno o varijacije u stvarnom vremenu u učitavanja putem Interneta. Performanse promet-usmjeravanje praćenje opterećenja na krajnjoj točki servisu. Međutim, ako krajnje postane dostupan, promet Upravitelj ne postoji u DNS upit odgovore.

Upućuje na Imajte na umu:

- Ako vaš profil sadrži više krajnje točke u istom Azure regiji, promet Upravitelj ravnomjerno distribuira promet preko krajnje točke dostupne u tom području. Ako biste radije raspodjele različite promet unutar područja, možete koristiti [ugniježđene promet upravitelj profila](traffic-manager-nested-profiles.md).

- Ako su sve omogućeno krajnje točke u određenom Azure regiji smanjena, promet Upravitelj distribuira promet preko sve druge dostupne krajnje točke umjesto dalje najbliže krajnjoj točki. Ovaj logike onemogućuje pojavljivanja tako da ne preopterećenje dalje najbliže krajnjoj točki oblikovanih pogreške. Ako želite definirati nizu Preferirani prebacivanje koristite [ugniježđene promet upravitelj profila](traffic-manager-nested-profiles.md).

- Prilikom korištenja performanse metodu usmjeravanja promet s vanjskim krajnje točke ili ugniježđene krajnje točke, morate odrediti mjesto te krajnje točke. Odaberite područje Azure najbliže za implementaciju sustava. Tih mjesta su vrijednosti podržava latencije tablicu Internet.

- Algoritam odabire krajnju točku je deterministic. Koji se ponavljaju DNS upite putem klijentskog programa isti su usmjereni na isti krajnje točke. Klijenti obično koriste različite rekurzivne DNS poslužitelji ako ste na putu. Klijent može biti proslijeđene različite krajnjoj točki. Usmjeravanje također mogu utjecati na ažuriranja latencije tablicu Internet. Stoga metodu promet usmjeravanje performanse jamči klijent uvijek usmjeravanja isti krajnjoj.

- Promjenama latencije tablicu Internet, mogli biste primijetiti da neki klijenti su usmjereni na različitim krajnjoj točki. Tu promjenu usmjeravanja točnije je na temelju trenutne Latencija podatke. Ta su ažuriranja ključna da biste zadržali točnosti performanse promet-usmjeravanje kao neprestano razvoja s Internetom.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o razvoju aplikacija visoku dostupnost pomoću [upravitelja promet krajnjoj točki nadzor](traffic-manager-monitoring.md)

Saznajte kako [stvoriti promet upravitelj profila](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png
