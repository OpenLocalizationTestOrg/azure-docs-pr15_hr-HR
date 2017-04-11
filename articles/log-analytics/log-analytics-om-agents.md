<properties
    pageTitle="Povezivanje Operations Manager prijava analitiku | Microsoft Azure"
    description="Za održavanje sustava postojećih ulaganja u sustavu centar za komponentu Operations Manager proširene mogućnosti za prijava analitiku, Operations Manager možete integrirati s radnim prostorom OMS."
    services="log-analytics"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="magoedte"/>

# <a name="connect-operations-manager-to-log-analytics"></a>Povezivanje Operations Manager zapisnika Analytics

Za održavanje sustava postojećih ulaganja u sustavu centar za komponentu Operations Manager proširene mogućnosti za prijava analitiku, Operations Manager možete integrirati s radnim prostorom OMS.  Time se omogućuje korištenje prilike OMS korištenje Operations Manager da biste:

- Nastavite praćenje stanja servisa IT s Operations Manager
- Održavanje Integracija s vašeg rješenja ITSM incidenta i problemima upravljanje za podršku
- Upravljanje životnim ciklusom agente implementiran na lokalnom i javne oblaka IaaS virtualnim strojevima koji nadzirati s Operations Manager

Integracija sa sustava centar za komponentu Operations Manager dodaje vrijednost strategije operacije servisa po korištenje brzine i učinkovitosti OMS prikupljanje, spremanje i analiza podataka iz komponente Operations Manager.  OMS omogućuje povezivanje i rade prepoznavanja kvarove probleme i pojavljivanje reoccurrences radi ispunjavanja proces za upravljanje postojeće problem.   Fleksibilnost tražilica da biste pregledali performanse, događaja i upozorenja podatke, s obogaćenim nadzornih ploča i mogućnosti izvješćivanja da bi se prikazale te podatke na smislen način pokazuje jačinu OMS unosi u complimenting Operations Manager.

Agenata izvješćivanje o pogreškama u grupi Upravljanje Operations Manager prikupi podatke od poslužitelja na temelju prijava analitiku izvora podataka i rješenja koje ste omogućili u vašoj pretplati OMS.  Ovisno o rješenje koje ste omogućili podataka iz tih rješenja ili šalju izravno iz programa Operations Manager poslužitelju za upravljanje web-uslugu OMS ili zbog količinu podataka prikupljenih u sustavu agent upravlja se šalju izravno agenta OMS web-servisa. Poslužitelj za upravljanje izravno prosljeđuje OMS podataka na web-uslugu OMS, ga nikada ne zapisuje OperationsManager ili OperationsManagerDW bazom podataka.  Kada poslužitelj za upravljanje gubi povezivanja s web-servisa OMS, ona lokalno dok je ponovno uspostaviti s OMS komunikacije predmemorira podatke.  Ako je poslužitelj za upravljanje izvanmrežne zbog planiranog održavanja ili neplanirano prekida, drugi poslužitelj za upravljanje u grupi Upravljanje će nastaviti s povezivanjem s OMS.  

Sljedeći dijagram prikazuje vezu između poslužitelji za upravljanje i agenata u grupu za upravljanje sustava centar za komponentu Operations Manager i OMS, uključujući smjer i priključaka.   

