<properties
   pageTitle="Testiranje VM ponudu za Marketplace | Microsoft Azure"
   description="Objašnjenje kako testirati VM sliku za Azure Marketplace."
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
   ms.date="08/01/2016"
   ms.author="hascipio" />

# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Testirajte svoje VM ponuda za Azure Marketplace u pripremna

Pripremna znači implementacija SKU-om u privatnu "s memorijom za testiranje" gdje možete testirati i provjeru njegove funkcionalnosti prije no što implementirate u trgovinu. Pojavit će se se SKU u pripremna baš kao i to klijentom koji je postavila. Da biste Završi pripremna mora biti potvrđen VM sliku.

## <a name="step-1-push-your-offer-to-staging"></a>Korak 1: Automatske tu ponudu za pripremna

1. Na kartici **Objavljivanje** kliknite **automatske pripremna**.

    ![Crtanje](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. Portal za objavljivanje obavještava vas o sve pogreške, ispravite ih.
3.  U na **tko može pristupiti ponude za postupnu?** dijaloški okvir, unesite popis Azure pretplata koje ćete koristiti da biste pretpregledali tu ponudu [portal Azure pretpregled](https://portal.azure.com).

    >[AZURE.NOTE] U slučaju virtualnim strojevima i rješenja predložaka, imajte **ne** whitelist pretplate vrste CSP, DreamSpark ili Azure otvorena.


    > U slučaju virtualnim strojevima prilikom klika na gumb **AUTOMATSKE PRIPREMNA**, sljedeće se izvode iza scene. Moći ćete prikaz tijeka svakog koraka na kartici OBJAVLJIVANJE u objavljivanje portal. Morate potvrdite ovu stranicu u pravilnim intervalima (dok je za status prikazana vrijednost po FAZAMA) da biste saznali sve pogreške koje je potrebno ispravak na kraj.

    > - Pripremna zahtjev isprva odlazak u tim certifikata koji je provjera valjanosti na vhd. Međutim, ako vaš zahtjev je imate samo marketinške promjena, zatim korak ustanova preskače.
    > - Nakon dovršetka na ustanova replikacije ponudu pokrenuti preko svih Azure podatkovnim centrima. Obično traje 24-48hours za replikaciju da biste dovršili no može potrajati i do tjedan ovisno o veličini u vhd. Međutim, ako vaš zahtjev je imate samo marketinške promjene, zatim na replikacije je brže.
    > - Po dovršetku na replikacije ponudu bit će dostupni u [Azure portal](http:/portal.azure.com). Na koji vremensku status će biti kopirana bez postavljanja u objavljivanje portal. Ponude za postupnu vidljiv na [portalu Azure](http:/portal.azure.com) samo pomoću ID-ovi e-pošte povezanog s pretplatom s kojom ponudu je kopirana bez postavljanja.

4. Prijavite se da biste [Azure pretpregled portal](https://portal.azure.com) pomoću neke od Azure pretplate naveden u prethodnom koraku.
5. Pronađite tu ponudu i provjeriti vaše točke VM slike:
  - Pripazite da marketinškog sadržaja prikazuje ispravno na tržištu.
  - Implementacija završetka do kraja VM slike.

      ![IMG, karte i portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [AZURE.IMPORTANT] Tu ponudu će ostati u pripremna dok obavijesti Microsoftu Portal za objavljivanje [kartica**Objavljivanje** > kliknite gumb **"zahtjev za odobrenje na automatske da biste radni"**] da ste spremni za slanje radnog. Ovo je idealna vrijeme da bi svi članovi vašeg tima potvrdite putem sve u Priprema za tu ponudu prelaska na popisu.

> Pripremna platformu namijenjen je testiranje ponudu u načinu pretpregleda izdavač. Ne možemo svakako radnju i pomoću ovog platofrm svrhe commerical.

## <a name="next-steps"></a>Daljnji koraci
Sad kad tu ponudu je "kopirana bez postavljanja" i testirate njegova funkcionalnost i marketinškog sadržaja, možete nastaviti konačni objavljivanje faza, **korak 4**: [Implementacija tu ponudu u trgovinu](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Vidi također
- [Početak rada: kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md)
