<properties
    pageTitle="Automatsko skaliranje servis u oblaku na portalu | Microsoft Azure"
    description="Saznajte kako pomoću portala za konfiguriranje pravila skaliranje automatski oblaka servisa web uloga ili uloga suradnika u Azure."
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
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Upute za automatsko skaliranje servis u oblaku

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-how-to-scale-portal.md)
- [Azure klasični portal](cloud-services-how-to-scale.md)

Uvjeta moguće je postaviti za tempiranja ulogu servisa oblaka koja pokretanje skalu ili smanjili operacija. Uvjeti za ulogu mogu se temeljiti na procesora, disk ili opterećenje mreže uloge. Možete postaviti i conditation na temelju red čekanja za poruke ili metriku neke Azure resursa povezan s pretplatom.

>[AZURE.NOTE] U ovom se članku usredotočuje se na servis u Oblaku web- a tempiranja uloge. Kada stvorite virtualnog računala (klasični) izravno, nalaziti u oblaku. Možete skaliranje standardne virtualnog računala povezivanjem pomoću programa [Postavljanje dostupnosti](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) i ručno ih uključili ili isključili.

## <a name="considerations"></a>Razmatranja

Prije no što konfigurirate skaliranje za aplikaciju, razmotrite sljedeće informacije:

- Promjena veličine utječe core korištenje. Veće instance uloga pomoću više jezgri. Zatvaranje samo unutar ograničenja jezgri mogu mijenjati veličinu za vašu pretplatu. Na primjer, pretplate ima ograničenje od dvadeset jezgri i pokretanje aplikacija s dva srednje veličine oblaka services (ukupno četiri jezgri), koje možete samo proširenja druge implementaciji servisa oblak u svoju pretplatu tako da šesnaest jezgri. Dodatne informacije o veličinama potražite u članku [Oblaka servisa veličine](cloud-services-sizes-specs.md) .

- Mogu mijenjati veličinu temelju praga reda čekanja poruka. Dodatne informacije o načinu korištenja redova potražite u članku [kako koristiti servis za pohranu red](../storage/storage-dotnet-how-to-use-queues.md).

- Možete skaliranja i ostale resurse povezan s pretplatom.

- Da biste omogućili visoke dostupnosti aplikacije, potrebno je provjeriti je li implementiran s dva ili više instanci uloge. Dodatne informacije potražite u članku [Ugovore o razini usluge](https://azure.microsoft.com/support/legal/sla/).

## <a name="where-scale-is-located"></a>Gdje se nalazi skala

Nakon što odaberete servis u oblaku, imat ćete vidljivi plohu servisa oblaka.

1. Na servis plohu oblak koji se nalazi na pločici **uloge i instance** odaberite naziv servisa u oblaku.   
**Važno**: svakako kliknite uloge servisa oblaka, ne uloga instancu ispod ulogu.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Odaberite pločicu **mjerilo** .

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatska promjena veličine

Možete konfigurirati postavke mjerila za uloge s dva načina rada **ručno** ili **automatsku**. Ručno kao što ste očekivali, postavite apsolutne broj instanci. Automatsko međutim omogućuje morate postaviti pravila koje određuju kako i kako mnogo vam mora skaliranje.

Mogućnost **Skaliranje tako da** postavite na **Raspored i performanse pravila**.

![Postavke mjerila oblaka usluge profila i pravila](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Postojeći profil.
2. Dodavanje pravila nadređenog profila.
3. Dodajte drugi profil.

Odaberite **Dodaj profila**. Profil određuje koji način na koji želite koristiti za skale: **uvijek** **ponavljanja**, **fiksnog datuma**.

Nakon što ste konfigurirali profil i pravila, odaberite ikonu **Spremi** pri vrhu.

#### <a name="profile"></a>Profil

Profil postavlja minimalne i maksimalne instanci skale i i kada je aktivna dosegu mjerilo.

* **Uvijek**

    Uvijek imajte dosegu instanci koje su dostupne.  

    ![Servis u oblaku koja se uvijek skaliranja](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Ponavljanje**

    Odaberite skup dane u tjednu za promjenu veličine.

    ![Oblak servisa Skaliranje s raspored ponavljanja](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Fiksni datum**

    Fiksni datum raspon za promjenu veličine ulogu.

    ![Oblak servisa Skaliranje s Fiksni datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Nakon što ste konfigurirali profil, odaberite gumb **u redu** pri dnu plohu profila.

#### <a name="rule"></a>Pravila

Pravila dodat će se na profil i predstavljaju uvjet koji će pokrenuti skalu. 

Pokretanje pravila temelji se na metrika tog servisa u oblaku (procesora, aktivnosti disk ili mreže aktivnosti) u koje možete dodati uvjetno vrijednost. Uz to može imati okidača na temelju red čekanja za poruke ili metriku neke Azure resursa povezan s pretplatom.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Nakon što ste konfigurirali pravilo, odaberite gumb **u redu** pri dnu plohu pravilo.

## <a name="back-to-manual-scale"></a>Povratak na Ručna promjena veličine

Idite na [Postavke mjerila](#where-scale-is-located) i postavite mogućnost **Skaliranje tako da** **na instancu broj koji se unosi ručno**.

![Postavke mjerila oblaka usluge profila i pravila](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Time se uklanjaju svi automatiziranog skaliranje ulogu, a zatim možete postaviti broj instanci izravno. 

1. Promjena veličine (ručno ili automatiziranog) mogućnost.
2. Klizač instancu uloga da biste postavili instanci za promjenu veličine da biste.
3. Instance uloge za promjenu veličine da biste.

Nakon što ste konfigurirali postavke mjerila, odaberite ikonu **Spremi** pri vrhu.

