<properties
    pageTitle="Analiza uzoraka korištenja Azure CDN | Microsoft Azure"
    description="Možete pogledati uzoraka korištenja vaše CDN pomoću sljedećih izvješća: propusnosti, prenijeti podatke, pristupa, statusi predmemorije, kliknite omjer predmemorije, IPV4/IPV6 podaci prenijeti."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-azure-cdn-usage-patterns"></a>Analiza uzoraka korištenja Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Možete pogledati uzoraka korištenja vaše CDN pomoću sljedećih izvješća:

- Propusnosti
- Podaci koji se prenose
- Pristupa
- Statusi predmemorije
- Omjer pronalaženja objekata u predmemoriji
- IPv4/IPV6 podataka koji se prenose

## <a name="accessing-advanced-http-reports"></a>Pristup dodatna izvješća HTTP

1. Iz profila plohu CDN kliknite gumb **Upravljanje** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-reports/cdn-manage-btn.png)

    Otvorit će se na portal za upravljanje CDN.

2. Zadržite pokazivač na kartici **analize** , a zatim zadržite pokazivač na Potpaleta **Core izvješća** .  Kliknite željeni izvješća na izborniku.

    ![Portal za upravljanje CDN - izbornik Core izvješća](./media/cdn-reports/cdn-core-reports.png)


## <a name="bandwidth"></a>Propusnosti

Izvješće o propusnosti sastoji se grafikon i podatkovne tablice koja označava korištenja propusnosti za HTTP i HTTPS tijekom određenog vremenskog razdoblja. Možete pogledati korištenja propusnosti preko svih CDN se pojavljuje ili određeni POP. Omogućuje prikaz krivina promet i distribucija duž CDN koji se pojavljuje u MB/s.

- Odaberite sve čvorove rub da biste vidjeli promet s sve čvorove ili odaberite određenu regiju/čvor s padajućeg popisa.
- Odabir raspona datuma za prikaz podataka za danas ovaj tjedan/ovog mjeseca, itd. ili unesite prilagođeni datume, a zatim kliknite "Idi" da biste provjerili odabir će se ažurirati.
- Možete izvesti i preuzmite podatke tako da kliknete ikonu list programa excel koja se nalazi uz "Idi".

Izvješće se ažurira svakih 5 minuta.

