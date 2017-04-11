<properties
    pageTitle="Krajnje točke servisa Azure"
    description="U članku se opisuje postavke krajnja točka servisa Azure u komplet alata za Azure za Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Krajnje točke servisa Azure #

Krajnje točke servisa Azure odredite hoće li se aplikacija implementiran na i upravlja globalni platforme Azure, Azure kojim upravlja 21vianet u Kini ili privatnu platforme Azure. Dijaloški okvir **Krajnje točke servisa** omogućuje određivanje krajnje točke servisa koji želite koristiti. Da biste otvorili dijaloški okvir **Krajnje točke servisa** unutar Eclipse, kliknite **prozor**, kliknite **Postavke**, proširite **Azure**, a zatim **Krajnje točke servisa**. Postavljanje polja **Aktivni postavljanje** određuje koje krajnje točke servisa Azure će se koristiti za Azure projektima u trenutnom radnog prostora.

Sljedeće prikazuje dijaloški okvir **Krajnje točke servisa** .

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Da biste postavili krajnje točke servisa ##

U dijaloškom okviru **Krajnje točke servisa** , učinite nešto od sljedećeg:

* Ako želite koristiti globalni platforme Azure, s padajućeg popisa **Aktivni postavljanje** odaberite **windowsazure.com** i kliknite **u redu**.
* Ako želite koristiti Azure putem davatelja 21Vianet u Kini, s padajućeg popisa **Aktivni postavljanje** odaberite **windowsazure.cn (Kina)** i kliknite **u redu**.
* Ako želite koristiti privatne platforme Azure:
    1. Kliknite **Uređivanje**.
    2. Otvara se dijaloški okvir, koja vas obavještava da zatvorit će se dijaloški okvir **Krajnje točke servisa** , i datoteku skupove preferenca će se otvoriti. Kliknite **u redu**.
    3. U datoteci preferencesets.xml, stvorite novu `preferenceset` element. Za taj novi element stvorite `name`, `blob`, `management`, `portalURL` i `publishsettings` atributi i dodajte vrijednosti za njih koji odgovaraju privatne Azure platformu Microsoft. Koristite li vrijednosti za postojeće `preferenceset` elemente kao predlošci. **Napomena**: vrijednost koja se koristi za na `blob` atributa mora sadržavati tekst "blob" u URL-u.
    4. Spremite i zatvorite preferencesets.xml.
    5. Ponovno otvorite dijaloški okvir **Krajnje točke servisa** .
    6. S padajućeg popisa **Aktivni postavljanje** odaberite Postavi aktivni koji ste stvorili, a zatim kliknite **u redu**.
    7. Nakon što stvorite svoju platformu privatne Azure `preferenceset` element, možete promijeniti vrijednosti dodijeljena klikom na gumb **Uređivanje** u dijaloškom okviru **Krajnje točke usluge** . Možete i stvoriti više privatne platforme Azure `preferenceset` elemenata, ako po volji.

## <a name="see-also"></a>Vidi također ##

[Azure komplet alata za Eclipse][]

[Instalacija alata za Azure za Eclipse][] 

[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse][]

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java][].

<!-- URL List -->

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png
