<properties
   pageTitle="Konfiguriranje postojeće aplikacije pristupnik za hostiranje više web-mjesta na portalu za Azure | Microsoft Azure"
   description="Ova stranica sadrži upute za konfiguriranje postojeće pristupnika Azure aplikacije za hostiranje više web-aplikacija na istom Pristupniku s portala za Azure."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfiguriranje postojeće aplikacije pristupnik za hostiranje više web-aplikacije

> [AZURE.SELECTOR]
- [Portal za Azure](application-gateway-create-multisite-portal.md)
- [Azure PowerShell Voditelj resursa](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Više hostiranja web-mjesta omogućuje implementacija više web-aplikacija na istom Pristupniku aplikacije. To ovisi o prisutnosti zaglavlja glavnog računala na dolazni zahtjev HTTP da biste utvrdili koji ga slušatelj dobiti promet. Ga slušatelj usmjerava promet na odgovarajuće pozadinskog skup konfigurirani u definiciji pravila pristupnika. U SSL omogućen web-aplikacijama pristupnika aplikacije ovisi o proširenje oznaka poslužitelja naziva (SNI) da biste odabrali ispravnu ga slušatelj za promet za web. Često se koristi za hostiranje web-mjesta za više je da biste učitali zahtjeva za različite web domene na drugi poslužitelj pozadinske grupe. Na sličan način više poddomena isti korijensku domenu može se nalaziti na istom Pristupniku aplikacije.

## <a name="scenario"></a>Scenarij

U sljedećem primjeru pristupnik za aplikaciju je posluživanje promet za contoso.com i fabrikam.com s dvije grupe pozadinskih poslužitelja: contoso resurse poslužitelja i resurse poslužitelja fabrikam. Postavljanje slične mogu se upotrijebiti za poddomene glavnog računala kao što su app.contoso.com i blog.contoso.com.

![scenarij dodjeli resursa][multisite]

## <a name="before-you-begin"></a>Prije početka

Ovaj scenarij dodaje podršci više web-mjesta u postojeću pristupnik za aplikaciju. Kako bi obavili ovaj scenarij scenarij postojeće pristupnik za aplikaciju mora biti dostupan za konfiguriranje. Posjetite [Stvaranje pristupnika za aplikaciju pomoću portala za](./application-gateway-create-gateway-portal.md) da biste saznali kako stvoriti pristupnik osnovne aplikacije na portalu.

## <a name="requirements"></a>Preduvjeti

- **Skup pozadinskih poslužitelja:** Popis IP adrese pozadinskih poslužitelja. IP adrese na popisu ili trebao bi se nalaziti podmreže virtualne mreže ili mora biti javnu IP/VIP. FQDN može se koristiti.
- **Skup postavke poslužitelja za pozadinsku:** Svaki skup ima postavke kao što su priključak, protokol i sustavom kolačića afinitet. Ove postavke je uz zajedničko područje, a primjenjuju se na svim poslužiteljima u na resurse.
- **Sučelja priključka:** U ovom priključak je javno priključak koja je otvorena na računala pristupnika. Promet dodirne priključak, a zatim prosljeđuje na neki od pozadinskih poslužitelja.
- **Ga slušatelj:** Ga slušatelj sadrži sučelja priključak protokol (Http ili Https, te su vrijednosti velika i mala slova), a naziv SSL certifikata (Ako je konfiguriranje SSL offload). Za pristupnika omogućeno aplikacije više web-mjesta, naziv glavnog računala i pokazatelje SNI dodaju.
- **Pravilo:** Povezuje ga slušatelj, skup pozadinskih poslužitelja i pravila definira grupe aplikacija pozadinskih poslužitelja promet treba usmjeriti na kada je dodirne određeni ga slušatelj.
- **Potvrde:** Svaki ga slušatelj zahtijeva jedinstveni certifikat, u ovom primjeru 2 slušače stvaraju više web-mjesta. Dva .pfx potvrde i lozinke za njih potreban će biti stvoren.

## <a name="create-an-application-gateway"></a>Stvaranje pristupnika za aplikaciju

Slijede korake potrebne za ažuriranje pristupnika za aplikaciju:

1. Stvaranje pozadinske grupe za svaku web-mjesta.
2. Stvorite novi ga slušatelj za će podržavati svaki pristupnika aplikacije web-mjesta.
3. Stvaranje pravila za mapiranje svakog ga slušatelj s odgovarajućim pozadinskoj.

## <a name="create-back-end-pools-for-each-site"></a>Stvaranje pozadinske grupe za svaku web-mjesta

Skup pozadinske za svako web-mjesto te aplikacije pristupnika će je potrebna podrška, u tom slučaju 2 stvorit će se, jedan za contoso11.com i jedan za fabrikam11.com.

### <a name="step-1"></a>Korak 1

Dođite do postojeće aplikacije pristupnika na portalu za Azure (https://portal.azure.com). Odaberite **pozadinskog grupe** , a zatim kliknite **Dodaj**

![Dodavanje pozadinske grupe][7]

### <a name="step-2"></a>Korak 2

Unesite podatke za u pozadinskoj skup **pool1**, dodavanje ip adresa ili certifikatom za pozadinskih poslužitelja, a zatim kliknite **u redu**

![pozadinskog skup pool1 postavke][8]

### <a name="step-3"></a>Korak 3

Na plohu pozadinskog grupe kliknite **Dodaj** da biste dodali na dodatne resurse pozadinsku **pool2**, dodavanje ip adresa ili CERTIFIKATOM za pozadinskih poslužitelja, a zatim kliknite **u redu**

![pozadinskog skup pool2 postavke][9]

### <a name="create-listeners-for-each-back-end"></a>Stvaranje slušače za svaki pozadinske

Pristupnik za aplikaciju ovisi o HTTP 1.1 zaglavlja glavno računalo za hostiranje više web-mjesta na istom javnu IP adresu i priključak. Osnovni ga slušatelj stvorena na portalu sadrže to svojstvo.

### <a name="step-1"></a>Korak 1

Kliknite **slušače** na postojeće pristupnika aplikacije, a zatim kliknite **više web-mjesta** da biste dodali prvi ga slušatelj.

![slušače pregled plohu][1]

### <a name="step-2"></a>Korak 2

Ispunite podatke da ga slušatelj, u ovom primjeru SSL prekid konfigurirana, stvorite novi priključak sučelju. Prenesite certifikat .pfx će se koristiti za prekid SSL. Jedina razlika na ovom plohu u usporedbi s plohu standardne osnovni ga slušatelj je glavnog računala.

![plohu ga slušatelj svojstva][2]

### <a name="step-3"></a>Korak 3

Kliknite **više web-mjesta** i stvorite drugi ga slušatelj opisan u prethodnom koraku drugog web-mjesta. Provjerite jeste li koristiti drugi certifikat za drugi ga slušatelj. Jedina razlika na ovom plohu u usporedbi s plohu standardne osnovni ga slušatelj je glavnog računala. Ispunite podatke da ga slušatelj, a zatim kliknite **u redu**.

![plohu ga slušatelj svojstva][3]

> [AZURE.NOTE] Stvaranje slušače Azure portalu za pristupnik za aplikaciju je dugo izvodi zadatak, može potrajati neko vrijeme da biste stvorili dva slušače u ovom scenariju. Kada dovršite Prikaži slušače na portalu kao što se vidi na sljedećoj slici.

![Pregled ga slušatelj][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Stvaranje pravila za mapiranje slušače pozadinskog grupe

### <a name="step-1"></a>Korak 1

Dođite do postojeće aplikacije pristupnika na portalu za Azure (https://portal.azure.com). Odaberite **pravila** , odaberite postojeće pravilo zadane **rule1** i kliknite **Uredi**.

### <a name="step-2"></a>Korak 2

Popunite plohu pravila kao što se vidi na sljedećoj slici. Odabir prve ga slušatelj i prvi skup i kliknete **Spremi** po završetku.

![uređivanje postojeće pravilo][6]

### <a name="step-3"></a>Korak 3

Kliknite **Osnovno je pravilo** da biste stvorili pravilo za 2. Ispunjavanje obrasca s drugom ga slušatelj i drugi skup pozadinskog sustava i kliknite **u redu** da biste spremili.

![Dodavanje plohu Osnovno je pravilo][10]

Time se dovršava konfiguriranje postojeće pristupnik za aplikacije s podrškom za više web-mjesta putem portala za Azure.

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako se zaštititi web-mjesta s [Računala pristupnika - vatrozid aplikaciju za Web](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png