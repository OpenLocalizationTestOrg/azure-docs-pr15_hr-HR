<properties
    pageTitle="Referenca za navigaciju portala za Azure"
    description="Naučite različita korisnička sučelja za web-aplikacije servisa između portal za upravljanje i Portal za Azure"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>Referenca za navigaciju portala za Azure

Azure web-mjesta sada se nazivaju [Aplikacije servisa web-aplikacije](http://go.microsoft.com/fwlink/?LinkId=529714). Ažuriramo sve naše dokumentacije odrazila ta promjena naziva i upute za Portal za Azure. Dok se ne završi postupak, možete koristiti ovaj dokument kao vodič za rad s web-aplikacije na portalu za Azure.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>Budućnost klasični Portal za Azure

Uočit ćete brendiranje promjene na portalu klasični Azure, taj portal je u tijeku zamijenjena Portal za Azure. Kao što je klasični portal izlaze, fokusa za nove razvoj prebacite se na Azure Portal. Sve nadolazećih novih značajki web-aplikacijama prosljeđivala na portalu za Azure. Početak korištenja portala za Azure da iskoristite prednost najnovije i najveće da imaju Web Apps nudi.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Izgled razlike između Azure klasični Portal i Portal za Azure

Na portalu klasični Azure usluge navedene su na lijevoj strani. Navigacija na portalu klasični slijedi stabla strukturu, gdje pokretanje iz servisa i idite u svaki element. Ovu strukturu funkcionira dobro kada upravljanje neovisno komponente. Međutim, aplikacija utemeljena na Azure su zbirke međusobno povezanih servisa, a ovu strukturu stabla nije prikladno za rad s zbirke servisa. 

Portal za Azure olakšava stvaranje aplikacije završetka do kraja komponentama iz više servisa. Na portalu organizirani kao *journeys*. *Putovanje* je niz *blades*, koje su spremnici za različite komponente. Ako, na primjer, postavite automatsko-skaliranje za web-aplikacijama je *putovanja* koji će vas odvesti nekoliko blades kao što je prikazano u sljedećem primjeru: plohu **web-mjesta** (da naslov plohu nije još ažurirano da biste koristili novi terminologija), plohu **Postavke** , a plohu **Skaliranje odgovor** . U ovom primjeru automatsko skaliranje se postavljena tako da ovise o procesora, pa i **Postotak procesora** plohu. Komponente unutar *blades* nazivaju *dijelove*koji izgleda kao pločice. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Primjer navigacije: Stvorite web-aplikaciju

Stvaranje nove web-aplikacije i dalje dovoljno je 1-2-3. Sljedeća slika prikazuje klasični portal i portalu po usporednu kao dokaz da ne približno promijenila se broj korake potrebne da biste pristupili web-aplikacijama prema gore i pokretanje. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Na portalu možete odabrati najčešće aplikacija za web, uključujući Popularni Galerija aplikacije kao što su WordPress. Potpuni popis dostupnih aplikacija, posjetite [Trgovine Windows Azure].

Kada stvorite web-aplikacijama, navedite URL, aplikacije servisa za planiranje i mjesto na portalu samo kao i na portalu klasični. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Uz to, na portalu omogućuje određivanje druge uobičajene postavke. Ako, na primjer, [grupa resursa](../azure-resource-manager/resource-group-overview.md) lakše ga da biste vidjeli i upravljanje povezane Azure resurse. 

## <a name="navigation-example-settings-and-features"></a>Primjer navigacije: postavki i značajki

Postavki i značajki sada logično grupiraju se u jednom plohu iz kojeg možete se kretati.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Na primjer, možete stvoriti prilagođene domene tako da kliknete **prilagođenih domena i SSL** u plohu **Postavke** .

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Da biste postavili nadzora upozorenja, kliknite **zahtjeva za te pogreške** , a zatim **Dodajte upozorenja**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Da biste omogućili dijagnostika, kliknite **Dijagnostički zapisnici** u plohu **Postavke** .

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
Da biste konfigurirali postavke aplikacije, kliknite **Postavke aplikacije** u plohu **Postavke** . 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Osim naziv marke je preimenovati ili drugačije grupirane radi lakšeg ih pronaći nekoliko stvari na portalu. Na primjer, u nastavku je snimka zaslona na stranicu za aplikaciju postavke (**Konfiguriraj**) na portalu klasični.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Dodatni resursi

[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](http://go.microsoft.com/fwlink/?LinkId=523751), gdje možete odmah stvoriti web-aplikacijama short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="whats-changed"></a>Što se promijenilo
* Vodič za promjenu iz aplikacije servisa za web-mjestima potražite u članku: [aplikacije servisa za Azure i Its utjecaj na postojećim Azure servisima](http://go.microsoft.com/fwlink/?LinkId=529714)
 
