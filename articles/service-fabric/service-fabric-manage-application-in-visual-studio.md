<properties
   pageTitle="Upravljanje aplikacija u Visual Studio | Microsoft Azure"
   description="Pomoću programa Visual Studio možete stvarati, razvoj, pakiranje, uvođenje i usluge tkanina aplikacije i servise za ispravljanje pogrešaka."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="seanmck;mikhegn"/>

# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Pomoću programa Visual Studio da biste pojednostavnili pisanje i upravljanje tkanina servisa aplikacija

Možete upravljati tkanina servisa Azure aplikacija i servisa putem Visual Studio. Kada ste [postavili okruženje za razvoj](service-fabric-get-started.md), koristite Visual Studio stvaranje aplikacije servisa tkanina, dodajte services ili paket, register i implementacija aplikacije u svoj klaster lokalne razvoj.

## <a name="deploy-your-service-fabric-application"></a>Implementacija aplikacije servisa tkanina

Prema zadanim postavkama implementacija aplikacije kombinira sljedeće korake u jednog jednostavnog postupka:

1. Stvaranje paketa aplikacije
2. Prijenos paketa aplikacije u trgovini slike
3. Registracija vrsta aplikacije
4. Uklanjanje bilo pokrenute instance aplikacije
5. Stvaranje nove instance aplikacije

