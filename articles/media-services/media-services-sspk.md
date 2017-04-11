<properties 
    pageTitle="Licenciranje Microsoft® izglađenim strujanje klijent Porting Kit" 
    description="Upute za licenciranje Microsoft® Smooth Streaming klijent Porting Kit." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Licenciranje Microsoft® izglađenim strujanje klijent Porting Kit

##<a name="overview"></a>Pregled

Smooth Streaming klijent Porting paket Microsoft (**SSPK** za kratki) je Smooth Streaming klijent implementacije u koja je optimizirana za pomoć ugrađene proizvođači, kabel i mobilni Operateri, sadržaja davateljima usluga, slušalice proizvođači, neovisni proizvođači softvera (ISV) i rješenja da biste stvorili proizvodi i servisi za strujanje prilagodljivo strujanja medijskih sadržaja u Smooth Streaming format. SSPK je s uređaja i platformu neovisno implementacija Smooth Streaming klijenta koji možete podrazumijeva po razumnoj bilo kojeg uređaja i platforme. 

Dio ispod je više razine arhitektura i okvir IIS Smooth Streaming Porting Kit je implementacija Smooth Streaming klijenta koji nudi Microsoft i obuhvaća sve core logike za reprodukciju Smooth Streaming sadržaja. To je zatim podrazumijeva partneri za određeni uređaj ili platforma implementacijom odgovarajuće sučelja. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Opis

SSPK licenciran na uvjetima koje nude izvrstan poslovne vrijednosti. Licenca SSPK omogućuje industrijskih sa:

- Smooth Streaming Porting Kit izvor u C++ 
  - implementira funkcionalnost Smooth Streaming klijenta
  - Dodaje oblik Raščlanjivanje heuristike u međuspremnik logike, itd.
- Aplikaciju za reprodukciju API-ji 
  - programskog sučelja za interakciju s aplikaciju za reprodukciju medijskih sadržaja
- Sučelje za platformu Abstraction Layer (PAL) 
  - programskog sučelja za interakciju s operacijskim sustavom (niti, sockets)
- Sučelje za sloj (HAL) apstrakcije hardvera 
  - programskog sučelja za interakciju s hardver A / V dekoderima (dekodiranje, vizualizacije)
