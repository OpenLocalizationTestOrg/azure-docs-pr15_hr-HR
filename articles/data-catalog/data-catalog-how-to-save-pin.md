<properties
   pageTitle="Kako spremiti pretraživanja i prikvačiti podataka imovine | Microsoft Azure"
   description="Upute u članku se isticanje mogućnosti Azure u katalogu podataka dodatka za spremanje izvora podataka i podataka resursi za kasnije ponovno korištenje."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>Kako spremiti pretraživanja i prikvačiti podataka resursi

## <a name="introduction"></a>Uvod

Katalog podataka za Microsoft Azure pruža mogućnosti za otkrivanje izvora podataka. Korisnici mogu brzo pretraživanje i filtriranje kataloga da biste pronašli izvore podataka i razumijevanje namijenjene svrhe, što olakšava pronalaženje odgovarajuće podatke za posao konkretnom.

No što je s kad korisnici moraju redovito rad s istim podacima? Je li moguće koristiti kada korisnici redovito doprinos svoje znanje isti izvorima podataka u katalogu? Ovih situacija potrebe za više puta problema isti pretraživanja može biti neučinkovit – to je gdje spremljeno pretraživanje i prikvačene podataka pridonosi resursi.

## <a name="saved-searches"></a>Spremljena pretraživanja

Pretraživanje u katalog podataka Azure je ponovno iskoristiv, definicija za pretraživanje po korisniku. Kada korisnik definirao pretraživanje – uključujući pojmova za pretraživanje, oznake i druge filtre – kojima možete spremiti za kasnije korištenje. Definiciju spremljeno pretraživanje možete pa ponovno pokrenuti kasnije, da biste se vratili imovine sve podatke koji odgovaraju kriterijima pretraživanja.

### <a name="creating-a-saved-search"></a>Stvaranje spremljeno pretraživanje

Da biste stvorili spremljeno pretraživanje, unesite kriterije koji će se može ponovno koristiti. Kliknite vezu "Spremi" u "Trenutno pretraživanje" okvir na portalu za Azure kataloga podataka.

 ![Odaberite "Spremi" da biste spremili postavke trenutnog pretraživanja](./media/data-catalog-how-to-save-pin/01-save-option.png)

Kada se to od vas zatraži, unesite naziv spremljeno pretraživanje. Odaberite ime koje je smislen i opisan imovine podataka koji će se rezultata pretraživanja.

 ![Navedite naziv spremljeno pretraživanje](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Upravljanje spremljena pretraživanja

Kada korisnik ima spremljena jedan ili više pretraživanja, "Spremljena pretraživanja" mogućnost pojavit će se na portalu Azure katalog podataka u odjeljku "Trenutno pretraživanje" okvir. Kada se Proširi, prikazat će se popis svih sprema pretraživanja.

 ![Popis spremljenog pretraživanja](./media/data-catalog-how-to-save-pin/03-list.png)

Na popisu odaberete spremljeno pretraživanje uzrokovat će se pokušati izvršiti.

Odabir padajućeg izbornika vam ponuditi skup mogućnosti upravljanja:

 ![Mogućnosti za upravljanje spremljena pretraživanja](./media/data-catalog-how-to-save-pin/04-managing.png)

Odabir "Preimenovati" tražit će od korisnika da biste unijeli novi naziv za spremljeno pretraživanje. Definiciju pretraživanja će se promijeniti.

Odabir "Brisanje" tražit će od korisnika za potvrdu, a zatim Ukloni spremljeno pretraživanje popisa korisnika.

Odabir "Spremi kao zadano" će se označiti s odabranim spremljeno pretraživanje kao zadano pretraživanje za korisnika. Ako korisnik izvodi "prazan" pretraživanje na početnoj stranici katalog podataka Azure, zadano pretraživanje korisnika će se izvršavati. Uz to, pretraživanje označen kao zadani pojavit će se pri vrhu popisa spremljeno pretraživanje.

### <a name="organizational-saved-searches"></a>Tvrtke ili ustanove spremljena pretraživanja

Svaki korisnik možete spremiti traži vlastitu upotrebu. Administratori katalog podataka možete spremiti pretraživanja za sve korisnike unutar tvrtke ili ustanove. Kad spremite datoteku pretraživanja, administratori su usmjereni s mogućnošću zajedničkog korištenja spremljeno pretraživanje unutar tvrtke. Ako je odabrana ta mogućnost, znači da je spremljeno pretraživanje uvrstiti u popis dostupnih pretraživanja za sve korisnike.

 ![Tvrtke ili ustanove spremljena pretraživanja](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>Resursi prikvačene podataka

Spremljena pretraživanja korisnicima omogućuje spremanje i ponovno korištenje pretraživanja definicije; Resursi podataka koji je vratio traženja može se promijeniti tijekom vremena sadržaj kataloga promjena. Prikvačivanje imovine podataka korisnicima omogućuje izričito prepoznavanje imovine određenih podataka da biste ih lakše pristupiti bez obzira na to koristite pretraživanja.

Prikvačivanje podataka resursa je jednostavne – korisnici možete jednostavno kliknite ikonu "PIN-a" imovine podataka da biste ga dodali na svoj popis prikvačene. Pojavit će se ikona u kutu pločicu resursa u prikazu pločicu, a zatim u lijevi stupac u prikazu popisa na portalu za Azure kataloga podataka.

![Prikvačivanje podataka resursa](./media/data-catalog-how-to-save-pin/05-pinning.png)

Unpinning sredstvo je jednako jednostavne – korisnici jednostavno kliknite ikonu "PIN-a" da biste se prebacivali postavku za odabrani resursa.

![Unpinning podataka resursa](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"Moje imovine"
Početnoj stranici portala katalog podataka Azure sadrži odjeljak "Moje imovine" koji prikazuje resursi koji vas zanimaju trenutnog korisnika. Ova sekcija sadrži oba prikvačene resursi i spremljena pretraživanja.

!["Moje imovine" na početnoj stranici](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Sažetak
Azure katalog podataka pruža mogućnosti koje olakšavaju za korisnike da biste otkrili izvori podataka koje su im potrebne da bi se provesti kraće traženje podataka i rad s njime vremena. Spremljena pretraživanja i prikvačene podataka imovine nadograđuju ove mogućnosti core tako da korisnici možete jednostavno prepoznati o izvorima podataka s kojom oni će funkcionirati više puta.
