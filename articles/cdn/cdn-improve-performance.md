<properties
    pageTitle="Poboljšanje performansi komprimiranjem u Azure CDN | Microsoft Azure"
    description="Saznajte kako da biste poboljšali brzinu prijenosa datoteka i povećava performansi učitavanja stranice za sažimanje datoteka u Azure CDN."
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

# <a name="improve-performance-by-compressing-files"></a>Poboljšanje performansi komprimiranjem

Sažimanje je jednostavno i učinkovito način da biste poboljšali brzinu prijenosa datoteka i poboljšati performanse učitavanja stranice smanjivanjem veličine datoteke prije slanja s poslužitelja. Smanjuje troškove propusnosti i omogućuje brže raditi sučelje za korisnike.

Da biste omogućili sažimanja na dva načina:

- Sažimanje možete omogućiti na poslužitelj za izvor, u kojem slova u CDN će proći kroz komprimirane datoteke i isporuka komprimirane datoteke klijenata kojima se traže ih.
- Možete omogućiti sažimanje izravno na CDN edge poslužitelji, u kojem slova u CDN će komprimiranje datoteka, a služe krajnjim korisnicima, čak i ako nije komprimiran polazište poslužitelj.

> [AZURE.IMPORTANT] Promjene konfiguracije CDN potrajati proširiti putem mreže.  Profili <b>CDN Azure s Akamai</b> prijenos se obično dovršava u odjeljku jedne minute.  Za <b>Azure CDN iz Verizon</b> profila, obično vidjet ćete promjene primijeniti u roku od 90 nekoliko minuta.  Ako je ovo prvi put postavite sažimanja za krajnju točku sustava CDN, trebali biste čekanje 1-2 sata da biste bili sigurni da spajanja postavke imaju propagirati iskače prije otklanjanje poteškoća

## <a name="enabling-compression"></a>Omogućivanje sažimanja

> [AZURE.NOTE] Razine Standard i Premium CDN – ista funkcija spajanja, ali razlikuje korisničkog sučelja.  Dodatne informacije o razlikama između Standard i Premium CDN razine potražite u članku [Pregled CDN Azure](cdn-overview.md).

### <a name="standard-tier"></a>Standardni sloju

> [AZURE.NOTE] U ovom se odjeljku primjenjuje se na **Azure CDN standardne iz Verizon** i **Azure CDN standardne iz Akamai** profila.

1. Iz profila plohu CDN kliknite krajnju točku CDN želite upravljati.

    ![Krajnje točke plohu za CDN profila](./media/cdn-file-compression/cdn-endpoints.png)

    Otvorit će se krajnjoj točki plohu CDN-a.

2. Kliknite gumb **Konfiguracija** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-file-compression/cdn-config-btn.png)

    Otvorit će se plohu CDN konfiguracije.

3. Uključite **sažimanja**.

    ![Mogućnosti CDN sažimanja](./media/cdn-file-compression/cdn-compress-standard.png)

4. Koristite zadane vrste ili izmjena popisa uklanjanjem ili dodavanje vrste datoteka.
    
    > [AZURE.TIP] Dok je to moguće, ne preporučuje se da biste primijenili sažimanje Komprimirana oblicima, kao što je ZIP MP3, MP4, JPG, itd.
    
5. Kada unesete promjene, kliknite gumb **Spremi** .

### <a name="premium-tier"></a>Razina Premium

> [AZURE.NOTE] U ovom se odjeljku primjenjuje se na **Azure CDN Premium s Verizon** profila.

1. Iz profila plohu CDN kliknite gumb **Upravljanje** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-file-compression/cdn-manage-btn.png)

    Otvorit će se na portal za upravljanje CDN.

2. Zadržite pokazivač na kartici **Velike HTTP** , a zatim zadržite pokazivač na Potpaleta **Postavke predmemorije** .  Kliknite **sažimanja**.

    Prikazuju se mogućnosti sažimanja.

    ![Sažimanje datoteke](./media/cdn-file-compression/cdn-compress-files.png)

