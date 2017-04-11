<properties
    pageTitle="Unaprijed učitavanje imovine na krajnje Azure CDN | Microsoft Azure"
    description="Saznajte kako unaprijed učitavanje predmemoriranog sadržaja na krajnjoj točki CDN."
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

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Unaprijed učitavanje imovine na krajnje Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Prema zadanim postavkama imovine najprije predmemoriraju kao što su ste tražili. To znači da na prvi pošalje zahtjev za svako područje može trajati dulje, jer edge poslužitelji neće imati sadržaj predmemorirani i morat će proslijediti zahtjev za izvorni poslužitelj. U ovom prvom uspješnosti Latencija unaprijed učitavati izbjegava.

Osim pruža bolje iskustvo klijenta, unaprijed učitavanja vaše predmemorirani imovine možete smanjiti mrežni promet na poslužitelju polazište.

> [AZURE.NOTE] Unaprijed učitavanja imovine je korisno za velike događaje ili sadržaj koji postane istodobno dostupan velikom broju korisnika, kao što je novo izdanje film ili ažuriranje softvera.

Pomoću ovog praktičnog vodiča vodit će vas kroz unaprijed učitavanja predmemoriranog sadržaja na sve čvorove rub Azure CDN.

## <a name="walkthrough"></a>Vodič

1. [Portal za Azure](https://portal.azure.com)pronađite CDN profil koji sadrži krajnju točku želite unaprijed učitati.  Otvorit će se plohu profila.

2. Kliknite krajnje točke na popisu.  Otvorit će se plohu krajnjoj točki.

3. Krajnja točka plohu CDN kliknite gumb Učitaj.

    ![Plohu CDN krajnje točke](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    Otvorit će se plohu Učitaj.

    ![CDN učitavanje plohu](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Unesite cijeli put svaki resursa koje želite učitati (npr., `/pictures/kitten.png`) u tekstni okvir **put** .

    > [AZURE.TIP] Više **puta** tekstne okvire prikazat će se nakon što unesete tekst omogućuje vam da biste sastavili popis više resursi.  Resursi s popisa možete izbrisati tako da kliknete gumb tri točke (...).
    >
    > Putovi mora biti relativni URL koji odgovara sljedeći [uobičajenog izraza](https://msdn.microsoft.com/library/az24scfc.aspx): `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Svaki resursa, morate imati vlastitu put.  Postoji funkciju zamjenskih unaprijed učitavanju imovine.

    ![Gumb učitavanja](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Kliknite gumb **Učitaj** .

    ![Gumb učitavanja](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] Nema ograničenja zahtjeva za 10 Učitaj u minuti po CDN profilu.

## <a name="see-also"></a>Vidi također
- [Čišćenje krajnje Azure CDN](cdn-purge-endpoint.md)
- [Azure referenca REST API-JA CDN - Pročistite ili unaprijed učitavanje krajnje točke](https://msdn.microsoft.com/library/mt634451.aspx)
