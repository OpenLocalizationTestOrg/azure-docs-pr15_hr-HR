<properties
   pageTitle="Razumijevanje izvješćivanja trgovine Windows Azure payout | Microsoft Azure"
   description="Saznajte kako pregledati i ingest payout izvješća servisa Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter="na"
   authors="v-jeana"
   manager="lakoch"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/19/2016"
   ms.author="v-jeana; hascipio; v-dabosl"/>

# <a name="understand-your-azure-marketplace-payout-reports"></a>Razumijevanje payout izvješća servisa Azure Marketplace

## <a name="access-and-view-your-payout-reports"></a>Pristup i prikaz izvješća payout

Dok ne možemo prijelaz u Razvojni centar za neki izvještaji payout možda dostupna u Razvojni centar za https://dev.windows.com/en-us dok drugi i dalje nalazi se na Portal za objavljivanje na https://publish.windowsazure.com.

Izvješćivanje o pogreškama payout sada bit će dostupna u **Razvojni centar** za sve trgovine ponude koje su vezane uz Moderna payouts; To trenutno obuhvaća:
- VMs
- Nudi B + C
- Podatke i razvojni usluge koje se nude u odjeljku EA

Izvješćivanje o pogreškama payout bit će i dalje **Portal za objavljivanje** za:
- Podatke i usluge razvojni nudi u odjeljku Web Izravni (koji i dalje koristi sustav naslijeđene payout).

Izvješća su dostupne 45 dana nakon zatvaranja kvartal i izračunavaju se nakon sve povrata novca.

### <a name="access-payout-reports-in-dev-center"></a>Razvojni centar za payout izvješća programa Access

1. Dođite do Razvojni centar za pri https://dev.windows.com/en-us.
2. Kliknite **nadzorna ploča**.

    ![LandingPageDashboardHighlight][1]

3. Kliknite **Sažetak Payout**.

    ![DashboardPayoutSummary][2]


## <a name="view-your-payout-reports-in-dev-center"></a>Prikaz izvješća payout u Razvojni centar

Izvješće payout za tromjesečju zapisuje sve transakcije nastalih iz tog tromjesečja.

- Rezervirane iznos označava sve uplate koje su accruing izvan ciklusa nadolazeće plaćanja (npr. iznos će se premjestiti nadolazeće plaćanje na sljedeći mjesec).  Iznos obično biti 0 kn (osim ako klijent plaća u advance).
- Nadolazeći plaćanja ili najnovije plaćanja **Prikaz detalja** kliknite veze da biste vidjeli bilješke o tim payouts.
- Kliknite **Izjave o plaćanju** za prikaz pojedinosti u odjeljku prihod pomoću aplikacije/proizvoda.
- Kliknite na vezu za **Prikaz** da biste vidjeli pojedinačne izvješća.

    ![PayoutSummaryUpcomingMostRecentLinksStatement][3]

- Pomoću filtra **Nastavlja analitički** pri dnu pojedinačne izjava da biste pogledali druge aplikacije/proizvode ako postoje.

    ![PayoutSummaryPaymentStatementsFilterControl][4]



## <a name="view-your-payout-reports-in-publishing-portal"></a>Prikaz izvješća payout u Portal za objavljivanje
Izvješće payout za tromjesečju zapisa sve transakcije nastalih iz tog tromjesečja.

1. Otvorite portal za objavljivanje na https://publish.windowsazure.com.
2. U odjeljku **izdavači** kliknite **Payout izvješća**.
3. Kliknite padajući popis da biste prikazali sve dostupne tromjesečni payout izvješća.

    ![accessingpayoutreport][5]


### <a name="read-your-payout-reports"></a>Pročitajte payout izvješća

Izvješće payout za tromjesečju zapisa sve transakcije nastalih iz tog tromjesečja.

- Ako tražite stavke koje se odnose na određene tromjesečje, odaberite izvješće payout za taj tromjesečje s padajućeg popisa. Ako, na primjer, ako vas zanima stavke za Travanj da biste lipnja 2015, odaberite rasponu datuma s padajućeg popisa.
- Ako tražite detalje o payouts koje se odnose na određene tromjesečje, odaberite izvješće payout za kasnije tromjesečje. Ako, na primjer, ako vas zanima na payouts za Travanj da biste lipnja 2015, te iznose pojavit će se u izvješću kasnije payout za srpanj za rujan 2015.
![readingpayoutreport][6]

- Ploču financijske sažetka prikazuje salda, kredita i dugovanja po kategoriji.
- Stavke prikaz pojedinačne transakcije.

## <a name="definitions"></a>Definicija

**Financije ploči sažetak:**

![financialdefinitions][7]

**Stavke:**

![ledgerdefinitions][8]

## <a name="payout-questions"></a>Payout pitanja

Ako imate pitanja vezanih uz vaše payouts, obratite se službi za podršku.

![payoutquestions][9]

1. Dođite do stranice za podršku.
2. Odaberite **Payouts**.
3. Odaberite **Payout odnose anketa**.
4. Kliknite **Start zahtjev**.

## <a name="next-steps"></a>Daljnji koraci

Druge upite za podršku, prijavite problem na <https://portal.azure.com>.

[1]: ./media/marketplace-publishing-report-payout/LandingPage-DashboardHighlight.png
[2]: ./media/marketplace-publishing-report-payout/Dashboard-PayoutSummary.png
[3]: ./media/marketplace-publishing-report-payout/PayoutSummary-UpcomingOrMostRecentPaymentLinksSingleStatementLink.png
[4]: ./media/marketplace-publishing-report-payout/PayoutSummary-PaymentStatements-SingleStatement-FilterControl.png
[5]: ./media/marketplace-publishing-report-payout/accessingpayoutreport.png
[6]: ./media/marketplace-publishing-report-payout/readingpayoutreport.png
[7]: ./media/marketplace-publishing-report-payout/financialdefinitions.png
[8]: ./media/marketplace-publishing-report-payout/ledgerdefinitions.png
[9]: ./media/marketplace-publishing-report-payout/payoutquestions.png
