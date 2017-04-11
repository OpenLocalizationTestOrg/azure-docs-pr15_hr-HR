<properties
     pageTitle="Stvaranje koncentratora za IoT pomoću portala za Azure | Microsoft Azure"
     description="Pregled uputa za stvaranje i upravljanje Azure IoT koncentratora putem portala za Azure"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Stvaranje koncentratora za IoT pomoću portala za Azure

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Uvod

U ovom se članku opisuje kako pronaći IoT koncentrator servisa na portalu za Azure i kako stvoriti i upravljanje IoT koncentratora.

## <a name="where-to-find-iot-hubs"></a>Gdje pronaći IoT koncentratora

Postoje razni mjesta gdje možete pronaći IoT koncentratora.

1. **+ Novo**: **Azure IoT koncentrator** je servis IoT i nalazi se u kategoriji **Internetske mogućnosti**, u odjeljku **+ Novo**, slično drugim servisima.

2. IoT koncentratora može se pristupiti putem trgovine kao glavni usluga u odjeljku **Internet stvari**.

## <a name="create-an-iot-hub"></a>Stvaranje koncentratora za IoT

Možete stvoriti koncentratora za IoT na sljedeće načine:

- Stvaranje koncentratora za IoT kroz mogućnost **+ Novo** vodi plohu prikazano na sljedećem zaslonu snimka. Jednake korake za stvaranje središtu IoT kroz ovu metodu i na tržištu.

- Stvaranje koncentratora za IoT putem trgovine: klikom na **Stvori** otvara plohu koja je jednaka prethodne plohu za **+ Novo** sučelje. U sljedećim odjeljcima navedeni nekoliko koraka u stvaranju koncentratora za IoT.

### <a name="choose-the-name-of-the-iot-hub"></a>Odaberite naziv središtu IoT

Da biste stvorili koncentratora za IoT, morate imenovati središtu. Taj naziv mora biti jedinstvena preko koncentratora. Nema dupliciranje koncentratora je dopušteno na pozadinska, pa se preporučuje se da taj koncentrator pod nazivom jedinstveno moguće.

### <a name="choose-the-pricing-tier"></a>Odaberite cijene sloju

Možete odabrati četiri razine: **slobodno**, **standardne 1** i **standardne 2**i **Standardne S3**. Besplatni sloju omogućuje samo 500 uređaji povezani koncentrator IoT i najviše 8.000 poruka dnevno.

**Standardni S1**: IoT koncentratora S1 edition namijenjen je IoT rješenja koja ste velik broj uređaja za generiranje relativno manjih količina podataka po uređaja. Svaku jedinicu S1 izdanja omogućuje do 400,000 poruka dnevno preko svih povezanih uređaja.

**Standardni S2**: IoT koncentrator S2 edition namijenjen je IoT rješenja u kojima uređaji za generiranje velike količine podataka. Svaku jedinicu S2 izdanja omogućuje do 6 milijuna poruka dnevno između svih povezanih uređaja.

**Standardni S3**: IoT koncentrator S3 edition namijenjen je IoT rješenja koja generiranje velike količine podataka. Svaku jedinicu S3 izdanja omogućuje do 300 milijuna poruka dnevno između svih povezanih uređaja.

![][4]

> [AZURE.NOTE] IoT koncentrator omogućuje samo jedan besplatne koncentrator po Azure pretplati.

### <a name="iot-hub-units"></a>Jedinice IoT koncentratora

Se jedinica koncentrator IoT sadrži određeni broj poruka dnevno. Ukupan broj poruke podržana za taj koncentrator je broj jedinica množi se broj poruka dnevno za taj sloju. Na primjer, ako želite IoT koncentrator za podršku ingress 700,000 poruka, odaberite dva S1 sloju jedinice.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Uređaj da biste particije oblaka i grupa resursa

Možete promijeniti broj particije za koncentratora za IoT. Zadani particije postavljene na 4; Međutim, možete odabrati različit broj particije s padajućeg popisa.

Grupa resursa, ne morate izričito Stvori grupu prazan resursa. Prilikom stvaranja resursa, možete odabrati da biste stvorili novu grupu resursa ili koristite postojeću grupu resursa.

![][5]

### <a name="choose-subscriptions"></a>Odaberite pretplate

Azure IoT koncentrator automatski se prikazuje na popisu Azure pretplate s kojim je povezan korisnički račun. Možete odabrati neku od izborniku mogućnosti da biste središte IoT pridružiti toj Azure pretplate.

### <a name="choose-the-location"></a>Odaberite mjesto

Mogućnost mjesta nalaze se popis regije u kojoj je ponuđen IoT koncentratora. Koncentrator IoT dostupna za implementaciju na sljedećim mjestima: Australija Istok, južnoazijske Australija, Azija Istok, Jugoistočne Azije, Sjeverna Europa, zapadni Europe, Istok Japan Japan Zapad, NAM Istok, Zapad NAM.

### <a name="create-the-iot-hub"></a>Stvaranje središtu IoT

Nakon dovršetka svih prethodnih koraka središtu IoT spreman je za stvaranje. Kliknite **Stvori** da biste pokrenuli postupak pozadinske stvaranja koncentratora za IoT s određene mogućnosti, a za implementaciju u navedenom mjestu.

To može potrajati nekoliko minuta središtu IoT će biti stvoren kao što je potrebno vrijeme za implementaciju pozadinske pojavljivati poslužitelje na odgovarajuće mjesto.

