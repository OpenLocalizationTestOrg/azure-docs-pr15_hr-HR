<properties
    pageTitle="Čišćenje krajnje Azure CDN | Microsoft Azure"
    description="Saznajte kako Pročistite sve predmemoriranog sadržaja iz CDN krajnje."
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

# <a name="purge-an-azure-cdn-endpoint"></a>Čišćenje krajnje Azure CDN

## <a name="overview"></a>Pregled

Azure čvorove rub CDN će predmemoriju imovine dok istekne vrijeme važenja sredstva (TTL).  Nakon sredstva TTL istekne, kada klijentsko zahtjeve imovine s čvor rub, čvor rub će dohvatiti nove ažuriranu kopiju resursa da bi služio zahtjev klijenta i spremište osvježavanja predmemorije.

Ponekad možda želite čišćenje predmemorije sadržaj iz sve čvorove rub i tjerajte ih sve da biste dohvatili nove ažurirane resursi.  To može biti ažuriranja web-aplikaciju ili da biste brzo ažuriranje sredstva koja sadrži pogrešne podatke.

> [AZURE.TIP] Imajte na umu da čišćenja samo čisti predmemoriranog sadržaja na poslužiteljima rub CDN.  Do predmemorije, kao što su proxy poslužitelji i lokalnom pregledniku predmemorije, i dalje držeći tipku predmemorirani kopiju datoteke.  Važno Imajte na umu sljedeće kada postavite datoteke vrijeme važenja je.  Možete pokrenuti do klijenta da bi se zatražila najnoviju verziju datoteke tako da ga date jedinstven naziv prilikom svakog ažuriranja ili putem [predmemorije niza upita](cdn-query-string.md).  

Pomoću ovog praktičnog vodiča vodit će vas kroz čišćenja imovine s sve čvorove rub krajnje točke.

## <a name="walkthrough"></a>Vodič

1. [Portal za Azure](https://portal.azure.com)pronađite CDN profil koji sadrži krajnju točku želite Pročistite.

2. Iz profila plohu CDN kliknite gumb za čišćenje.

    ![Plohu CDN profila](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Otvorit će se plohu čišćenje.

    ![CDN čišćenje plohu](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. Na plohu čišćenje odaberite adresa servisa koji želite čišćenja na padajućem izborniku URL-a.

    ![Čišćenje obrasca](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] Čišćenje plohu možete pristupiti i klikom na gumb **Očisti** na krajnjoj točki plohu CDN.  U tom slučaju polja **URL-a** bit će prethodno popunjen s adresom servisa te određene krajnje točke.

4. Odaberite što imovinu koju želite Pročistite iz čvorove ruba.  Ako želite da biste uklonili sva sredstva, potvrdite okvir **Očisti sve** .  U suprotnom, upišite cijeli put svaki resursa koje želite za čišćenje (npr., `/pictures/kitten.png`) u tekstni okvir **put** .

    > [AZURE.TIP] Više **puta** tekstne okvire prikazat će se nakon što unesete tekst omogućuje vam da biste sastavili popis više resursi.  Resursi s popisa možete izbrisati tako da kliknete gumb tri točke (...).
    >
    > Putovi mora biti relativni URL koje odgovaraju sljedeći [uobičajenog izraza](https://msdn.microsoft.com/library/az24scfc.aspx): `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  **Azure CDN iz Verizon** (standardna i Premium), zvjezdicu (\*) može koristiti kao zamjenski znak (npr., `/music/*`).  Zamjenski znakovi i **Čišćenje svih** nije dopuštena s **CDN Azure s Akamai**.
    
5. Kliknite gumb **Očisti** .

    ![Gumb za čišćenje](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Zahtjevi za čišćenje potrajati približno 2-3 minuta proces s **CDN Azure s Verizon** (standardna i Premium) i približno 7 minute s **CDN Azure s Akamai**.  Azure CDN ima ograničenje od 50 Istodobni Pročistite zahtjeve u bilo kojem trenutku. 

## <a name="see-also"></a>Vidi također
- [Unaprijed učitavanje imovine na krajnje Azure CDN](cdn-preload-endpoint.md)
- [Azure referenca REST API-JA CDN - Pročistite ili unaprijed učitavanje krajnje točke](https://msdn.microsoft.com/library/mt634451.aspx)
