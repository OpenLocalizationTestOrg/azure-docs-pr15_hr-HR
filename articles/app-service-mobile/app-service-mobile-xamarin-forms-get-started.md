<properties
    pageTitle="Početak rada s aplikacijama Mobile pomoću Xamarin.Forms"
    description="Slijedite ovaj Praktični vodič da biste počeli koristiti Azure mobilne aplikacije za razvoj Xamarin.Forms"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Stvaranje Xamarin.Forms aplikacije

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Pregled

Pomoću ovog praktičnog vodiča prikazuje kako dodati servise u oblaku pozadinskog Xamarin.Forms mobilne aplikacije pomoću programa pozadinskog Azure mobilnu aplikaciju. Morate stvoriti novi pozadinskog mobilnu aplikaciju i jednostavan _popis obveza popis_ aplikacije Xamarin.Forms kojoj su pohranjeni podaci aplikacije u Azure.

Dovršavanje ovog praktičnog vodiča je preduvjeta za sve aplikacije Mobile vodiče za Xamarin.Forms.

##<a name="prerequisites"></a>Preduvjeti

Da biste dovršili ovaj Praktični vodič, potrebno je sljedeće:

* Aktivni Azure račun. Ako nemate račun, možete registrirati za probnu programa Azure i dobiti do 10 besplatne mobilne aplikacije koje možete nastaviti koristiti i nakon vaše probno razdoblje završava. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio sa Xamarin. Potražite u članku [Postavljanje i instalacije za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) upute. 

