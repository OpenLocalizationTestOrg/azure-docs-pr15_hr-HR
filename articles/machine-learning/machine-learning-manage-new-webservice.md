<properties
    pageTitle="Upravljanje web-servisa pomoću portala za Azure strojnog učenja Web Serivces | Microsoft Azure"
    description="Upravljanje pristupom radne prostore Azure strojnog učenja, uvođenje i upravljanje ML API web-servisi"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Upravljanje web-servisa pomoću portala za Azure strojnog učenja web-servisi

Možete upravljati strojnog učenja nove i klasične web-servisa pomoću portala za Microsoft Azure strojnog učenja Web Services. Budući da se klasične Web-servisima i novo web-mjesto servisa temelje se na različitim pozadini tehnologije, imate mogućnosti upravljanja malo drugačije za svaku od njih.

Na portalu strojnog učenja Web Services možete učiniti sljedeće:

- Praćenje kako se koristi web-servisa.
- Konfiguriranje opis, ažurirajte tipki web-servisa (samo za novo), ažurirajte vaše prostora za pohranu računa ključa (Novo samo) Omogući bilježenje, i omogućiti ili onemogućiti ogledne podatke.
- Brisanje web-servisa.
- Stvaranje, brisanje ili ažuriranje naplata tarife (samo za novo).
- Dodavanje i brisanje krajnje točke (samo za Classic)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Upravljanje uslugama za novo web-mjesto

Da biste upravljali novo web-mjesto servisa:

