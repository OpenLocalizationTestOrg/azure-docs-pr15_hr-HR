<properties
   pageTitle="Praćenje i odgovarati na njih sigurnosnih upozorenja u operacije upravljanja paket sigurnost i rješenje nadzora | Microsoft Azure"
   description="Ovaj dokument pomaže vam da biste koristili mogućnost obavještavanje za prijetnju OMS sigurnost i nadzora za praćenje poruka i odgovaranje na sigurnosnih upozorenja."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Nadzor i odgovaranje na sigurnosnih upozorenja u operacije upravljanja paket sigurnost i rješenje nadzora

Ovaj dokument pomaže vam mogućnost obavještavanje za prijetnju OMS sigurnost i nadzora koristite za praćenje poruka i odgovaranje na sigurnosnih upozorenja.

## <a name="what-is-oms"></a>Što je OMS?

Microsoft operacije upravljanja paket (OMS) je tvrtke Microsoft cloud koji se temelje IT rješenja upravljanja koji olakšava upravljanje i zaštita lokalnih i infrastrukture u oblaku. Dodatne informacije o OMS potražite u članku [Paket za upravljanje operacije](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Obavještavanje prijetnju

U okruženju tvrtke gdje na koji korisnici imaju općenite pristup mreži i koristiti za široku lepezu uređaja za povezivanje s podacima tvrtke, izuzetne aktivno praćenje resurse i brzo odgovoriti sa sigurnošću je. To je posebice važne iz perspektive životni ciklus sigurnost jer se neke cybersecurity prijetnji možda Potenciranje upozorenja ili sumnjive aktivnosti koje možete otkrije tradicionalni sigurnosne tehničke kontrole. 

Pomoću mogućnosti **Prijetnju obavještavanje** koja je dostupna u OMS sigurnost i trag IT administratorima možete identificirati sigurnosnih prijetnji protiv okruženju, na primjer, Utvrdite li određenog računala je dio [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Računala mogu postati čvorove u na botnet prilikom attackers neovlašteno instalacije tajno povezuje ovo računalo naredba i kontrola od zlonamjernog softvera. Možete identificirati i potencijalnih prijetnji koji dolaze iz underground komunikacijski kanali, kao što su [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

Da biste izradili ovaj prijetnju obavještavanje, OMS sigurnost i nadzora koriste podatke koji dolaze iz više izvora u programu Microsoft. Sigurnost OMS i nadzora će pod utjecajem ti podaci za identifikaciju potencijalnih prijetnji protiv vaše okruženje.

Okno za obavještavanje prijetnju sastoji se po tri glavna mogućnosti:
- Poslužitelji odlazni promet zlonamjerni
- Otkriven prijetnji vrste
- Prijetnju obavještavanje karte

> [AZURE.NOTE] Pregled tih mogućnosti, pročitajte [Uvod u sigurnost paket za upravljanje operacije i rješenje nadzora](oms-security-getting-started.md).

### <a name="responding-to-security-alerts"></a>Odgovaranje na sigurnosnih upozorenja

Neki od koraka procesa [Sigurnost incidenta odgovor](https://technet.microsoft.com/library/cc512623.aspx) je prepoznavanje težinu system(s) narušena pouzdanost. U ovoj fazi moraju izvršiti sljedeće zadatke:

- Određivanje prirode na napada
- Određivanje točke napada origin
- Odredite svrhu u napada. Je napada posebno usmjereni u tvrtki ili ustanovi dobiti određenih informacija ili je slučajni?
- Prepoznavanje sustavima koji imaju omogućeno ugrožena
- Prepoznavanje datoteka koje ste pristupili i odrediti osjetljivost te datoteke

Omogućuje korištenje **Inteligencije prijetnju** informacije u OMS sigurnost i rješenje nadzora će vam pomoći u sljedeće zadatke. Slijedite korake u nastavku da biste pristupili ta mogućnost **Prijetnju obavještavanje** :

1. U **Paketu za upravljanje operacije Microsoft** glavna nadzorna ploča kliknite **Sigurnost i nadzora** pločica.

    ![Sigurnost i nadzora](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Nadzornoj ploči **Sigurnost i nadzora** vidjet ćete mogućnosti **Obavještavanje prijetnju** u desnom kutu kao što je prikazano u nastavku:

    ![Intel prijetnju](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Ove tri pločice steći ćete pregled trenutnog prijetnji. Na **poslužitelju za izlazni promet zlonamjerni** moći ćete prepoznati ako je bilo kojem računalu koje su nadzor (unutar ili izvan mreže) koje šalje zlonamjerni promet s Internetom. 

Pločica **Detected prijetnju vrste** prikazuje sažetak prijetnji koje su trenutno "koji", ako kliknete na toj pločici vidjet ćete dodatne detalje o te prijetnji kao što je prikazano u nastavku:

![Otkriven prijetnju vrste](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Dodatne informacije o svakom prijetnju možete izdvojiti tako da kliknete na njega. U sljedećem primjeru pokazuje dodatne detalje o Botnet:

![Dodatne informacije o prijetnju](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Kao što je opisano u početku ovog odjeljka, te podatke može biti vrlo korisna tijekom slučaj incidenta odgovor. Može biti važno tijekom forensic istraga, tamo gdje vam je potreban da biste pronašli izvor na napada, koji sustav je ugrožena i na vremensku crtu. U ovom izvješću možete jednostavno prepoznati neke ključne detalje o napada, kao što su: izvora na napada, lokalna IP koji je ugrožena i trenutna sesija stanje veze. 

**Karta obavještavanje prijetnju** će vam pomoći da biste odredili trenutnog mjesta oko globusa koji imaju zlonamjerni promet. Su narančastu (ulazni) i crvene (izlazni) strelice u ovu mapu koje označavaju smjer promet ako kliknete na jedan od ovih strelice, ona će se prikazati vrste prijetnju i smjer promet kao što je prikazano u nastavku:

![prijetnju intel karte](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [AZURE.NOTE] Vidjet ćete Pokaz kako koristiti tu mogućnost tijekom incident postupak odgovor tako da pogledate prezentacije [Mitigate podatkovnog centra sigurnosnih prijetnji s vodičem istrage pomoću operacije upravljanja paket](https://myignite.microsoft.com/videos/5000) isporučeno na Microsoft Ignite.

## <a name="see-also"></a>Vidi također

U ovom dokumentu naučili kako koristiti mogućnost **Prijetnju obavještavanje** OMS sigurnost i rješenje nadzora odgovoriti sigurnosnih upozorenja. Dodatne informacije o sigurnosti OMS potražite u sljedećim člancima:

- [Pregled operacije upravljanja paket (OMS)](operations-management-suite-overview.md)
- [Uvod u sigurnost paket za upravljanje operacije i rješenje nadzora](oms-security-getting-started.md)
- [Nadzor resursa u operacije upravljanja paket sigurnost i rješenje nadzora](oms-security-monitoring-resources.md)
