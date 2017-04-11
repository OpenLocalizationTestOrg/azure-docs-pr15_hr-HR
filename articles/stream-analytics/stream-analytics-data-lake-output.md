<properties
    pageTitle="Izlaz strujanje analize podataka Lake spremište | Microsoft Azure"
    description="Konfiguriranje provjere autentičnosti i autorizacije spremišta Lake podataka Azure u analize toka posla"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Izlaz spremišta Lake za strujanje analize podataka

Strujanje analize Poslovi podržava nekoliko metoda izlaz nešto u [Spremištu Lake za Azure podataka](https://azure.microsoft.com/services/data-lake-store/)u tijeku. Azure podataka Lake spremište je spremište hyper skaliranje enterprise razini analitički radnih opterećenja velikih skupova podataka. Pohrana podataka Lake omogućuje vam da biste pohranili podatke veličinu, vrstu i ingestion brzinu domena i exploratory analitičkih.

## <a name="authorize-a-data-lake-store-account"></a>Autorizirajte spremišta podataka Lake računa

1.  Pohrana podataka Lake odabrano kao na Izlaz na portalu za upravljanje Azure zatražit će se da biste autorizirali korištenje postojeće spremište Lake podataka ili da biste zatražili pristup pretpregleda pohrane podataka Lake putem portala za klasični Azure.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  Ako već imate pristup spremišta Lake podataka, kliknite "Autorizirali sada" i kratko vrijeme stranice pojavit će se koji označava "Redirecting za autorizaciju...". Stranica automatski će se zatvoriti, a koje će se prikazivati stranica koje će vam omogućuju da Konfiguriranje izlazne spremišta Lake podataka.

Ako ste se ne registrirali za pregled spremište Lake podataka, slijedite vezu "Registrirajte" da biste započeli zahtjev ili slijedite [Početak rada upute](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>Konfiguriranje izlazne svojstava spremišta Lake podataka

Nakon što dodate račun spremišta podataka Lake autentičnost, možete konfigurirati svojstva za izlaz iz trgovine Lake podataka. U tablici u nastavku je popis naziva svojstava i njihov opis da biste konfigurirali spremišta podataka Lake izlaz.

<table>
<tbody>
<tr>
<td><B>NAZIV SVOJSTVA</B></td>
<td><B>OPIS</B></td>
</tr>
<tr>
<td>Pseudonim za izlaz</td>
<td>Ovo je neslužbeni naziv koji se koristi u upitima za usmjeravanje izlaz upita u ovom Lake spremišta podataka.</td>
</tr>
<tr>
<td>Pohrana podataka Lake računa</td>
<td>Naziv računa za pohranu koju šaljete na izlaz. Primit ćete s padajućeg popisa računa spremišta podataka Lake kojoj prijavljeni na portal korisnik ima pristup.</td>
</tr>
<tr>
<td>Put prefiks uzorak [<I>neobavezni</I>]</td>
<td>Put datoteke za pisanje datoteka unutar navedeni račun pohrane podataka Lake. <BR>{date}, {time}<BR>Primjer 1: folder1/zapisnika / {date} / {vremena}<BR>Primjer 2: folder1/zapisnika / {date}</td>
</tr>
<tr>
<td>Oblik datuma [<I>neobavezni</I>]</td>
<td>Ako se u put prefiks koristi token datum, možete odabrati oblik datuma u kojima su organizirane datotekama. Primjer: Gggg/MM/DD</td>
</tr>
<tr>
<td>Oblik vremena [<I>neobavezni</I>]</td>
<td>Ako se u put prefiks koristi token vrijeme, navedite oblik vremena u kojem su organizirane datotekama. Trenutno je jedini podržani vrijednost HH.</td>
</tr>
<tr>
<td>Događaj serijaliziranog oblika</td>
<td>Oblik serijalizacije za izlazne podatke. Podržane su JSON, CSV i Avro.</td>
</tr>
<tr>
<td>Šifriranje</td>
<td>Ako je oblik CSV ili JSON, kodiranja mora biti navedena. UTF-8 trenutno samo podržani oblik kodiranja.</td>
</tr>
<tr>
<td>Graničnik</td>
<td>Samo primjenjivo za serijalizacije CSV. Analitički strujanje podržava nekoliko uobičajenih razdjelnici za Serijalizacija CSV podatke. Podržani vrijednosti su zarezom sa zarezom, razmak, uvlaka i okomitu crtu.</td>
</tr>
<tr>
<td>Oblikovanje</td>
<td>Samo primjenjivo za serijalizacije JSON. Redak odvojene određuje izlaz će se oblikovati tako da svaki objekt JSON odvojene novi redak. Polja određuje izlaz će se oblikovati kao polje JSON objekata.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Obnavljanje autorizacije spremišta Lake podataka

Trenutno postoji ograničenje gdje token za provjeru autentičnosti potrebno ručno osvježiti svakih 90 dana za sve zadatke s spremišta podataka Lake izlaz. Također morat ćete ponovno autentičnost računa spremišta podataka Lake ako ste promijenili lozinku jer je stvoren ili zadnje autentičnost posla. Simptoma tog problema je bez izlaz posla i pogreške u zapisnik postupka koji označava potrebe za ponovnu provjeru autentičnosti.

Da biste riješili taj problem, zaustavljanje pokrenute posla i idite na Izlaz spremišta Lake podataka. Kliknite vezu "Obnavljanje autorizacije", a za kratko vrijeme stranice pojavit će se koji označava "Redirecting za autorizaciju...". Na stranici automatski će se zatvoriti, a ako ne uspije, će vas upozoriti "Autorizacije je uspješno obnovljena". Zatim treba kliknite "Spremi" pri dnu stranice, a možete nastaviti ponovnim pokretanjem posla s prekinuli posljednjeg da biste izbjegli gubitak podataka.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)
