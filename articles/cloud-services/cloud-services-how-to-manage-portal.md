<properties 
    pageTitle="Uobičajene zadatke upravljanja servisa oblaka | Microsoft Azure" 
    description="Informirajte se o upravljanju servise u oblaku na portalu za Azure. U ovim se primjerima pomoću portala za Azure." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>Kako upravljati servise u Oblaku

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-how-to-manage-portal.md)
- [Azure klasični portal](cloud-services-how-to-manage.md)

Servis u oblaku upravlja se u područje za **Servise u Oblaku (klasični)** portala za Azure. U ovom se članku opisuju neke uobičajene akcije osvojio tijekom upravljanja servise u oblaku. Koja sadrži ažuriranje, brisanje, promjena veličine i prosljeđivanja postupnu implementacije radnog.

Dodatne informacije o skaliranje servis u oblaku dostupna je [u nastavku](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Kako: ažuriranje uloge servisa oblaka ili uvođenje

Ako trebate ažurirati kod aplikacije servisa za oblak, koristite **Ažuriranje** na servis plohu oblaka. Možete ažurirati ulogu jedne ili svih uloga. Da biste ažurirali, možete prenijeti nove paketa ili servisa konfiguracijska datoteka.

1. [Portal za Azure][]odaberite servisa u oblaku koji želite ažurirati. Ovaj korak otvara plohu instanca servisa za oblaka.

2. U plohu, kliknite gumb **Ažuriraj** .

    ![Gumb Ažuriraj](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Ažurirajte implementacijskih nove datoteke servisa paketa (.cspkg) i servis konfiguracijska datoteka (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Po želji** ažurirajte oznaku implementacije i račun za pohranu. 

5. Ako sve uloge imate samo jednu instancu uloge, odaberite na **Implementacija čak i ako jednu ili više uloga sadrže jednokratan** za omogućivanje nadogradnje da biste nastavili. 

    Azure možete samo jamči dostupnost usluge postotak 99.95 tijekom ažuriranja servisa oblaka ako svaka uloga ima najmanje dva uloga instance (virtualnim strojevima). Pomoću dvije instance uloga jedan virtualnog računala obradit će se zahtjevi klijenta dok drugi ažurira.

6. Provjerite ažurirati primjenjuje nakon dovršetka prijenosa paketa **pokretanje implementacije** .

7. Kliknite **u redu** da biste započeli ažuriranje servis.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Kako: zamjena implementacijama Promicanje postupnu implementacije radnog

Nakon što ste odlučili implementacije novo izdanje oblaku faza i testirati novo izdanje u svom okruženju pripremna oblaka servisa. Da biste se prebacili URL-ovi koji dva implementacijama su i Promicanje novo izdanje da biste radni pomoću **Zamijeni** . 

Možete zamijenite implementacije na stranici za **Servise u Oblaku** ili na nadzornu ploču.

1. [Portal za Azure][]odaberite servisa u oblaku koji želite ažurirati. Ovaj korak otvara plohu instanca servisa za oblaka.

2. U plohu, kliknite gumb **Zamijeni** .

    ![Zamjena Services oblaka](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Otvorit će se sljedeće odzivnik za potvrdu.

    ![Zamjena Services oblaka](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Kada potvrdite informacije o implementaciji, kliknite **u redu** da biste zamijenili u implementacijama.

    Zamjena implementacije odvija brzo jer je jedini način mijenja virtualne IP adrese (VIPs) za na implementacije.

    Troškova računalnim, možete izbrisati pripremna implementacije kada potvrdite da implementaciju sustava radnog funkcioniraju kako želite.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Kako: veza resursa na neki servis u oblaku

Portal za Azure veza resursi zajedno kao da se ne trenutni Azure klasični portal. Umjesto toga implementirati dodatne resurse u grupu resursa koristi servis u Oblaku.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Kako: brisanje implementacije i servise u oblaku

Da biste izbrisali neki servis u oblaku, morate izbrisati svaki postojeće implementacije.

Troškova računalnim, možete izbrisati pripremna implementacije kada potvrdite da implementaciju sustava radnog funkcioniraju kako želite. Se naplatiti za računalnim trošak instance distribuiranih uloge koje su zaustavljena.

Pomoću sljedećeg postupka možete izbrisati implementacije ili servis u oblaku. 

1. [Portal za Azure][]odaberite servisa u oblaku koji želite izbrisati. Ovaj korak otvara plohu instanca servisa za oblaka.

2. U plohu, kliknite gumb **Izbriši** .

    ![Zamjena Services oblaka](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Možete izbrisati cijelu oblaku tako da **u oblaku i njegova implementacija** ili odaberite **implementacije proizvodnje** ili **pripremna implementacije**.

    ![Zamjena Services oblaka](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. Kliknite gumb **Izbriši** pri dnu.

5. Da biste izbrisali servisa u oblaku, kliknite **Izbriši u oblaku**. Zatim, kada se zatraži potvrda, kliknite **da**.

> [AZURE.NOTE]
> Kada se briše servis u oblaku i opširno nadzor konfiguriran, podatke morate izbrisati ručno s računa za pohranu. Informacije o tome gdje da biste pronašli metriku tablice potražite u [ovom](cloud-services-how-to-monitor.md) članku.

[Portal za Azure]: https://portal.azure.com

## <a name="next-steps"></a>Daljnji koraci

* [Općenito konfiguracije servis u oblaku](cloud-services-how-to-configure-portal.md).
* Saznajte kako [implementirati servis u oblaku](cloud-services-how-to-create-deploy-portal.md).
* Konfiguriranje [Prilagođeni naziv domene](cloud-services-custom-domain-name-portal.md).
* Konfiguriranje [ssl certifikata](cloud-services-configure-ssl-certificate-portal.md).