3. Omogućite sažimanje tako da kliknete izborni gumb **Sažimanja omogućeno** .  Unesite MIME vrstama želite komprimirati kao popis razdvojen zarezom (bez razmaka) u tekstni okvir **Vrste datoteka** .
        
    > [AZURE.TIP] Dok je to moguće, ne preporučuje se da biste primijenili sažimanje Komprimirana oblicima, kao što je ZIP MP3, MP4, JPG, itd. 

4. Kada unesete promjene, kliknite gumb **Ažuriraj** .


## <a name="compression-rules"></a>Sažimanje pravila

U ovim su tablicama opisuju Azure CDN sažimanja ponašanje za svaku scenarij.

> [AZURE.IMPORTANT] Za **Azure CDN iz Verizon** (standardna i Premium), spojene će se samo uvjete datoteke.  Da biste ispunjava uvjete za sažimanje, datoteke mora:
>
> - Biti veća od 128 bajtova.
> - Biti manja od 1 MB.
> 
> Za **Azure CDN iz Akamai**uvjete za sažimanje su sve datoteke.
>
> Svi proizvodi Azure CDN datoteke mora biti MIME vrsta koja nije [konfiguriran za spajanje](#enabling-compression).
>
> Profili **CDN Azure s Verizon** (standardna i Premium) podržava **gzip**, **Umanji**ili **bzip2** kodiranja.  Profili **CDN Azure s Akamai** podržavaju samo **gzip** kodiranja.
>
> **Azure CDN iz Akamai** krajnje točke uvijek zahtjev **gzip** kodirani datoteke od izvora, bez obzira na zahtjev klijenta.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Sažimanje onemogućena ili datoteke se neće podržavati sažimanja

|Klijent traženi oblik (putem Prihvati kodiranje zaglavlje)|Oblik predmemorirane datoteke|CDN odgovor na klijentu|Bilješke|
|----------------|-----------|------------|-----|
|Sažeti|Sažeti|Sažeti|   |
|Sažeti|Koje nisu komprimirane|Koje nisu komprimirane|    | 
|Sažeti|Ne predmemorirani|Sažete ili dekomprimirati|Ovisi o polazište odgovor|
|Koje nisu komprimirane|Sažeti|Koje nisu komprimirane|    |
|Koje nisu komprimirane|Koje nisu komprimirane|Koje nisu komprimirane|    |   
|Koje nisu komprimirane|Ne predmemorirani|Koje nisu komprimirane|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Sažimanje omogućeno i datoteku ispunjava uvjete za spajanje

|Klijent traženi oblik (putem Prihvati kodiranje zaglavlje)|Oblik predmemorirane datoteke|CDN odgovor na klijentu|Bilješke|
|----------------|-----------|------------|-----|
|Sažeti|Sažeti|Sažeti|CDN transcodes između Podržani oblici|
|Sažeti|Koje nisu komprimirane|Sažeti|CDN izvodi sažimanja|
|Sažeti|Ne predmemorirani|Sažeti|CDN izvodi sažimanja polazište vraća koje nisu komprimirane.  **Azure CDN iz Verizon** će prenesite datoteke koje nisu komprimirane na zahtjev za prvi i zatim sažimanja i predmemoriju za daljnji zahtjevi.  Datoteke s `Cache-Control: no-cache` zaglavlje nikad se sažeti. 
|Koje nisu komprimirane|Sažeti|Koje nisu komprimirane|CDN izvodi dekompresiju|
|Koje nisu komprimirane|Koje nisu komprimirane|Koje nisu komprimirane|     |  
|Koje nisu komprimirane|Ne predmemorirani|Koje nisu komprimirane|     |

## <a name="media-services-cdn-compression"></a>Media Services CDN sažimanja

Za Media Services CDN omogućena strujanje krajnje točke, sažimanje prema zadanim postavkama omogućen za sljedeće vrste sadržaja: vnd.ms/aplikacije-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, aplikacije/f4m + xml. Koju ne možete omogućiti ili onemogućiti sažimanje spomenuta vrste pomoću portala za Azure.  

## <a name="see-also"></a>Vidi također
- [Sažimanje datoteke CDN za otklanjanje poteškoća](cdn-troubleshoot-compression.md)    
