<properties
    pageTitle="Upravljanje radnom prostoru strojnog učenja | Microsoft Azure"
    description="Upravljanje pristupom radne prostore Azure strojnog učenja, uvođenje i upravljanje ML API web-servisi"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Upravljanje programa Azure strojnog učenja radnog prostora

>[AZURE.NOTE] Postupci u ovom članku relevantne Azure strojnog učenja klasične Web services. Informacije o upravljanju web-usluge na portalu strojnog učenja Web Services potražite u članku [Upravljanje web-servisa pomoću portala za Azure strojnog učenja Web Services](machine-learning-manage-new-webservice.md).

Pomoću portala za Azure klasični, možete upravljati strojnog učenja radnih prostora za:

- Praćenje kako se koristi radnog prostora
- Konfiguriranje radnog prostora da biste dopustili ili uskraćivanje pristupa
- Upravljanje web-servisi stvorene u radnom prostoru
- Brisanje radnog prostora

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Osim toga, kartica nadzorna ploča pregled Upotreba radnog prostora i nudi sažet podatke radnog prostora.  

> [AZURE.TIP] U Azure strojnog učenja Studio na kartici **Web-SERVISI** možete dodavanje, ažuriranje i brisanje web-strojnog učenja servisa.

Da biste upravljali radnog prostora:

