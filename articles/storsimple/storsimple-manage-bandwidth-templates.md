<properties
   pageTitle="Upravljanje predlošcima za propusnosti vaše StorSimple | Microsoft Azure"
   description="U članku se opisuje Upravljanje predlošcima StorSimple propusnost koje omogućuju kontrolu utrošak propusnosti."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>Upravljanje predlošcima propusnosti StorSimple pomoću servisa StorSimple Manager

## <a name="overview"></a>Pregled

Predlošci propusnosti omogućuju vam da biste konfigurirali korištenja propusnosti mreže preko više rasporeda vrijeme dana da biste tier podatke s uređaja StorSimple u oblak.

S propusnosti ograničavanje rasporede možete učiniti sljedeće:

- Navedite prilagođeni propusnosti rasporede ovisno o upotrebe za radno opterećenje mreže.

- Centralizirano upravljanje i ponovno korištenje rasporede na više uređaja jednostavno i objedinjenog način.

> [AZURE.NOTE] Ta je značajka dostupna samo za StorSimple fizičkih uređaja, a ne za virtualne uređaja.

Svi predlošci propusnosti servisa prikazuju se u tabličnom obliku, a sadrže sljedeće informacije:

- **Naziv** – jedinstveni naziv dodijeljen propusnosti predloška prilikom stvaranja.

- **Raspored** – broj rasporede sadrži propusnosti zadani predložak.

- **Korišteni po** – broj količine pomoću predložaka propusnosti.

Stranice za **Konfiguriranje** StorSimple Upravitelj servisa na portalu za Azure klasični upravljate pomoću predložaka propusnosti.

Možete pronaći i dodatne informacije pomoću kojih se konfiguriranje propusnosti predložaka u:

- Pitanja i odgovori o predlošcima propusnosti
- Praktični savjeti za propusnost predložaka

## <a name="add-a-bandwidth-template"></a>Dodavanje predloška propusnosti

Izvršite sljedeće korake da biste stvorili novi predložak propusnosti.

#### <a name="to-add-a-bandwidth-template"></a>Da biste dodali predloška propusnosti

1. Na stranici **Konfiguracija** servisa upravitelja StorSimple kliknite **Dodavanje/uređivanje propusnosti predložak**.

2. U dijaloškom okviru **Dodavanje/uređivanje propusnosti predložak** :

   1. S padajućeg popisa **predloška** odaberite **Stvori novi** da biste dodali novi predložak propusnosti.
   2. Navedite jedinstveni naziv za predložak propusnosti.

