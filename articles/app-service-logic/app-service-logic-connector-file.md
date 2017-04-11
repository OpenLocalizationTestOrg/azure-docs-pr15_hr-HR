<properties
    pageTitle="Pomoću poveznika datoteka u aplikacijama logike | Aplikacije servisa za Microsoft Azure"
    description="Kako stvoriti i konfigurirati poveznik za datoteku ili API aplikacije i koristiti u aplikaciji logike u aplikacije servisa za Azure"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Početak rada s poveznik za datoteke i njezino dodavanje logike aplikacije
>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju.

Povezivanje sustava datoteke za prijenos, preuzmite i još mnogo datotekama na glavno računalo. Možete pokrenuti logike aplikacije koji se temelji na raznim izvorima podataka i ponuda poveznici za dohvat i obrada podataka. Poveznik za datoteke možete dodati tijek rada i proces podataka tvrtke kao dio ovaj tijek rada u aplikaciji za logiku. 

Poveznik za datoteka koristi Upravitelja veze hibridnog hibridnog veza sa sustavom datotečnom sustavu glavnog računala.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Stvaranje datoteke poveznik za logike aplikacije ##
Da biste koristili poveznik za datoteke, morate najprije stvoriti instancu aplikaciju API poveznik za datoteke. To možete učiniti na sljedeći način:

1.  Otvorite trgovine Windows Azure pomoću na + NOVO mogućnost na lijevoj strani portala za Azure.
2.  Traženje "datoteka poveznik".
3.  Odaberite **Datoteku Connector (pretpregled)** u rezultatima pretraživanja.
4.  Odaberite gumb **Stvori**
5.  Konfiguriranje poveznik za datoteke na sljedeći način:  
![][1]

    - **Ime** - dati naziv poveznik za datoteke
    - **Postavke paketa**
        - **Korijenska mapa** - Navedite put do korijenske mape na glavnom računalu. Npr. D:\FileConnectorTest
        - **Niz za povezivanje servisa Bus** – pružaju niza za povezivanje Bus servisa. Provjerite je li servis bus prostora za naziv vrste standardnu i ne osnovni omogućuju korištenje servisa Bus preusmjeravanje.  Servis Bus preusmjeravanja se koristi za povezivanje za Upravitelja veze hibridnog.
    - **Aplikacije servisa za planiranje** – odaberite ili stvorite plan aplikacije servisa
    - **Cijene sloju** – odaberite sloj cijene za poveznik
    - **Grupa resursa** - odaberite ili stvorite grupu resursa gdje treba nalaziti poveznik
    - **Pretplata** - odaberite pretplatu koju želite da se ovaj poveznik će biti stvoren u
    - **Mjesto** – odaberite geografsku lokaciju na kojoj želite poveznik uvesti

4. Kliknite na Stvori. Stvorit će se novi poveznik za datoteke

## <a name="configure-hybrid-connection-manager"></a>Konfiguriranje Hibridne Upravitelja veze ##
Nakon stvaranja instance aplikacije API-JA, pronađite njegov nadzorne ploče.  To možete učiniti tako da kliknete na Pregledaj > API aplikacije > odaberite datoteku poveznik aplikacija API-JA.  Na tom mjestu Upravitelja veze hibridnog potrebno je konfigurirati.
Dodatne informacije o konfiguriranju i Upravitelja veze hibridnog otklanjanje poteškoća potražite u članku [Korištenje upravitelja veze hibridnog].

## <a name="using-the-file-connector-in-your-logic-app"></a>Pomoću poveznika datoteke u aplikaciji logike ##
Nakon stvaranja aplikacije API-JA, sada možete koristiti poveznik datoteke kao akciju logike aplikacije. Da biste to učinili, morate:

1.  Stvorite novu aplikaciju logike, a zatim odaberite grupu resursa koji ima poveznik za datoteke. Slijedite upute za [Stvaranje nove aplikacije logike].

2.  Otvorite "Okidača i akcije" stvorene aplikacije logike za otvaranje aplikacije logike Designer i konfiguracija vašeg tijeka.

3.  Poveznik za datoteke pojavit će se u odjeljku "API aplikacije u ovu grupu resursa" u galeriji na desnoj strani.

4.  Ispustite aplikaciju API poveznik za datoteku u uređivaču tako da kliknete "Connector datoteke". poveznik za datoteke izlaže jedan okidača i 4 akcije:  
![][5]

6.  Svakoga od njih izlaže određena svojstva. Na slici u nastavku navedeni svojstava okidača i dobiti datoteku akcija:  
![][6]

7. Kada to su konfigurirana okidača i akcija može se koristiti u vašem tijek. Isto tako, druge akcije moguće je konfigurirati kao i.

> [AZURE.NOTE] Okidača datoteke će izbrisati datoteku nakon što je uspješno čitanje iz mape.

## <a name="file-connector-rest-apis"></a>Datoteka poveznik REST API-ji ##
Da biste koristili poveznik izvan logike aplikacije, možete leveraged REST API-ji koji prikazuje poveznik. Koje možete prikaz ovaj API definicije pomoću Pregledaj -> Api aplikacije -> poveznik za datoteke. Sada kliknite na definiciju API-JA u odjeljku sažetak da biste pogledali sve API-ji koji prikazuje ovaj poveznik:  
![][7]

Detalje o API-ji možete pronaći na [poveznik datoteku definicije API-JA].

## <a name="do-more-with-your-connector"></a>Bolje iskorištavanje vaše poveznika
Sad kad je poveznik stvorili, možete ga dodati u tijeku rada poslovne logike aplikacija. U odjeljku [koje su aplikacije logike?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Ako želite započeti s Azure logike aplikacije prije registracije za račun za Azure, idite na [pokušajte logike aplikacije](https://tryappservice.azure.com/?appservice=logic), gdje možete odmah stvorite short-lived starter logike aplikaciju u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

Prikaz reference Swagger REST API-JA na [poveznika i API aplikacije Reference](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Možete pregledati i performanse Statistika i kontrola sigurnost da biste poveznik. U odjeljku [Upravljanje i praćenje ugrađene aplikacije API -JA i poveznik](app-service-logic-monitor-your-connectors.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Stvorite novu aplikaciju logike]: app-service-logic-create-a-logic-app.md
[Poveznik za datoteku definicije API-JA]: https://msdn.microsoft.com/library/dn936296.aspx
[Pomoću Upravitelja veze hibridnog]: app-service-logic-hybrid-connection-manager.md
