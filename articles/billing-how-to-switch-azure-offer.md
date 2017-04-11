<properties
    pageTitle="Prebacivanje Azure pretplatu na drugu ponudu | Microsoft Azure"
    description="Saznajte kako promijeniti pretplate Azure i prijeđite na drugi ponudu pomoću portala za Upravljanje pretplatom"
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="genli"/>

# <a name="switch-your-azure-subscription-to-another-offer"></a>Prebacivanje Azure pretplatu na drugu ponudu

Kao [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) kupca, možda ćete moći pretplate Azure prijelaz na drugu ponudu u [Centar za račun](https://account.windowsazure.com/Subscriptions). Ako, na primjer, možete koristiti ovu značajku da iskoristite prednost [mjesečni zahvale za pretplatnike na Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Ako se nalazite na [Besplatnu probnu verziju](https://azure.microsoft.com/free/), Saznajte kako [nadograditi na Pay-As-You-Go](billing-upgrade-azure-subscription.md).

#### <a name="whats-supported"></a>Što je podržano:

| Iz                                                              | Da biste                                                                                      |
|-------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Razvojni Test pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0023p/)              |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)          |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)     |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [MSDN platforme](https://azure.microsoft.com/offers/ms-azr-0062p/)                      |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)            |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Enterprise (Bizspark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |

> [AZURE.NOTE] Za ostale nude promjene, [obratite se podršci](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
    
## <a name="switch-subscription-offer"></a>Pretplata na promjenu

> [AZURE.VIDEO switch-to-a-different-azure-offer]

1.  Prijavite se na adresi [Azure centar za račun](https://account.windowsazure.com/Subscriptions).

2.  Odaberite pretplatu Pay-As-You-Go.

3.  Kliknite **Da biste prešli na drugu ponudu**. Gumb samo nije dostupna ako se nalazite na Pay-As-You-Go i gotovo s prvim razdobljem za naplatu.

    ![Obratite pozornost na gumb ponudu Prijeđi na desnoj strani stranice](./media/billing-how-to-switch-azure-offer/switchbutton.png)
    
4.  **Odaberite željenu ponudu** na popisu ponuda za vašu pretplatu možete prijeći. Ovaj popis razlikuje se ovisno o članstva koji je pridružen vaš račun. Ako ništa nije dostupan, provjerite [popis dostupnih ponuda koje možete prijeći](#whats-supported) i provjerite je li desnom članstva. 

    ![Odaberite ponudu koju želite prijeći](./media/billing-how-to-switch-azure-offer/selectoffer.png)

5.  Ovisno o ponudi prijelaz, možda ćete vidjeti bilješku o utjecaju prijelaza. Pažljivo proći kroz ovaj popis, a zatim slijedite upute prije nastavka.

    ![Pregled bilješke](./media/billing-how-to-switch-azure-offer/thingstonote.png)

6.  Pretplatu možete preimenovati. Prema zadanim postavkama, ne možemo postavite ga na novi naziv za ponudu. Kliknite **Parametar ponuditi** da biste dovršili postupak.

    ![Kliknite zeleni gumb](./media/billing-how-to-switch-azure-offer/confirmpage.png)

7.  Uspjeh! Vaša pretplata sada prešli na novu ponudu.

## <a name="why-cant-i-switch-offers"></a>Zašto ne mogu promijeniti ponude?

**Da biste prešli na drugu ponudu** možda nećete vidjeti ako:

- Niste na [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/). Trenutno samo Pay-As-You-Go pretplate možete prijeći drugu ponudu.

    - Ako se nalazite na [Besplatnu probnu verziju](https://azure.microsoft.com/free/), Saznajte kako [nadograditi na Pay-As-You-Go](billing-upgrade-azure-subscription.md).

    - Da biste se prebacili ponudu na neku drugu pretplatu, [obratite se podršci](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- Koristite i dalje vaš prvi razdoblje naplate; morate pričekati razdoblje za prvu naplate da biste završili prije promjene ponude.

Možda **postoje nije dostupna u vašoj regiji ili država trenutno ponuda** Ako vidite:

- Niste uvjete za sve parametre ponudu. Provjerite [popis dostupnih ponuda koje možete prijeći u](#whats-supported).

## <a name="what-does-switching-azure-offers-do-to-my-service-and-billing"></a>Što označava prijelaz učinite ponuda za Azure na moj servis i naplatu?

Evo što se događa kada promijenite Azure detalje tarife u centru za račun.

### <a name="access-to-services"></a>Pristup servisima

Postoji bez isključiti servis za sve korisnike povezan s pretplatom. Međutim, prijeđite na ponudu možda ograničenja. Ako, primjerice, neke ponude sprječavaju korištenje radnog pa ćete morati premjestiti resursi proizvodnje na drugu pretplatu.

### <a name="billing"></a>Naplata

Dana prijeđete generira se faktura za sve preostale troškove. Nakon toga pretplate naplate po novu ponudu cijene uvjeta. Godišnjica naplatu vaše pretplate mijenja se u datum na koji ste promijenili ponude. Korištenje i naplati podataka prije ne zadržava se promijeni ponuda, stoga preporučujemo da prije promjene Preuzmi kopiju.

> [AZURE.NOTE] Zbog ograničenja vezane uz naplatu ponudu parametri nisu moguće unutar prvog odabranom ciklusu naplate nakon stvaranja pretplate.

## <a name="can-i-migrate-from-pay-as-you-go-to-cloud-solution-providerhttpspartnermicrosoftcomsolutionscloud-reseller-overview-csp-or-enterprise-agreementhttpsazuremicrosoftcompricingenterprise-agreement-ea"></a>Može li se pri migraciji iz Pay-As-You-Go [Davatelju rješenja oblaka](https://partner.microsoft.com/Solutions/cloud-reseller-overview) (CSP) ili [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (EA)?

Trenutno ne podržavamo ponudu da biste prešli na CSP ili EA u centru za račune. Da biste premjestili postojeće pretplate u EA, imati administratoru registraciju Dodavanje računa u na EA. Nakon toga ćete primiti pozivnicu e-pošte. Kada slijedite upute da biste prihvatili pozivnicu, pretplate automatski će se premjestiti u odjeljku Enterprise Agreement. Da biste migrirali CSP, potražite u članku [Azure migracije pretplate na CSP](https://blogs.technet.microsoft.com/hybridcloudbp/2016/08/26/azure-subscription-migration-to-csp/).

## <a name="next-steps"></a>Daljnji koraci

- Saznajte kako [upravljati administratorskih uloga](billing-add-change-azure-subscription-administrator.md) za vašu pretplatu

- Praćenje Upotreba tako da [preuzmete podataka o korištenju i fakture](billing-download-azure-invoice-daily-usage-date.md)

## <a name="need-help-contact-support"></a>Potrebna vam je pomoć? Obratite se podršci.

Ako i dalje imate dodatno pitanja, ponovno se [obratiti službi za podršku](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) da biste dobili problem riješen brzo.
