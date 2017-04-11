<properties 
   pageTitle="Stvaranje B2B postupka u aplikacije servisa za Azure | Microsoft Azure" 
   description="Pregled stvaranja tvrtke poslovni proces" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>Stvaranje B2B postupka

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Scenarij tvrtke 
Web-mjesto tvrtke Contoso i u okvir za Northwind su dvije poslovni partneri. Contoso (prodavača) šalje narudžbenice Northwind (dobavljača) putem industrijskih razine prijenosa kao što su AS2. Northwind pohranjuje sve dolazne narudžbe njezinu za pohranu u Oblaku. Narudžbenica su XML poruka između tih dvaju partnera. Kada poruka se sprema u Northwind mjesta za pohranu u oblaku Northwind, internog procesa redoslijedom od tog trenutka rukovati na.
 
Cilj ovog praktičnog vodiča je da biste odredili način na koji možete Northwind uspostaviti poslovni proces putem koji može primati poruke (narudžbenice u XML-u) od partnera Contoso putem AS2 i zatim zadržava u njegov za pohranu u Oblaku.


## <a name="capabilities-demonstrated"></a>Mogućnosti planirati 
Pomoću ovog praktičnog vodiča pomaže demonstracije sljedeće mogućnosti: 

- **Poruka prijevoza**: prodavača i dobavljača može biti različite platforme, ali se i dalje možete postići komunikacija među njima. U ovom ćete praktičnom vodiču oni su komunikaciju putem AS2 (primjenjivošću kako bi izjava 2). AS2 je popularan način prijenosa podataka između poslovnih partnere iz tvrtke za poslovnu komunikaciju.
- **Postojanost podataka**: kada poruku zaprimljeni putem AS2, a zatim Northwind želi zadržava prije daljnje obrade. Ga pomoću poveznika zadržava poruke u njegov za pohranu u Oblaku. U ovom ćete praktičnom vodiču Azure blob-Ova je u tijeku leveraged kao za pohranu u oblaku za Northwind.
- **Stvaranje poslovnog procesa**: U tijeku, aplikacije više API može biti od zajedno da biste postigli rezultat za tvrtke, kao što je prikazano ovdje.


## <a name="before-you-begin"></a>Prije početka
Pomoću ovog praktičnog vodiča pretpostavlja da ste osnovni razumijevanje Azure aplikacije servisa, Saznajte kako stvoriti API aplikacije i spojite zajedno s tijek.


## <a name="steps-to-achieve-the-business-scenario"></a>Korake da biste postigli scenarij tvrtke
**Stvaranje i konfiguriranje potrebna aplikacije API-JA**

1. Stvaranje instance komponente **Azure spremište blobova platforme poveznik**. Potreban je vjerodajnice za račun za Azure prostora za pohranu. Provjerite jeste li spremni prije nego što počnete stvaranje to.
2. Stvaranje instance komponente **BizTalk trgovina upravljanje partnera**. Potreban je prazna baza podataka SQL funkciju. Provjerite je li spremni prije nego što počnete stvaranje to.
3. Stvaranje instance komponente **AS2 poveznik**. Za tu radnju i prazne baze podataka SQL funkciju. Provjerite je li spremni prije nego što počnete stvaranje to. Osim toga, ako želite arhivirati poruke kao dio AS2 obrade možda unesite vjerodajnice da biste je blobova platforme Azure prilikom njegova stvaranja.
4. Konfiguriranje servisa za TPM (trgovina partnera za upravljanje) koji je stvoren:  
    1. Pronađite instanca servisa TPM koji je stvoren kao dio gore navedene korake.
    2. Pomoću mogućnosti za **partnere** u odjeljku *komponente* **Dodavanje** novog partnera pod nazivom **Contoso** i njezinu profilu dodajte potrebne AS2 identitet.
    3. Pomoću mogućnosti za **partnere** u odjeljku *komponente* **Dodavanje** novog partnera pod nazivom **Northwind** i njezinu profilu dodajte potrebne AS2 identitet.
    4. Pomoću mogućnosti **ugovore o** *komponenti* za **Dodavanje** nove AS2 ugovor između Northwind i Contoso. Northwind bit će ovdje glavnom računalu partnera i Contoso bit će partnera za goste. Sukladno situaciji konfiguriranje potpisivanje, šifriranje, sažimanja i acknowledgements tijekom stvaranja ovog ugovora. U slučaju da potvrda morate koristiti, oni moguće ih je prenijeti putem mogućnosti **potvrde** pri pregledavanju servis TPM koja je stvorena.


## <a name="create-a-flow--business-process"></a>Stvaranje na tijek / obradi tvrtke
1. Stvorite novi tijek u kojoj je u prvom koraku AS2. Povucite i ispustite **AS2 poveznik** pa odaberite instancu već stvorili. Odaberite okidača kao funkcije:  
    ![][1]  
2. Zatim povucite i ispustite **Azure spremište blobova platforme poveznik** pa odaberite instancu već stvorili. Odaberite akciju kao funkcionalnost i unutar koji, **Prijenos Blob** kao odaberite funkciju željenog. Konfiguriranje sukladno situaciji.
3. Sada stvoriti i implementirati tijeka.


## <a name="message-processing--troubleshooting"></a>Obrada poruke i otklanjanje poteškoća
1. Vrijeme je za testirajte protok smo uveli. Pošaljite XML poruke prelomljeni u AS2 (po AS2 ugovor stvorena iznad) krajnjoj AS2 kada povučete prema AS2Connector instancu koju ste stvorili. Možda ćete morati konfiguriranje provjere autentičnosti za krajnju točku tako da je javno dostupno.
2. O izvođenja tijeka je kada povučete pregledavanje tijek i zatim korak u instancu tijeka koje imate izvršava
3. AS2 obrada informacije pronađite instancu AS2Connector koji je uključen, a zatim slijedite prolaskom u dio praćenja. Pomoću filtara koji je uključen da biste ograničili prikaz informacije koje je potrebno.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
