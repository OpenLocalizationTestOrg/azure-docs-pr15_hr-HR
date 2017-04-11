<properties
    pageTitle="Obrada servisa kvotama i ograničenja | Microsoft Azure"
    description="Dodatne informacije o kvotama zadane grupe za Azure, ograničenja i ograničenja i povećava zatražiti kvote"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Kvota i ograničenja servisa za grupu za Azure

Kao što je s drugim servisima za Azure postoje ograničenja na određene resurse povezan sa servisom grupe. Mnoge od tih ograničenja su zadane kvote po Azure primjenjuje na razini računa ili pretplate. U ovom se članku govori o tim zadane postavke, a kako možete zatražiti kvote povećava.

Ako namjeravate pokrenuti radnih opterećenja radnih grupa, možda morati povećati jedan ili više kvote iznad zadani. Ako želite podići ograničenja, možete otvoriti na online [korisničkoj podršci zahtjev](#increase-a-quota) bez troškova.

>[AZURE.NOTE] Ograničenja je ograničenje kreditne kartice, ne jamstva kapaciteta. Ako imate veliki kapaciteta potrebama, obratite se podršci za Azure.

## <a name="subscription-quotas"></a>Kvota za pretplatu
**Resurs**|**Zadano ograničenje**|**Maksimalno ograničenje**
---|---|---
Računi za obradu po regijama po pretplati | 1 | 50

## <a name="batch-account-quotas"></a>Grupe za račun kvote
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Druga ograničenja
**Resurs**|**Maksimalno ograničenje**
---|---
[Istodobni zadataka](batch-parallel-node-tasks.md) po računalnim čvor | 4 x broj jezgri čvor
[Aplikacije](batch-application-packages.md) po računu grupe        | 20
Pakete po aplikacije  | 40
Veličina paketa aplikacije (svaki)       | Approx. 195GB<sup>1</sup>

<sup>1</sup> azure ograničenje prostora za pohranu veličina blob najveći bloka

## <a name="view-batch-quotas"></a>Prikaz kvote za obradu

Prikaz kvote za račun obradu [Azure portal][portal].

1. Odaberite **računi grupe** na portalu, a zatim odaberite račun za grupu koja vas zanima.

2. Odaberite **Svojstva** na račun za obradu izbornik plohu

3. Svojstva plohu prikazuje **kvote** trenutno primjenjuje se na račun za obradu

    ![Grupe za račun kvote][account_quotas]

## <a name="increase-a-quota"></a>Povećajte ograničenja

Slijedite korake u nastavku da biste zatražili ograničenja povećati pomoću [portala za Azure][portal].

1. Odaberite pločicu **Pomoć + podrške** na nadzornu ploču portala ili upitnik (****?) u gornjem desnom kutu na portalu.

2. Odaberite **Novi zahtjev za podršku** > **Osnove**.

3. Na plohu **Osnove** :

    na. **Vrsta problema** > **kvote**

    b. Odaberite pretplatu.

    c. **Vrsta kvote** > **grupe**

    d. **Podržava plan** > **podršku kvote - dio**

    Kliknite **Dalje**.

4. Na plohu **Problem** :

    na. Odaberite **težinu** prema [utjecaj tvrtke][support_sev].

    b. U **detalje**, navedite svaki kvote koji želite promijeniti, naziv računa grupe i novi ograničenje.

    Kliknite **Dalje**.

5. Na plohu **Podaci za kontakt** :

    na. Odaberite **željena vrsta kontakta**.

    b. Provjerite je li, a zatim unesite potrebne detalje o kontaktu.

    Kliknite **Stvori** da biste poslali zahtjev za podršku.

Nakon što ste poslali zahtjev za podršku, Azure podrška će vam obratiti. Imajte na umu da dovršavanje zahtjev može potrajati do 2 dana tvrtke.

## <a name="related-topics"></a>Povezane teme

* [Stvorite račun grupe za Azure pomoću portala za Azure](batch-account-create-portal.md)

* [Pregled značajki za Azure grupe](batch-api-basics.md)

* [Azure pretplate i ograničenja servisa, kvota i ograničenjima](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