* Mac s Xcode v7.0 ili noviji i Xamarin Studio zajednice instaliran. Potražite u članku [Postavljanje i instalacije za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) i [Postavljanje, instalacija, i verifications za korisnike Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Ako želite započeti s aplikacije servisa za Azure prije registracije za račun za Azure, idite na [Pokušajte aplikacije servisa](https://tryappservice.azure.com/?appServiceName=mobile), gdje možete odmah stvoriti short-lived starter mobilnu aplikaciju u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

## <a name="create-a-new-azure-mobile-app-backend"></a>Stvaranje nove pozadinske Azure mobilnu aplikaciju

Slijedite ove korake da biste stvorili novi pozadinskog za mobilnu aplikaciju.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Sada ste dodjeli Azure mobilnu aplikaciju pozadinskog sustava koje je moguće koristiti mobilnu verziju klijentskog aplikacija. Nakon toga će preuzimati project server za jednostavan "obveze popis" pozadinskog sustava i objavite Azure.

## <a name="configure-the-server-project"></a>Konfiguriranje server project

Slijedite korake u nastavku da biste konfigurirali project server da biste koristili Node.js ili .NET pozadinskog.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Preuzmite i pokrenite Xamarin.Forms rješenja

Ovdje imate nekoliko mogućnosti. Možete preuzeti rješenja za Mac i otvorite je u Xamarin Studio ili možete preuzeti rješenje na računalu sa sustavom Windows i otvorite je u Visual Studio pomoću umreženog Mac za izgradnju aplikaciju iOS. Pogledajte odjeljak [Postavljanje i instalacije za Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) ako trebate detaljnije upute za postavljanje scenariji Xamarin.

Pogledajmo nastavite:

 1. Na računalu Mac ili na računalu sa sustavom Windows, otvorite [Azure Portal] u prozoru preglednika.
 2. Na plohu postavke za mobilnu aplikaciju, kliknite **Početak rada** (u odjeljku Mobilni) > **Xamarin.Forms**. U odjeljku korak 3, kliknite **Stvori novu aplikaciju** ako već nije odabrano.  Zatim kliknite gumb **Preuzmi** .

    Time će se preuzeti projekt koji sadrži klijentska aplikacija koja je povezana s mobilne aplikacije. Spremite datoteku Komprimirana projekta s vašim lokalnim računalom i zabilježite koja je spremiti.

 3. Izdvajanje projekta koji ste preuzeli, a zatim je otvorite u Xamarin Studio ili Visual Studio.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Neobavezno) Pokretanje projekta za iOS

U ovom se odjeljku je za pokretanje projekta iOS Xamarin za uređaje sa sustavom iOS. Ako ne radite s uređajima sa sustavom iOS, preskočite ovaj odjeljak.

####<a name="in-xamarin-studio"></a>U Xamarin Studio

1. Desnom tipkom miša kliknite projekt iOS, a zatim **Postavite kao pokretanje projekta**.
2. Na izborniku za **pokretanje** kliknite **Start ispravljanje pogrešaka** omogućuje stvaranje projekta i pokretanje aplikacije u iPhone emulator.

####<a name="in-visual-studio"></a>U Visual Studio
1. Desnom tipkom miša kliknite projekt iOS, a zatim **Postavi kao pokretanje projekta**.
2. Na izborniku **Sastavljanje** kliknite **Upravitelj konfiguracije**.
3. U dijaloškom okviru **Upravitelj konfiguracije** potvrdite okvire **Sastavljanje** i **uvođenja** iOS projekta.
4. Omogućuje stvaranje projekta i pokretanje aplikacije u iPhone emulator, pritisnite tipku **F5** .

    >[AZURE.NOTE] Ako imate problema sa sastavljanjem, pokrenite NuGet Upravitelj paketa i ažuriranje najnoviju verziju sustava Xamarin podržavaju paketa. Ponekad može projekata brzi početak rada kašnjenja u ažurira najnoviji.    

U aplikaciji, upišite smislen tekst, kao što su _Dodatne Xamarin_ , a zatim kliknite u **+** gumb.

![][10]

To nove aplikacije za mobilne uređaje pozadinski smješten u Azure šalje zahtjev za objavu. Podaci iz zahtjeva za umetnut je u tablici TodoItem. Vraća stavke koje se pohranjuju u tablici pozadinskog mobilne aplikacije, a podaci se prikazuju na popisu.

>[AZURE.NOTE]
> Možete pronaći kod koji se može pristupiti mobilne aplikacije pozadinskog sustava na TodoItemManager.cs C# datoteka prijenosne razrednom projektu biblioteke vašeg rješenja.

##<a name="optional-run-the-android-project"></a>(Neobavezno) Pokretanje projekta za Android

U ovom se odjeljku je za pokretanje projekta droid Xamarin za Android. Ako ne radite s uređajima sa sustavom Android, preskočite ovaj odjeljak.

####<a name="in-xamarin-studio"></a>U Xamarin Studio

1. Desnom tipkom miša kliknite Android projekta, a zatim **Postavite kao pokretanje projekta**.
2. Na izborniku za **pokretanje** kliknite omogućuje stvaranje projekta i pokretanje aplikacije u Android emulator **Pokrenuti ispravljanje pogrešaka** .

####<a name="in-visual-studio"></a>U Visual Studio
1. Desnom tipkom miša kliknite projekt Android (Droid), a zatim kliknite **Postavi kao pokretanje projekta**.
4. Na izborniku **Sastavljanje** kliknite **Upravitelj konfiguracije**.
5. U dijaloškom okviru **Upravitelj konfiguracije** potvrdite okvire **Sastavljanje** i **uvođenja** Android projekta.
6. Omogućuje stvaranje projekta i pokretanje aplikacije u Android emulator, pritisnite tipku **F5** .

    >[AZURE.NOTE] Ako imate problema sa sastavljanjem, pokrenite NuGet Upravitelj paketa i ažuriranje najnoviju verziju sustava Xamarin podržavaju paketa. Ponekad može projekata brzi početak rada kašnjenja u ažurira najnoviji.    


U aplikaciji, upišite smislen tekst, kao što su _Dodatne Xamarin_ , a zatim kliknite u **+** gumb.

![][11]

To nove aplikacije za mobilne uređaje pozadinski smješten u Azure šalje zahtjev za objavu. Podaci iz zahtjeva za umetnut je u tablici TodoItem. Vraća stavke koje se pohranjuju u tablici pozadinskog mobilne aplikacije, a podaci se prikazuju na popisu.

> [AZURE.NOTE]
> Možete pronaći kod koji se može pristupiti mobilne aplikacije pozadinskog sustava na TodoItemManager.cs C# datoteka prijenosne razrednom projektu biblioteke vašeg rješenja.


##<a name="optional-run-the-windows-project"></a>(Neobavezno) Pokrenite Windows projekta


U ovom se odjeljku je za pokretanje Xamarin WinApp project za uređaje sa sustavom Windows. Ako ne radite s uređaje sa sustavom Windows, preskočite ovaj odjeljak.


####<a name="in-visual-studio"></a>U Visual Studio
1. Desnom tipkom miša kliknite bilo koji od projekata sustava Windows, a zatim **Postavi kao pokretanje projekta**.
4. Na izborniku **Sastavljanje** kliknite **Upravitelj konfiguracije**.
5. U dijaloškom okviru **Upravitelj konfiguracije** potvrdite okvire **Sastavljanje** i **uvođenja** Windows projekta koji ste odabrali.
6. Omogućuje stvaranje projekta i pokretanje aplikacija u sustavu Windows emulator, pritisnite tipku **F5** .

    >[AZURE.NOTE] Ako imate problema sa sastavljanjem, pokrenite NuGet Upravitelj paketa i ažuriranje najnoviju verziju sustava Xamarin podržavaju paketa. Ponekad može projekata brzi početak rada kašnjenja u ažurira najnoviji.    


U aplikaciji, upišite smislen tekst, kao što su _Dodatne Xamarin_ , a zatim kliknite u **+** gumb.

To nove aplikacije za mobilne uređaje pozadinski smješten u Azure šalje zahtjev za objavu. Podaci iz zahtjeva za umetnut je u tablici TodoItem. Vraća stavke koje se pohranjuju u tablici pozadinskog mobilne aplikacije, a podaci se prikazuju na popisu.

![][12]

> [AZURE.NOTE]
> Možete pronaći kod koji se može pristupiti mobilne aplikacije pozadinskog sustava na TodoItemManager.cs C# datoteka prijenosne razrednom projektu biblioteke vašeg rješenja.

##<a name="next-steps"></a>Daljnji koraci

* [Dodavanje provjere autentičnosti za aplikaciju](app-service-mobile-xamarin-forms-get-started-users.md)  
Upute za provjeru autentičnosti korisnika aplikacije s davateljem identiteta.

* [Automatske obavijesti dodati aplikacije](app-service-mobile-xamarin-forms-get-started-push.md)  
Saznajte kako dodati automatske obavijesti podržava aplikacije i konfigurirati mobilnu aplikaciju pozadinskog sustava da biste koristili koncentratora Azure obavijesti da biste poslali automatske obavijesti.

* [Omogućivanje izvanmrežne sinkronizacije za aplikaciju](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Saznajte kako dodati aplikacije pomoću programa pozadinskog mobilnu aplikaciju Izvanmrežna podrška. Izvanmrežna sinkronizacija omogućuje krajnjim korisnicima omogućuje interakciju s mobilne aplikacije&mdash;prikaz, dodavanje ili izmjenu podataka&mdash;čak i kad je nema mrežne veze.

* [Kako koristiti upravljane klijent za Azure mobilne aplikacije](app-service-mobile-dotnet-how-to-use-client-library.md)  
Saznajte kako raditi s upravljanih klijent SDK Xamarin aplikacije. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Portal za Azure]: https://portal.azure.com/

