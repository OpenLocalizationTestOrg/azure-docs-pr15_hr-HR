<properties 
    pageTitle="Kako konfigurirati servis u oblaku (klasični portal) | Microsoft Azure" 
    description="Saznajte kako konfigurirati servise u oblaku u Azure. Saznajte kako ažurirati konfiguracija servisa oblaka i konfiguriranje daljinski pristup uloga instance." 
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>




# <a name="how-to-configure-cloud-services"></a>Konfiguriranje servisa u Oblaku

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-how-to-configure-portal.md)
- [Azure klasični portal](cloud-services-how-to-configure.md)

Na portalu za Azure klasični možete konfigurirati najčešće korištenih postavki za servise u oblaku. Ili ako želite izravno ažuriranje datoteka konfiguracije preuzeti datoteku za konfiguraciju servisa da biste ažurirali, i prenijeli ažuriranu datoteku i ažurirajte servisa u oblaku promjene konfiguracije. U svakom slučaju, ažuriranja konfiguracija pomiču se da biste sve instance uloge.

Azure klasični portal omogućuje vam da biste [omogućili Remote Desktop Connection za uloga u Azure servise u Oblaku](cloud-services-role-enable-remote-desktop.md)

Azure možete samo osiguraj posto 99.95 servisa vrijeme konfiguraciju obnavljanja ako imate barem dvije instance uloge za svaku ulogu. Koji omogućuje da obradi zahtjeve klijenta dok drugi ažurira jednu virtualnog računala. Dodatne informacije potražite u članku [Ugovore o razini usluge](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Promijenite neki servis u oblaku

1. [Azure klasični portal](http://manage.windowsazure.com/)kliknite **Servise u Oblaku**, kliknite naziv servisa u oblaku i kliknite **Konfiguriraj**.

    ![Stranica za konfiguraciju](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    Na stranici **Konfiguracija** možete konfigurirati nadzor, ažuriranje postavki uloga i odaberite goste operacijski sustav i obitelji instanci uloge. 

2. **Nadzor**, nadzor razinu postavite tekstni ili minimalnog, i konfigurirati niza veze Dijagnostika koji su potrebni za opširno nadzor.

3. Za uloge servisa (grupirane po ulogama), možete ažurirati sljedeće postavke:
    
    >**Postavke**  
    >Izmijenite vrijednosti razne konfiguracijske postavke koje su navedene u elementima *ConfigurationSettings* konfiguracije (.cscfg) datoteke servisa.
    >
    >**Certifikati**  
    >Promijenite otisak prsta certifikat koji se koristi u šifriranja SSL za uloge. Da biste promijenili certifikat, ne morate prenijeti novi certifikat (na stranici **Potvrda** ). Zatim ažurirajte otisak prsta u niz certifikat koji se prikazuje u odjeljku postavke uloge.

4. U **operacijskom sustavu**, promjena operacijski sustav obitelj ili verziju uloga instanci ili odaberite **Automatsko** da biste omogućili automatsko ažuriranje trenutne verzije operacijski sustav. Postavke operacijski sustav primijeniti na webu uloge i uloge suradnika, ali ne utječe na virtualnim računalima.

    Tijekom implementacije, najnovija verzija operacijskog sustava je instaliran na svim instancama uloge, a operacijski sustavi automatski se ažuriraju prema zadanim postavkama. 
    
    Ako vam je potrebna za vaš servis u oblaku da biste pokrenuli verzije za drugi operacijski sustav zbog preduvjeti kompatibilnosti u kodu, možete odabrati obitelji operacijski sustav i verziju. Kad odaberete verzije za određeni operacijski sustav, obustavljeno se automatski operacijski sustav ažuriranja za servis u oblaku. Morat ćete zajamčila ažuriranja operacijske sustave.
    
    Ako riješiti svi problemi s kompatibilnošću s aplikacijama s najnovija verzija operacijskog sustava, možete omogućiti automatsko operacijski sustav ažuriranja postavljanjem verzija operacijskog sustava za **Uvoz tablica**. 
    
    ![Postavke OS](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. Spremanje postavki konfiguriranje i automatske ih instance ulogu, kliknite **Spremi**. (Kliknite **Odbaci** da biste odustali od promjene). **Spremanje** i **Odbaci** dodaju na naredbenoj traci nakon što promijenite postavke.

## <a name="update-a-cloud-service-configuration-file"></a>Ažuriranje konfiguracijska datoteka servisa za oblaka

1. Preuzimanje oblaka servisa konfiguracije datoteke (.cscfg) s trenutnom konfiguracijom. Na stranici **Konfiguracija** servisa u oblaku, kliknite **Preuzmi**. Zatim kliknite **Spremi**ili kliknite **Spremi kao** za spremanje datoteke.

2. Nakon što ažurirate konfiguracijska datoteka servisa, prijenos i ažuriranje konfiguracije:

    1. Na stranici **Konfiguracija** kliknite **Prenesi**.
    
        ![Prijenos konfiguracija](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. U **konfiguracijskoj datoteci**, koristite **Pregledaj** da biste odabrali datoteku ažurirane .cscfg.
    
    3. Ako servis u oblaku sadrži sve uloge koje imate samo jednu instancu, uključite potvrdni okvir **Primjena konfiguracije čak i ako jednu ili više uloga sadrže jednokratan** da biste omogućili ažuriranja konfiguracija za uloge da biste nastavili.
    
        Osim ako definirate barem dvije instance programa svaku ulogu, Azure ne jamči barem 99.95 posto raspoloživost servis u oblaku tijekom ažuriranja za konfiguraciju servisa. Dodatne informacije potražite u članku [Ugovore o razini usluge](https://azure.microsoft.com/support/legal/sla/).
    
    4. Kliknite **u redu** (kvačica). 


## <a name="next-steps"></a>Daljnji koraci

* Saznajte kako [implementirati servis u oblaku](cloud-services-how-to-create-deploy.md).
* Konfiguriranje [Prilagođeni naziv domene](cloud-services-custom-domain-name.md).
* [Upravljanje servis u oblaku](cloud-services-how-to-manage.md).
* [Omogućivanje Remote Desktop Connection za uloge servisa Azure oblak sustava](cloud-services-role-enable-remote-desktop.md)
* Konfiguriranje [ssl certifikata](cloud-services-configure-ssl-certificate.md).
