<properties
    pageTitle="Otklanjanje poteškoća s sažimanje datoteke u Azure CDN | Microsoft Azure"
    description="Otklanjanje poteškoća s Azure CDN sažimanje datoteke."
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
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>Sažimanje datoteke CDN za otklanjanje poteškoća

Ovaj članak sadrži otkloniti poteškoće vezane uz [CDN sažimanje datoteke](cdn-improve-performance.md).

Ako vam je potrebna dodatna pomoć u ovom članku u bilo kojem trenutku, možete se obratiti Azure stručnjaka na [MSDN Azure i na forumima prelijevanje stogu](https://azure.microsoft.com/support/forums/). Osim toga, možete uputiti incident Azure podršci. Idi na [web-mjesto za podršku za Azure](https://azure.microsoft.com/support/options/) i kliknite **Dohvati podrška**.

## <a name="symptom"></a>Simptoma

Sažimanje na krajnjoj točki je omogućen, ali datoteke koja se vraćaju koje nisu komprimirane.

>[AZURE.TIP] Da biste provjerili je li datoteka koja se vraćaju komprimirana, morate koristiti alat kao što su [Fiddler](http://www.telerik.com/fiddler) ili web-pregledniku [alate za razvojne inženjere](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).  Potvrdite HTTP odgovor zaglavlja vraćaju s vašeg predmemorirani CDN sadržaja.  Ako je zaglavlje pod nazivom `Content-Encoding` je vrijednost **gzip**, **bzip2**ili **Umanji**sadržaj komprimiranja.
>
>![Sadržaj kodiranje zaglavlja](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>Uzrok

Postoji nekoliko mogućih razloga, uključujući:

- Tražene sadržaj ne ispunjava uvjete za sažimanje.
- Sažimanje nije omogućena za vrstu tražene datoteke.
- HTTP zahtjev niste uključili zaglavlja traži vrsta valjani kompresije.

## <a name="troubleshooting-steps"></a>Korake za otklanjanje poteškoća

> [AZURE.TIP] Kao i u slučaju implementacije novog krajnje točke promjene konfiguracije CDN potrajati proširiti putem mreže.  Obično se promjene primjenjuju za 90 minuta.  Ako je ovo prvi put postavite sažimanja za krajnju točku sustava CDN, razmislite o čekanje 1-2 sata da biste bili sigurni da spajanja postavke ste propagirati na iskače. 

### <a name="verify-the-request"></a>Provjerite je li zahtjev

Najprije ćemo biste trebali učiniti provjere brzi sanity na zahtjev.  Možete koristiti u web-pregledniku [razvojnih alata](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) da biste vidjeli zahtjeve unose.

- Provjerite je li zahtjev koje se šalju na krajnjoj točki URL-a `<endpointname>.azureedge.net`, a ne na izvor.
- Provjerite je li zahtjev sadrži zaglavlja za **Prihvaćanje kodiranje** , a vrijednost za tog zaglavlja **gzip**, **Umanji**ili **bzip2**.

> [AZURE.NOTE] Profili **CDN Azure s Akamai** podržavaju samo **gzip** kodiranja.

![CDN zahtjev za zaglavlja](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Provjera postavki sažimanja (standardni CDN profil)

> [AZURE.NOTE] Ovaj korak primjenjuje se samo ako je vaš profil CDN profil programa **Azure CDN standardno iz Verizon** ili **Azure CDN standardne iz Akamai** . 

Idite na krajnjoj točki [Azure portal](https://portal.azure.com) i kliknite gumb **Konfiguracija** .

- Provjerite je li omogućen sažimanja.
- Provjerite je li MIME vrsta sadržaja da biste se sažeti je sve obuhvaćeno popisa Komprimirana oblika.

![CDN postavki sažimanja](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Provjera postavki sažimanja (Premium CDN profil)

> [AZURE.NOTE] Ovaj korak primjenjuje se samo ako je vaš profil CDN profil programa **Azure CDN Premium s Verizon** .

Idite na krajnjoj točki [Azure portal](https://portal.azure.com) i kliknite gumb **Upravljanje** .  Portal za dodatni će se otvoriti.  Zadržite pokazivač na kartici **Velike HTTP** , a zatim zadržite pokazivač na Potpaleta **Postavke predmemorije** .  Kliknite **sažimanja**. 

- Provjerite je li omogućen sažimanja.
- Provjerite je li na popisu **Vrsta datoteka** s popisom odvojenih zarezom (bez razmaka) MIME vrstama.
- Provjerite je li MIME vrsta sadržaja da biste se sažeti je sve obuhvaćeno popisa Komprimirana oblika.

![Postavke kompresije premium CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Provjerite je li nije lokalno sadržaju

> [AZURE.NOTE] Ovaj korak primjenjuje se samo ako je vaš profil CDN profila **CDN Azure s Verizon** (standardni prikaz ili Premium).

Pomoću preglednika razvojnih alata, potvrdite okvir zaglavlja odgovora da biste bili sigurni datoteka može biti spremljen u regiji u kojima se traži.

- Potvrdite okvir zaglavlja odgovora **poslužitelja** .  U zaglavlju mora imati oblik **platformu (POP poslužitelju ID)**, kao što se vidi u sljedećem primjeru.
- Potvrdite okvir zaglavlja odgovora **X predmemoriju** .  U zaglavlju namijenjen **KLIKNITE**.  

![CDN odgovor zaglavlja](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Provjerite je li datoteka zadovoljava preduvjete veličina

> [AZURE.NOTE] Ovaj korak primjenjuje se samo ako je vaš profil CDN profila **CDN Azure s Verizon** (standardni prikaz ili Premium).

Da biste se ispunjava uvjete za spajanje, datoteke mora zadovoljiti sljedeće preduvjete veličina:

- Veće od 128 bajtova.
- Manja od 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Potvrdite zahtjev na poslužitelju izvor zaglavlja **putem**

Zaglavlje **putem** HTTP-a pokazuje na web-poslužitelj zahtjev je u tijeku proslijeđena proxy poslužitelj.  Microsoft IIS web-poslužiteljima prema zadanim postavkama ne sažimaj odgovora kada zahtjev sadrži zaglavlja **putem** .  Da biste nadjačali taj problem, učinite sljedeće:

- **IIS 6**: [Postavljanje HcNoCompressionForProxies = "FALSE" u IIS metabaze svojstva](https://msdn.microsoft.com/library/ms525390.aspx)
- **IIS 7 i do**: [Postavljanje **noCompressionForHttp10** i **noCompressionForProxies** FALSE u konfiguraciji poslužitelja](http://www.iis.net/configreference/system.webserver/httpcompression)