3. Definiranje **propusnosti raspored**. Da biste stvorili rasporeda:

   1. S padajućeg popisa odaberite dane u tjednu raspored je konfiguriran za. Možete odabrati više dana tako da potvrdite okvire koji se nalazi prije odgovarajući dana na popisu.
   2. Ako raspored Poboljšana je za cijeli dan, odaberite mogućnost **Cijeli dan** . Kada je ta mogućnost, više ne možete odrediti **Vrijeme početka** i **Vrijeme završetka**. Raspored pokreće od 12.00 11:59 poslije Podne.
   3. S padajućeg popisa odaberite **Vrijeme početka**. Ovo je kad će početi raspored.
   4. Na padajućem popisu odaberite neko **Vrijeme završetka**. Ovo je kad u raspored će se zaustaviti.

         > [AZURE.NOTE] Nije dopušteno preklapajućih rasporeda. Ako vremena početka i završetka, rezultirat će preklapajućih raspored, vidjet ćete poruku o pogrešci da snazi.

   5. Navedite **propusnosti stopa**. Ovo je propusnost u megabita u sekundi (MB/s) koristi uređaj StorSimple operacije koje obuhvaćaju s oblakom (prijenosi i preuzimanja). Navedite broj između 1 i 1000 za to polje.

   6. Kliknite ikonu provjeri ![ikonu Provjera](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Predložak koji ste stvorili dodat će se popis predložaka propusnosti na stranici **Konfiguracija** servisa.

    ![Stvaranje novog predloška propusnosti](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)

4. Kliknite **Spremi** pri dnu stranice, a zatim kliknite **da** kada se to od vas zatraži potvrdu. To će spremiti promjene konfiguracije koje ste napravili.

## <a name="edit-a-bandwidth-template"></a>Uređivanje predloška propusnosti

Izvršite sljedeće korake da biste uredili predloška propusnosti.

### <a name="to-edit-a-bandwidth-template"></a>Da biste uredili predloška propusnosti

1. Kliknite **Dodavanje/uređivanje propusnosti predložak**.

2. U dijaloškom okviru **Dodavanje/uređivanje propusnosti predložak** :

   1. Na padajućem popisu **predložak** odaberite postojeći propusnosti predložak koji želite izmijeniti.
   2. Unesite promjene. (Možete promijeniti postojeće postavke.)
   3. Kliknite ikonu provjeri ![Ikonu Provjera](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Vidjet ćete izmjene predloška u popis predložaka propusnosti na stranici Konfiguracija servisa.

3. Da biste spremili promjene, kliknite **Spremi** pri dnu stranice. Kliknite **da** kada se to od vas zatraži potvrdu.

> [AZURE.NOTE] Će nije dopušteno da biste spremili promjene, ako je uređivati raspored preklapa s postojećeg rasporeda u predlošku propusnosti koju mijenjate.

## <a name="delete-a-bandwidth-template"></a>Brisanje predloška propusnosti

Izvršite sljedeće korake da biste izbrisali predloška propusnosti.

#### <a name="to-delete-a-bandwidth-template"></a>Da biste izbrisali predloška propusnosti

1. Na popisu tablični predložaka propusnosti za uslugu odaberite predložak koji želite izbrisati. Ikona Izbriši (**x**) pojavit će se ekstremne desno od odabranog predloška. Kliknite ikonu **x** da biste izbrisali predložak.

2. Zatražit će se za potvrdu. Kliknite **u redu** da biste nastavili.

Ako je predložak koristi sve volume(s), koje neće moći će se izbrisati. Prikazat će se poruka o pogrešci koja označava da je predložak koristi. Pojavit će se dijaloški okvir poruke pogreške navodi da se ukloniti sve reference na predlošku.

Reference na predložak možete izbrisati tako da pristupa stranici **Spremnika glasnoću** i izmjenu glasnoću spremnika u koji koristite ovaj predložak tako da koristi drugi predložak ili prilagođeni ili neograničeno propusnosti postavka. Kada uklonite sve reference možete izbrisati predložak.

## <a name="use-a-default-bandwidth-template"></a>Koristili zadani predložak propusnosti

Zadani predložak propusnosti navedeni su i koristi glasnoću spremnika prema zadanim postavkama da biste nametnuli propusnosti kontrole prilikom pristupanja oblaka. Zadani predložak služi i kao referencu jeste li spremni za korisnike koji stvaranje vlastitih predložaka. Detalje o ovom zadani predložak su:

- **Naziv** – neograničeno noći i dane vikenda

- **Raspored** – jedan raspored od ponedjeljka do petka koji se primjenjuje propusnosti stopa 1 MB/s između 8 Prijepodne i 5 PM uređaj vremena. Propusnost postavljen je na neograničeno za ostatak u tjednu.

Moguće je uređivati zadani predložak. Korištenje ovog predloška (uključujući promijenjenim verzijama) prati.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Stvaranje cjelodnevnog propusnosti predloška koji će se pokrenuti u određeno vrijeme

Slijedite ovaj postupak da biste stvorili raspored koji se pokreće u određeno vrijeme i pokreće cijeli dan. U ovom primjeru raspored počinje 9 AM u Jutro i pokreće do 9 AM sljedeći Jutro. Nije važno Imajte na umu koji vremena početka i završetka za dani raspored moraju oboje da se nalaze na istom 24 sata dnevno, a ne može obuhvaćati više dana. Ako vam je potrebna postavljanje predložaka za propusnost koje se protežu na više dana, morat ćete koristiti više rasporeda (kao što je prikazano u primjeru).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Da biste stvorili predložak cjelodnevni propusnosti

1. Izradite raspored koji počinje 9 AM u Jutro i pokreće do ponoći.

2. Dodajte drugi raspored. Konfiguriranje drugi raspored da biste pokrenuli od ponoći do 9 AM u Jutro.

3. Spremanje predloška propusnosti.

Složeni raspored pa ćete pokrenuti po vašem odabiru i koristiti cjelodnevni.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Pitanja i odgovori o predlošcima propusnosti

**Q**. Što se događa propusnosti kontrole kada ste između rasporedima? (Je završen raspored i neki drugi još nije započelo.)

**A**. U tim slučajevima će zapošljavanju bez kontrola propusnosti. To znači da uređaja možete koristiti neograničeno propusnosti kada tiering podataka s oblakom.

**Q**. Možete izmijeniti propusnosti predložaka na uređaju sa sustavom izvanmrežno?

**A**. Nećete moći izmjena predložaka propusnosti na spremnika količine ako odgovarajući uređaj je izvan mreže.

**Q**. Možete urediti predložak propusnosti povezan s spremniku glasnoću kad povezani jedinicama u izvanmrežnom načinu?

**A**. Možete izmijeniti predložak propusnosti pridružene spremniku glasnoću čija jedinice u izvanmrežnom načinu. Imajte na umu da kada van količine podataka će se tiered s uređaja s oblakom.

**Q**. Možete izbrisati zadani predložak?

**A**. Iako ne možete izbrisati zadani predložak, nije dobro da to učinite. Korištenje zadani predložak, uključujući promijenjenim verzijama prati. Praćenje podataka je analizirati i tijekom dvotjednog vrijeme, koriste se za poboljšanje zadani predložak.

**Q**. Kako ste odredili da predloške propusnosti moraju se mijenjati?

**A**. Jedna od predznaku ćete morati izmijeniti predložaka propusnosti je prilikom pokretanja prikazuju s mrežom usporiti ili choke više puta u dana. Ako se to dogodi, praćenje mreže za pohranu i korištenje tako da pogledate grafikoni/i performanse i propusnost mreže.

Iz podataka propusnost mreže, odredite doba dana i spremnika jedinice u kojima se pojavljuje usko grlo mreže. Ako se to dogodi kada podataka tiered je u tijeku s oblakom (se ti podaci iz/i performanse za sve spremnike jedinice za uređaj da biste u oblaku), zatim morat ćete izmjena predložaka propusnosti povezan s vašeg spremnika glasnoću.

Nakon izmijenjene predlošci se koriste, morat ćete praćenje na mreži ponovno značajan latencies. Ako oni i dalje postoji, zatim morat ćete ponovo posjetite predloške propusnosti.

**Q**. Što se događa ako imate više spremnika glasnoće na uređaju Zakazuje te preklapanje, ali različite ograničenja primjenjuju se na svakom?

**A**. Recimo da imate li uređaj s 3 spremnika glasnoću. Rasporedi potpuno pridružene te spremnika se preklapaju. Za svaki od tih spremnika ograničenja propusnosti koristi su 5, 10 i 15 MB/s odnosno. Kada I/Os se pojavljuje na sve te spremnika u isto vrijeme, može primijeniti najmanje 3 ograničenja propusnosti: u ovom slučaju 5 MB/s kao te izlazni/i zahtjeva za zajedničko korištenje u istom red.

## <a name="best-practices-for-bandwidth-templates"></a>Praktični savjeti za propusnost predložaka

Slijedite ove praktične savjete za svoj uređaj StorSimple:

- Konfiguriranje predložaka propusnosti na uređaju da biste omogućili varijable ograničavanje od propusnost mreže uređaj na različita doba dana. Predloške tih propusnosti kada se koristi s sigurnosne kopije rasporede učinkovito na raspolaganju propusnost dodatne mreže za operacije oblak najfrekventnija.

- Izračun stvarni propusnost potrebnu za određeni implementacije na temelju veličine uvođenje i vrijeme cilj potrebnim (RTO).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [korištenju StorSimple Upravitelj servisa za administraciju StorSimple uređaj](storsimple-manager-service-administration.md).
