<properties
    pageTitle="Tomcat na virtualnog računala | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča koristi resursa koje su stvorene pomoću model klasični implementacije i prikazuje kako stvoriti Windows virtualnog računala i konfigurirajte ga tako da biste pokrenuli Apache Tomcat aplikacijski poslužitelj."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Pokretanje aplikacije poslužitelju Java na virtualnog računala stvorene pomoću model klasični implementacije

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


S Azure, možete koristiti virtualnog računala omogućuju mogućnosti poslužitelja. Na primjer, virtualnog računala koja se izvodi na Azure moguće je konfigurirati za hostiranje Java aplikacijski poslužitelj, kao što su Apache Tomcat. Kada dovršite ovaj vodič, imat ćete poznavati kako stvoriti virtualnog računala koja se izvodi na Azure i konfigurirajte ga tako da biste pokrenuli Java aplikacijski poslužitelj.

Saznat ćete:

* Kako stvoriti virtualnog računala koja ima u Java Development Kit (JDK) već instaliran.
* Upute za daljinsko prijavite se u virtualnog računala.
* Kako instalirati aplikaciju poslužitelju Java na virtualnog računala.
* Upute za stvaranje krajnje točke za virtualnog računala.
* Kako otvoriti priključak u vatrozidu za poslužitelj aplikacije.

Za potrebe ovog praktičnog vodiča, poslužitelj aplikacije Apache Tomcat će biti instalirana na virtualnog računala. Instalacija Tomcat kao što su sljedeće rezultirat će dovršene instalacije.