- Digital Rights Management (DRM) sučelja 
  - programskog sučelja za rukovanje DRM kroz na DRM Abstraction Layer (DAL)
  - Microsoft PlayReady Porting Kit isporučuje zasebno, ali integrira putem ovog sučelja. Dodatne informacije o licenciranju Microsoft PlayReady Device, kliknite [ovdje](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- Uzorci implementacije 
  - Implementacija PAL uzorka za Linux
  - Implementacija HAL uzorka za GStreamer

##<a name="licensing-options"></a>Mogućnosti licenciranja

Microsoft Smooth Streaming klijent Porting Kit postane dostupna za licensees u odjeljku dva različita licencnih ugovora: jedan za razvoj Smooth Streaming klijent privremeni proizvodi i drugi za distribucija izglađenim strujanje klijent konačni proizvodi krajnjim korisnicima.
 
- Za chipset proizvođači, integrators sustava ili neovisni proizvođači softvera (ISV) kojima je potreban izvornog koda porting komplet za razvoj privremeni proizvodi Smooth Streaming klijent Porting paket Microsoft **Privremeni licencu za proizvod** je potrebno izvršiti.
- Proizvođači ili ISV-ovi kojima je potreban pravima raspodjele za izglađenim strujanje klijent konačni proizvode krajnjim korisnicima, Smooth Streaming klijent Porting paket Microsoft **Konačni licencu za proizvod** je potrebno izvršiti.

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft prelazili putem glatke strujanje klijent Porting Kit privremeni licencu za proizvod

U odjeljku licence, Microsoft nudi Smooth Streaming klijent Porting komplet i prava potrebne intelektualnog vlasništva razvijaju i distribuirati Smooth Streaming klijenta privremeni proizvoda drugih licensees uređaj Smooth Streaming klijent Porting Kit distribuirate izglađenim strujanje klijent konačni proizvoda.

####<a name="fee-structure"></a>Struktura naknada

Jednokratno licence naknadu sad 50.000 omogućuje pristup Smooth Streaming klijent Porting komplet. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft prelazili putem glatke strujanje klijent Porting Kit konačne licencu za proizvod

U odjeljku licence, Microsoft nudi sve prava potrebne intelektualnog vlasništva primanje Smooth Streaming klijent privremeni Proizvodi s drugim licensees Smooth Streaming klijent Porting Kit i distribuciju tvrtke brandiranog izglađenim strujanje klijent konačni proizvodi krajnjim korisnicima.

####<a name="fee-structure"></a>Struktura naknada

U odjeljku royalty modela kao u odjeljku je ponuđen izglađenim strujanje klijent konačni proizvoda:

- $0.10 po uređaj implementaciju otpremljene
- Na royalty je capped pri 50.000 svake godine
- Nema royalty za prvih 10 000 uređaj implementacije svake godine 

##<a name="licensing-procedure-and-sspk-access"></a>Licenciranje postupak i SSPK pristup

Ponovno slanje e-poštom [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) za sve licenciranje upita.

[Portal za raspodjelu SSPK](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) je dostupan registrirani privremeni licensees.

Privremeni i konačni SSPK licensees mogu poslati tehnička pitanja na koja [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft prelazili putem glatke strujanje Licensees ugovor privremeni proizvoda za klijenta

- Adroit Business Solutions, Inc
- Napredni Pacifička digitalni emitiranje
- AirTies Kablosuz Iletism Sanayive Odbaci ure Ticaret A.S.
- Albis tehnologije Ltd.
- Alticast Corporation
- Amazon digitalni Services, Inc.
- AVC Co. u softver Multimedija, Ltd.
- Cavium, Inc.
- Kupnja Corporation EchoStar
- Enseo, Inc.
- Fluendo Libros
- HANDAN BroadInfoCom Co., Ltd.
- Infomir GMBH
- Irdeto sad Inc.
- Liberty globalni servisi BV
- MediaTek Inc.
- Snimka zaslona poruke o MStar, Ltd
- Nintendo Co., Ltd.
- OpenTV, Inc.
- Ograničeno digitalni saffron
- Sichuan Co. u električni Changhong, Ltd
- SoftAtHome
- Sony Corporation
- Tatung tehnologije Inc.
- TCL Technoly elektroničkom (Huizhou) Co., Ltd.
- Ukloni Vestel Elektronik Sanayi Ticaret A.S.
- VisualOn, Inc.
- ZTE Corporation

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft prelazili putem glatke strujanje Licensees ugovoru za krajnje klijenta

- Napredni Pacifička digitalni emitiranje
- AirTies Kablosuz Iletism Sanayive Odbaci ure Ticaret A.S.
- Albis tehnologije Ltd.
- Amazon digitalni Services, Inc.
- AmTRAN tehnologije Co., Ltd.
- Arcadyan tehnologije Corporation
- ATMACA ELEKTRONİK SAN. UKLONI TİC. A.Ş
- Britanski Sky emitiranje ograničeno
- Shenzhen CastPal tehnologije Inc.
- Compal elektroničkom, Inc.
- Dongguan digitalni AV tehnologije korporacija, Ltd.
- Kupnja Corporation EchoStar
- Enseo, Inc.
- Ograničeno Filmflex filmova
- Fluendo Libros
- Ograničeno inovacije odvjetnički ured
- Haier informacije Applicantion S.R.L
- HANDAN BroadInfoCom Co., Ltd.
- Homecast Co., Ltd
- Hon Hai preciznosti industrijskih Co., Ltd.
- Infomir GMBH
- Kaonmedia Co., Ltd.
- Corporation kddi tvrtke
- Nintendo Co., Ltd.
- Pacifička Narančasta
- Ograničeno digitalni saffron
- Sagemcom širokopojasnoj mreži SAS
- Shenzhen CO. u elektroničkom Coship, LTD
- Shenzhen Co. u električni Jiuzhou, Ltd
- Shenzhen Skyworth digitalni tehnologije Co., Ltd
- Sichuan Co. u električni Changhong, Ltd.
- Skardin Industrijska glazba korporacija
- Sky Deutschland Fernsehen GmbH & Co. KG
- SmarDTV Libros
- SoftAtHome
- Sony Corporation
- Ograničeno TCL Overseas Marketing (Offshore komercijalni Makao)
- SAS Technicolor isporuke tehnologija
- Globalni Ltd. Tongfang
- Stilu života Toshiba proizvoda i usluga Corporation
- Univerzalni Corporation Media /Slovakia/ s.r.o.
- VIZIO, Inc.
- Wistron Corporation
- ZTE Corporation

##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
