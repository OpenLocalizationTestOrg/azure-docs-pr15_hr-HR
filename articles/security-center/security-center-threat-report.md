<properties
   pageTitle="Centar za sigurnost Azure prijetnju obavještavanje izvješća | Microsoft Azure"
   description="Ovaj dokument omogućuje korištenje Azure sigurnost centar prijetnju Inteligentna izvješća tijekom istrazi da biste pronašli dodatne informacije o sigurnosnom upozorenju."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-threat-intelligence-report"></a>Centar za sigurnost Azure prijetnju obavještavanje izvješća
Ovaj dokument objašnjava kako Azure sigurnost centar prijetnju Inteligentna izvješća mogu pomoći dodatne informacije o prijetnju koji je generirao sigurnosno upozorenje.

## <a name="what-is-a-threat-intelligence-report"></a>Izvješće za obavještavanje prijetnju
Centar za sigurnost prijetnju otkrivanje funkcionira prateći sigurnosnih informacija iz Azure resursa, mreže i povezani partnerskih rješenja. Ga analizira te informacije, često correlating podatke iz više izvora, da biste odredili prijetnji. Ovaj postupak je dio centar za sigurnost [otkrivanje mogućnosti](security-center-detection-capabilities.md). 

Kada centar za sigurnost prepozna prijetnju, će pokrenuti u [sigurnosnom upozorenju](security-center-managing-and-responding-alerts.md)koja sadrži detaljne informacije o određenom događaju, uključujući prijedloge za olakšava. Kako bi incidenta odgovor timovima istražili i remediate prijetnji, centar za sigurnost obuhvaća izvješća za obavještavanje prijetnju koji sadrži informacije o prijetnju otkrivenog, uključujući podatke kao što su na: 

- Identitet napadač korisnika ili pridruživanja (Ako je taj podatak je dostupan)
- Ciljevi attackers'
- Trenutni i prošli napada kampanje (Ako je taj podatak je dostupan)
- Attackers' taktike, alate i postupci
- Pridruženi pokazatelja narušena pouzdanost (IoC) kao što su URL-ovi i raspršivanja datoteka
- Victimology koji je industrijskih i geografske rasprostranjenosti kao pomoć u određivanju ako su resursi za Azure
- Informacije o ublažiti i potražite alat za

>[AZURE.NOTE] Razlikovat će se količina podataka u bilo kojem određenog izvješća; razinu detalja temelji se na aktivnosti i rasprostranjenosti na zlonamjernog softvera.

Centar za sigurnost ima tri vrste prijetnju izvješća, koja može se razlikovati ovisno o na napada. Izvješća koja je dostupna su:

- **Izvješće o aktivnosti grupe**: nudi precizno dives u attackers, njihove ciljeva i taktike.
- **Izvješće o kampanje**: usredotočuje se na Detalji o određenim napada kampanja. 
- **Sažetak prijetnju**: pokriva sve stavke u prethodna dva izvješća.

Takvu vrstu podataka vrlo je praktična tijekom postupka [incidenta odgovor](security-center-incident-response.md) , gdje je istrazi tijeku da biste shvatili izvora na napada, na napadač motivations i što učiniti da biste smanjili taj se problem naprijed. 

## <a name="how-to-access-the-threat-intelligence-report"></a>Kako pristupiti obavještavanje izvješća prijetnju?

Trenutni upozorenja možete pregledati tako da pogledate pločicu **sigurnosnih upozorenja** . Otvorite Azure Portal i slijedite korake u nastavku da biste vidjeli dodatne detalje o svakom upozorenju:

1. Na nadzornoj ploči za centar za sigurnost, vidjet ćete pločicu **sigurnosnih upozorenja** .

2. Kliknite pločicu da biste otvorili plohu **sigurnosnih upozorenja** koja sadrži više detalja o upozorenja, a zatim kliknite u sigurnosnom upozorenju koje želite da biste dobili dodatne informacije o.

    ![Sigurnosna upozorenja](./media/security-center-threat-report/security-center-threat-report-fig1.png)

3. U ovom slučaju plohu **sumnjiva postupak izvršava** prikazuje detalje o upozorenje kao što je prikazano na slici u nastavku:

    ![Sigurnosna upozorenja detais](./media/security-center-threat-report/security-center-threat-report-fig2.png)

4.  Iznos informacija koje su dostupne za svaki sigurnosnom upozorenju razlikuju se ovisno o vrsti upozorenja. U polju **izvješća** dostupna je veza za obavještavanje izvješća prijetnju. Kliknite je, a drugi prozor preglednika pojavit će se s PDF datoteke.

    ![Spremanje odabira](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Na tom mjestu možete preuzeti PDF-a za izvješća i čitanje dodatne informacije o sigurnošću otkrivenog i poduzeti koji se temelji na informacije.

## <a name="see-also"></a>Vidi također

U ovom dokumentu naučili kako može pomoći Azure sigurnost centar prijetnju Inteligentna izvješća tijekom istrazi sigurnosnih upozorenja o. Da biste saznali više o centru za sigurnost Azure, pogledajte sljedeće:

- [Najčešća pitanja o centru za sigurnost azure](security-center-faq.md). Najčešća pitanja o korištenju servis za pretraživanje.
- [Korištenje centra za sigurnost Azure incidenta odgovor](security-center-incident-response.md)
- [Centar za sigurnost Azure otkrivanje mogućnosti](security-center-detection-capabilities.md)
- [Vodič za planiranje azure centar za sigurnost i operacije](security-center-planning-and-operations-guide.md). Saznajte kako planirati i razumijevanje zahtjevi dizajna prihvaćaju Azure centar za sigurnost.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md). Saznajte kako upravljati i odgovaranje na sigurnosnih upozorenja.
- [Rukovanje incident vezan uz sigurnost u centru za sigurnost Azure](security-center-incident.md)
- [Blog azure sigurnost](http://blogs.msdn.com/b/azuresecurity/). Pronađite bloga o Azure sigurnost i usklađenost.
