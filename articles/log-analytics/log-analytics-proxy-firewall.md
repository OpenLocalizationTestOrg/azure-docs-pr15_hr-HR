<properties
    pageTitle="Konfiguriranje postavki proxy poslužitelja i vatrozida u prijava analitiku | Microsoft Azure"
    description="Konfiguriranje postavki proxy poslužitelja i vatrozida kada agenata ili usluge OMS morate koristiti određene priključke."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>

# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>Konfiguriranje postavke proxy poslužitelja i vatrozida u zapisnik Analytics

Akcije potrebne za konfiguriranje proxy i postavke vatrozida za analizu zapisnika u OMS razlikuju kada koristite Operations Manager i njegovih agenata i Microsoftove agente u nadzorni u koji se povezuje izravno s poslužiteljima. Pregledajte u sljedećim se odjeljcima za vrstu koji koristite.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>Konfiguriranje postavki proxy poslužitelja i vatrozida uz Microsoft Agent za nadzor

Za Microsoft nadzor agenta za povezivanje i registrirati sa servisom OMS, mora imati pristup broj priključka domene i URL-ove. Ako koristite proxy poslužitelj za komunikaciju između agenta i OMS usluga, morate da bi bili odgovarajuće resurse pristupačne. Ako koristite vatrozid da biste ograničili pristup Internetu, morate konfigurirati vatrozida za OMS dopustiti pristup. U sljedećim tablicama navedeni priključci koju je potrebno OMS.

|**Agent resursa**|**Priključci**|**Zaobilaženje HTTPS provjere**|
|--------------|-----|--------------|
|\*. ods.opinsights.azure.com|443|Da|
|\*. oms.opinsights.azure.com|443|Da|
|\*. blob.core.windows.net|443|Da|
|ods.systemcenteradvisor.com|443| |

