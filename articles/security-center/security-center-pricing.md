<properties
   pageTitle="Centar za sigurnost cijene | Microsoft Azure"
   description="Ovaj članak sadrži informacije na cijene za centar za sigurnost Azure."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/12/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-pricing"></a>Centar za sigurnost Azure cijene

Centar za sigurnost Azure pomaže spriječiti, otkrivanje i odgovoriti prijetnji s bolje vidio u i kontrolu nad sigurnost Azure resurse. Pruža integriranu sigurnost nadzor i pravila upravljanja preko pretplate Azure, pomaže u otkrivanju prijetnji koje možda u suprotnom otvorite uočen i radi s Bogata paleta sigurnost rješenja.

## <a name="pricing-tiers"></a>Cijene razine

Centar za sigurnost je ponuđen u dvije razine:

- **Besplatni sloju** je automatski omogućena na sve Azure pretplate. Besplatni sloju daje uvid u stanja sigurnosti Azure resursa, osnovni sigurnosna pravila, preporuke o sigurnosti i Integracija sa sigurnosnim proizvodima i uslugama od partnera.
- **Standardni sloju** dodaje značajke otkrivanja napredne prijetnju, uključujući prijetnju obavještavanje, behavioral analizu, značajkom prepoznavanja, sigurnošću i prijetnju rizika izvješća. **Besplatna probna verzija na 90 dana** dostupna je za standardne sloju.

Dodatne informacije potražite u članku centar za sigurnost [cijene stranice](https://azure.microsoft.com/pricing/details/security-center/).

> [AZURE.NOTE] Centar za sigurnost koristi Azure prostora za pohranu za spremanje sigurnosne podatke koji su stvorene iz vaše zaštićeni čvorove. Troškove vezane uz ovaj prostor za pohranu nisu uvršteni u cijenu servisa i se naplaćuju zasebno po običnog [stope Azure prostora za pohranu](https://azure.microsoft.com/pricing/details/storage/blobs/). Prostor za pohranu naplaćuje čak i tijekom probnog razdoblja.

## <a name="try-standard-free-for-90-days"></a>Pokušajte standardna besplatno za 90 dana

90 dana za besplatnu probnu verziju dostupna je za standardne sloju. Da biste dobili besplatnu probnu verziju sustava standardne sloju, odaberite pločicu **pravila** na plohu **Centar za sigurnost** . Odaberite pretplatu u koju želite li nadograditi na standardni. Na plohu **Sigurnosna pravila** odaberite **sloj određivanje cijena**. Na plohu **Odabir vaše cijene sloju** odaberite **standardnu – besplatnu probnu verziju**.

![Besplatna probna verzija][1]

Na kraju 90 dana, trebali biste odabrati da biste nastavili koristiti servisa, ne možemo će se automatski pokrenuti puni za korištenje.

## <a name="upgrade-to-standard"></a>Nadogradnja na standardnu

Nadogradnja na standardni sloju da biste dodali dodatne prijetnju otkrivanje. Da biste dobili standardne sloju, odaberite pločicu **pravila** na plohu **Centar za sigurnost** . Odaberite pretplatu u koju želite li nadograditi na standardni. Na plohu **Sigurnosna pravila** odaberite **sloj određivanje cijena**. Na plohu **Odabir vaše cijene sloju** odaberite **Standard**.

![Standardni sloju][2]

## <a name="why-upgrade-to-standard"></a>Zašto se nadograditi na standardni?

Standardni sloj centar za sigurnost nudi sve značajke besplatne sloju plus otkrivanje napredne prijetnju. Otkrivanje napredne prijetnju pomaže prepoznati aktivan prijetnji ciljanja Azure resurse i daje uvid potrebno odgovoriti brzo.

Centar za sigurnost uključuje dodatnom sigurnošću analize koja je daleko okružje utemeljen na potpis pristupa. Otkrića u velikih skupova podataka i strojnog učenja tehnologije leveraged se treba vrednovati događaje preko cijele oblaka tkanina – otkrivanje prijetnji koje bi nemoguće prepoznavanje pomoću ručno pristupa i procjenjivanje proizvod napada.

Analitički sigurnosti koje se isporučuju sa standardnom sloju su:

- **Obavještavanje prijetnju** - traži poznati bad Glumci pomoću globalni prijetnju obavještavanje iz Microsoftove proizvode i usluge, Microsoft digitalni Crimes jedinica, Microsoft Security Response Center i vanjske sažetaka sadržaja
- **Behavioral analizu** - primjenjuje poznati uzoraka da biste otkrili zlonamjerni ponašanje
- **Otkrivanje značajkom** - koristi statističke Profiliranje radi stvaranja povijesne referentne vrijednosti. Upozorava na devijacija od utvrđene referente vrijednosti koje odgovaraju potencijalne vektor napada

U na plohu **sigurnosnih upozorenja** ispod, centar za sigurnost otkrio sigurnost **incident**. Sigurnost incident je prikupljene sva upozorenja za resurs poravnavanje uzorcima lanac za uklanjanje. Odabir incident vezan uz sigurnost otkriva dodatne detalje o incidentu web-mjesta i popisa povezanih upozorenja. Odabir upozorenja navedene su dodatne informacije o tom pojavljivanje.

![Incident vezan uz sigurnost][3]

Upozorenje o **mrežnu komunikaciju** u nastavku navedeni Detalji o upozorenju. Detalji sadržavati njezin opis cijelog, njegov težinu, njegovu trenutnom stanju (koji se u tom slučaju je nehotice, što znači da korisnik snimljene akcije da biste odbacili) napada resursa i potražite alat za korake. Postoji popis veza na Microsoft prijetnju obavještavanje izvješća. Ta izvješća moguće je koristiti za defensive svrhu i potražite alat za sigurnost.

![Upozorenja pojedinosti o sigurnosti][4]

## <a name="enable-data-collection"></a>Omogućivanje zbirke podataka

Da biste omogućili behavioral analize virtualnog računala, mora biti uključeno prikupljanje podataka. Možda ćete morati omogućiti prikupljanje podataka kada pristupite centru za sigurnost ili prilikom stvaranja sigurnosna pravila.

Da biste provjerili je li omogućeno prikupljanje podataka, odaberite pločicu **pravila** . **Sigurnosna pravila** plohu otvara popis pretplate Azure. Odaberite pretplatu. **Prikupljanje podataka** je isključeno, promijenite na i spremite promjene. Potražite u članku [Omogućivanje prikupljanje podataka u centru za sigurnost Azure](security-center-enable-data-collection.md).

## <a name="next-steps"></a>Daljnji koraci

- U ovom dokumentu uvedene cijene za centar za sigurnost. Dodatne podatke o cijenama, potražite u članku centar za sigurnost [cijene stranice](https://azure.microsoft.com/pricing/details/security-center/).
- Da biste saznali više o mogućnostima napredne otkrivanje centar za sigurnost, potražite u članku [Centar za sigurnost Azure otkrivanje mogućnosti](security-center-detection-capabilities.md).
- Ako imate pitanja o korištenju centar za sigurnost, potražite u članku [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md).
- Ako i dalje imate pitanja o korištenju centar za sigurnost niti Azure, posjetite [Forume za Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/free-trial.png
[2]: ./media/security-center-pricing/standard.png
[3]: ./media/security-center-pricing/incident.png
[4]: ./media/security-center-pricing/network-alert.png
