<properties
   pageTitle="Vodič za stvaranje predloška rješenja za Marketplace | Microsoft Azure"
   description="Detaljne upute kako stvoriti, potvrđivanje i uvođenje predloška višestruki VM slika rješenja za kupnju na Azure Marketplace."
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
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Vodič za stvaranje predloška rješenja za Azure Marketplace
Nakon dovršetka postupka korak 1, [stvaranja računa i registraciju][link-acct-creation], ne možemo Vođena na stvaranja predloška rješenja Azure kompatibilnog pri [Tehnički preduvjeti za stvaranje predloška rješenja](marketplace-publishing-solution-template-creation-prerequisites.md). Sad ćemo će vas voditi kroz korake za stvaranje predloška rješenja za više VMs za [Portal za objavljivanje] [ link-pubportal] za Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Stvaranje ponude za predložak rješenja u Portal za objavljivanje
Idite na [https://publish.windowsazure.com](http://publish.windowsazure.com). Prilikom prijave za prvo na [Portal za objavljivanje](https://publish.windowsazure.com/)koristiti isti račun s kojim je registrirana vaša tvrtka Prodavač profila. Naknadno možete dodati sve zaposlenika tvrtke ko-administrator na portalu za objavljivanje.

### <a name="1-select-solution-templates"></a>1. odaberite "Rješenja predložaka"

  ![Crtanje][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Stvaranje novog predloška rješenja

  ![Crtanje][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. počinju topologija
Predložak rješenje je "parent" na sve njegove topologija. Možete definirati više topologija u jedan predložak za ponudu/rješenja. Ponude je pritisak na pripremna, pritisak sa svim njegov topologija. Slijedite korake u nastavku da biste definirali tu ponudu:     

- Stvorite topologiju: "Identifikator topologije" obično je naziv topologije predloška rješenja. Identifikator topologije koristi se u URL-u kao što je prikazano u nastavku:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure Portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Dodajte novu verziju.

### <a name="4-get-your-topology-versions-certified"></a>4. dohvatiti vaše verzije topologije certificirane
Prenesite zip datoteku koja sadrži sve potrebne datoteke Dodjela tu određenu verziju topologije. Zip datoteka mora sadržavati sljedeće:

- *mainTemplate.json* i *createUiDefinition.json* datoteku na njegov korijenski direktorij.
- Sve povezane predlošci i sve potrebne skripti.

  > [AZURE.TIP] Dok radite na razvojnim inženjerima na stvaranje rješenja topologija predložak, a zatim ih je potvrđen, tvrtke, odjela marketinga i/ili pravni vaše tvrtke možete raditi na marketinške i pravnog sadržaja.

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste stvorili predložak rješenja i prenijeli zip datoteke, slijedite upute u [vodič trgovine marketinške sadržaja](marketplace-publishing-push-to-staging.md) prije margina ponudu za pripremna. Da biste vidjeli potpunog skupa trgovine objavljivanje članaka, posjetite [Početak rada: kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md).

Koje možda će vas zanimati te srodnih članaka:

- Slika VM: [O virtualnog računala slike u Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- Proširenja VM: [VM Agent i pregled proširenja VM](https://msdn.microsoft.com/library/azure/dn832621.aspx) i [proširenja VM Azure i značajke](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- Azure Voditelj resursa: [omogućeno Azure ARM](../resource-group-authoring-templates.md) i [jednostavno ARM predložak Primjeri](https://github.com/rjmax/ArmExamples)
- Račun za pohranu regulira: [kako monitora za ograničavanje račun za pohranu](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) i [Premium prostora za pohranu](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
