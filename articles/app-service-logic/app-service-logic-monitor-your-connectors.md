<properties
    pageTitle="Upravljanje i nadzirati poveznika i API aplikacije u aplikacije servisa za | Microsoft Azure"
    description="Performanse poveznika i API aplikacije u aplikacijama logike; microservices arhitekture"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>Upravljanje i nadzirati ugrađene aplikacije za API-JA i poveznika

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju.

Stvara ugrađene aplikacije za API-JA. Što sad?

U Azure, svaku aplikaciju API je zasebnom web-mjesto hostirano na Azure. Zbog toga jednostavno vidjet ćete zahtjeve za koliko vrše i potražite u članku kojom količinom podataka koristi poveznik. Sigurnosna kopija aplikacije API-JA, stvaranje upozorenja, omogućite Tinfoil zaštitu, i dodavanje korisnika i uloga.

U ovoj se temi opisuju neke od različite mogućnosti da biste upravljali aplikacije API-JA.

Da biste vidjeli ove ugrađenih značajki, otvorite aplikaciju API [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Ako aplikaciju API nalazi na vašem startboard, odaberite da biste otvorili svojstva. Možete odabrati i **Pregled**odaberite **API aplikacije**, a zatim API aplikacije:

![][browse]

## <a name="see-the-properties-you-entered"></a>Prikaz svojstava koje ste unijeli

Kada otvorite aplikaciju API-JA, postoji nekoliko značajki i zadaci dostupne su:

![][settings]

možeš:

- **Postavke** prikazuje konkretne informacije o aplikaciji API Detalji o pretplati, uključujući popise i korisnicima koji imaju pristup u aplikaciju za API-JA. Možete povećati ili smanjiti broj pojavljivanja API aplikacije pomoću značajke skaliranje; među druge značajke.
- Pomoću gumba **Start** i **Stop** kontrola App API-JA.
- Kada se ažuriranja proizvoda u podlozi datoteke koristi API aplikacije, možete kliknuti **Ažuriranje** da biste dobili najnoviju verziju. Ako, na primjer, ako postoji popravak ili sigurnosnog ažuriranja objavio Microsoft, automatski kliknete **Update** ažurira API aplikacije da biste uvrstili taj se popravak.
- Odaberite **Promijeni planiranje** nadogradnje ili nadogradnju na stariju verziju na temelju podatkovni promet API aplikacije. Tu značajku možete koristiti i da biste vidjeli podatkovni promet.
- Prilikom stvaranja Konektor koji se sastoji od tablica, kao što je poveznik za SQL po želji možete unijeti naziv tablice možete povezati. Shema temeljen na tablici je automatski stvorene i dostupne kada kliknete **Preuzimanje sheme**. Da biste stvorili pretvaranjem ili karte možete koristiti ovu preuzeti shemu.

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>Promjena poveznika ili API konfiguracije vrijednostima koje ste unijeli

Kada je konfiguriran ili stvorili vaše ugrađeni-connector, možete promijeniti vrijednostima koje ste unijeli. Ako, na primjer, ako ste konfigurirali poveznik za SQL i želite promijeniti naziv SQL Server ili naziv tablice, možete to u plohu API aplikacije za vaše poveznik.

Koraci obuhvaćaju sljedeće:

1. Otvorite poveznika ili aplikacije API-JA. Kada to učinite, otvorit će se plohu API aplikacije.
2. U **Essentials**, kliknite hipervezu u odjeljku Svojstva glavnog računala. Hiperveza je s nazivom kao što su *slackconnector* ili *microsoftsqlconnector123*:

    ![][apiapphost]

3. U plohu Host API aplikaciju odaberite **Postavke**. U plohu postavke odaberite **Postavke aplikacije**. Konfiguracijskih vrijednosti su navedene u odjeljku **Postavke aplikacije**:

    ![][hostsettings]

4. Kliknite željenu postavku da biste promijenili, unesite novu vrijednost i **spremite** promjene.


## <a name="install-the-hybrid-connection-manager---optional"></a>Instalacija Manager veze hibridnog - neobavezno

![][hcsetup]

Upravitelj veze hibridnog nudi mogućnost povezivanja s lokalnog sustava, kao što je SQL Server ili SAP-a. Povezivanje s ovom hibridnog koristi Bus servisa Azure da biste se povezali i da biste odredili sigurnost između Azure resurse i lokalnog resursa.

Pogledajte odjeljak [Korištenje hibridnog Upravitelja veze u servisu Azure aplikacije](app-service-logic-hybrid-connection-manager.md).

> [AZURE.NOTE] Upravitelj veze hibridnog potreban je samo ako se povezujete s programa lokalnog resursa iza vatrozida. Ako se ne povezujete s lokalnim sustavom, Upravitelja veze hibridnog možda se neće prikazati u vašem plohu poveznik.

## <a name="monitor-the-performance"></a>Praćenje performansi
Performanse metriku ugrađene su značajke i uključiti uz svaku aplikaciju API stvorite. Ove metriku specifične za API aplikacije smješten u Azure. Ogledna metriku:

![][monitoring]

možeš:

- Odaberite **zahtjeve i pogrešaka** da biste dodali drugi performanse metriku uključujući najčešće – poznati kodovi pogreške o HTTP, kao što su 200, 400 ili 500 HTTP kodovima stanja. Možete i potražite u članku odgovor puta, potražite u članku koliko zahtjeve vrše aplikaciju API-JA i potražite u članku kojom količinom podataka dolazi i koliko je podataka dolazi. Na temelju metrike performanse, možete stvoriti upozorenja e-pošte ako metrike premašuje prag vašem odabiru.
- U odjeljku **Korištenje**, vidjet ćete **procesora** koristi aplikaciju API, pregledajte trenutni **Kvota korištenja** MB i potražite u članku vašeg maksimalan podatkovni promet na temelju vašeg sloju trošak. **Procijenjena provedu** pojednostavljuju određivanje potencijalnih troškovi pokretanja aplikacije API-JA.
- Odaberite **procesa** da otvorite Eksplorer za proces. Time se prikazuje na instance web i njihovim svojstvima, uključujući korištenje niti count i memorije.

Pomoću ovih alata, možete odrediti ako planiranje aplikacije servisa moraju biti skalirana prema gore ili skalirana prema dolje, ovisno o poslovnim potrebama. Te su značajke ugrađene portalu s bez dodatne alate potrebne.

## <a name="control-the-security"></a>Kontrola sigurnost

API aplikacije pomoću na temelju uloga sigurnost. Te uloge primjenjuju se na cijelu Azure iskustva, uključujući API aplikacije i ostale resurse Azure. Uloge obuhvaćaju sljedeće:

Uloga | Opis
--- | ---
Vlasnik | Imali puni pristup sučelje za upravljanje, a možete dati pristup drugim korisnicima ili grupama.
Suradnik | Imali puni pristup sučelje za upravljanje. Ne možete dati pristup drugim korisnicima ili grupama.
Čitač | Možete pogledati sve resurse osim tajne.
Korisnički pristup administratora | Možete pogledati sve resursa, stvaranje i Upravljanje ulogama i stvaranje i upravljanje podržava karticama.

Potražite u članku [Upravljanje pristupom utemeljeno na ulogama na portalu Microsoft Azure](../active-directory/role-based-access-control-configure.md).

Možete jednostavno dodajte korisnike i dodjela određene uloga u aplikaciju za API-JA. Na portalu prikazuje korisnici koji imaju pristup i njihovih dodijeljena uloga:

![][access]  

- Odabir **korisnika** za dodavanje korisnika, Dodjela uloge i uklanjanje korisnika.
- Odaberite **uloge** da biste vidjeli sve korisnike u ulozi određene Dodavanje korisnika u grupu uloga i uklanjanje korisnika iz uloge.


## <a name="more-good-stuff"></a>Dodatni sadržaji dobro
- Odaberite **API definicija** da biste otvorili datoteku Swagger automatski stvara za određenu aplikaciju API-JA.
- Odaberite **ovisnosti** da biste prikazali datoteke potrebnih aplikacije API-JA. Ako, na primjer, ako koristite poveznik za SAP, instalirate neke dodatne datoteke na s lokalnog hibridnog Upravitelja veze. Te ovisnosti prikazuju se u svoje plohu aplikacije API-JA.

>[AZURE.IMPORTANT] Kada otvorite aplikaciju svojstva API-JA, a u odjeljku **Osnove**, postoje **glavno računalo** i **pristupnik** veze koje otvorite novi blades:
>
> ![][host]
>
>Na web-mjesto na kojem je smještena aplikacije API vrijede tih svojstava. Kada koristite ugrađeni API aplikacije ili poveznika, većinu tih svojstava ne zaista primjenjuju se i preporučujemo da ne ažurirate tih svojstava. Ako ste stvorili API aplikacije u Visual Studio, i implementiran u pretplatu za Azure, možete koristiti blades glavno računalo i pristupnik. <br/><br/>


>[AZURE.NOTE] Da biste započeli s aplikacijama logika prije registracije za račun za Azure, idite na [Pokušajte logika aplikacije](https://tryappservice.azure.com/?appservice=logic). Možete stvoriti aplikaciju za logika short-lived starter. Nema kreditne kartice potrebna i ne p.

## <a name="read-more"></a>Saznajte više

[Praćenje aplikacija u logike](app-service-logic-monitor-your-logic-apps.md)<br/>
[Poveznici i popisa aplikacija API-JA u aplikaciji servisa](app-service-logic-connectors-list.md)<br/>
[Kontrola pristupa na temelju uloga na portalu Microsoft Azure](../active-directory/role-based-access-control-configure.md)<br/>
[Korištenje upravitelja veze hibridnog u aplikacije servisa za Azure](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