1.  Prijavite se na portal sustava [Microsoft Azure strojnog učenja web-servisi](https://services.azureml.net/quickstart) pomoću računa sustava Microsoft Azure – pomoću računa koji je pridružen Azure pretplate.
2.  Na izborniku kliknite **Web-servisa**.

Prikazat će se popis distribuiranih web-servisi za vašu pretplatu. 

Da biste upravljali web-servisa, kliknite web-servisa. Na stranici web-servisi možete učiniti sljedeće:

- Kliknite web-servisa za upravljanje ga.
- Kliknite na naplatu Plan za web-servis je ažurirajte.
- Brisanje web-servisa.
- Kopirajte web-servisa i implementacija u drugoj regiji.

Kada kliknete web-servisa, otvorit će se web-stranici servisa brzi početak rada. Web-stranici servisa brzi početak rada sastoji se od dviju mogućnosti izbornika koji omogućuju upravljanje web-servisa:

- **Nadzorna PLOČA** - omogućuje prikaz korištenja servisa za Web.
- **Konfiguriranje** - omogućuje dodavanje opisni tekst, ažurirajte ključ za račun za pohranu koji je povezan s web-servisa i omogućiti ili onemogućiti ogledne podatke.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Kako se koristi web-servisa za nadzor ###

Kliknite karticu **nadzorne PLOČE** .

Na nadzornoj ploči možete pogledati cjelokupan korištenje web-usluge vremenskom razdoblju. Možete odabrati razdoblje da biste pogledali razdoblja padajućem izborniku u gornjem desnom kutu grafikona za korištenje. Na nadzornoj ploči prikazuje sljedeće informacije:

- **Zahtjevi za iznad vremena** prikazuje grafikon korak broj zahtjeva odabranom vremenskom razdoblju. Pomoću nje možete prepoznati ako ste naišli na krivina u odjeljku korištenje.
- **Zahtjevi za odgovor na zahtjev** prikazuje ukupni broj poziva odgovor na zahtjev je servis primio na odabranom vremenskom razdoblju i koliko ih nije uspjelo.
- **Vrijeme izračunati prosjek odgovor na zahtjev** prikazuje prosječnu vrijednost potrebno izvršiti primljene zahtjeva za vrijeme.
- **Zahtjevi za obradu** prikazuje ukupan broj obrade zahtjeva servis je primio na odabranom vremenskom razdoblju i koliko ih nije uspjelo.
- **Prosječne latencije posla** prikazuje prosječnu vrijednost potrebno izvršiti primljene zahtjeva za vrijeme.
- **Pogreške** prikazuje zbrajanja broj pogrešaka koji se odvijaju na pozive na web-servisa.
- **Troškovi servisa** prikazuje naknade za naplatu plan povezan sa servisom.

### <a name="configuring-the-web-service"></a>Konfiguriranje web-usluge ###

Kliknite **KONFIGURIRAJ** mogućnosti izbornika.

Možete ažurirati sljedeća svojstva:

* **Opis** možete unijeti opis web-servisa.
* **Naslov** omogućuje unesite naslov web-usluge
* **Tipke** omogućuje vam da biste zakrenuli primarnih i sekundarnih ključeva API-JA.
* **Ključ za račun za pohranu** omogućuje vam da biste ažurirali ključ za račun za pohranu koji je povezan s promjenama servisa za Web. 
* **Omogućivanje oglednih podataka** omogućuje vam slanje oglednih podataka koje možete koristiti da biste testirali servis za odgovor na zahtjev. Ako ste stvorili web-servisa u strojnog učenja Studio, oglednih podataka koristi se iz podataka sustava korištenih uvježbati modela. Ako ste stvorili servis programski, podataka koristi se s oglednim podacima koje ste naveli kao dio paketa JSON.

### <a name="managing-billing-plans"></a>Upravljanje naplatom tarife ###

Kliknite izbornik mogućnosti **tarife** s web-stranice za brzo pokretanje servisa. Možete kliknuti i planiranje povezan s određenim web-servisa za upravljanje tom plan.

* **Novo** omogućuje vam da biste stvorili novu tarifu.
* **Dodaj/Ukloni Plan instancu** omogućuje "skaliranje" postojećeg plana da biste dodali kapacitet.
* **Nadogradnju na stariju nadogradnje/verziju** omogućuje "skaliranja" postojećeg plana da biste dodali kapacitet.
* **Brisanje** omogućuje brisanje plana.

Kliknite plan da biste vidjeli njezin nadzorne ploče. Na nadzornoj ploči vam snimka ili tarifu korištenja odabrane vremenskom razdoblju. Da biste odabrali vremensko razdoblje da biste pogledali, kliknite padajući izbornik **razdoblje** u gornjem desnom kutu nadzorne ploče. 

Nadzorna ploča za planiranje nalaze se sljedeće informacije:

* **Planiranje opis** prikazuje informacije o troškove i kapacitet pridružene plan.
* **Planiranje korištenja** prikazuje broj transakcije i računalnim sati koje su naplaćena protiv plan.
* **Web-servisi** prikazuje se broj web-servisi koji koriste tu shemu.
* **Vrh Web tako da pozive za servis** prikazuje vrha četiri web-servisi koji su upućivanje poziva koje se naplaćuju u odnosu na željenu tarifu.
* **Vrh web-servisi po izračunati sati** prikazuje vrha četiri web-servisi koji koriste računalnim resursa koje se naplaćuju u odnosu na željenu tarifu.

## <a name="manage-classic-web-services"></a>Upravljanje klasične web-servisi

> [AZURE.NOTE] Postupci u ovom odjeljku se odnose na upravljanje klasični web-servisi putem portala za Azure strojnog učenja Web Services. Informacije o upravljanju klasične web-servisi putem učenje Studio računala i Azure klasični portal potražite u članku [Upravljanje radnog prostora programa Azure strojnog učenja](machine-learning-manage-workspace.md).

Da biste upravljali klasične Web servisa:

1.  Prijavite se na portal sustava [Microsoft Azure strojnog učenja web-servisi](https://services.azureml.net/quickstart) pomoću računa sustava Microsoft Azure – pomoću računa koji je pridružen Azure pretplate.
2.  Na izborniku kliknite **Klasični Web Services**.

Da biste upravljali klasične web-servisa, kliknite **Klasični Web Services**. Na stranici klasične web-servisi možete učiniti sljedeće:

- Kliknite web-servisa da biste vidjeli povezane krajnje točke.
- Brisanje web-servisa.

Kad sami upravljate klasične web-servisa, upravljate svih krajnjih točaka zasebno. Kada kliknete web-servisa na stranici web-servisi, otvorit će se popis krajnje točke povezan sa servisom. 

Na stranici krajnje točke klasične web-servisa, možete dodati i brisanje krajnje točke na servis. Dodatne informacije o dodavanju krajnje točke potražite u članku [Stvaranje krajnje točke](machine-learning-create-endpoint.md).

Kliknite jednu od krajnje točke da biste otvorili web-stranici servisa brzi početak rada. Na stranici za brzi početak rada postoje dvije mogućnosti izbornika koji omogućuju upravljanje web-servisa:

- **Nadzorna PLOČA** - omogućuje prikaz korištenja servisa za Web.
- **Konfiguriranje** - omogućuje vam da biste dodali opisni tekst, uključite zapisivanje pogrešaka prilikom Uključivanje i isključivanje ažuriranje ključ za prostora za pohranu račun povezan s web-servisa i omogućivanje i onemogućivanje ogledne podatke.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Kako se koristi web-servisa za nadzor ###

Kliknite karticu **nadzorne PLOČE** .

Na nadzornoj ploči možete pogledati cjelokupan korištenje web-usluge vremenskom razdoblju. Možete odabrati razdoblje da biste pogledali razdoblja padajućem izborniku u gornjem desnom kutu grafikona za korištenje. Na nadzornoj ploči prikazuje sljedeće informacije:

- **Zahtjevi za iznad vremena** prikazuje grafikon korak broj zahtjeva odabranom vremenskom razdoblju. Pomoću nje možete prepoznati ako ste naišli na krivina u odjeljku korištenje.
- **Zahtjevi za odgovor na zahtjev** prikazuje ukupni broj poziva odgovor na zahtjev je servis primio na odabranom vremenskom razdoblju i koliko ih nije uspjelo.
- **Vrijeme izračunati prosjek odgovor na zahtjev** prikazuje prosječnu vrijednost potrebno izvršiti primljene zahtjeva za vrijeme.
- **Zahtjevi za obradu** prikazuje ukupan broj obrade zahtjeva servis je primio na odabranom vremenskom razdoblju i koliko ih nije uspjelo.
- **Prosječne latencije posla** prikazuje prosječnu vrijednost potrebno izvršiti primljene zahtjeva za vrijeme.
- **Pogreške** prikazuje zbrajanja broj pogrešaka koji se odvijaju na pozive na web-servisa.
- **Troškovi servisa** prikazuje naknade za naplatu plan povezan sa servisom.

### <a name="configuring-the-web-service"></a>Konfiguriranje web-usluge ###

Kliknite **KONFIGURIRAJ** mogućnosti izbornika.

Možete ažurirati sljedeća svojstva:

* **Opis** možete unijeti opis web-servisa. Opis nije obavezno.
* **Bilježenje u zapisnik** omogućuje vam da biste omogućili ili onemogućili zapisivanje na krajnjoj pogrešaka. Dodatne informacije o zapisivanje potražite u članku Omogući [bilježenje u zapisnik za web-servise strojnog učenja](machine-learning-web-services-logging.md).
* **Omogućivanje oglednih podataka** omogućuje vam slanje oglednih podataka koje možete koristiti da biste testirali servis za odgovor na zahtjev. Ako ste stvorili web-servisa u strojnog učenja Studio, oglednih podataka koristi se iz podataka sustava korištenih uvježbati modela. Ako ste stvorili servis programski, podataka koristi se s oglednim podacima koje ste naveli kao dio paketa JSON.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Dodjela ili obustavljanje pristupa web-servisi za korisnike na portalu

Pomoću portala za Azure klasični, možete dopustiti ili zabraniti pristup određenim korisnicima.

### <a name="access-for-users-of-new-web-services"></a>Pristup za korisnike servisa novo web-mjesto

Da biste omogućili drugim korisnicima za rad s web-usluge na portalu servisa Azure strojnog učenja Web Services, morate ih dodati kao ko adminstrators Azure pretplate.

Prijava na [portal za Azure klasični](https://manage.windowsazure.com/) pomoću računa sustava Microsoft Azure – pomoću računa koji je pridružen Azure pretplate.

1. U navigacijskom oknu kliknite **Postavke**, a zatim kliknite **Administratori**.
2. Pri dnu prozora kliknite **Dodaj**. 
3. U dijaloškom okviru DODAJ A dodatnih ADMINISTRATORA upišite adresu e-pošte osobe kojoj želite Dodaj kao suradnika administrator, a zatim odaberite pretplatu u koju želite zajednički administratorski pristup.
4. Kliknite **Spremi**.

### <a name="access-for-users-of-classic-web-services"></a>Pristup za korisnike klasične web-servisa

Da biste upravljali radnog prostora:

Prijava na [portal za Azure klasični](https://manage.windowsazure.com/) pomoću računa sustava Microsoft Azure – pomoću računa koji je pridružen Azure pretplate.

1. U oknu servisa Microsoft Azure kliknite **STROJNOG UČENJA**.
1. Kliknite radni prostor u kojima želite upravljati.
1. Kliknite karticu **KONFIGURACIJA** .

Na kartici konfiguracije možete obustaviti pristup radnom prostoru strojnog učenja tako da kliknete **USKRATI**. Korisnici neće moći otvoriti radni prostor u Studio strojnog učenja. Da biste vratili programa access, kliknite **DOPUSTI**.

Za određene korisnike:

Da biste upravljali dodatne račune koji imaju pristup radni prostor na računalu učenje Studio, kliknite **Prijava ML Studio** na kartici **nadzorne PLOČE** . Otvara se radni prostor na u Studio strojnog učenja. Na tom mjestu, kliknite karticu **Postavke** i zatim **korisnika**. Kliknite **POZOVI još korisnika** odobravanje pristupa u radni prostor ili odaberite korisnika i kliknite **UKLONI**.

> [AZURE.NOTE] Veza za **prijavu ML Studio** otvara strojnog učenja Studio pomoću Microsoftova Account trenutačno niste prijavljeni u. Microsoft Account koji ste koristili za prijavu na portalu Azure klasični za stvaranje radnog prostora automatski nemate dozvolu za otvaranje tog radnog prostora. Da biste otvorili radni prostor, morate biti prijavljeni Microsoftovim Account koja je definirana kao vlasnik radnog prostora ili morate primiti pozivnicu od vlasnika da biste se uključili u radni prostor.
