<properties 
   pageTitle="Konfiguriranje VPN pristupnika na portalu za Azure klasični | Microsoft Azure"
   description="U ovom se članku vodit će vas kroz konfiguriranje virtualne mreže VPN pristupnika i mijenjanje pristupnika usmjeravanje Vrsta VPN-a."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>Konfigurirati pristupnik za VPN-a za uvođenje klasičnog model


Ako želite stvoriti sigurne više lokacija veze između Azure i lokalne lokacije, morate konfigurirati pristupnik VPN veza. U modelu uvođenje klasičnog pristupnik može biti jedna od dvije vrste usmjeravanje VPN: statički ili dinamički. Vrsta odaberete ovisi o i dizajn plan mreže i uređaja VPN lokalnog koji želite koristiti. 

Na primjer, neke mogućnosti povezivanja, kao što su vezu točke web zahtijevaju dinamički pristupnik za usmjeravanje. Ako želite konfigurirati pristupnikom za podršku veze točke-na-web-mjesta (P2S) i veze web-mjesto (S2S), morate konfigurirati dinamičke usmjeravanje pristupnik čak i ako se web-mjesto može konfigurirati pristupnik usmjeravanje Vrsta VPN-a. 

Uz to, morate provjerite podržava li uređaj koji želite koristiti za vezu VPN usmjeravanje vrstu koju želite stvoriti. Potražite u članku [o uređajima VPN-a](vpn-gateway-about-vpn-devices.md).


**O ovom članku** 

