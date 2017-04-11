<properties
 pageTitle="Omogućivanje upravljanim uređajima iza pristupnik za IoT | Microsoft Azure"
 description="Smjernice teme pomoću pristupnik za IoT stvoren pomoću pristupnika SDK IoT zajedno s uređaja upravlja IoT koncentratora."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>Omogućivanje upravljanim uređajima iza pristupnik za IoT

## <a name="basic-device-isolation"></a>Osnovni uređaj odvajanja

Tvrtke i ustanove često koriste pristupnika IoT da biste povećali cjelokupan sigurnost njihova IoT rješenja. Neke uređaje morate slanje podataka u oblaku, ali se ne možete sami zaštiti od prijetnji na Internetu. Ove uređaje s vanjskim niti možete shield tako da ih komunikaciju izvan svijeta preko pristupnika.

Pristupnik nalazi se na obrubu između sigurnog okruženja i otvaranje internetske. Uređaji Razgovarajte s pristupnika i pristupnika prosljeđuje poruke duž odredište točan oblaka. Pristupnika hardened na temelju vanjskih niti te blokira neovlašteno zahtjeve, omogućuje ovlašteni u granica promet i prosljeđuje te u granica promet na odgovarajući uređaj.

![][1]

Možete postaviti i uređaje koje možete zaštititi same iza pristupnik za dodatnu razinu zaštite. U ovom scenariju samo morate zadržati pristupnika OS patched protiv najnovije slabe točke, umjesto ažuriranje OS-a na svim uređajima.

## <a name="isolation-plus-intelligence"></a>Odvajanja plus obavještavanje

Hardened usmjerivač nije dovoljno pristupnika jednostavno izdvojiti uređaja. Međutim, IoT rješenja često potrebno da pristupnik nudi više obavještavanje od jednostavno izoliranja uređaja. Na primjer, preporučujemo vam da možete upravljati iz oblaka. Vi ste moći koristiti [LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki), protokol za upravljanje standardne uređaj, za upravljanje oblaka dio rješenja. Međutim, uređaje pošaljite telemetrijskih pomoću koje nisu TCP/IP omogućen protokol. Osim toga, uređaje daju veći broj podataka, a samo želite prenijeti filtrirani podskup za telemetriju. Možete izrađivati rješenje koje sadrži IoT pristupnika možete Postupanje s dva različita strujanja podataka. Pristupnik je potrebno:

-   Razumijevanje **Telemetrijskih**, filtrirati, a zatim prijenos ga s oblakom pomoću pristupnika. Pristupnik je više jednostavne usmjerivač koji se jednostavno prosljeđuje podataka između uređaja i s oblakom.

-   Jednostavno razmjenjivati **LWM2M uređaj upravljanje podataka** između uređaja i s oblakom. Pristupnik nije potrebno je razumjeti podatke koji dolaze u nju, a samo mora da biste bili sigurni dohvaća podatke natrag i naprijed proslijeđena između uređaja i s oblakom.

Sljedeća slika prikazuje scenarij:

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>Rješenje: upravljanje uređajima Azure IoT i pristupnika SDK IoT 

Javno pretpregled izdanja sustava [upravljanja uređajima Azure IoT] [ lnk-device-management] i beta verziju programa [Azure IoT pristupnika SDK] Omogući ovaj scenarij. Svaki strujanja podataka pristupnika obrađuje na sljedeći način:

-   **Telemetrijskih**: pristupnika SDK IoT možete koristiti da biste sastavili kanal koji možete koristiti, filtri i šalje telemetrijskih podataka s oblakom. Pristupnik SDK IoT sadrži kod koji implementira dijelove ovaj kanal ime za razvojne inženjere. Dodatne informacije o arhitektura SDK možete pronaći u [IoT pristupnika SDK - početak] [ lnk-gateway-get-started] vodič.

-   **Upravljanje uređajima**: upravljanje uređajima Azure nudi klijent za LWM2M koja se izvršava na uređaju, kao i oblaka sučelje za izdavanje naredbi upravljanja na uređaju.
    
    Bilo koji posebnu logiku na pristupnika nije potrebna jer nije potrebna za obradu podataka LWM2M razmjenjivati između uređaja i koncentratora za IoT. Možete omogućiti zajedničko korištenje internetske veze, značajka mnogo Moderna operacijskim sustavima, pristupnika da biste omogućili exchange LWM2M podataka. Možete odabrati odgovarajući operacijski sustav za taj scenarij jer pristupnika SDK IoT podržava brojne operacijske sustave. Slijede upute za omogućivanje zajedničkog korištenja na [Windows 10] i [Ubuntu], dva mnogo podržani operacijski sustavi internetske veze.

Sljedeća ilustracija prikazuje više razine arhitektura omogućite scenarij pomoću [upravljanja uređajima Azure IoT] [ lnk-device-management] i [Azure IoT pristupnika SDK].

![][3]

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali kako koristiti pristupnika SDK IoT, pogledajte sljedeći vodiči za:

- [Pristupnik IoT SDK - prvi koraci pri korištenju Linux][lnk-gateway-get-started]
- [Pristupnik IoT SDK – slanje poruka uređaj u oblak s Simulirani uređaja koji koristi Linux][lnk-gateway-simulated]

Da biste dodatno Istražite mogućnosti IoT koncentrator, pogledajte:

- [Vodič za razvojne inženjere][lnk-devguide]
- [Simulaciju uređaj s pristupnika SDK IoT][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[SDK Azure IoT pristupnika]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md