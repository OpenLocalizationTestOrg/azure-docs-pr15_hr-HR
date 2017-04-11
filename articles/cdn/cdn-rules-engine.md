<properties
    pageTitle="Nadjačavanje zadano ponašanje HTTP CDN Azure pomoću pravila modul | Microsoft Azure"
    description="Modul pravila možete prilagoditi kako HTTP zahtjeva rješava Azure CDN, kao što su blokira isporuku određene vrste sadržaja, definirati predmemoriranja pravilo i izmjena HTTP zaglavlja."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="override-default-http-behavior-using-the-rules-engine"></a>Zanemarivanje zadano HTTP ponašanje pomoću modul pravila

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Pregled

Modul pravila omogućuje vam da biste prilagodili kako se upravlja HTTP zahtjeva kao što su blokira isporuku određene vrste sadržaja, koji definira predmemoriranja pravila i izmjena HTTP zaglavlja.  Pomoću ovog praktičnog vodiča će demonstrirati stvaranje pravila koja će se promijeniti predmemoriranja ponašanje CDN resursi.  I videosadržaj dostupna je u odjeljku "[Vidi također](#see-also)".

## <a name="tutorial"></a>Praktični vodič

1. Iz profila plohu CDN kliknite gumb **Upravljanje** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-rules-engine/cdn-manage-btn.png)

    Otvorit će se na portal za upravljanje CDN.

2. Kliknite **Veliki HTTP -a** na kartici slijedi **Modul pravila**.

    Prikazuju se mogućnosti za novo pravilo.

    ![Nove mogućnosti pravila CDN](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] Redoslijed kojim se prikazuju više pravila utječe na način na koji se rukuje. Naknadna pravila zaobići akcija koji se navode prethodne pravilom.
    
3. Unesite naziv u **naziv i opis** tekstni okvir.

4. Odredite vrstu zahtjeva za pravilo će se primijeniti na.  Prema zadanim postavkama, match uvjet **uvijek** je odabran.  Ćete koristiti **uvijek** za ovog praktičnog vodiča, pa ostavite koje odaberete.

    ![CDN podudaranje uvjet](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] Postoje razne vrste podudaranje uvjeta dostupna na padajućem popisu.  Klikom na plavu Informativna ikonu s lijeve strane uvjet podudaranje će se objašnjava što trenutno odabrane uvjet detaljno.
    >
    >Cijelog popisa uvjeta za podudaranje detaljno potražite u članku [pravila modul zadovoljavaju uvjet i značajka pojedinosti](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0).

5.  Kliknite na **+** gumb pokraj **značajke** da biste dodali novu značajku.  Na padajućem popisu s lijeve strane odaberite **Nametni Interna Max-dob**.  U tekstni okvir koji će se pojaviti upišite **300**.  Ostavite preostale zadane vrijednosti.

    ![Značajka CDN](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] Kao s podudaranje uvjeta, klikom na plavi Informativna ikonu s lijeve strane nove značajke prikazat će se pojedinosti o toj značajci.  Slučaju **Prisilno Interna Max-dob**smo su nadjačavanje sredstva **Predmemorijom** i **istječe** zaglavlja kontrolu kada čvor rub CDN osvježavanja resursa iz izvor.  Našeg primjera 300 sekundi znači čvor rub CDN će predmemoriju sredstvo za pet minuta prije osvježavanja resursa iz njezinu izvoru.
    >
    >Cijeli popis značajki pojedinosti potražite u članku [pravila modul podudaranje uvjet i značajka pojedinosti](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).

6.  Kliknite gumb **Dodaj** da biste spremili novo pravilo.  Novo pravilo sada čeka odobrenje. Nakon što je odobren, status će se promijeniti iz **XML-a na čekanju** u **Aktivni XML**.

    >[AZURE.IMPORTANT] Promjene pravila može proći do 90 minuta proširiti kroz na CDN.

## <a name="see-also"></a>Vidi također
* [Azure Fridays: Azure CDN naprednih novi Premium značajki](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (videozapis)
* [Pravila modul podudaranje uvjet i značajka pojedinosti](https://msdn.microsoft.com/library/mt757336.aspx)
