<properties
   pageTitle="Onemogućivanje, omogućite ili brisanje profila promet Upravitelj | Microsoft Azure"
   description="U ovom se članku pronaći ćete raditi na promet upravitelj profila."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>Onemogućivanje, omogućite ili brisanje profila


Postojeći profil promet Manager možete onemogućiti tako da se zahtjevi za korisnika će se na njegov konfigurirani krajnje točke. Kada onemogućite promet upravitelj profila, samom profilu i informacije koje se nalaze u profilu ostaje netaknuta, a mogu se uređivati u sučelju Upravitelj promet. Kada želite ponovno omogućiti profila, jednostavno možete učiniti tako da u na Azure će nastaviti portal i referenci. Kada stvorite profil Upravitelj promet na portalu za Azure, on je automatski omogućena.

## <a name="to-disable-a-profile"></a>Da biste onemogućili profila

1. Izmjena DNS zapisa resursa na Internet DNS poslužitelju da biste koristili upišite odgovarajući zapis i pokazivačem na neki drugi naziv ili IP adresu na određeno mjesto na Internetu. Drugim riječima, promijenite DNS zapisa resursa na Internet DNS poslužitelj tako da više ne koristi CNAME zapis za resursa koji upućuje na naziv domene promet upravitelj profila.
1. Promet prekinut će se usmjereni na krajnje točke putem upravitelja promet postavke profila.
1. Odaberite profil koji želite onemogućiti. Da biste odabrali profila na stranici upravitelja promet istaknite profil tako da kliknete stupac pokraj naziva profila. Kliknite naziv profila ili strelicu pokraj naziva, kao što je to će vas odvesti na stranicu Postavke profila.
1. Nakon odabira profila, kliknite Onemogući pri dnu stranice.

## <a name="to-enable-a-profile"></a>Da biste omogućili profila

1. Odaberite profil koji želite omogućiti. Da biste odabrali profila na stranici upravitelja promet istaknite profil tako da kliknete stupac pokraj naziva profila. Kliknite naziv profila ili strelicu pokraj naziva, kao što je to će vas odvesti na stranicu Postavke profila.
1. Nakon odabira profila, kliknite Omogući pri dnu stranice.
1. Izmjena DNS zapisa resursa na poslužitelj za Internet DNS-a da biste koristili vrsta CNAME zapisa koji mapira naziva domene tvrtke naziv domene promet upravitelj profila. Dodatne informacije potražite u članku [točke domene tvrtke Internet promet Upravitelj domena](traffic-manager-point-internet-domain.md).
1. Promet počet će se usmjereni na krajnjih točaka ponovno.

## <a name="delete-a-profile"></a>Brisanje profila


1. Provjerite je li DNS zapisa resursa na Internet DNS poslužitelj više ne koristi CNAME zapis za resursa koji upućuje na naziv domene promet upravitelj profila.
1. Odaberite profil koji želite izbrisati. Da biste odabrali profila na stranici upravitelja promet isticanje profil s
1. Klikom na stupcu uz profil. Kliknite naziv profila ili strelicu pokraj naziva, kao što je to će vas odvesti na stranicu Postavke profila.
1. Nakon odabira profila, kliknite Izbriši pri dnu stranice.

## <a name="next-steps"></a>Daljnji koraci

[Promet Upravitelj – onemogućivanje i omogućivanje krajnje točke](disable-or-enable-an-endpoint.md)

[Konfiguriranje metodu usmjeravanja prebacivanje](traffic-manager-configure-failover-routing-method.md)

[Konfiguriranje metodu usmjeravanja kružnog](traffic-manager-configure-round-robin-routing-method.md)

[Konfiguriranje metodu usmjeravanja performansi](traffic-manager-configure-performance-routing-method.md)

[Otklanjanje poteškoća promet Upravitelj smanjena stanja](traffic-manager-troubleshooting-degraded.md)

