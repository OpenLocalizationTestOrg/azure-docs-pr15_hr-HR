<properties
    pageTitle="Ograničiti pristup sadržaju Azure CDN prema državi | Microsoft Azure"
    description="Saznajte kako ograničiti pristup sadržaju Azure CDN pomoću značajke zemlj filtriranje."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="casoper"/>

#<a name="restrict-access-to-your-content-by-country---verizon"></a>Ograničiti pristup sadržaju prema državi - Verizon

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Standardna Akamai](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>Pregled

Kad korisnik zatraži sadržaju, po zadanom, sadržaj se poslužena bez obzira na to koju korisnik unijeli pošalje zahtjev. U nekim slučajevima, preporučujemo vam da biste ograničili pristup sadržaju prema državi. U ovoj se temi objašnjava kako koristiti značajku **Zemlj filtriranje** da bi se konfiguriranje servisa da biste dopustili ili blokiranje pristupa po državi.

> [AZURE.IMPORTANT] Proizvodi Verizon i Akamai – ista funkcija zemlj filtriranja, ali razlikuje korisničkog sučelja. Ovaj dokument opisuje sučelje za **Azure CDN standardni iz Verizon** i **Azure CDN Premium s Verizon**. Zemlj filtriranje s **Azure CDN standardni iz Akamai**, potražite u članku [ograničiti pristup sadržaju prema državi - Akamai](cdn-restrict-access-by-country-akamai.md).

Informacije o temama koje se odnose na konfiguriranje ovu vrstu ograničenja, potražite u odjeljku [pitanja](cdn-restrict-access-by-country.md#considerations) na kraju temu.  

>[AZURE.NOTE] Kada postavite konfiguraciju, to će primijeniti na sve **CDN Azure s Verizon** krajnje točke u ovom Azure CDN profilu.

![Filtriranje države](./media/cdn-filtering/cdn-country-filtering.png)

##<a name="step-1-define-the-directory-path"></a>Korak 1: Definiranje put direktorija

Prilikom konfiguriranja filtra države, morate navesti relativni put do mjesta na kojem korisnici će biti dopušteno ili zabranjen pristup. Možete primijeniti filtriranje država za sve datoteke s "/" ili odabrane mape navođenjem putova direktorija.

Primjer direktorija put filtar:

    /                                 
    /Photos/
    /Photos/Strasbourg

##<a name="step-2-define-the-action-block-or-allow"></a>Korak 2: Definiranje akciju: blokirati ili dopustiti

**Bloka:** Korisnici iz navedeni država bit će odbijena imovini iz taj rekurzivne put. Ako to mjesto nije konfiguriran nijedan drugi države mogućnosti filtriranja, zatim Svi ostali korisnici imati dopuštenje za pristup.

**Dopusti:** Samo korisnici s navedenom države imati dopuštenje za pristup imovini iz taj rekurzivne put.

##<a name="step-3-define-the-countries"></a>Korak 3: Definiranje države

Odaberite država koje želite blokirati ili dopustiti za put. Dodatne informacije potražite u članku [Azure CDN kodova država Verizon](https://msdn.microsoft.com/library/mt761717.aspx).

Na primjer, pravilo blokiranja /Photos/Strasbourg/će filtrirati datoteke uključujući:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Kodovi država

Značajka **Zemlj filtriranje** koristi kodovi država da biste definirali države iz kojeg zahtjev će biti dopušteno ili blokirane za zaštićenim direktorija. Kodovi država tražit će u [Azure CDN kodova država Verizon](https://msdn.microsoft.com/library/mt761717.aspx). Ako navedete "EU" (Europe) ili "AP (države/Pacifičke) podskup koja proizlazi iz sve države u tom područja će blokirane ili dopuštene IP adrese.


##<a id="considerations"></a>Razmatranja

- Može potrajati do 90 minuta da bi promjene konfiguracije filtriranja države stupile na snagu.
- Ta značajka ne podržava zamjenskih znakova (na primjer, "*").
- Država filtriranje konfiguracije pridružene relativni put bit će rekurzivno primijenjeni na taj put.
- Samo jedno pravilo koje se mogu primijeniti na istom relativni put (ne možete stvoriti više filtara država koje upućuju na isti relativni put. Međutim, mapa može imati više filtara države. To je zbog prirode rekurzivne države filtre. Drugim riječima, podmapa prethodno konfigurirane mape se mogu dodijeliti različite države filtra.
