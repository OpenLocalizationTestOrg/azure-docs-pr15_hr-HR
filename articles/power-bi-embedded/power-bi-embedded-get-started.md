<properties
   pageTitle="Uvod u Microsoft Power BI ugrađeni"
   description="Power BI ugrađene, dodajte interaktivnih izvješća servisa Power BI u poslovne inteligencije aplikacije"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Uvod u Microsoft Power BI ugrađeni

**Power BI ugrađeni** je Azure servis koji omogućuje razvojnim inženjerima da biste dodali interaktivnih izvješća servisa Power BI u svoje aplikacije. **Power BI ugrađene** surađuje postojeće aplikacije bez potrebe za promijeni dizajn ili promjena korisnika način prijave.

Resursi za **Microsoft Power BI ugrađene** su dodjeli kroz [Azure ARM API -ji](https://msdn.microsoft.com/library/mt712306.aspx). U ovom slučaju resursa koje dodijelite je **Prikupljanje radnog prostora za Power BI**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>Stvaranje zbirke radnog prostora
**Radni prostor zbirke** je web-mjesto najviše razine Azure resursa i u okvir za spremnik za sadržaj koji će biti uložen u aplikaciji. **Radni prostor zbirke** moguće stvoriti na dva načina:

   -    Ručno pomoću portala za Azure
   -    Programski pomoću Manager(ARM) API-ji resursa Azure

Pogledajmo prođite kroz korake za stvaranje **Radnog prostora zbirke** pomoću portala za Azure.

   1.   Otvaranje i prijavite se na **Portal za Azure**: [http://portal.azure.com](http://portal.azure.com).

   2.   Kliknite **+ Novo** na vrhu ploče.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   U odjeljku **podataka + analize** kliknite **Power BI ugrađeni**.
   4.   Na **Stvaranje plohu**unesite potrebne informacije. **Određivanje cijena**, potražite u članku [Power BI ugrađene cijene](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Kliknite **Stvori**.

**Radni prostor zbirke** se trenutak za dodjelu resursa. Kada završi, koje ćete se otvorila **Plohu zbirke radnog prostora**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

**Stvaranje plohu** sadrži podatke koje morate nazvati API-ji za stvaranje radnih prostora i implementirati sadržaj.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Prikaz Power BI API Access tipki

Jedan od najvažnijih dijelove podatke koji su potrebni za pozivanje REST API-ji BI Power su **Pristupnih tipki**. Oni se koriste za stvaranje **tokena aplikacije** koje se koriste za provjeru autentičnosti zahtjeva za API-JA. Da biste pogledali **Pristupnih tipki**, kliknite **Pristupnih tipki** na **Plohu postavke**. Dodatne informacije o **aplikaciji tokeni**potražite u članku [Authenticating i dopuštanja uz Power BI ugrađeni](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Morat ćete "obavijest da imate dvije tipke.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Kopirajte ove tipke i sprema ih sigurno u aplikaciji. Vrlo je važno da obrađuje ove tipke kao što to činite lozinkom, jer će vam omogućuju pristup svim sadržajima u sklopu **Zbirke radnog prostora**.

Dok navedene su dvije tipke, u određenom trenutku potreban je samo jedan ključ. Drugi ključ navedeni su tako da povremeno možete Obnovi tipke bez prekida pristup servisu.

Sad kad ste instance komponente Power BI za računala i **Pristupnih tipki**, izvješća možete uvesti vlastitu aplikacije. Prije saznat ćete kako uvesti izvješća, u sljedećem odjeljku opisuju stvaranje skupova podataka za Power BI i izvješća da biste ugradili u aplikaciju.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Stvaranje skupova podataka za Power BI i izvješća da biste ugradili u aplikaciju

Sad kad ste stvorili instance komponente Power BI za aplikaciju, a sadrži **Tipke za pristup**, morat ćete stvoriti skupova podataka za Power BI i izvješća koji želite ugraditi. Pomoću značajke **Power BI Desktop**moguće je stvoriti skupova podataka i izvješća. Možete preuzeti [Power BI Desktop za besplatno](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). Ili za brzo pokretanje, možete preuzeti na [maloprodaja analizu uzorka PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547). Da biste saznali više o tome kako koristiti **Power BI Desktop**, potražite u članku [Uvod u Power BI Desktop](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

Uz **Power BI Desktop**povezati s izvorom podataka uvozom kopiju podataka u **Power BI Desktop** ili povezivanje izravno u izvoru podataka koristeći **DirectQuery**.

Ovdje su razlike između korištenja **Uvoz** i **DirectQuery**.

|Uvoz | DirectQuery
|---|---
|Tablice, stupce *i podataka iz* uvezene ili kopirati u **Power BI Desktop**. Tijekom rada s vizualizacijama **Power BI Desktop** upiti kopiju podataka. Da biste vidjeli sve promjene koje je došlo u podatke u podlozi, morate osvježiti ili uvoz dovršen, trenutni skup podataka ponovno.|Uvezeni ili kopirali u **Power BI Desktop** *tablica i stupaca* . Tijekom rada s vizualizacijama **Power BI Desktop** upiti temeljnom izvoru podataka, što znači da uvijek gledate trenutne podatke.

Dodatne informacije o povezivanju s izvorom podataka potražite u članku [Povezivanje s izvorom podataka](power-bi-embedded-connect-datasource.md).

Nakon što spremite svoj rad u **Power BI Desktop**, stvara se PBIX datoteke. Datoteka sadrži izvješća. Osim toga, ako uvezete podatke u PBIX sadrži cijeli skup podataka ili ako koristite **DirectQuery**, u PBIX sadrži samo shemu skup podataka. Programski implementirati na PBIX u radnom prostoru pomoću [Dodatka Power BI uvoz API -JA](https://msdn.microsoft.com/library/mt711504.aspx).

> [AZURE.NOTE] **Power BI ugrađene** ima dodatne API-ji za promjenu poslužitelj i bazu podataka koja pokazuje na skup podataka i postavljanje vjerodajnica za račun servisa kojima skupu podataka će se koristiti za povezivanje s bazom podataka. Potražite u članku [SetAllConnections objavu](https://msdn.microsoft.com/library/mt711505.aspx) i [izvora podataka zakrpu pristupnika](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Daljnji koraci
U prethodnim koracima, stvorite zbirku radnog prostora i vaše ime izvješća i skup podataka. Sada je vrijeme da biste saznali kako pisanje koda za **Power BI ugrađeni**. Da biste lakše početak rada, koju smo stvorili web-aplikacijama uzorka: [Početak rada s uzorka](power-bi-embedded-get-started-sample.md). Uzorak prikazuje kako da biste:

  - Dodjela resursa za sadržaj
      - Stvaranje radnog prostora
      - Uvoz datoteke PBIX
      - Ažurirajte nizove veze i postavljanje vjerodajnica za svoje skupove podataka.

  - Sigurno ugrađivanje izvješća

## <a name="see-also"></a>Vidi također
- [Početak rada s uzorak](power-bi-embedded-get-started-sample.md)
- [Provjera autentičnosti i dopuštanja uz Power BI ugrađeni](power-bi-embedded-app-token-flow.md)
- [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
