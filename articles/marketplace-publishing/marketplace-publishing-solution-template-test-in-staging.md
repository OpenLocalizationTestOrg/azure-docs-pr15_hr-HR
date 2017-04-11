<properties
   pageTitle="Testiranje tu ponudu predložak rješenja za Marketplace | Microsoft Azure"
   description="Objašnjenje kako testirati vaše rješenje predložak ponuda za Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/04/2015"
   ms.author="hascipio; v-divte" />

# <a name="test-your-solution-template-offer-in-staging"></a>Testirajte tu ponudu predložak rješenja u pripremna
Pripremna znači implementacija tu ponudu u privatnu "s memorijom za testiranje" gdje možete testiranja i provjere njegova funkcionalnost prije margina za radni. Pojavljuje se pripremna baš kao i to klijentom koji je postavila ponudu. Da biste Završi pripremna mora biti potvrđen tu ponudu.

Nakon što ponudu kopirana bez postavljanja, možete pregledavati i testiranje ponudu [Azure Portal](https://portal.azure.com/).

Slijedite korake u nastavku automatske tu ponudu za pripremna i testiranje [Azure Portal](https://portal.azure.com/):

1.  Idite na [Portal za objavljivanje](https://publish.windowsazure.com) > kartica**Rješenje predlošci** > tu ponudu > **Objavi** > **automatske pripremna**.
2.  Navedite popis Azure pretplata koje ćete koristiti da biste pretpregledali i testirali tu ponudu.
3.  Prijavite se na portal Azure pretpregled pomoću ID pretplate koje ste koristili u prethodnom koraku.
4.  Izvršavanje barem jedan round testiranja na portalu Azure pretpregled točke navedenim ispod:
  - Pripazite da marketinškog sadržaja prikazuje ispravno trgovine Windows Azure.
  - Implementacija završetka do kraja topologije.
  - Izvođenje testiranja performanse i testiranje opterećenjem.
  - Provjerite je li da se pridržava topologiji najbolje prakse.

## <a name="next-steps"></a>Daljnji koraci
Ako ste zadovoljni rezultatom, a zatim možete nastaviti objavljivanje konačni ponudu faza, **korak 4**: [Implementacija tu ponudu u trgovinu](marketplace-publishing-push-to-production.md). U suprotnom, unesite promjene u tu ponudu i zatražite ponovno certifikata.

> [AZURE.NOTE] Za marketing promjena sadržaja ustanova nije potrebna.

U odjeljku [Početak rada: kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md) vodič za sve zadatke u programu publisher.
