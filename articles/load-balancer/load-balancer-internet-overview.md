
<properties
   pageTitle="Internet nasuprotne pregled raspoređivača opterećenja | Microsoft Azure "
   description="Pregled za internetske nasuprotne opterećenja i njegove značajke. Kako funkcionira raspoređivača opterećenja za Azure korištenje virtualnih računala i servisa u oblaku."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internet-facing-load-balancer-overview"></a>Internet dostupnog učitavanja opterećenja pregled

Azure raspoređivača opterećenja karata javnu IP adresa i priključaka broj promet privatnog IP adresa i priključaka broja virtualnog računala i obrnuto za odgovor promet iz virtualnog računala. Pravila za ujednačavanje opterećenja omogućuju distribucija određene vrste promet između više virtualnih računala ili servisa. Ako, na primjer, možete šire učitavanja promet zahtjev za web u više web-poslužiteljima ili uloge web.

Za servise u oblaku koja sadrži instance web uloge i uloge suradnika, možete definirati javno krajnje točke u datoteci definicije (.csdef) servisa.

Datoteka _servicedefinition.csdef_ sadrži konfiguraciju krajnju točku i ako imate više instanci uloga uloga implementacije weba ili tempiranja, raspoređivača opterećenja bit će instalacijski program za njega. Način za dodavanje instance implementaciju sustava oblaka mijenja broj instanci na servis konfiguracijska datoteka (.csfg).

Na sljedećoj je slici prikazan na uravnoteženja krajnja točka za promet šifrirane web koji se zajednički koristi tri virtualnim strojevima za u javnim i privatnim TCP priključak 443. Ove tri virtualnim strojevima su u skupu uravnoteženja.

![primjer raspoređivača opterećenja javno](./media/load-balancer-internet-overview/IC727496.png))

Slika 1 - uravnoteženja krajnja točka za šifriranu web promet

Kada klijenti Internet Pošalji zahtjeve za web-stranice na javnu IP adresu servisa u oblaku na TCP priključak 443, Azure raspoređivača opterećenja distribuira zahtjeva između tri virtualnim strojevima u skupu uravnoteženja. Dodatne informacije o algoritmima raspoređivača opterećenja potražite u članku [Učitavanje opterećenja implicitno](load-balancer-overview.md#load-balancer-features).

Prema zadanim postavkama Azure opterećenja distribuira mrežni promet ravnomjerno među više instanci virtualnog računala. Možete konfigurirati i afinitet sesiju, dodatne informacije potražite u članku [način za raspodjele raspoređivača učitavanja](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Daljnji koraci

Informirajte se o [internog učitati opterećenja](load-balancer-internal-overview.md) da biste bolje razumjeli koje opterećenja je bolje odgovaraju za implementaciju sustava oblaka.

Možete i [Početak rada prilikom stvaranja internetsku nasuprotne raspoređivača opterećenja](load-balancer-get-started-internet-arm-ps.md) i konfiguriranje koju vrstu [način distribucije](load-balancer-distribution-mode.md) za promet ponašanje određenih učitavanja raspoređivača mreže.

Ako aplikacija nije potrebno održavanje veza aktivnosti za poslužitelje iza raspoređivača opterećenja, koje mogu razumjeti dodatne informacije o [neaktivnosti TCP vremenskog ograničenja postavke raspoređivača opterećenja](load-balancer-tcp-idle-timeout.md). To će pomoći da biste saznali o ponašanju neaktivne veze prilikom korištenja Azure raspoređivača opterećenja.
