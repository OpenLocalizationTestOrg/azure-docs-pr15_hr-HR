<properties 
   pageTitle="Konfiguriranje i korištenje prostora za pohranu Emulator pomoću Visual Studio | Microsoft Azure"
   description="Konfiguriranje i korištenje prostora za pohranu Emulator pomoću Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Konfiguriranje i korištenje prostora za pohranu Emulator pomoću Visual Studio

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Pregled
Platforme Azure SDK sadrži emulator prostora za pohranu, utility koji simulira Blob, reda čekanja i tablice za pohranu servisa dostupni servisu Azure na vašem računalu lokalne razvoj. Ako ste stvaranje servis u oblaku koji uključuje servise za Azure pohranu ili bilo koji vanjski program koji poziva servise za pohranu za pisanje, možete testirati kod lokalno u odnosu na emulator prostora za pohranu. Alati za Azure za Microsoft Visual Studio integrirati upravljanje emulator prostora za pohranu u Visual Studio. Alati za Azure pokretanje baze podataka za pohranu emulator prilikom prvog korištenja, pokrenuti servis za pohranu emulator kada pokrenete ili ispravljanje pogrešaka u kodu iz Visual Studio, a omogućuje pristup samo za čitanje podataka emulator prostora za pohranu putem prostora za pohranu Explorer Azure.

Detaljne informacije o emulator pohrane, uključujući sistemski preduvjeti i prilagođena konfiguracija upute, potražite u članku [korištenje prostora za pohranu Emulator Azure za razvoj i testiranje](./storage/storage-use-emulator.md).

>[AZURE.NOTE] Postoje neke razlike u funkcijama između emulator simulacije prostora za pohranu i servise za Azure pohranu. [Razlike između Emulator u prostor za pohranu i servise za pohranu Azure](./storage/storage-use-emulator.md) potražite u dokumentaciji Azure SDK informacije o određenim razlike.

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Konfiguriranje niza za povezivanje za emulator prostora za pohranu

Da biste pristupili emulator prostora za pohranu iz koda unutar uloge, želite konfigurirati niza za povezivanje koji ukazuje na emulator prostora za pohranu i koje možete kasnije promijeniti tako da pokazuje na račun za Azure prostora za pohranu. Niz za povezivanje je postavka konfiguracije koji vaša uloga može čitati prilikom izvođenja da biste se povezali s računom za pohranu. Dodatne informacije o stvaranju nizu za povezivanje potražite u članku [Konfiguriranje aplikacije Azure](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

>[AZURE.NOTE] Referenca na račun za pohranu emulator iz koda možete vratiti korištenjem svojstva **DevelopmentStorageAccount** . Taj se način radi ispravno ako želite pristupiti emulator prostora za pohranu u kodu, ali ako namjeravate objaviti aplikaciju Azure, morat ćete stvaranje niza za povezivanje za pristup računu Azure prostora za pohranu i izmjenu svoj kod tako da koristi taj niz za povezivanje prije objavljivanja. Ako se prebacujete između emulator račun za pohranu i račun za Azure prostora za pohranu često, niza za povezivanje će pojednostavili postupak.

## <a name="initializing-and-running-the-storage-emulator"></a>Pokretanje i pokretanje emulator prostora za pohranu

Možete odrediti da kada pokrenete ili uslugu u Visual Studio za ispravljanje pogrešaka Visual Studio automatski pokreće emulator prostora za pohranu. U pregledniku rješenja, otvorite izbornik prečaca za **Azure** projekta i odaberite **Svojstva**. Na kartici **razvoj** na popisu **Početak Emulator Azure prostora za pohranu** odaberite **True** (Ako već nije postavljena na tu vrijednost).

Prvo pokretanje ili na servisu iz Visual Studio za ispravljanje pogrešaka za pohranu emulator pokreće proces Inicijalizacija. Ovaj postupak rezervira lokalne priključke za emulator prostora za pohranu i stvara emulator baza podataka za pohranu. Kada završi, taj postupak morati ponovno pokrenuti osim ako ih ne briše se emulator baza podataka za pohranu.

>[AZURE.NOTE] Počevši od lipnja 2012 izdanje alata za Azure, emulator prostora za pohranu, po zadanom se pokreće, u SQL Express LocalDB. Iz prethodnih izdanja alata za Azure emulator prostora za pohranu pokreće protiv zadane instance sustava SQL Express 2005 ili 2008, koje morate instalirati prije nego što možete instalirati Azure SDK. Možete i pokrenuti emulator prostora za pohranu protiv imenovani instancu sustava SQL Express ili imenovanog ili zadane instance sustava Microsoft SQL Server. Ako morate konfigurirati emulator prostora za pohranu za koje su pokrenute instance osim zadane instance, potražite u članku [korištenje prostora za pohranu Emulator Azure za razvoj i testiranje](./storage/storage-use-emulator.md).

Prostor za pohranu emulator nudi korisničko sučelje za prikaz statusa servisa lokalno spremište i da biste započeli, zaustaviti, a zatim ih vratiti. Kada se pokrene emulator servis za pohranu prikaz korisničkog sučelja ili pokrenuti ili zaustaviti servis klikom desnom tipkom miša ikona u području obavijesti za Azure Emulator Microsoft na programskoj traci sustava Windows.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Prikaz podataka emulator prostora za pohranu u programu Explorer poslužitelja

Čvor Azure prostora za pohranu u programu Explorer Server omogućuje prikaz podataka i promijeniti postavke za blob i tablicu podataka u svoje račune za pohranu, uključujući emulator prostora za pohranu. Dodatne informacije potražite u [pregledavanja i upravljanje resursa za pohranu pomoću programa Explorer poslužitelja](https://msdn.microsoft.com/library/azure/ff683677.aspx) .
