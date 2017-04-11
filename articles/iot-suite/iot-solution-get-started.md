<properties
    pageTitle="Primjer MyDriving Azure IoT: kratke | Microsoft Azure"
    description="Početak rada s aplikaciju koja nije potpun pokazni o mijenjanje arhitekture u sustavu IoT pomoću Microsoft Azure, uključujući strujanje analize, strojnog učenja i koncentratora za događaj."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT sustava: brzi početak rada

MyDriving je sustav koji pokazuje dizajna i implementaciju uobičajena rješenja [Internet stvari](iot-suite-overview.md) (IoT) prikuplja telemetrijskih s uređaja, obrađuje tih podataka u oblaku i primjenjuje strojnog učenja omogućuje prilagodljivo odgovor. Tekstnog zapisnike podataka o vašem trips automobila pomoću podataka iz mobilnog telefona i prilagodnik koji prikuplja informacije iz sustava automobil kontrolu. Ove podatke koristi da biste poslali povratne informacije vožnju stil usporedbi s drugim korisnicima.

Realni Svrha MyDriving je za početak u stvaranju IoT rješenje. No prije toga ćemo vam četak rada s MyDriving samoj aplikaciji – kao član naš tim za testiranje korisnika. Tako ćete dobiti iskustvo aplikacije i u sustavu iza ga kao potrošača, prije delve u arhitektura. Također koji predstavlja da biste HockeyApp, zanimljivih način upravljanja distribucija alfa i beta aplikacije da biste testirali korisnika.

## <a name="use-the-mobile-experience"></a>Korištenje na mobilnim uređajima

Ako imate uređaju sa sustavom Android, iOS ili Windows 10 možete koristiti aplikaciju MyDriving.

### <a name="android-and-windows-10-mobile-installation"></a>Instalacija sustava Windows 10 Mobile i android

Na uređaju:

1.  Dopusti razvoj aplikacija:

    -   Android: U **postavkama** > **Sigurnost**Dopusti aplikacije iz **nepoznatih izvora**.

    -   Windows 10: U **postavkama** > **ažuriranja** > **Za razvojne inženjere**, postavite **način rada za razvojne inženjere**.