U Visual Studio tipku **F5** će implementacija aplikacije i priložite program za ispravljanje pogrešaka sve instance aplikacije. **Ctrl + F5** možete koristiti za implementaciju aplikacije bez pogrešaka ili možete objaviti na lokalnom ili udaljenom klaster s profilom Objavi. Dodatne informacije potražite u članku [Objavljivanje aplikacija na udaljenom klaster pomoću Visual Studio](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Način rada za ispravljanje pogrešaka aplikacije

Prema zadanim postavkama Visual Studio uklanja pojavljivanja vrste aplikacije kada prekinete ispravljanje pogrešaka ili (Ako ste implementirati aplikaciju bez prilaganja program za ispravljanje pogrešaka), kada ponovno implementirate aplikaciju. U tom slučaju uklanjaju se podaci za sve aplikacije. Tijekom ispravljanje pogrešaka lokalno, želite zadržati podatke koje ste već stvorili kada testiranje novu verziju aplikacije, koje želite zadržati pokretanje aplikacije ili želite da se kasnije ispravljanje pogrešaka sesije za nadogradnju aplikacije. Visual Studio Tools tkanina servisa omogućuju svojstvo **Način za ispravljanje pogrešaka aplikacije**koje kontrole li **F5** treba Deinstaliranje aplikacije, zadržavanje aplikacije koje se izvode kada sesije ispravljanja pogrešaka završava ili omogućivanje aplikacije da biste nadogradili na sljedećim pogrešaka sesijama, umjesto uklonjen i ponovno implementirati.

#### <a name="to-set-the-application-debug-mode-property"></a>Da biste postavili svojstvo način za ispravljanje pogrešaka aplikacije

1. Na izborniku prečaca projekta aplikacije, odaberite **Svojstva** (ili pritisnite tipku **F4** ).
2. U prozoru **Svojstva** svojstvo **Način za ispravljanje pogrešaka aplikacije** .

    ![Postavite svojstvo način rada za ispravljanje pogrešaka aplikacije][debugmodeproperty]

To su dostupne mogućnosti **Načina za ispravljanje pogrešaka aplikacije** .

1. **Nadogradnja automatsko**: aplikacija nastavlja se izvoditi kada istekne sesije ispravljanja pogrešaka. Sljedeći **F5** će Smatraj implementaciju nadogradnje pomoću automatiziranog automatski način rada za brzu nadogradnju aplikacije na noviju verziju s nizom datuma dodan. Postupak nadogradnje zadržava sve podatke koje ste unijeli u prethodnoj sesiji ispravljanje pogrešaka.

2. **Zadrži aplikacije**: aplikacija zadržava se izvodi u klasteru kada istekne sesije ispravljanja pogrešaka. Na sljedećem **F5** aplikacije uklonit će se i upravo ugrađeni aplikacija će biti implementirano u klaster.

3. **Uklanjanje aplikacije** uzrokuje aplikacije koje će biti uklonjene kada istekne sesije ispravljanja pogrešaka.

**Automatsko** nadogradnju podataka zadržava se primjenom mogućnosti nadogradnje aplikacije servisa tkanina, ali postavljen je na optimiziranja performansi umjesto sigurnost. Dodatne informacije o nadogradnji aplikacije i kako mogu izvršiti nadogradnju u okruženju realni potražite u članku [Nadogradnja servisa tkanina aplikacije](service-fabric-application-upgrade.md).

![Primjer novu verziju aplikacije s datumom dodan][preservedata]

>[AZURE.NOTE] To svojstvo ne postoji prije verzije 1.1 tkanina alata za servis za Visual Studio. Prije no što 1.1, koristite svojstvo **Sačuvaj podatke pri pokretanju** da biste postigli iste ponašanje. Mogućnost "Zadržati aplikacije" je uvedena u verzija 1.2 tkanina alata za servis za Visual Studio.

## <a name="add-a-service-to-your-service-fabric-application"></a>Dodavanje servisa u aplikaciji tkanina servisa

U aplikaciji za proširivanje njegova funkcionalnost možete dodati nove servise.  Da biste bili sigurni da je servis uključen u Aplikacijski paket, dodajte usluge putem stavku izbornika **Novi tkanina servis...** .

![Dodavanje novog servisa tkanina u aplikaciji][newservice]

Odaberite vrstu projekta tkanina servisa da biste dodali aplikaciju, a zatim navedite naziv za servis.  U odjeljku [Odabir framework servisa](service-fabric-choose-framework.md) da biste lakše odlučili koju vrstu servisa da biste koristili.

![Odaberite vrstu projekta tkanina servisa za dodavanje aplikacije][addserviceproject]

Servis za novi će se dodati rješenje i postojećeg paketa aplikacije. Reference servisa i zadane instance servisa dodaje se programski manifest. Servis bit će stvoriti i pokrenuti sljedeći put implementacija aplikacije.

![Novi servis dodat će se programski manifest][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Pakiranje tkanina servisa aplikacija

Da biste implementirali aplikacija i uslugama na klaster, potrebnih za stvaranje paketa aplikacije.  Paket organizira programski manifest, manifest(s) servisa i ostale potrebne datoteke u određeni raspored.  Visual Studio postavlja i upravlja njima paketa u mapi aplikacije projekta u direktoriju 'pkg'.  Kliknete **paket** na kontekstnom izborniku **aplikacije** Stvori ili ažurira Aplikacijski paket.  Preporučujemo vam da biste to učinili ako pokrenete aplikaciju pomoću prilagođene skripte komponente PowerShell.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Uklanjanje aplikacije i vrsta aplikacija pomoću programa Explorer oblaka

Možete izvršiti operacija upravljanja osnovni klaster iz programa Visual Studio pomoću programa Explorer oblaka, koje možete pokrenuti iz izbornika **Prikaz** . Ako, primjerice, možete izbrisati aplikacije i Oslobađanje resursa vrsta aplikacija na lokalnom ili udaljenom klastere.

![Uklanjanje aplikacije](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Bogatiji funkcija upravljanja klaster, potražite u članku [vizualizacija svoj klaster pomoću programa Explorer tkanina servisa](service-fabric-visualizing-your-cluster.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

- [Servis tkanina modelu](service-fabric-application-model.md)
- [Uvođenje servisa tkanina aplikacije](service-fabric-deploy-remove-applications.md)
- [Upravljanje parametara aplikacije za više okruženja](service-fabric-manage-multiple-environment-app-configuration.md)
- [Ispravljanje pogrešaka aplikacije usluge tkanina](service-fabric-debugging-your-application.md)
- [Vizualizacija svoj klaster pomoću programa Explorer tkanina servisa](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