## <a name="change-the-settings-of-the-iot-hub"></a>Promjena postavki središtu IoT

Postavke postojeće koncentrator za IoT možete promijeniti nakon stvaranja iz koncentratora IoT plohu.

![][8]

**Zajedničko korištenje pravilnike za pristup**: ta pravila određuju dozvole za uređaje i servise za povezivanje s IoT koncentratora. Ta pravila možete pristupiti tako da kliknete **Zajednički pristup pravila** u odjeljku **Općenito**. U ovom plohu možete izmijeniti postojeća pravila ili dodavanje novog pravila.

### <a name="create-a-policy"></a>Stvaranje pravila

- Kliknite **Dodaj** da biste otvorili na plohu. Ovdje unesite novi naziv pravila, a dozvole koje želite pridružiti ovo pravilo, kao što je prikazano na sljedećoj slici.

    Postoji nekoliko dozvole koje se mogu se pridružiti ove zajedničke pravila. Prva dva pravilnike, **registra čitanje** i **Pisanje registra**, grant čitati i prava pristupa za pisanje spremišta identiteta uređaja ili registra identitet. Odabir mogućnosti zapisivanja automatski odabire kao i dodatne mogućnosti.

    Pravilnik za **Povezivanje servisa** daje dozvolu za pristup krajnje točke oblaka strane kao što su grupe potrošača za usluge povezivanja s središtu IoT. Pravila **povežite uređaj** daje dozvole za slanje i primanje poruka na krajnje točke uređaj strani od središta IoT.

- Kliknite **Stvori** da biste dodali to novostvoreni pravila na postojeći popis.

![][10]

## <a name="messaging"></a>Razmjena poruka

Kliknite **poruka** da bi se prikazao popis poruka svojstava središtu IoT koji se mijenjaju. Postoje dvije glavne vrste svojstava koja možete izmijeniti ili kopiranje: **oblaka uređaj** , a **uređaj oblak**.

- Postavke **oblaka uređaj** : Ova postavka sadrži dvije subsettings: **oblaka uređaj TTL** (vrijeme važenja) i **vrijeme zadržavanja** za poruke. Kad je stvoreno središtu IoT, obje te postavke stvaraju se uz zadanu vrijednost od jednog sata. Da biste prilagodili te vrijednosti, pomoću klizača ili upišite vrijednosti.

- Postavke **audiouređaja oblak** : Ova postavka sadrži nekoliko subsettings neki od njih pod nazivom/dodjeljuju kada središtu IoT se stvara i može se samo kopirati u drugi subsettings koje je moguće prilagoditi. Ove postavke su navedene u sljedećem odjeljku.

**Particije**: tu vrijednost postavljena središtu IoT se stvara i može se promijeniti putem tu postavku.

**Naziv događaja koncentrator kompatibilnog i krajnje točke**: stvara se kada u IoT koncentrator, koncentratora za događaj stvara interno morati pristup u određenim uvjetima. Ovo kompatibilan s preglednikom događaja koncentrator ime i krajnje točke nije moguće prilagoditi, ali je dostupan za korištenje putem gumba **Kopiraj** .

**Koliko dugo**: postavite na jedan dan po zadanom, no može se prilagoditi radi druge vrijednosti pomoću padajućeg popisa. Ova vrijednost je u danima za uređaj oblak, a ne u sati, kao što je slično postavku za oblak uređaj.

**Grupa korisnika**: korisničke grupe su slična drugi sustavi za razmjenu poruka koji se mogu koristiti za izvlačenje podataka u određenim načinima povezivanja druge programe ili servise IoT koncentrator postavku. Svaki IoT koncentrator stvara se pomoću zadane grupe korisnika. Međutim, možete dodati ili brisanje grupa korisnika s koncentratorima vaše IoT.

> [AZURE.NOTE] Zadane grupe korisnika ne mogu uređivati ni izbrisati.

![][11]

## <a name="pricing-and-scale"></a>Cijene i promjena veličine

Cijene postojeće koncentrator za IoT mogu mijenjati postavkama za **određivanje cijena** , uz sljedeće iznimke:

- U trenutnom implementaciji sustava IoT koncentrator pomoću besplatnog SKU nije moguće promijeniti razine na neki od plaćenu SKU-ove i obrnuto.
- Može postojati samo jedan koncentrator IoT besplatne sloju Azure pretplate.

![][12]

Prijelaz s više razina (S2 ili S3) u donji sloju (S1 ili S2) je dopušteno samo kada se broj poruka poslana za taj dan nisu u sukobu. Ako, na primjer, ako se broj poruka dnevno premašuje 400,000, zatim sloju za središtu IoT mogu se promijeniti. Međutim, ako promijenite u sloju S1 zatim središtu je ograničio vrijeme za taj dan.

## <a name="delete-the-iot-hub"></a>Brisanje središtu IoT

Možete pregledavati koncentrator IoT koju želite izbrisati tako da kliknete **Pregled**, a zatim odaberete odgovarajući koncentrator za brisanje. Kliknite gumb **Izbriši** ispod naziva koncentrator da biste izbrisali središtu.

## <a name="next-steps"></a>Daljnji koraci

Slijedite ove veze da biste saznali više o upravljanju Azure IoT koncentratora:

- [Skupno upravljanje uređajima IoT][lnk-bulk]
- [Korištenje mjerenja][lnk-metrics]
- [Operacije nadzora][lnk-monitor]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway]
- [Sigurne rješenje IoT na prema gore][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md