![Izvješće o propusnosti](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Podaci koji se prenose

Izvješće sastoji se od grafikona i podataka iz tablice koji označava korištenje promet za HTTP i HTTPS tijekom određenog vremenskog razdoblja. Korištenje promet možete pogledati preko svih CDN se pojavljuje ili određeni POP. Omogućuje prikaz krivina promet i distribucija duž CDN koji se pojavljuje u GB.

- Odaberite sve čvorove rub da biste vidjeli promet s sve bilješke ili odaberite određenu regiju/čvor s padajućeg popisa.
- Odabir raspona datuma za prikaz podataka za danas ovaj tjedan/ovog mjeseca, itd. ili unesite prilagođeni datume, a zatim kliknite "Idi" da biste provjerili odabir će se ažurirati.
- Možete izvesti i preuzmite podatke tako da kliknete ikonu list programa excel koja se nalazi uz "Idi".

Izvješće se ažurira svakih 5 minuta.

![Podaci prenose izvješća](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Pristupa (kodovi stanja)

Izvješće opisuju raspodjele kodovi stanja zahtjev za sadržaj. Svaki zahtjev za sadržaj generirat će HTTP kod stanja. Šifra stanja opisuje kako rukovati rub iskače zahtjev. Ako, na primjer, kodovi stanja 2xx označavaju da zahtjev je uspješno slanje klijent, dok je kod stanja 4xx označava pojavila se pogreška. Dodatne informacije o Šifra stanja HTTP potražite u članku [Kodovi status](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Odabir raspona datuma za prikaz podataka za danas ovaj tjedan/ovog mjeseca, itd. ili unesite prilagođeni datume, a zatim kliknite "Idi" da biste provjerili odabir će se ažurirati.
- Možete izvesti i preuzmite podatke klikom na list programa excel koja se nalazi uz "Idi".

![Izvješće pristupa](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Statusi predmemorije

Izvješće opisuju raspodjele pronalaženje objekata u predmemoriji i neuspjele akcije u predmemoriji za klijentski zahtjev. Budući da se najbrže performanse potječe iz predmemorije pristupa, možete optimizirati podataka isporuke brzine minimiziranje neuspjele akcije u predmemoriji i pronalaženje objekata u predmemoriji istekle. Neuspjele akcije u predmemoriji mogu biti lošije konfiguriranjem poslužitelja polazište da biste izbjegli Dodjela odgovor "no-cache" zaglavlja, tako da izbjegavanje nizu upita predmemoriranje osim isključivo potrebno i izbjegavanje kodova koje nisu predmemorirati odgovor. Istekle predmemorije pristupa mogu se izbjegavati tako da sredstvo dugo moguće da biste minimizirali broj zahtjeva s poslužiteljem polazište je max dob.

![Predmemorija statusi izvješća](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Statusi glavnu predmemoriju obuhvaćaju sljedeće:

- TCP_HIT: Poslužena od ruba. Objekt je u predmemoriji i premašila njegov max dob.
- TCP_MISS: Poslužena od izvora. Objekt nije u predmemoriji i odgovora nije natrag na polazište.
- TCP_EXPIRED _MISS: Poslužena od izvora nakon revalidation s polazište. Objekt je u predmemoriji, ali premašila njegov max dob. Revalidation s polazište rezultirala predmemorije objekta zamijenjena novi odgovor polazište.
- TCP_EXPIRED _HIT: Poslužena od ruba nakon revalidation s polazište. Objekt je u predmemoriji, ali premašila njegov max dob. Revalidation s izvorni poslužitelj rezultirala se nepromijenjenu predmemorije objekta.

- Odabir raspona datuma za prikaz podataka za danas ovaj tjedan/ovog mjeseca, itd. ili unesite prilagođeni datume, a zatim kliknite "Idi" da biste provjerili odabir će se ažurirati.
- Možete izvesti i preuzmite podatke tako da kliknete ikonu list programa excel koja se nalazi uz "Idi".

### <a name="full-list-of-cache-statuses"></a>Potpuni popis statusi predmemorije

- TCP_HIT – ovaj status prijavljuje kada zahtjeva poslužena izravno iz sustava POP klijentu. Sredstva odmah poslužena s POP kada nije lokalno na najbliži klijent POP, a sadrži valjani vrijeme važenja i TTL. TTL određuju sljedeće zaglavlja odgovora:

    - Predmemorijom: s maxage
    - Predmemorije kontrola: max starost
    - Istječe

- TCP_MISS – ovaj status označava da predmemorirane verzije tražene resursa nije pronađen na najbliži klijent POP. Će je sredstvo zatražio izvorni poslužitelj ili štita poslužitelj za izvor. Ako na origin ili poslužitelju štita polazište vraća sredstvo, će biti poslužena klijentu i predmemorirane na klijentu i rubni poslužitelj. U suprotnom, kod koji nisu 200 stanja (npr 403 zabranjeno, 404 nije pronađeno, itd.) će vratiti.

- _HIT TCP_EXPIRED – ovaj status prijavljuje kada je zahtjev za ciljani imovine s istekle TTL, kao što su kada je istekla sredstva max-dob poslužena izravno iz sustava POP klijentu.

    Zahtjev za istekle obično rezultira zahtjeva za revalidation s poslužiteljem polazište. Kako bi TCP_EXPIRED _HIT da se pokreće, izvorni poslužitelj morate naznačiti novijom verzijom sredstva ne postoji. Ta vrsta situacija ažurirat će se obično predmemorijom i zaglavlja istječe tog resursa.

- _MISS TCP_EXPIRED – ovaj status prijavljuje kada novijom verzijom istekle predmemorirani resursa poslužena iz na POP klijentu. To se događa kada je istekla TTL predmemorirane stavke (npr., istekao i max dob) i izvorni poslužitelj vraća novijom verzijom tog resursa. Novu verziju imovine će posluživanje klijentu umjesto predmemorirane verzije. Uz to, će se spremiti na rubnom poslužitelju i klijenta.

- CONFIG_NOCACHE – ovaj status označava specifična konfiguracije na našem rubu POP spriječio imovine s predmemoriranje.

- NIŠTA – ovaj status označava da sadržaj svježina provjere predmemorije nije izvršena.

- _MISS TCP_ CLIENT_REFRESH – ovaj status prijavljuje kada klijent HTTP (npr., u pregledniku) nameće rub POP dohvatiti novu verziju zastarjele resursa s poslužiteljem polazište.

    Prema zadanim postavkama, naših poslužitelja onemogućuju klijent za HTTP prisilno naše edge poslužitelji za dohvaćanje novu verziju imovine s poslužitelja polazište.

- PARTIAL_HIT TCP_ – ovaj status prijavljuje kada zahtjev za raspon bajt rezultira uspješnosti djelomično predmemorirane stavke. Raspon tražene bajt odmah poslužena iz na POP klijentu.

- UNCACHEABLE – ovaj status prijavljuje kada zaglavlja predmemorijom ili istječe za sredstvo označava da je potrebno ne predmemoriju POP ili HTTP klijenta. Ove vrste zahtjeva za su poslužena s poslužitelja polazište

## <a name="cache-hit-ratio"></a>Omjer pronalaženja objekata u predmemoriji

Izvješće prikazuje postotak predmemorirani zahtjeva za koja su služila izravno iz predmemorije.

Izvješće sadrži sljedeće detalje:

- Tražene sadržaja su predmemorirane na najbliži podnositelj POP.
- Zahtjev je poslužena izravno od ruba našu mrežu.
- Zahtjev nije potreban revalidation s poslužiteljem polazište.

Izvješće ne obuhvaća:

- Zahtjevi za koje su zabranjen zbog države mogućnostima filtriranja.
- Zahtjevi za imovinu čiji zaglavlja označava da su treba ne predmemoriju. Ako, na primjer, predmemorijom: privatne, predmemorijom: bez predmemorije ili Pragma: bez predmemorije zaglavlja, onemogućuje sredstvo predmemoriranje.
- Bajt raspon zahtjeva za djelomično predmemoriranog sadržaja.

Formula je: (REZULTAT TCP_ / (REZULTAT TCP_ + TCP_MISS)) * 100

- Odabir raspona datuma za prikaz podataka za danas ovaj tjedan/ovog mjeseca, itd. ili unesite prilagođeni datume, a zatim kliknite "Idi" da biste provjerili odabir će se ažurirati.
- Možete izvesti i preuzmite podatke tako da kliknete ikonu list programa excel koja se nalazi uz "Idi".


![Omjer izvješća pronalaženja objekata u predmemoriji](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPV6 podataka koji se prenose

Izvješće prikazuje distribuciju korištenje promet na IPV4 Dodavanje veze za vanjskih IPV6.

![IPv4/IPV6 podataka koji se prenose](./media/cdn-reports/cdn-ipv4-ipv6.png)

- Odabir raspona datuma za prikaz podataka za danas ovaj tjedan/ovog mjeseca, itd., ili pak ručno unesite prilagođene datume.
- Nakon toga kliknite "Idi" da biste bili sigurni da se ažurira odabir.


## <a name="considerations"></a>Razmatranja

Izvješća moguće je generirati samo unutar zadnja 18 mjeseca.