![OMS-operacije-Upravitelj-Integracija-dijagrama](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

## <a name="system-requirements"></a>Sistemski preduvjeti
Prije početka, pregledajte sljedeće da biste provjerili zadovoljava preduvjete.

- OMS podržava samo operacije Upravitelj 2012 SP1 UR6 i veće, te operacije Upravitelj 2012 R2 UR2 i veće.  Podrška za proxy je dodana u operacije Upravitelj 2012 SP1 UR7 i operacije Upravitelj 2012 R2 UR3.
- Svi agenti Operations Manager mora zadovoljavati uvjete Minimalna podrška. Provjerite je li agenata su minimalni ažuriranja, u suprotnom neće uspjeti promet agent za Windows i mnoge pogreške možda ispune u zapisniku događaja Operations Manager.
- Pretplatu na OMS.  Dodatne informacije pročitajte članak [Početak rada s zapisnika analize](log-analytics-get-started.md).

## <a name="connecting-operations-manager-to-oms"></a>Povezivanje Operations Manager OMS
Izvođenje sljedeće niz koraka da biste konfigurirali Operations Manager grupu za upravljanje u da biste se povezali s nekim od OMS radnih prostora.

1. Na konzoli za komponentu Operations Manager odaberite radnog prostora za **administraciju** .
2. Proširite čvor paket za upravljanje operacije, a zatim kliknite **vezu**.
3. Kliknite vezu **registrirati u paketu za upravljanje operacije** .
4. Na na **čarobnjaka za Uhodavanje paket za upravljanje operacija: provjera autentičnosti** stranica, unesite adresu e-pošte ili telefonski broj i lozinku za administratorski račun koji je povezan s pretplatom OMS i kliknite **Prijava**.
5. Nakon što ste uspješno autorizirani, na na **čarobnjaka za Uhodavanje paket za upravljanje operacije: Odaberite radnog prostora** stranica, od vas će se zatražiti da biste odabrali OMS radnog prostora.  Ako imate više od jedne radni prostor, odaberite radnog prostora koji želite registrirati s grupom upravljanje Operations Manager s padajućeg popisa, a zatim kliknite **Dalje**.

    >[AZURE.NOTE] Operations Manager podržava samo jedan radni prostor OMS odjednom. Povezivanje i računala na kojima su registrirane OMS s radnim prostorom za prethodni uklonjeni su iz OMS.

6. Na na **čarobnjaka za Uhodavanje paket za upravljanje operacije: sažetak** stranice, potvrdite postavke i ako su ispravni, kliknite **Stvori**.
7. Na na **čarobnjaka za Uhodavanje paket za upravljanje operacije: Završi** stranice, kliknite **Zatvori**.

### <a name="add-agent-managed-computers"></a>Dodavanje agent upravlja računala
Nakon konfiguriranja Integracija s radnim prostorom OMS, to je samo uspostavlja vezu s OMS, podaci se prikupljaju iz agenata izvješća u upravljanje grupu. To se neće dogoditi do Nakon konfiguriranja određene agent upravlja računalu na kojem prikupili podatke za analizu zapisnika. Možete odabrati računalne objekte pojedinačno ili možete odabrati grupu koja sadrži objekte računala za Windows. Ne možete odabrati grupu koja sadrži pojavljivanja drugi predmete, kao što su logički diskova ili baze podataka SQL.

1. Otvorite konzole za komponentu Operations Manager, a zatim odaberite radnog prostora za **administraciju** .
2. Proširite čvor paket za upravljanje operacije, a zatim kliknite **vezu**.
3. Kliknite vezu **Dodaj računala/grupe** u odjeljku akcije naslov na desnoj strani okna.
4. U dijaloškom okviru za **Pretraživanje na računalu** možete potražiti računala ili grupe nadzire Operations Manager. Odaberite računala ili grupe da biste onboard da biste OMS, kliknite **Dodaj**, a zatim **u redu**.

Možete pogledati računala i grupe konfigurirano tako da biste prikupili podatke iz čvor upravlja računala u odjeljku operacija upravljanja paketa u radnom prostoru **Administracija** konzole za operacije.  Na tom mjestu možete je dodati ili ukloniti računala i grupe prema potrebi.

### <a name="configure-oms-proxy-settings-in-the-operations-console"></a>Konfiguriranje postavki proxyja OMS na konzoli operacije
Ako je poslužitelj za interne proxy između grupa Upravljanje i OMS web-servisa, poduzeti sljedeće korake.  Te postavke središnje se upravlja iz grupe upravljanje i distribuirati agent upravlja sustavima koji su uključeni u opsegu da biste prikupili podatke za OMS.  To je korisno kada određene rješenja zaobići poslužitelju za upravljanje i poslati podatke izravno OMS web-servisa.

1. Otvorite konzole za komponentu Operations Manager, a zatim odaberite radnog prostora za **administraciju** .
2. Proširite paket za upravljanje operacije, a zatim kliknite **veze**.
3. U prikazu OMS veze kliknite **Konfiguriraj Proxy poslužitelj**.
4. Na **čarobnjaka paket za upravljanje operacije: Proxy poslužitelj** stranice, odaberite **koristi proxy poslužitelj za pristup paket za upravljanje operacije**i zatim upišite URL s broj priključka, na primjer, http://corpproxy:80 i zatim kliknite **Završi**.

Ako proxy poslužitelj zahtijeva provjeru autentičnosti, poduzeti sljedeće korake da biste konfigurirali vjerodajnice i postavke koje je potrebno prenijeti na upravljanih računala koja prijavljuje OMS u grupi Upravljanje.

1. Otvorite konzole za komponentu Operations Manager, a zatim odaberite radnog prostora za **administraciju** .
2. U odjeljku **Konfiguriranje RunAs**odaberite **Profili**.
3. Otvorite profila **Sustava centra Savjetnik za pokretanje kao Proxy profila** .
4. Čarobnjaka za pokretanje kao profila, kliknite Dodaj račun Pokreni kao. Možete stvoriti novi [račun za pokretanje kao](https://technet.microsoft.com/library/hh321655.aspx) ili koristi postojeći račun. Ovaj račun mora imati dostatne dozvole za proći kroz proxy poslužitelj.
5. Da biste postavili račun za upravljanje, odaberite **odabrani predmet, grupi ili objekt**, kliknite **Odaberi...** a zatim kliknite **grupa...** Da biste otvorili okvir za **Pretraživanje za grupu** .
6. Traženje, a zatim odaberite **Microsoft sustava centra Savjetnik za nadzor poslužitelja grupe**.  Nakon što odaberete grupe da biste zatvorili okvir za **Pretraživanje za grupu** , kliknite **u redu** .
7.  Kliknite **u redu** da biste zatvorili okvir **Dodavanje računa za pokretanje kao** .
8.  Kliknite **Spremi** da biste dovršili Čarobnjak i spremili promjene.

Nakon stvaranja veze i konfiguriranje koje agenata bit će prikupljanje i izvješća podataka OMS, sljedeću konfiguraciju se primjenjuje u grupi Upravljanje ne nužno redoslijedom:

- Stvorit će se pokrenuti kao račun **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** .  Povezan s profilom Pokreni kao **Microsoft sustava centra Savjetnik za pokretanje kao profila Blob** i određivanje ciljne dva klase - **Poslužitelju zbirke** i **Grupu za upravljanje Upravitelj operacije**.
- Dva poveznika stvaraju.  Prvi pod nazivom **Microsoft.SystemCenter.Advisor.DataConnector** i automatski konfigurirana za pretplatu na koji će se proslijediti sva upozorenja generirao pojavljivanja sve klase u grupi Upravljanje OMS zapisnika analize. Poveznik za drugi je **Savjetnik poveznika**, koji je zadužen za komunikaciju s OMS web-servisa i zajedničkog korištenja podataka.
- Agenata i grupe koje ste odabrali da biste prikupili podatke u grupi Upravljanje dodaje se **Microsoft sustava centra Savjetnik za nadzor poslužitelja grupe**.

## <a name="management-pack-updates"></a>Upravljanje paketa ažuriranja
Po dovršetku konfiguracije grupu upravljanje Operations Manager uspostavlja vezu sa servisom OMS.  Poslužitelj za upravljanje će se sinkronizirati s web-servisa i prima informacije o konfiguraciji ažurirane u obliku paketa za upravljanje za koje ste omogućili rješenja koja se integrirati s Operations Manager.   Operations Manager češće provjerava ima li ažuriranja te upravljanje paketi automatski preuzimati i uvesti ih kada postanu dostupne.  Postoje dva pravila posebno kontrole takvo ponašanje:

- **Microsoft.SystemCenter.Advisor.MPUpdate** - ažurira osnovni OMS paketi za upravljanje. Po zadanom se pokreće svaki 12 (12) sati.
- **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** - ažuriranja rješenja upravljanja paketi omogućen u radnom prostoru. Po zadanom se pokreće svakih pet (5) minuta.

Možete nadjačati ta dva pravila da biste spriječili automatsko preuzimanje onemogućivanjem ih ili izmjena učestalost za koliko često poslužitelju za upravljanje sinkronizira s OMS da biste odredili ako je dostupan novi paket za upravljanje i moguće preuzeti.  Slijedite [upute da biste nadjačali pravila ili Monitor](https://technet.microsoft.com/library/hh212869.aspx) za izmjenu parametar **Učestalost** s vrijednošću u sekundama da biste promijenili raspored sinkronizacije ili izmjena parametar **omogućeno** da biste onemogućili pravila.  Ciljne nadjačavanja svim objektima klase operacije Upravitelj upravljanje grupe.

Ako želite nastaviti sljedeća postojeće koraka kontrola promjena kojima se kontrolira izdanja paket za upravljanje u grupi Upravljanje radnog možete onemogućiti pravila i omogućiti ih tijekom određeno vrijeme kada su dopuštene ažuriranja. Ako imate razvoj ili grupu upravljanje značajke pitanja i odgovora u okruženju sustava i ima povezani s Internetom, možete konfigurirati te upravljanje grupe s OMS radnog prostora da biste podržavaju taj scenarij.  To će vam omogućiti da pregledavaju i procjenu izračun s iteracijama izdanja paketa za upravljanje OMS prije izdavanja u grupi Upravljanje radnog.

## <a name="switch-an-operations-manager-group-to-a-new-oms-workspace"></a>Grupe sustava Operations Manager prijeđite novog OMS radnog prostora
1. Prijavite se u pretplatu OMS i stvaranje novog radnog prostora u [Paketu za upravljanje Microsoft operacije](http://oms.microsoft.com/).
2. Otvorite konzole za komponentu Operations Manager pomoću računa koji je član uloge operacije Upravitelj administratori i odaberite radnog prostora za **administraciju** .
3. Proširite operacije upravljanja paket, pa odaberite **veze**.
4. Odaberite vezu **Ponovno konfiguriranje paket za upravljanje operacija** na sredini strani okna.
5. Pratite upute **Čarobnjaka za Uhodavanje paket za upravljanje operacije** , a zatim unesite adresu e-pošte ili telefonski broj i lozinku za administratorski račun koji je povezan s novog radnog prostora OMS.

    > [AZURE.NOTE] Na **čarobnjaka za Uhodavanje paket za upravljanje operacije: Odaberite radnog prostora** stranice prikazati postojeći radnog prostora koji se koristi.


## <a name="validate-operations-manager-integration-with-oms"></a>Provjerite valjanost operacije Upravitelj Integracija s OMS
Postoji nekoliko različitih načina možete provjeriti je li vaša OMS Operations Manager integraciju sustava uspješno.

### <a name="to-confirm-integration-from-the-oms-portal"></a>Da biste potvrdili Integracija s portala sustava OMS

1.  Na portalu OMS kliknite pločicu **Postavke**
2.  Odaberite **povezani izvora**.
3.  U tablici u odjeljku sustav centar za komponentu Operations Manager, trebali biste vidjeti naziv grupe upravljanje navedeni broj agenata i status zadnje primljena podataka.

    ![OMS postavke connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)

4.  Imajte na umu **Radni prostor ID** vrijednosti u odjeljku lijeve strane stranice s postavkama.  Će ga provjeriti prema dolje Operations Manager upravljanje grupu.  

### <a name="to-confirm-integration-from-the-operations-console"></a>Da biste potvrdili Integracija s konzole operacije

1.  Otvorite konzole za komponentu Operations Manager, a zatim odaberite radnog prostora za **administraciju** .
2.  Odaberite **Upravljanje paketi** i na **potražite:** tekstni okvir upišite **Advisor** ili **Obavještavanje**.
3.  Ovisno o rješenja koje ste omogućili, vidjet ćete odgovarajuće paket za upravljanje naveden u rezultatima pretraživanja.  Na primjer, ako ste omogućili rješenja upozorenja upravljanja, paket za upravljanje upravljanje upozorenja sustavom centra Savjetnik za Microsoft bit će na popisu.
4.  U prikazu za **nadzor** otvorite Prikaz **Postupaka upravljanja Suite\Health stanje** .  Odaberite poslužitelj za upravljanje u oknu **Stanje poslužitelja za upravljanje** i u oknu **Prikaza detalja** potvrdite vrijednosti za svojstvo **URI za servis za provjeru autentičnosti** odgovara ID OMS radnog prostora

    ![OMS-opsmgr-MG-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)


## <a name="remove-integration-with-oms"></a>Uklanjanje Integracija s OMS
Kada više ne zahtijeva Integracija između grupa Upravljanje Operations Manager i OMS radnog prostora, postoji nekoliko koraka koji su potrebni za ispravno uklonili vezu i konfiguraciji u grupi Upravljanje. Sljedeći postupak imat će vam ažuriranje radnog prostora OMS tako da izbrišete referencu grupe za upravljanje, izbrišite OMS poveznika, a zatim izbrišite podrške OMS paketi za upravljanje.   

1.  Otvorite naredbe ljuske Upravitelj operacije pomoću računa koji je član uloge operacije Upravitelj administratori.

    >[AZURE.WARNING] Provjerite je li nemate sve prilagođene upravljanje pakete pomoću riječi Advisor ili IntelligencePack u nazivu prije nastavka, a u suprotnom sljedeće će ih izbrisati iz grupe upravljanje.

2.  Iz naredbenog retka ljuske upišite`Get-SCOMManagementPack -name "*advisor*" | Remove-SCOMManagementPack`

3.  Sljedeće vrste`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack`

4.  Otvorite konzolu operacije Upravitelj operacije pomoću računa koji je član uloge operacije Upravitelj administratori.
5.  U odjeljku **Administracija**odaberite čvor **Paketi za upravljanje** i na **potražite:** okvir upišite **Savjetnik** i provjerite je li sljedećim paketima za upravljanje se i dalje uvoze u grupi Upravljanje:

    - Centar za sustav Microsoft Savjetnik
    - Centar za sustav Microsoft Savjetnik za interne

6. Na portalu OMS kliknite pločicu **Postavke** .
7.  Odaberite **povezani izvora**.
8.  U tablici u odjeljku sustav centar za komponentu Operations Manager, trebali biste vidjeti naziv grupe upravljanje koji želite ukloniti iz radnog prostora.  U odjeljku stupac **Zadnje podataka**, kliknite **Ukloni**.  

    >[AZURE.NOTE] Ako postoji bez aktivnosti otkriven iz grupe povezanih upravljanje **Ukloni** vezu neće biti dostupnih tek nakon 14 dana.  
   
9.  Pojavit će se prozor s pitanjem želite da biste potvrdili da želite nastaviti s uklanjanjem.  Kliknite **da** da biste nastavili. 

Da biste izbrisali dva poveznika - Microsoft.SystemCenter.Advisor.DataConnector poveznik Savjetnik za spremanje skriptu PowerShell ispod na računalo i izvršiti pomoću sljedećih primjera.

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

>[AZURE.NOTE] Računalo pokrenete ovu skriptu iz, a ako ne na poslužitelju za upravljanje moraju imati operacije Upravitelj 2012 SP1 ili R2 naredbe ljuske instaliran ovisno o verziji grupe za upravljanje.

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

U budućnosti namjeravate na ponovno vašoj grupi Upravljanje s radnim prostorom OMS, morat ćete ponovno uvezli u `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` upravljanja datoteku paketa iz najnovije Kumulativno ažuriranje primjenjuje se na vašoj grupi Upravljanje.  Možete pronaći datoteke u na `%ProgramFiles%\Microsoft System Center 2012` ili `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` mapu.

## <a name="next-steps"></a>Daljnji koraci

- [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md) za dodavanje funkcionalnosti i njihovo prikupljanje podataka.
- [Konfiguracija postavki proxy poslužitelja i vatrozida u zapisnik analize](log-analytics-proxy-firewall.md) ako vaša tvrtka koristi proxy poslužitelj ili vatrozid tako da se agenata mogli komunicirati sa servisom zapisnika analize.
