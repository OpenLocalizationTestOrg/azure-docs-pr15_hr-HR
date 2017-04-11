<properties 
    pageTitle="Korištenje značajki aplikacije logike | Microsoft Azure" 
    description="Saznajte kako koristiti napredne značajke logike aplikacije." 
    authors="stepsic-microsoft-com" 
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
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>Korištenje značajki logike aplikacije

Na [prethodnu temu](app-service-logic-create-a-logic-app.md)stvara prvi logike aplikacije. Sada Pokazat ćemo vam sastavljanje potpuniji procesa pomoću servisa logike aplikacija. U ovoj se temi predstavlja sljedeće nove aplikacije logike koncepata:

- Sustava SharePoint, koji izvršava akciju samo kada se ispuni određeni uvjet.
- Prikaz koda da biste uredili postojeću logike aplikacije.
- Mogućnosti za pokretanje tijeka rada.

Prije dovršetka u ovoj se temi, dovršite korake u odjeljku [Stvaranje nove aplikacije logike](app-service-logic-create-a-logic-app.md). [Portal za Azure]pronađite logike aplikacije, a zatim kliknite **okidača i akcija** u sažetku da biste uredili definiciju aplikacije logike.

## <a name="reference-material"></a>Pregled materijala

Sljedeće dokumente mogu pronaći korisne:

- [Upravljanje i izvođenja REST API -ji](https://msdn.microsoft.com/library/azure/mt643787.aspx) – uključujući izravno pozivanje logike aplikacije
- [Jezična referenca](https://msdn.microsoft.com/library/azure/mt643789.aspx) - sveobuhvatan popis svih podržanih funkcije/izraza
- [Vrste okidača i akcija](https://msdn.microsoft.com/library/azure/mt643939.aspx) : različitih vrsta akcije i unosa proizvela
- [Pregled aplikacije servisa](../app-service/app-service-value-prop-what-is.md) - opis koje komponente da biste odredili vrijeme da biste sastavili rješenje

## <a name="adding-conditional-logic"></a>Dodavanjem sustava SharePoint

Iako izvorni tijek rada, postoje neka područja koja mogu poboljšati. 


### <a name="conditional"></a>Uvjetno
Logika aplikacija može uzrokovati vam početak mnogo poruka e-pošte. Sljedeći koraci dodavanje logike da biste bili sigurni da samo primite poruku e-pošte kada se tweet ne šalje netko s određenim brojem prate. 

1. Kliknite gumb plus i pronađite akcije *Korisnika se* za Twitter.

2. Prenesite u polju **Tweeted tako da** iz okidača radi dobivanja informacija o korisniku Twitter.

    ![Početak korisnika](./media/app-service-logic-use-logic-app-features/getuser.png)

3. Ponovno kliknite gumb plus, ali ovaj put odaberite **Dodaj uvjet**

4. U prvom okviru kliknite **...** ispod njega **Se korisnik** pronađite polje **koje slijede count** .

5. Na padajućem popisu odaberite **veće od**

6. U drugi okvir upišite broj prate želite dodijeliti korisnicima.

    ![Uvjetno](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  Na kraju okvira povlačenje i ispuštanje e-pošte u okvir **Ako da** . To znači samo dobit ćete poruke e-pošte kada je broj follower zadovoljeni.

## <a name="repeating-over-a-list-with-foreach"></a>Ponavljajuća preko popisa s forEach

Petlja forEach određuje polja da biste ponovili akciju iznad. Ako nije polja tijeka neće uspjeti. Na primjer, ako imate action1 koji proizvodi polja poruka, a želite poslati svake poruke možete obuhvatiti izjavu o zaštiti forEach svojstva radnju: forEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>Prikaz koda za uređivanje koristi logike aplikacije

Osim designer, možete izravno uređivati kod koji definira logike aplikaciju, na sljedeći način. 

1. Kliknite gumb **Prikaz koda** na naredbenoj traci. 

    Otvorit će se cijeli uređivač koji prikazuje definicije samo uređivati.

    ![Prikaz koda](./media/app-service-logic-use-logic-app-features/codeview.png)

    Pomoću uređivač teksta, možete je kopirati i zalijepiti bilo koji broj od akcija unutar iste logike aplikacije ili između logike aplikacije. Jednostavno možete dodati ili ukloniti cijele sekcije iz definicije, a možete omogućiti zajedničko korištenje definicije s drugim korisnicima.

2. Kad unesete promjene u prikazu koda, jednostavno kliknite **Spremi**. 

### <a name="parameters"></a>Parametri
Postoje neke mogućnosti logike aplikacije koje možete koristiti samo u prikazu koda. Primjer od njih je parametara. Parametri olakšavaju iskoristite vrijednosti cijeloj logike aplikacije. Na primjer, ako imate adresu e-pošte koji želite koristiti u nekoliko postupaka, potrebno je definirati kao parametar.

Postojeće logike aplikacije da biste koristili parametara termina upita ažuriranja.

1. U prikazu koda pronađite u `parameters : {}` objekt, a zatim umetnite sljedeći objekt tema:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. Pomaknite se na `twitterconnector` akcija, pronađite vrijednost upita i zamijeniti ga s `#@{parameters('topic')}`.
    Funkcija **Slika** nije moguće koristiti i za spajanje dva ili više nizova, na primjer: `@concat('#',parameters('topic'))` jednak navedenog. 
 
Parametri se dobar način izvući vrijednosti koje ste vjerojatno će se promijeniti mnogo. Osobito korisni su kada je potrebno da biste nadjačali parametara u različitim okruženjima. Dodatne informacije o tome da biste nadjačali parametara ovise o okruženju potražite u članku naš [dokumentaciju REST API -JA](https://msdn.microsoft.com/library/mt643787.aspx).

Sada, kada kliknete **Spremi**, svaki sat dobiti sve nove tweets koji imaju više od 5 retweets koji se isporučuju u mapu pod nazivom **tweets** u vaše zajedničke mrežne mape.

Da biste saznali više o definicije logike aplikacije, potražite u članku [Autor definicije logike aplikacije](app-service-logic-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Pokretanje tijeka rada logike aplikacije
Postoji nekoliko različitih mogućnosti za pokretanje tijeka rada definirano u logike aplikacije. Tijek rada može uvijek se pokrenuti na zahtjev za [Azure portal].

### <a name="recurrence-triggers"></a>Ponavljanje okidača
Ponavljanje okidača pokreće intervalu koji navedete. Kada okidača ima sustava SharePoint, okidača određuje hoće li se tijek rada mora se pokrenuti. Okidač označava je izvoditi vraćanjem na `200` Šifra stanja. Kada je potrebno za pokretanje, vratit će na `202` Šifra stanja.

### <a name="callback-using-rest-apis"></a>Povratnog pomoću REST API-ji
Services da biste uputili poziv na logike aplikacije krajnjoj točki pokretanje tijeka rada. Dodatne informacije potražite u članku [logike aplikacije kao callable krajnje točke](app-service-logic-connector-http.md) . Da biste pokrenuli tu vrstu logike aplikacije na zahtjev, kliknite gumb **Pokreni odmah** na naredbenoj traci. 

<!-- Shared links -->
[Portal za Azure]: https://portal.azure.com 