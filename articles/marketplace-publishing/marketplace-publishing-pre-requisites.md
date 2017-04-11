<properties
   pageTitle="Nisu tehničke prirode preduvjeta za stvaranje ponude za Azure Marketplace | Microsoft Azure"
   description="Razumijevanje zahtjevima za stvaranje i implementacija ponude Azure Marketplace drugima da biste kupili."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="08/18/2016"
  ms.author="hascipio"/>

# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Općenito preduvjeta za stvaranje ponude za Azure Marketplace
Objašnjenje preduvjeta Opće, poslovni proces-usmjereni na koji su potrebni za kretanje naprijed s postupak stvaranja ponudu.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Provjerite da su registrirana kao Prodavač s Microsoftom
Detaljne upute za registriranje Prodavač račun s Microsoftom, idite na [Stvaranje računa i registracije](marketplace-publishing-accounts-creation-registration.md).

- **Ako vaša tvrtka već je registrirana kao Prodavač u Razvojni centar i želite stvoriti novu ponudu,** a zatim prijava za objavljivanje portala isti ID e-pošte s koje Razvojni centar za registraciju obavlja. Ovaj korak potreban je tako da se međusobno povezani su portal Razvojni centar za i objavljivanje.
- **Ako vaša tvrtka već je registrirana kao Prodavač u Razvojni centar i želite urediti postojeće ponuda,** a zatim ili prijava za objavljivanje portala s administratorskog računa ili računa koja se dodaje ko-administrator u objavljivanje portal. Upute za dodavanje računa ko administrator navedeni su dolje.

## <a name="steps-to-add-a-co-admin-in-the-publishing-portal"></a>Korake da biste dodali administratore ko na portalu za objavljivanje
Administratori objavljivanje portal možete dodati ostalih članova tvrtke, koji rade na aplikaciju, ko-administrator u objavljivanje portal. **Uz pretpostavku da ste administrator sustava** dolje su korake da biste dodali administrator ko

>[AZURE.NOTE] Za nove korisnike, prije dodavanja ko administrator u objavljivanje portala bili sigurni da ste stvorili barem jedan aplikacije u objavljivanje portal. Ovo je obavezan kao što je kartica **IZDAVAČI** pojavljuje se tek nakon stvaranja barem jedan aplikacije u objavljivanje portal.

1. Provjerite je li ko administrator e-pošte ID-a Microsoft account(MSA). Ako nije, registrirati kao MSA pomoću ove [veze](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Provjerite postoji li barem jedan aplikacije u odjeljku administratorskog računa prije no što pokušate dodati administrator ko
3. Po završetku prema gore navedenim uputama se prijave za objavljivanje portala s id ko administrator e-pošte i zatim zapisnika odgovor.
4. Sada prijave za objavljivanje portala administrator e-pošte ID-ja.
5. Dođite do izdavači -> odaberite svoj račun -> administratori -> Dodaj ko-administrator (snimka dolje)

    ![Crtanje](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)

6. Provjerite je li za sve komunikacije od Microsofta provjeravaju e-pošte ID-a na različite faze objavljivanje procesa (primjerice Razvojni centar, portal za objavljivanje).
7. Za registraciju Razvojni centar za izbjegavanje račun povezan s jednoj osobi. Time se predlaže za uklanjanje ovisnost jedna osoba.
8. Ako licem probleme s Razvojni centar za registraciju, zatim ponovno Potenciranje karata pomoću ove [veze](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-to-delete-a-co-admin-in-the-publishing-portal"></a>Upute za brisanje ko-administratora u objavljivanje portal
**Uz pretpostavku da ste administrator sustava** navedene u nastavku su navedeni koraci za da biste izbrisali administrator ko

1. Prijavite se na objavljivanje portala administrator e-pošte ID-ja.
2. Dođite do **izdavači** -> odaberite svoj račun -> **Administratori** -> **- Suadministratora**.
3. Kliknite gumb sa **znakom X** pokraj ko-administrator želite osiguranje Izbriši (snimka dolje).

    ![Crtanje](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [AZURE.IMPORTANT] Ne morate dovršiti tvrtke poreza i bankovni podaci ako namjeravate objaviti samo besplatne ponuda (ili Premjesti vlastitu licence).

> Registracija tvrtke moraju dovršiti da biste započeli. Međutim, tijekom rada vaše tvrtke o porez i banka u račun za Microsoft Developer za razvojne inženjere možete početi s radom na stvaranje slike virtualnog računala [Portal za objavljivanje](https://publish.windowsazure.com)bude potvrđen, i testiranje u Azure pripremna okruženju. Trebat će vam račun odobrenje dovršeno Prodavač samo za završnom koraku objavljivanje tu ponudu Azure Marketplace.

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>Nabava Azure "pay-as-you-go" pretplate
To je pretplata na koje ćete koristiti za stvaranje VM slike i rukom preko slike [Trgovine Windows Azure](https://azure.microsoft.com/marketplace/). Ako nemate postojeću pretplatu, zatim prijavite na https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>"Pošiljke s" države
> [AZURE.WARNING]
Da bi se prodajete servisa na trgovine Windows Azure, morate provjerite nalazi li se vaše registrirani entitet neku od države odobrene "Prodaja-šalje". To ograničenje je payout i taxation razloga. Ne možemo aktivno traženi da biste proširili popis države u skorijoj budućnosti, tako ostati definirati. Cjelovit popis potražite sekciji 1b [trgovine Windows Azure sudjelovanje pravila](http://go.microsoft.com/fwlink/?LinkID=526833).

## <a name="next-steps"></a>Daljnji koraci
Kada su ispunjeni nisu tehničke prirode stara requisites, sljedeće su ponudu postavite određene tehničke preduvjeti. Kliknite vezu na članak za vrstu odgovarajući ponudu koju želite stvoriti za Azure Marketplace.

- [VM tehničke prije requisites](marketplace-publishing-vm-image-creation-prerequisites.md)
- [Rješenje predložak tehničke prije requisites](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Vidi također
- [Početak rada: kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md)