2.  Uključivanje naš tim za testiranje beta Registracija s ili prva prijava u [HockeyApp](https://rink.hockeyapp.net). HockeyApp olakšava distribucija Prijevremeni izdanja aplikacije da biste testirali korisnika.

    Ako koristite Windows 10, pomoću preglednika Edge.

    Ako ste sudionik Sastavi 2016, prijavite se pomoću isti Microsoftov račun e-pošte da ste registrirali za u konferenciju pomoću neke od gumba Microsoft. Već prijavili ste se s HockeyApp.

    ![Zaslon za prijavu u HockeyApp](./media/iot-solution-get-started/image1.png)

3.  Preuzmite i instalirajte aplikaciju na tom mjestu:

    -   [Android](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10](http://rink.io/spMyDrivingUWP)

    Postoje dvije stavke. Instalirajte certifikat u **Pouzdane osobe**. Zatim instalirajte aplikaciju.

*Pokretanje aplikacije u sustavu Windows 10 Mobile probleme?* Možda je telefonu ažuriranje ili dvije iza. Pripazite da imate najnovija ažuriranja ili ste instalirali:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>Instalacija iOS

Ako kojima ste sudjelovali Sastavi 2016, preuzmite aplikaciju kao član naš tim za testiranje na HockeyApp:

1.  Na uređaju sa sustavom iOS, prijavite se u [HockeyApp](https://rink.hockeyapp.net).
    Koristite neku od gumbe za prijavu u Microsoft i prijavite se pomoću isti Microsoftov račun e-pošte koji registriran za konferencije. (Nemojte koristiti polja e-pošte i lozinke.)

    ![Zaslon za prijavu u HockeyApp](./media/iot-solution-get-started/image1.png)

2.  Na nadzornoj ploči HockeyApp odaberite MyDriving i preuzmite ga.

3.  Autorizirajte beta izdanje od HockeyApp:

    na. Idite na **Postavke** > **Općenito** > **profili i upravljanje uređajima.**

    b. Pouzdanosti certifikata **Bitne Stadium GmbH** .

Ako niste Sudjelujte Sastavi 2016, možete izraditi i implementacija aplikacije:

1.   Preuzimanje kod [iz GitHub].

2.   Stvorite i implementirajte pomoću [Xamarin].

Dodatne detalje potražite u [MyDriving vodič](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Početak prilagodnik OBD (neobavezno)

To je dio koji omogućuje realni Internet stvari sustava! Možete koristiti aplikacije bez onaj koji se, ali je dodatna Zabava uz stvarni objekt, a nisu skupi.

Ploči Dijagnostika (OBD) je značajka automobil primijenjenom na garaže za ugađanje automobil i dijagnosticiranje odd Zvukovi i svjetiljke upozorenje. Osim ako je automobil odlično antiquity, pronaći ćete socket negdje u komora obično iza preklapa koja se nalazi u odjeljku na nadzornoj ploči. S desne connector možete dobiti mjernih podataka o performansama u modul i neke prilagodbe. Poveznik za OBD možete kupiti cheaply od uobičajenog mjesta. Povezuje korištenjem Bluetooth ili Wi-Fi za aplikaciju na telefonu.

U ovom slučaju ne možemo ćete povezati automobil s oblakom. Izravna veza s u OBD je na telefon, ali naša aplikacija funkcionira kao na prijenos. Automobil telemetrijskih poslane izravno u središtu MyDriving IoT gdje obrađuju da se prijavi na putu trips i procijenite vožnju stil.

Da biste se povezali uređaju sa sustavom OBD:

1.  Provjerite ima li automobil programa socket OBD.

2.  Nabavite prilagodnik OBD:

    -   Ako koristite telefon sa sustavom Android ili Windows, morat ćete prilagodnik za Bluetooth omogućeno OBD II. Koristi [BAFX proizvodi 34t5 Bluetooth OBDII skeniranje alata].

    -   Ako koristite telefon sa sustavom iOS, morat ćete prilagodnik Wi-Fi-omogućeno OBD. Koristi [ScanTool OBDLink MX Wi-Fi mreže: OBD prilagodnika/Dijagnostika skenera].

3.  Slijedite upute koje se isporučuju uz OBD prilagodnik za povezivanje s telefona. Imajte sljedeće na umu:

    -   Prilagodnik za Bluetooth mora biti Uparena s telefonom, na stranici **Postavke** .

    -   Wi-Fi prilagodnik mora imati adresu 192.168.xxx.xxx raspona.

4.  Ako imate nekoliko Automobili, možete dobiti zasebnom prilagodnik za svaki (najviše tri).

Ako nemate prilagodnik OBD, aplikacija će i dalje slati mjesto i brzinu podatke iz tekstnog okvira na telefonu GPS pozadinska i će vas pitati želite li kao zamjenu za OBD.

Možete saznati više o tome kako aplikaciju koristi podatke iz prilagodnik OBD i o mogućnostima za stvaranje OBD uređaj u odjeljku 2.1, "IoT uređaji" [Referentni vodič za MyDriving](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Korištenje aplikacije

Pokrenite aplikaciju. Nema početne brzi početak rada za vas voditi kroz načina funkcioniranja tijeka rada.

### <a name="track-your-trips"></a>Praćenje vaše trips

Dodirnite gumb zapis (veliki Crveni krug pri dnu zaslona) da biste pokrenuli putovanja, a ponovno da biste završili.

![Slika gumba zapis za putovanje praćenja](./media/iot-solution-get-started/image2.png)

Svaki put kada pokrenete putovanja, ako postoji nijedan OBD uređaj, koji će se zatražiti ako želite koristiti simulator.

Na kraju putovanja, dodirnite gumb Zaustavi, a nailazite na sažetak.

![Primjer sažetka putovanja](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Pregledajte svoje trips

![Primjer proteklih putovanja](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Pregled profila

![Primjer vožnju stil profila](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Pošaljite nam povratne informacije test

Budući da ne možemo vlastitu sustavi IoT stvorili MyDriving da biste lakše jumpstart, certainly želimo mišljenje o koliko će se dobro funkcionira. Javite nam ako:

- Naiđete na poteškoće ili izazove.

- Postoji točku proširenje da bi neka prikladniji za vaš scenarij.

- Pronađite učinkovitiji način da biste izvršili određenim potrebama.

- Imate sve prijedloge za poboljšanje MyDriving ili ove dokumentacije.

MyDriving aplikacije sam koristite ugrađeni HockeyApp mehanizam za povratne informacije: na iOS i Android, samo dati telefona na potresanja ili pomoću naredbe izbornika **povratne informacije** . To će automatski priložiti snimke zaslona, tako da se ne možemo znali što se radi o. A ako je bilo koji unfortunate ruši, HockeyApp prikuplja zapisnike rušenje da biste nam o njima. Možete dati i povratne informacije putem [HockeyApp portal].

Možete arhivirati [problem na GitHub], ili ostavite komentar ispod (en-us edition).

Ne možemo izgled naprijed sluha od vas!

## <a name="next-steps"></a>Daljnji koraci

-   Istražite [MyDriving referentnog vodiča](http://aka.ms/mydrivingdocs) da biste razumjeli kako ćemo ste dizajniran i ugrađena cijelog MyDriving sustava.

-   [Stvaranje i implementacija sustava vlastitu](iot-solution-build-system.md) putem skripti naš Azure Voditelj resursa. [Referentni vodič za MyDriving](http://aka.ms/mydrivingdocs) i vodi vas kroz područja ćete odabirati većinu prilagodbi.

  [iz GitHub]: https://github.com/Azure-Samples/MyDriving
  [Korištenje Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [BAFX proizvodi 34t5 Bluetooth OBDII alata za skeniranje]: http://www.amazon.com/gp/product/B005NLQAHS
  [ScanTool OBDLink MX Wi-Fi: OBD prilagodnika/Dijagnostika skenera]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [Portal za HockeyApp]: https://rink.hockeyapp.org
  [problema na GitHub]: https://github.com/Azure-Samples/MyDriving/issues