![Virtualnog računala radi Apache Tomcat][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Da biste stvorili virtualnog računala

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2. Kliknite **Novo**, kliknite **izračunati**, kliknite **virtualnog računala**, a zatim **Iz galerije**.
3. U dijaloškom okviru **Odabir slike virtualnog računala** odaberite **JDK 7 Windows Server 2012**.
Imajte na umu da je dostupna ako imate stari aplikacije koje nisu spremni pokrenuti u JDK 7 **JDK 6 Windows Server 2012** .
4. Kliknite **Dalje**.
5. U dijaloškom okviru **Konfiguriranje virtualnog računala** :
    1. Navedite naziv virtualnog računala.
    2. Odredite veličinu da biste koristili virtualnog računala.
    3. U polje **Korisničko ime** unesite naziv za administratora. Imajte na umu ovo ime i lozinku ćete unijeti sljedeće, će ih koristite kada se daljinsko prijaviti virtualnog računala.
    4. Unesite lozinku u polju **novu lozinku** , a zatim ponovno unesite u polje **Potvrdi** . Ovo je administratorsku lozinku za račun.
    5. Kliknite **Dalje**.
6. U sljedećem **Konfiguracija virtualnog računala** dijaloškom okviru:
    1. Za **servis u Oblaku**, koristite zadani **Stvori novi servis u oblaku**.
    2. Vrijednost za **naziv DNS servis oblaka** mora biti jedinstvena preko cloudapp.net. Ako je potrebno, tu vrijednost promijeniti tako da se taj Azure označava jedinstveni.
    2. Navedite regija, afinitet grupe ili virtualne mreže. Za potrebe ovog praktičnog vodiča, navedite regiju kao što su **Zapad SAD -a**.
    2. **Račun za pohranu**, odaberite **Koristi račun automatski generirani prostora za pohranu**.
    3. **Postavljanje dostupnosti**, odaberite **(ništa)**.
    4. Kliknite **Dalje**.
7. U konačnih dijaloški okvir **Konfiguracija virtualnog računala** :
    1. Prihvatite zadanih krajnje točke unosa.
    2. Kliknite **dovrši**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Da biste daljinski prijavite se u virtualnog računala

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2. Kliknite **virtualnih računala**.
3. Kliknite naziv virtualnog računala koje želite da se prijavite.
4. Nakon virtualnog računala, u skočnom izborniku pri dnu stranice omogućuje veze.
5. Kliknite **Poveži**.
6. Odgovaranje na upite po potrebi možete povezati virtualnog računala. To treba entail spremanje i otvaranje .rdp datoteku koja sadrži pojedinosti veze. Možda ćete morati kopirati url: priključak kao u posljednjem dijelu prvi redak .rdp datoteku i zalijepiti ga u aplikaciji udaljene prijavu.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Da biste instalirali Java aplikacijskog poslužitelja na virtualnog računala

Možete kopirati Java aplikacijski poslužitelj virtualnog računala ili instalirate Java aplikacijski poslužitelj putem instalacijski program.

Za potrebe ovog praktičnog vodiča, instalirat će se Tomcat.

1. Kada ste prijavljeni na virtualnog računala, otvorite sesiju u pregledniku da biste [Apache Tomcat](http://tomcat.apache.org/download-70.cgi).
2. Dvokliknite vezu za **32-bitne i 64-bitni Windows Installer za servis**. Pomoću ove tehnike Tomcat instalirat će se kao usluge sustava Windows.
3. Kada se to od vas zatraži, odaberite da biste pokrenuli instalacijski program.
4. U čarobnjaku za **Postavljanje Tomcat Apache** , slijedite upute za instalaciju Tomcat. Za potrebe ovog praktičnog vodiča, prihvatite zadane postavke je u redu. Kada dođete do dijaloški okvir **dovršavanje čarobnjaka za postavljanje Tomcat Apache** , po želji možete provjeriti **Pokrenuti Apache Tomcat** da bi Tomcat Pokreni odmah. Kliknite **Završi** da biste dovršili postupak postavljanja Tomcat.

## <a name="to-start-tomcat"></a>Da biste pokrenuli Tomcat
Ako niste odabrali da biste pokrenuli Tomcat u dijaloškom okviru **dovršavanje čarobnjaka za postavljanje Tomcat Apache** , pokrenite ga tako da otvorite naredbeni redak na virtualnog računala i pokretanje **net start Tomcat7**.

Prikazat će se sada Tomcat izvodi ako pokrenete virtualnog računala preglednika, a zatim otvorite <http://localhost:8080>.

Da biste vidjeli Tomcat pokrenuti s vanjskim računalima, morate stvaranje krajnje i otvorite priključak.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Da biste stvorili krajnje točke za virtualnog računala
1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com).
2. Kliknite **virtualnih računala**.
3. Kliknite naziv virtualnog računala sa sustavom poslužitelj aplikacije Java.
4. Kliknite **krajnje točke**.
5. Kliknite **Dodaj**.
6. U dijaloškom okviru **Dodavanje krajnje** provjerite **Dodaj samostalni krajnje** je odabrana, a zatim kliknite **Dalje**.
7. U dijaloškom okviru **novim detaljima o krajnjoj točki** :
    1. Navedite naziv krajnje točke; na primjer, **HttpIn**.
    2. Navedite **TCP** protokol.
    3. Navedite **80** javno priključka.
    4. Navedite **8080** privatne priključka.
    5. Kliknite gumb **dovrši** da biste zatvorili dijaloški okvir. Stvorit će se sada na krajnjoj točki.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Da biste otvorili priključak u vatrozidu za virtualnog računala
1. Prijavite se na virtualnog računala.
2. Kliknite **Start sustava Windows**.
3. Kliknite **Upravljačka ploča**.
4. Kliknite **sustav i sigurnost**, kliknite **Vatrozid za Windows**, a zatim **Napredne postavke**.
5. Kliknite **Ulazna pravila**, a zatim kliknite **Novo pravilo**.
 ![Novo pravilo ulazne][NewIBRule]
6. **Vrste pravila**odaberite **priključak**i zatim kliknite **Dalje**.
 ![Novi priključak ulazna pravila][NewRulePort]
7. Na zaslonu **protokol i priključaka** odaberite **TCP**, odredite **8080** kao **lokalne priključka**i zatim kliknite **Dalje**.
 ![Novo pravilo ulazne][NewRuleProtocol]
8. Na zaslonu **Akcije** odaberite **Dopusti veze**, a zatim kliknite **Dalje**.
 ![Nove akcije ulazna pravila][NewRuleAction]
9. Na zaslonu **profila** bili sigurni da **domene**, **privatni**i **javni** nije odabrana, a zatim kliknite **Dalje**.
 ![Novi profil ulazna pravila][NewRuleProfile]
10. Na zaslonu **naziv** unesite naziv pravila, kao što su **HttpIn** (naziv pravila nije obavezno no odgovara nazivu krajnje točke), a zatim kliknite **Završi**.  
 ![Novi naziv ulazna pravila][NewRuleName]

Sada Tomcat web-mjesta mora biti vidljivi iz vanjske preglednika pomoću URL-a obrasca * *http://*vaše\_DNS\_naziv*. cloudapp.net**pri čemu ** *vaše\_DNS\_naziv*** DNS ime koje ste naveli prilikom stvaranja virtualnog računala.

## <a name="application-lifecycle-considerations"></a>Razmatranja životnog ciklusa aplikacije
* Nije moguće stvoriti vlastiti web-aplikacije arhiva (WAR) i dodajte ga u mapu **webapps** . Na primjer, stvaranje osnovnog projekta dinamički web Java servisa stranice (JSP) i izvesti kao datoteku WAR, kopirajte u WAR mapu **webapps** Apache Tomcat na virtualnog računala, pa je pokrenite u pregledniku.
* Prema zadanim postavkama nakon instalacije servis Tomcat je postavljen za ručno pokretanje. Možete se prebacivati ga na automatsko pokretanje pomoću dodatka za servise. Pokrenite dodatak Servisi tako da kliknete **Start sustava Windows**, **Administrativni alati**, a zatim **servise**. Dvaput kliknite servis **Apache Tomcat** i postavljanje **Vrsta pokretanja** na **Automatsko**.

    ![Postavljanje servisa za automatsko pokretanje][service_automatic_startup]

    Prednost automatski Pokreni Tomcat je da će početi ako ponovo virtualnog računala ne pokrene (na primjer, nakon instalacije ažuriranja softver koji zahtijeva ponovno pokretanje).

## <a name="next-steps"></a>Daljnji koraci
Saznajte više o ostalim servisima (kao što su Azure prostora za pohranu, bus servisa i baze podataka SQL) koje ćete htjeti uključiti s aplikacijama sustava Java pregledom informacije koje su dostupne u [Centru za razvojne inženjere Java](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png
