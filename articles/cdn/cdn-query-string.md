<properties
    pageTitle="Kontroliranje Azure CDN predmemoriranje ponašanje zahtjeva s nizovima upita | Microsoft Azure"
    description="Azure CDN nizu upita predmemoriranje kontrole kako su datoteke predmemoriju kada oni sadrže nizove upita."
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings"></a>Omogućivanje predmemoriranja ponašanje zahtjeva za CDN s nizovima upita

> [AZURE.SELECTOR]
- [Standardna](cdn-query-string.md)
- [Azure CDN Premium s Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Pregled

Niz upita predmemoriranje kontrole kako su datoteke predmemoriju kada oni sadrže nizove upita.

> [AZURE.IMPORTANT] Proizvodi Standard i Premium CDN nude isti niz upita za predmemoriranje funkcionalnost, ali razlikuje korisničkog sučelja.  Ovaj dokument opisuje sučelje za **Azure CDN standardne iz Akamai** i **Azure CDN standardne iz Verizon**.  Upit niz predmemoriranje s **Azure CDN Premium s Verizon**potražite u članku [Controlling predmemoriranja ponašanje zahtjeva za CDN s nizovima upita - Premium](cdn-query-string-premium.md).

Dostupne su tri načina:

- **Zanemari upita nizova**: to je zadani način.  Čvor rub CDN proći kroz nizu upita iz podnositelja zahtjeva za izvor na prvi zahtjev i predmemorije sredstava.  Sve daljnji zahtjevi za tog resursa koja su služila čvorovi rub će zanemariti nizu upita dok se ne ističe predmemorirani resursa.
- **Predmemoriranje zaobilazak za URL-a s nizovima upit**: U tom načinu rada se zahtjevi za s nizovima upita su predmemorirani na rub čvor CDN.  Čvor rub dohvaća imovine izravno iz izvor i prosljeđuje podnositelja zahtjeva svaki zahtjev.
- **Predmemoriju svaki jedinstveni URL**: ovaj način smatra svaki zahtjev s nizom upita jedinstveni imovine s vlastitom predmemoriju.  Na primjer, odgovor polazište za zahtjev za *foo.ashx?q=bar* će biti predmemorirani na rub čvor sustavu i vraćen za narednih predmemorije s tom istog niza upita.  Zahtjev za *foo.ashx?q=somethingelse* bi biti predmemoriranih kao zasebna sredstva s vlastitom vrijeme Live.

##<a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Promjena predmemoriranje postavke za standardni profili CDN nizu upita

1. Iz profila plohu CDN kliknite krajnju točku CDN želite upravljati.

    ![Krajnje točke plohu za CDN profila](./media/cdn-query-string/cdn-endpoints.png)

    Otvorit će se krajnjoj točki plohu CDN-a.

2. Kliknite gumb **Konfiguracija** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-query-string/cdn-config-btn.png)

    Otvorit će se plohu CDN konfiguracije.

3. Odaberite postavku na padajućem izborniku **predmemoriranje ponašanje nizu upita** .

    ![Niz upita CDN predmemoriranje mogućnosti](./media/cdn-query-string/cdn-query-string.png)

4. Nakon toga odabir, kliknite gumb **Spremi** .

> [AZURE.IMPORTANT] Promjene postavki možda neće biti vidljive, kao što je potrebno vrijeme za registraciju proširiti kroz na CDN.  Prijenos će za <b>Azure CDN iz Akamai</b> profila, obično dovrši unutar jedne minute.  Profili <b>CDN Azure s Verizon</b> prijenos će obično dovrši unutar 90 minuta, ali u nekim slučajevima može trajati dulje.