1.  Prijava na [portal za Azure klasični](https://manage.windowsazure.com/) pomoću računa sustava Microsoft Azure – pomoću računa koji je pridružen Azure pretplate.
2.  U oknu servisa Microsoft Azure kliknite **STROJNOG UČENJA**.
3.  Kliknite radni prostor u kojima želite upravljati.

Stranica za radni prostor sastoji se od tri kartice:

- **Nadzorna PLOČA** - omogućuje vam korištenje radnog prostora za prikaz i podacima
- **Konfiguriranje** - omogućuje upravljanje pristupom u radni prostor
- **Web-SERVISI** – vam omogućuje upravljanje web-usluge koje su objavljene ovog radnog prostora

## <a name="to-monitor-how-the-workspace-is-being-used"></a>Praćenje kako se koristi radnog prostora

Kliknite karticu **nadzorne PLOČE** .

Na nadzornoj ploči možete pogledati cjelokupan korištenje radnog prostora i dobiti sažet informacija o radnom prostoru.

- Grafikon **IZRAČUNATI** prikazuje računalnim resursa koja se koristi za radni prostor. Možete promijeniti prikaz relativne i apsolutne vrijednosti, a možete promijeniti vremenskog razdoblja prikazane u grafikonu.
- **Pregled korištenja** prikazuje Azure koja se koristi za radni prostor za pohranu.
- **Brzi pogled** daje sažetak informacija o radnom prostoru i korisne veze.

> [AZURE.NOTE] Veza za **prijavu ML Studio** otvara strojnog učenja Studio pomoću Microsoftova Account trenutačno niste prijavljeni u. Microsoft Account koji ste koristili za prijavu na portalu Azure klasični za stvaranje radnog prostora automatski nemate dozvolu za otvaranje tog radnog prostora. Da biste otvorili radni prostor, morate biti prijavljeni Microsoftovim Account koja je definirana kao vlasnik radnog prostora ili morate primiti pozivnicu od vlasnika da biste se uključili u radni prostor.


## <a name="to-grant-or-suspend-access-for-users"></a>Da biste dodijelili ili obustavljanje pristupa za korisnike ##

Kliknite karticu **KONFIGURACIJA** .

Na kartici konfiguracije možete učiniti sljedeće:

- Obustavljanje pristupa u radni prostor strojnog učenja tako da kliknete USKRATI. Korisnici neće moći otvoriti radni prostor u Studio strojnog učenja. Da biste vratili programa access, kliknite DOPUSTI.

Da biste upravljali dodatne račune koji imaju pristup radni prostor na računalu učenje Studio, kliknite **Prijava ML Studio** na kartici **nadzorne PLOČE** (vidi bilješku prethodnoj vezane uz **Prijava ML Studio**). Otvara se radni prostor na u Studio strojnog učenja. Na tom mjestu, kliknite karticu **Postavke** i zatim **korisnika**. Kliknite **POZOVI još korisnika** odobravanje pristupa u radni prostor ili odaberite korisnika i kliknite **UKLONI**.


## <a name="to-manage-web-services-in-this-workspace"></a>Upravljanje web-servisi u ovom radnom prostoru

Kliknite karticu **Web-SERVISA** .

Prikazat će se popis web-servisi objavljene u radni prostor.
Da biste upravljali web-servisa, kliknite naziv popisa da biste otvorili web-stranici servisa.

Web-servisa može imati jednu ili više krajnje točke definirani.

- Možete definirati više krajnje točke uz krajnju točku "Zadano". Da biste dodali krajnju točku, kliknite **Upravljanje krajnje točke** pri dnu zaslona nadzornu ploču da biste otvorili portal servisa Azure strojnog učenja Web Services.

- Da biste izbrisali krajnje (ne možete izbrisati krajnju točku "Zadano"), potvrdite okvir na početku retka krajnjoj točki i pritisnite **Izbriši**. Time se uklanja krajnju točku web-servisa.

    > [AZURE.NOTE] Ako aplikacija koristi krajnja točka servisa web nakon brisanja krajnju točku, aplikacija će primiti pogrešku sljedeći put pokuša pristup servisu.

Kliknite naziv programa Web krajnja točka servisa da biste ga otvorili. 

Na nadzornoj ploči možete pogledati cjelokupan korištenje web-usluge vremenskom razdoblju. Možete odabrati razdoblje da biste pogledali razdoblja padajućem izborniku u gornjem desnom kutu grafikona za korištenje. Na nadzornoj ploči prikazuje sljedeće informacije:

- **Zahtjevi za iznad vremena** prikazuje grafikon korak broj zahtjeva odabranom vremenskom razdoblju. Pomoću nje možete prepoznati ako ste naišli na krivina u odjeljku korištenje.
- **Zahtjevi za odgovor na zahtjev** prikazuje ukupni broj poziva odgovor na zahtjev je servis primio na odabranom vremenskom razdoblju i koliko ih nije uspjelo.
- **Vrijeme izračunati prosjek odgovor na zahtjev** prikazuje prosječnu vrijednost potrebno izvršiti primljene zahtjeva za vrijeme.
- **Zahtjevi za obradu** prikazuje ukupan broj obrade zahtjeva servis je primio na odabranom vremenskom razdoblju i koliko ih nije uspjelo.
- **Prosječne latencije posla** prikazuje prosječnu vrijednost potrebno izvršiti primljene zahtjeva za vrijeme.
- **Pogreške** prikazuje zbrajanja broj pogrešaka koji se odvijaju na pozive na web-servisa.
- **Troškovi servisa** prikazuje naknade za naplatu plan povezan sa servisom.

Na stranici Konfiguracija možete ažurirati sljedeća svojstva:

* **Opis** možete unijeti opis web-servisa. Opis nije obavezno.
* **Bilježenje u zapisnik** omogućuje vam da biste omogućili ili onemogućili zapisivanje na krajnjoj pogrešaka. Dodatne informacije o zapisivanje potražite u članku Omogući [bilježenje u zapisnik za web-servise strojnog učenja](machine-learning-web-services-logging.md).
* **Omogućivanje oglednih podataka** omogućuje vam slanje oglednih podataka koje možete koristiti da biste testirali servis za odgovor na zahtjev. Ako ste stvorili web-servisa u strojnog učenja Studio, oglednih podataka koristi se iz podataka sustava korištenih uvježbati modela. Ako ste stvorili servis programski, podataka koristi se s oglednim podacima koje ste naveli kao dio paketa JSON.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