Koristite ovaj postupak da biste konfigurirali postavke proxy poslužitelja za Microsoft nadzor agenta pomoću upravljačke ploče. Morat ćete koristiti postupak za svaki poslužitelj. Ako imate mnogo poslužitelja koje su vam potrebne da biste konfigurirali, možda je lakše koristiti skriptu da biste automatizirali postupak. Ako je tako, pogledajte sljedeći postupak [Da biste konfigurirali postavke proxy poslužitelja za Microsoft nadzor agenta pomoću skripte](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>Da biste konfigurirali postavke proxy poslužitelja za Microsoft nadzor agenta pomoću upravljačke ploče

1. Otvorite **upravljačku ploču**.

2. Otvorite **Microsoft Agent za nadzor**.

3. Kliknite karticu **Postavke proxyja** .<br>  
  ![kartica postavke proxy poslužitelja](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Odaberite **koristi proxy poslužitelj** , upišite URL i priključka broj, ako je potrebno, slično primjeru. Proxy poslužitelj zahtijeva provjeru autentičnosti, upišite korisničko ime i lozinku da biste pristupili proxy poslužitelj.

Koristite sljedeći postupak da biste stvorili skriptu PowerShell koje možete pokrenuti da biste postavili postavke proxy poslužitelja za svaki pojedini agent koja se povezuje izravno s poslužitelja.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>Da biste konfigurirali postavke proxy poslužitelja za Microsoft nadzor agenta pomoću skripte

Kopirajte sljedeće ogledne, ažurirati informacije specifične za vaše okruženje, spremite s datotečnim nastavkom PS1 i pokrenuti skriptu na svim računalima na kojima se povezuje izravno OMS usluga.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>Konfiguriranje postavki proxy poslužitelja i vatrozida uz Operations Manager

U grupi Upravljanje Operations Manager za povezivanje i registrirati sa servisom OMS ga mora imati pristup brojevi priključka domena i URL-ova. Ako koristite proxy poslužitelj za komunikaciju između poslužitelju za upravljanje Operations Manager i OMS usluga, morate da bi bili odgovarajuće resursi dostupni. Ako koristite vatrozid da biste ograničili pristup Internetu, morate konfigurirati vatrozida za OMS dopustiti pristup. Čak i ako na poslužitelju za upravljanje Operations Manager nije iza proxy poslužitelja, možda će biti njegovih agenata. U ovom slučaju proxy poslužitelj trebali biste ga konfigurirati na isti način kao agenata su da biste mogli omogućiti i Dopusti sigurnost i upravljanje zapisnikom rješenje podataka se šalje na OMS web-servisa.

U redoslijedu za komponentu Operations Manager možete komunicirati s OMS usluga (uključujući agenata) infrastruktura za komponentu Operations Manager moraju imati postavke proxyja ispravne i verziju. Postavka za agente proxyja naveden je u konzole za komponentu Operations Manager. Vaša verzija mora biti nešto od sljedećeg:

- Operacije Upravitelj 2012 SP1 zbirna 7 ili noviji
- Operacije Upravitelj 2012 R2 zbirna 3 ili noviji


U sljedećim tablicama navedeni priključci vezane uz sljedeće zadatke.

>[AZURE.NOTE] Neki od navedenih resursa spomenuti Savjetnik i radu uvide, oba su prethodnih verzija OMS. Međutim, navedenih resursa promijenit će se u budućnosti.

Slijedi popis resursa agent i priključke:<br>

|**Agent resursa**|**Priključci**|
|--------------|-----|
|\*. ods.opinsights.azure.com|443|
|\*. oms.opinsights.azure.com|443|
|\*.blob.Core.Windows.NET/\*|443|
|ods.systemcenteradvisor.com|443|
<br>
Slijedi popis resurse poslužitelja za upravljanje i priključke:<br>

|**Resurs za upravljanje poslužitelja**|**Priključci**|**Zaobilaženje HTTPS provjere**|
|--------------|-----|--------------|
|Service.systemcenteradvisor.com|443| |
|\*. service.opinsights.azure.com|443| |
|\*. blob.core.windows.net|443|Da| 
|Data.systemcenteradvisor.com|443| | 
|ods.systemcenteradvisor.com|443| | 
|\*. ods.opinsights.azure.com|443|Da| 
<br>
Slijedi popis OMS i Operations Manager konzole za resurse i priključaka.<br>

|**OMS i Operations Manager konzole resursa**|**Priključci**|
|----|----|
|Service.systemcenteradvisor.com|443|
|\*. service.opinsights.azure.com|443|
|\*. live.com|Priključak 80 i 443|
|\*. microsoft.com|Priključak 80 i 443|
|\*. microsoftonline.com|Priključak 80 i 443|
|\*. mms.microsoft.com|Priključak 80 i 443|
|login.Windows.NET|Priključak 80 i 443|
<br>

Da biste registrirali vašoj grupi Upravljanje Operations Manager sa servisom OMS pomoću sljedećih postupaka. Ako imate poteškoća s komunikacije između grupa Upravljanje i OMS usluga, otklanjanje poteškoća s prijenos podataka sa servisom OMS pomoću postupke za provjeru valjanosti.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Da biste zatražili iznimke za krajnje točke servisa OMS

1. Pomoću podataka iz prve tablice prikazuju prethodno da bi bili resursa koja su potrebna za poslužitelj za upravljanje Operations Manager dostupan je kroz sve vatrozidima možda imate.
2. Pomoću podataka iz druge tablice prikazuju prethodno da bi bili s resursima koji su potrebni za konzolu operacije u Operations Manager i OMS pristupiti kroz sve vatrozidima možda imate.
3. Ako koristite proxy poslužitelj s preglednikom Internet Explorer, provjerite je konfiguriran i funkcionira ispravno. Da biste provjerili, možete otvoriti na sigurne internetske veze (HTTPS), primjerice [https://bing.com](https://bing.com). Ako na sigurne internetske veze ne funkcionira u pregledniku, postupak vjerojatno neće funkcionirati u konzoli za upravljanje Operations Manager s web-servisi u oblaku.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>Da biste konfigurirali proxy poslužitelj na konzoli za komponentu Operations Manager

1. Otvorite konzole za komponentu Operations Manager, a zatim odaberite radnog prostora za **administraciju** .

2. Proširite **Radu uvide**, a zatim **Radu uvida veze**.<br>  
    ![Operacije OMS Upravitelja veze](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. U prikazu OMS veze kliknite **Konfiguriraj Proxy poslužitelj**.<br>  
    ![Operacije Upravitelj OMS vezu konfiguriranje Proxy poslužitelja](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. U radu sa servisom uvida čarobnjaka postavki: Proxy poslužitelj, odaberite **koristi proxy poslužitelj za pristup radu uvida web-servisa**, a zatim upišite URL s priključak broj, na primjer, **http://myproxy:80**.<br>  
    ![Operacije Upravitelj OMS proxy adrese](./media/log-analytics-proxy-firewall/proxy-om03.png)


### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Da biste odredili vjerodajnice ako proxy poslužitelj zahtijeva provjeru autentičnosti
 Postavke i vjerodajnice za proxy poslužitelj morati prenijeti na upravljanih računala koja će dojavite OMS. Ti poslužitelji mora biti u *Microsoft sustava centra Savjetnik za nadzor poslužitelja grupe*. Vjerodajnice šifrirane u registru svakom poslužitelju u grupi.

1. Otvorite konzole za komponentu Operations Manager, a zatim odaberite **Administracija** radnog prostora.
2. U odjeljku **Konfiguriranje RunAs**odaberite **Profili**.
3. Otvorite profila **Sustava centra Savjetnik za pokretanje kao Proxy profila** .  
    ![slike profila sustava centra Savjetnik za pokretanje kao proxy poslužitelja](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. Čarobnjaka za pokretanje kao profila, kliknite **Dodaj** račun Pokreni kao. Možete stvoriti novi račun Pokreni kao ili koristi postojeći račun. Ovaj račun mora imati dostatne dozvole za proći kroz proxy poslužitelj.  
    ![Slika čarobnjaka za pokretanje kao profila](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. Da biste postavili račun za upravljanje, odaberite **odabrani predmet, grupi ili objekta** da biste otvorili okvir za pretraživanje objekta.  
    ![Slika čarobnjaka za pokretanje kao profila](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Traženje, a zatim odaberite **Microsoft sustava centra Savjetnik za nadzor poslužitelja grupe**.  
    ![Slika okvira za pretraživanje objekta](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Kliknite **u redu** da biste zatvorili okvir dodajte Pokreni kao račun.  
    ![Slika čarobnjaka za pokretanje kao profila](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. Dovršite čarobnjak i spremite željene promjene.  
    ![Slika čarobnjaka za pokretanje kao profila](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Da biste provjerili valjanost te upravljanje OMS paketi preuzimaju

Ako ste dodali rješenja OMS, možete ih pregledavati na konzoli za komponentu Operations Manager kao paketi za upravljanje u odjeljku **Administracija**. Traženje *Savjetnik za centar sustava* da biste brzo pronašli.  
    ![Upravljanje paketi preuzeti](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) ili paketi za upravljanje OMS možete potražiti pomoću sljedeću naredbu komponente Windows PowerShell u poslužitelju za upravljanje Operations Manager:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Da biste provjerili valjanost tog Operations Manager šalje podataka OMS usluga

1. Na poslužitelju za upravljanje Operations Manager otvorite nadzor performansi (perfmon.exe) pa odaberite **Praćenje performansi**.
2. Kliknite **Dodaj**, a zatim odaberite **Upravljanje grupe za stanje servisa**.
3. Dodajte mjerača koji počinju **HTTP-a**.  
    ![Dodavanje mjerača](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Ako je konfiguraciju Operations Manager dobro, vidjet ćete aktivnosti za upravljanje stanje servisa mjerača za događaja i druge stavke podataka, na temelju paketi za upravljanje koju ste dodali u OMS i zbirku pravila konfigurirani zapisnika.  
    ![Performanse Monitor prikazuje aktivnosti](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## <a name="next-steps"></a>Daljnji koraci

- [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md) za dodavanje funkcionalnosti i njihovo prikupljanje podataka.
- Upoznavanje s [zapisnika pretraživanja](log-analytics-log-searches.md) da biste pogledali detaljne podatke prikupljene putem rješenja.
