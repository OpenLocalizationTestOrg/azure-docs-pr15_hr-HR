<properties
    pageTitle="Kontroliranje Azure CDN Premium s Verizon predmemoriranje ponašanje zahtjeva s nizovima upita | Microsoft Azure"
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

#<a name="controlling-caching-behavior-of-cdn-requests-with-query-strings---premium"></a>Omogućivanje predmemoriranja ponašanje zahtjeva za CDN s nizovima upita - Premium

> [AZURE.SELECTOR]
- [Standardna](cdn-query-string.md)
- [Azure CDN Premium s Verizon](cdn-query-string-premium.md)

##<a name="overview"></a>Pregled

Niz upita predmemoriranje kontrole kako su datoteke predmemoriju kada oni sadrže nizove upita.

> [AZURE.IMPORTANT] Proizvodi Standard i Premium CDN nude isti niz upita za predmemoriranje funkcionalnost, ali razlikuje korisničkog sučelja.  U ovom dokumentu opisuje sučelje za **Azure CDN Premium s Verizon**.  Upit niz predmemoriranje s **Azure CDN standardne iz Akamai** i **Azure CDN standardne iz Verizon**potražite u članku [Controlling predmemoriranja ponašanje zahtjeva za CDN s nizovima upita](cdn-query-string.md).

Dostupne su tri načina:

- **Standardna predmemorije**: to je zadani način.  Čvor rub CDN proći kroz nizu upita iz podnositelja zahtjeva za izvor na prvi zahtjev i predmemorije sredstava.  Sve daljnji zahtjevi za tog resursa koja su služila čvorovi rub će zanemariti nizu upita dok se ne ističe predmemorirani resursa.
- **bez predmemorije**: U tom načinu rada se zahtjevi za s nizovima upita su predmemorirani na rub čvor CDN.  Čvor rub dohvaća imovine izravno iz izvor i prosljeđuje podnositelja zahtjeva svaki zahtjev.
- **Jedinstveni predmemorije**: ovaj način smatra svaki zahtjev s nizom upita jedinstveni imovine s vlastitom predmemoriju.  Na primjer, odgovor polazište za zahtjev za *foo.ashx?q=bar* će biti predmemorirani na rub čvor sustavu i vraćen za narednih predmemorije s tom istog niza upita.  Zahtjev za *foo.ashx?q=somethingelse* bi biti predmemoriranih kao zasebna sredstva s vlastitom vrijeme Live.

##<a name="changing-query-string-caching-settings-for-premium-cdn-profiles"></a>Promjena predmemoriranje postavki profila CDN premium nizu upita

1. Iz profila plohu CDN kliknite gumb **Upravljanje** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-query-string-premium/cdn-manage-btn.png)

    Otvorit će se na portal za upravljanje CDN.

2. Zadržite pokazivač na kartici **Velike HTTP** , a zatim zadržite pokazivač na Potpaleta **Postavke predmemorije** .  Kliknite **predmemoriranje nizu upita**.

    Prikazuju se mogućnosti za predmemoriranja niza upita.

    ![Niz upita CDN predmemoriranje mogućnosti](./media/cdn-query-string-premium/cdn-query-string.png)

3. Nakon toga odabir, kliknite gumb **Ažuriraj** .


> [AZURE.IMPORTANT] Promjene postavki možda neće biti vidljive, kao što je potrebno vrijeme za registraciju proširiti kroz na CDN.  Profili <b>CDN Azure s Verizon</b> prijenos će obično dovrši unutar 90 minuta, ali u nekim slučajevima može trajati dulje.
