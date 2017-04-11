<properties
    pageTitle="Kontroliranje Azure web app promet programa Azure promet Manager"
    description="Ovaj članak sadrži sažetak informacija za Azure promet upravitelja dok se odnosi na Azure web-aplikacije."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Kontroliranje Azure web app promet programa Azure promet Manager

> [AZURE.NOTE] Ovaj članak sadrži sažetak informacija za Microsoft Azure promet upravitelja dok se odnosi na Azure aplikacije servisa web-aplikacije. Dodatne informacije o Azure promet upravitelju sam pronaći ćete tako što ste posjetili veze pri kraju ovog članka.

## <a name="introduction"></a>Uvod
Pomoću upravitelja promet Azure da biste odredili kako se na web-aplikacije u aplikacije servisa za Azure raspodijeljenih zahtjeve iz web-klijentima. Kada krajnje točke web app dodaju se u upravitelju promet Azure profil, Upravitelj promet Azure evidentira status (pokrenut, prestao ili izbrisani) web aplikacije tako da možete odlučiti koju od tih krajnje točke trebale primiti promet.

## <a name="load-balancing-methods"></a>Učitavanje ujednačavanje metode
Azure promet Upravitelj koristi tri načina za ujednačavanje opterećenja različite. Te su opisane u pomoću sljedećeg popisa kao one se odnose na Azure web-aplikacije.

* **Prebacivanje**: Ako imate web app clones u različitim područjima, možete ovaj postupak koristite da biste konfigurirali jedan web aplikaciju servisa promet klijenta za sve web i konfigurirati drugi web-aplikacije u nekoj drugoj regiji za servis te promet za slučaj da prvi web-aplikaciji postane nedostupan.

* **Kružno**: Ako imate web app clones u različitim područjima, možete koristiti ovu metodu za web-aplikacije u različitim područjima ravnomjerno raspodijelite promet.

* **Performanse**: način u performanse distribuira promet na temelju trajanja pretraživanje po najkraćoj putovanja klijentima. Web-aplikacijama unutar iste područja ili u različitim područjima može se koristiti metodu performansi.

##<a name="web-apps-and-traffic-manager-profiles"></a>Web-aplikacije i promet upravitelj profila
Da biste konfigurirali kontrolu web app prometa, stvaranje profila u programu Azure promet Manager koji koristi neku od tri učitali ujednačavanje metode prethodno opisane, a zatim dodajte krajnjih točaka (u ovom slučaju web-aplikacije) za koju želite upravljati promet na profil. Status aplikacija web (radi, prekinuli ili izbrisati) redovito prenosi profilu tako da se Azure promet Manager možete usmjeriti promet sukladno tome.

Ako koristite Upravitelj promet Azure uz Azure, imajte na umu sljedeće točke:

* U slučaju web app samo implementacije unutar iste područja web-aplikacije već omogućuje prebacivanje i funkcionalnost kružnog bez obzira na način web app.

* U slučaju implementacije u istom području koje koriste web-aplikacijama zajedno s nekom drugom Azure u oblaku, možete kombinirati obje vrste krajnje točke da biste omogućili scenarija hibridnog.

* U profilu možete navesti samo jedan web app krajnje točke po regijama. Kada odaberete web-aplikacijama kao krajnje točke za određenu regiju, Preostali web-aplikacije u tom području postaju dostupne za odabir za taj profil.

* Krajnje točke web app koji navedete u profilu Azure promet Upravitelj pojavit će se u odjeljku **Naziva domena** na stranici Konfiguracija za web-aplikacije u profilu, ali neće biti moguće konfigurirati postoji.

* Nakon što dodate web-aplikacijama profila, **URL web-mjesta** na nadzornoj ploči za stranici portala za web-aplikaciji prikazat će URL prilagođene domene web-aplikacije ako nešto ste postavili. U suprotnom će prikazati promet upravitelj profila URL-a (na primjer, `contoso.trafficmgr.com`). Oba Izravni naziv domene web-aplikacije i URL promet Upravitelj bit će vidljivo na stranici Konfiguracija web-aplikaciju u odjeljku **Naziva domena** .

* Naziva prilagođene domene će funkcioniraju, no uz ih dodate na web-aplikacije, morate konfigurirati i DNS karti na promet Upravitelj URL-a. Informacije o tome kako postaviti prilagođene domene za Azure web-aplikacijama, potražite u članku [Konfiguriranje prilagođenog naziva domene za Azure web-mjesto](web-sites-custom-domain-name.md).

* Možete dodavati samo web-aplikacije koje su u standardnom načinu Azure promet Upravitelj profil.

## <a name="next-steps"></a>Daljnji koraci

Konceptualni i Tehnički pregled Azure promet upravitelja, potražite u članku [Pregled upravitelja promet](../traffic-manager/traffic-manager-overview.md).

Dodatne informacije o korištenju upravitelja promet s web-aplikacije potražite u članku objava na blogu o [Korištenju Azure promet Upravitelj Azure Web-mjesta](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) i [Azure promet Upravitelj sada možete integrirati s Azure web-mjesta](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).
