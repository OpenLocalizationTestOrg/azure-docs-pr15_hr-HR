<properties
    pageTitle=" Skaliranje strujanje krajnje točke s portala za Azure | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake skaliranje strujanje krajnje točke s portala za Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Promjena veličine strujanje krajnje točke s portala za Azure

##<a name="overview"></a>Pregled

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). 

Prilikom rada s nešto Najčešći scenariji isporučuje videozapis putem prilagodljivu streaming brzina prijenosa u klijentima servisa Azure Media Services. Media Services podržava sljedeće prilagodljivo brzina prijenosa strujanje tehnologije: HTTP Live strujanje (HLS), Smooth Streaming, MPEG CRTICE i HDS (za PrimeTime pristup Adobe licensees samo).

Media Services nudi dinamički pakiranje koja vam omogućuje da izlaganja vaš prilagodljivo brzina prijenosa MP4 kodirani sadržaja u strujanje oblici koje podržava Media Services (MPEG CRTICE, HLS, Smooth Streaming, HDS) samo-u – vrijeme, bez potrebe za pohranu unaprijed zapakirani verzije svaki od ovih strujanje oblici.

Da biste iskoristili dinamički pakiranje, morate učinite sljedeće:

- Kodiranje datoteku mezzanine (izvor) u skupu prilagodljivo brzina prijenosa MP4 datoteke (kodiranja korake su što je prikazano u nastavku ovog praktičnog vodiča).  
- Stvorite najmanje jednu strujanje jedinicu u *strujanje krajnjoj točki* iz koje namjeravate isporuku sadržaja. Koraci u nastavku pokazuju kako da biste promijenili broj strujanje jedinica.

Dinamični pakiranje samo morate spremiti i platiti za datoteke u obliku jedan prostora za pohranu i Media Services će Sastavljanje i posluživanje prikladan odgovor na temelju zahtjeve iz klijenta.

Osim toga, možete kontrolirati kapacitet servis za strujanje krajnja točka za rukovanje sve veći propusnosti potrebama prilagodbom strujanje jedinice. Preporučuje se dodijeliti jednu ili više jedinica mjerilo za aplikacije u radnom okruženju. Strujanje jedinice pružaju i namjenski izlazne kapacitet koji može se kupiti u koracima od 200 MB/s i dodatnim funkcijama koje funkcionalnost koja obuhvaća: [dinamički pakiranje](media-services-dynamic-packaging-overview.md), Integracija CDN i Napredna konfiguracija. Dodatne informacije potražite u članku [Upravljanje strujanje krajnje točke s portala za Azure](media-services-portal-manage-streaming-endpoints.md).

## <a name="scale-streaming-endpoints"></a>Promjena veličine strujanje krajnje točke

Stvaranje i promjena broja strujanja rezervirana jedinice, učinite sljedeće:

1. [Portal za Azure](https://portal.azure.com/)odaberite svoj račun servisa Azure Media Services.
2. U prozoru **Postavke** odaberite **strujeće krajnje točke**.
3. Kliknite krajnju točku strujanje koju želite za promjenu veličine. 
4. Pomaknite klizač da biste naveli broj strujanje jedinica
 
![Strujanje krajnje točke](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

Primjena Imajte na umu sljedeće:

- Dodjela novih strujanje jedinica može potrajati oko 20 minuta da biste dovršili. 
- Trenutno, prijelaz s bilo kojeg pozitivnu vrijednost strujanje jedinice na ništa, možete onemogućiti na zahtjev za strujanje jedan sat.
- Najveći broj jedinica navedene za razdoblje 24-satni služi za izračun trošak. Informacije o cijenama detalje potražite u članku [Media Services cijene pojedinosti](http://go.microsoft.com/fwlink/?LinkId=275107).

##<a name="next-steps"></a>Daljnji koraci

Pregledajte Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


