<properties
   pageTitle="Onemogućivanje i omogućivanje krajnjoj točki promet Upravitelj | Microsoft Azure"
   description="Ovaj će vam članak pomoći onemogućivanje i omogućivanje vaše krajnje točke profila Upravitelja promet."
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

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>Onemogućivanje i omogućivanje krajnjoj točki Upravitelj promet

Možete i onemogućiti pojedinačne krajnje točke koje su dio promet upravitelj profila. Krajnje točke obuhvaćaju servise u oblaku i web-mjesta. Onemogućivanje krajnje ostavlja kao dio profila, ali profil se ponaša kao krajnja točka nije obuhvaćen ga. Ova akcija vrlo je praktična za privremeno uklanjanje krajnje točke u načinu za održavanje ili se ponovno implementirati. Kada na krajnjoj točki i pokretanje ponovno, on može se omogućiti

>[AZURE.NOTE] **Onemogućivanje krajnje ne sadrži ništa učiniti s stanje implementacije u Azure. Dobar krajnjoj točki će ostati i primati promet čak i kad je onemogućen u upravitelju promet. Uz to, onemogućivanje krajnje točke u jednom profilu ne utječe na njezino će se stanje drugi profil.**

## <a name="to-disable-an-endpoint"></a>Da biste onemogućili krajnje točke

1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti, a zatim strelicu s desne strane naziva profila. Otvorit će se na stranicu Postavke profila.
1. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke uvrštene u konfiguraciji.
1. Kliknite krajnju točku koju želite onemogućiti, a zatim kliknite **Onemogući** pri dnu stranice.
1. Promet će prekinuti slijedi krajnjoj temelju na DNS vrijeme važenja (TTL) konfigurirana za promet Upravitelj naziva domene. Na stranici Konfiguracija profila Upravitelja promet možete promijeniti na TTL.

## <a name="to-enable-an-endpoint"></a>Da biste omogućili krajnje točke


1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti, a zatim strelicu s desne strane naziva profila. Otvorit će se na stranicu Postavke profila.
1. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke uvrštene u konfiguraciji.
1. Kliknite krajnju točku koju želite omogućiti, a zatim kliknite **Omogući** pri dnu stranice.
1. Promet započinje slijedi servis ponovno kao Diktirani koju profil.

## <a name="next-steps"></a>Daljnji koraci

[Promet Upravitelj – onemogućivanje, Omogući ili brisanje profila](disable-enable-or-delete-a-profile.md)

[Otklanjanje poteškoća promet Upravitelj smanjena stanja](traffic-manager-troubleshooting-degraded.md)

[Razmatranja promet Upravitelj performansi](traffic-manager-performance-considerations.md)