Ovaj članak uvođenje klasičnog modela pomoću [klasične portal](https://manage.windowsazure.com) (ne Azure portalu). 

**O modelima Azure implementacije**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>Pregled konfiguracija

Sljedeći koraci će vas voditi kroz konfiguriranje pristupnik za VPN-a na portalu za Azure klasični. Ove se upute odnose na pristupnika za virtualne mreže koje su stvorene pomoću klasične implementacije modela. Trenutno ne i svih konfiguracijske postavke pristupnika dostupni su na portalu za Azure. Kada su, će ćemo stvoriti novi skup uputa koje se odnose na portalu za Azure.


1. [Stvaranje pristupnika za VPN-a za vaše VNet](#create-a-vpn-gateway)

1. [Prikupite informacije o konfiguraciji uređaja VPN-a](#gather-information-for-your-vpn-device-configuration)

1. [Konfiguriranje uređaju VPN-a](#configure-your-vpn-device)

1. [Provjerite je li raspona lokalne mreže i VPN pristupnik IP adresa](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Prije početka

Prije no što konfigurirate pristupnikom, najprije morate stvoriti virtualne mreže. Koraci za stvaranje virtualne mreže za povezivanje s više lokacija, potražite u članku [Konfiguracija virtualne mreže s vezom za VPN-a web-mjesto](vpn-gateway-site-to-site-create.md)ili [Konfiguracija virtualne mreže s vezom za VPN točke na web](vpn-gateway-point-to-site-create.md). Zatim, poduzmite sljedeće korake možete konfigurirati pristupnik za VPN-a i Prikupite informacije morate konfigurirati uređaju VPN-a. 

Ako već imate pristupnik VPN-a, a želite promijeniti vrstu usmjeravanje VPN, potražite u članku [kako promijeniti vrstu za usmjeravanje VPN-a za pristupnikom](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Stvaranje pristupnika za VPN-a

1. [Azure klasični portal](https://manage.windowsazure.com)na stranici **mreža** , provjerite je li stupcu stanje za virtualne mreže **stvorili**.

1. U stupcu **naziv** kliknite naziv virtualne mreže.

1. Na stranici **nadzorne ploče** obratite pozornost na ovom VNet ne postoji još konfiguriran pristupnik. Vidjet ćete taj status kao proći kroz korake da biste konfigurirali pristupnikom.

![Pristupnik nije stvorena](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Nakon toga pri dnu stranice kliknite **Stvaranje pristupnika**. Možete odabrati *Statički usmjeravanje* ili *Dinamički usmjeravanja*. Usmjeravanje Vrsta VPN-a odaberete ovisi o nekoliko čimbenika. Ako, na primjer, što podržava uređaju VPN-a i trebate li podržava veze točke na web. Provjerite [O VPN uređaji za virtualne mrežne veze](vpn-gateway-about-vpn-devices.md) da biste provjerili VPN usmjeravanje vrstu koje su vam potrebne. Nakon što pristupnika, ne možete promijeniti između načina usmjeravanja pristupnika VPN bez brisanja i ponovno stvaranje pristupnika. Sustav od vas traži da biste potvrdili da želite da pristupnik stvorili, kliknite **da**.

![Pristupnik za VPN usmjeravanje vrsta](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Kada se stvara pristupnikom uočite grafiku pristupnika na stranici mijenja žutu piše *Stvaranje pristupnika*. Može potrajati do 45 minuta za pristupnik za stvaranje. Pričekajte dok se ne dovrši pristupnika prije pomicati prema naprijed s drugih postavki konfiguriranje.

![Stvaranje pristupnika](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Kad pristupnika promijeni *Povezivanje*, možete prikupljanje podataka potreban vam je za svoj uređaj VPN-a.

![Povezivanje pristupnika](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>Prikupite informacije o konfiguraciji uređaja VPN-a

Nakon stvaranja pristupnika Prikupite informacije o konfiguraciji uređaja VPN-a. Ove informacije nalazi se na stranicu **nadzorne ploče** za virtualne mreže:

1. **Pristupnik IP adresa –** IP adresa možete pronaći na stranicu **nadzorne ploče** . Nećete moći vidjeti tek nakon što završite stvaranje pristupnikom.

1. **Zajednički se koristi tipku-** Kliknite **Upravljanje ključem** pri dnu zaslona. Kliknite ikonu pokraj ključ za kopiranje u međuspremnik, a zatim zalijepite i spremanje tipku. Ovaj gumb funkcionira samo ako postoji jedan tunelom S2S VPN-a. Ako imate više tunnels S2S VPN, koristite *Se virtualne mreže zajednički ključ pristupnika* API-JA ili cmdlet ljuske PowerShell.

![Upravljanje ključem](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>Konfiguriranje uređaju VPN-a

Nakon dovršetka prethodne korake, vi ili vaš administrator mreže morat ćete konfigurirati VPN-a da biste stvorili vezu. Dodatne informacije o VPN uređajima potražite u članku [O VPN uređaji za virtualne veza s mrežom](vpn-gateway-about-vpn-devices.md) .

Nakon VPN uređaj konfiguriran, podatke s ažuriranim veza možete vidjeti na stranicu nadzorne ploče za vaše VNet.

Možete i pokrenuti jednu od sljedećih naredbi testirajte vezu:

|                      | Cisco ASA             | Cisco ISR ASR         | SSG ISG tvrtke Juniper | SRX J tvrtke Juniper                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Provjera SAs glavnog načina**  | Prikaži šifrirana uslugu isakmp Pacifička | Prikaži šifrirana uslugu isakmp Pacifička | Dohvati kolačić ike  | Prikaz sigurnosnih sigurnost pridruživanje ike   |
| **Provjera SAs brzom načinu rada** | Prikaži šifrirana ipsec Pacifička  | Prikaži šifrirana ipsec Pacifička  | Početak sa          | Prikaz sigurnosnih ipsec sigurnost-pridruživanja |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Provjerite je li raspona lokalne mreže i VPN pristupnik IP adresa

### <a name="verify-your-vpn-gateway-ip-address"></a>Provjerite je li VPN pristupnik IP adrese

Za pristupnik za povezivanje ispravno IP adresa za svoj uređaj VPN-a mora biti ispravno konfiguriran za lokalnu mrežu koja ste naveli za konfiguraciju više lokacija. Obično je to konfiguriran tijekom postupka konfiguracije web-mjesto. Međutim, ako ste već koristili lokalne mreže s drugog uređaja ili IP adresu promijenio za lokalne mreže, uredite postavke da biste odredili točan pristupnik IP adresa.

1. Da biste provjerili pristupnik IP adrese, kliknite **mrežama** u lijevom oknu portala, a zatim odaberite **Lokalne mreže** pri vrhu stranice. Prikazat će se adresa pristupnika VPN-a za svaki lokalne mreže koju ste stvorili. Da biste uredili IP adresu, odaberite na VNet, a zatim kliknite **Uređivanje** pri dnu stranice.

1. Na stranici **određivanje pojedinosti lokalne mreže** uređivanje IP adresu, a zatim dalje strelicu pri dnu stranice.

1. Na stranicu za **navođenje adrese prostora** kliknite kvačicu dolje desno da biste spremili postavke.

### <a name="verify-the-address-ranges-for-your-local-networks"></a>Provjerite je li rasponi adresa za vaše lokalne mreže

Za točan promet na tijek kroz pristupnik lokalne lokacije morate provjeriti je li navedena svakom rasponu IP adresa. Svaki raspon mora biti navedena u konfiguraciji Azure **Lokalne mreže** . Ovisno o mrežnoj konfiguraciji lokalne lokacije, to može biti Pomalo velike zadatka. Promet koja je povezana IP adresa koji se nalazi unutar popisa raspona poslat će se preko VPN pristupnika virtualne mreže. Raspone koji navedete ne moraju biti privatne raspona, iako će želite provjeriti konfiguraciju lokalnog možete primati dolazni promet.

Za dodavanje ili uređivanje raspona za lokalne mreže, poduzmite sljedeće korake.

1. Da biste uredili rasponi IP adresa za lokalnu mrežu, kliknite **mrežama** u lijevom oknu portala, a zatim odaberite **Lokalne mreže** pri vrhu stranice. Na portalu Najlakši način za prikaz raspona koji ste naveden je na stranici **Uređivanje** . Da biste vidjeli raspone, odaberite na VNet, a zatim kliknite **Uređivanje** pri dnu stranice.

1. Na stranici **određivanje pojedinosti lokalne mreže** ne unesite željene promjene. Kliknite Dalje strelicu pri dnu stranice.

1. Na stranicu za **navođenje prostor adrese** promijenite mreže adresu prostora. Kliknite kvačicu da biste spremili konfiguraciju.

## <a name="how-to-view-gateway-traffic"></a>Kako prikazati promet pristupnika

Pristupnik i promet pristupnika možete pogledati sa stranice virtualne mreže **nadzorne ploče** .

Na stranici **nadzornu ploču** možete prikazati sljedeće:

- Količinu podataka koje je slijedi putem pristupnika, podataka i podataka.

- Nazivi DNS poslužitelji navedenima za virtualne mreže.

- Veza između pristupnikom i uređaju VPN-a.

- Zajednički ključ koji se koristi za konfiguraciju pristupnika vezu s uređajem VPN-a.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Kako promijeniti vrstu za usmjeravanje VPN-a za pristupnikom

Jer nekim konfiguracijama povezivanje dostupne su samo za određene vrste usmjeravanje pristupnika, vidjet ćete morati promijeniti pristupnika VPN usmjeravanje vrstu postojeće pristupnik za VPN-a. Ako, na primjer, možda želite dodati povezivanje točke na web već postojeće web-mjesto veze koja sadrži statične pristupnika. Točke web veze zahtijevaju dinamički pristupnika. To znači da biste konfigurirali P2S veze, imate da biste promijenili pristupnika VPN usmjeravanje vrsta statične u dinamičku.

Ako morate promijeniti pristupnika usmjeravanje Vrsta VPN, ćete izbrisati postojeće pristupnika, a zatim stvorite novi pristupnik s novu vrstu usmjeravanja. Ne morate da biste izbrisali cijelu virtualne mreže da biste promijenili vrste usmjeravanje pristupnika.

Prije promjene pristupnika usmjeravanje Vrsta VPN, svakako provjerite podržava li vaš uređaj VPN vrstu usmjeravanje koju želite koristiti. Preuzimanje nove usmjeravanje konfiguracije uzorke i provjerite VPN uređaju preduvjete potražite u članku [O VPN uređaji za virtualne mrežne veze](vpn-gateway-about-vpn-devices.md).

>[AZURE.IMPORTANT] Kada izbrišete pristupnik VPN virtualne mreže, izdala je VIP dodijeljene pristupnika. Kada se ponovno stvaranje pristupnika, novi VIP joj je dodijeljena.

1. **Brisanje postojeće pristupnika VPN-a.**

    Na stranici **nadzorne ploče** za virtualne mreže pomaknite se do dna stranice, a zatim kliknite **Brisanje pristupnika**. Pričekajte obavijesti pristupnika je izbrisana. Kada primite obavijest na zaslonu pristupnikom je izbrisan, možete stvoriti novi pristupnik.

1. **Stvaranje novog pristupnika VPN-a.**

    Pomoću postupka pri vrhu stranice da biste stvorili novi pristupnik: [Stvaranje pristupnika VPN-a](#create-a-vpn-gateway).


## <a name="next-steps"></a>Daljnji koraci

Možete dodati virtualnim strojevima virtualne mreže. Saznajte [kako stvoriti prilagođene virtualnog računala](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Ako želite konfigurirati vezu za VPN točke na web, potražite u članku [Konfiguriranje VPN veza točke na web](vpn-gateway-point-to-site-create.md).

 
