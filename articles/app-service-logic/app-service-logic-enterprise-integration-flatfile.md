<properties
    pageTitle="Saznajte kako šifrirati ili dekodiranje paušalni datoteka pomoću aplikacije paket Enterprise integracije i logike | Aplikacije servisa za Microsoft Azure | Microsoft Azure"
    description="Pomoću značajke aplikacije paket Enterprise integracije i logika kodiranje ili dekodiranje paušalni datoteke"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-flat-files"></a>Enterprise Integracija s nehijerarhijskom datoteka

## <a name="overview"></a>Pregled

Trebali biste kodiranje XML sadržaja prije slanja partneru u scenariju tvrtke za tvrtke (B2B). U aplikaciji logike stvoren pomoću značajke aplikacije logike aplikacije servisa Azure, možete koristiti kodiranje poveznik za nehijerarhijsku datoteku da biste to učinili. Logika aplikacije koje ste stvorili možete dobiti njegov XML sadržaja iz raznih izvora, uključujući s okidača HTTP zahtjev, iz neke druge aplikacije ili čak i iz jedne od mnogo [poveznike](../connectors/apis-list.md). Dodatne informacije o aplikacijama logike potražite u [dokumentaciji logike aplikacije](./app-service-logic-what-are-logic-apps.md "Dodatne informacije o aplikacijama logike").  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>Kako stvoriti nehijerarhijskom datotekom kodiranje poveznika

Slijedite ove korake da biste dodali s nehijerarhijskom datotekom kodiranje poveznika logike aplikacije.

1. Stvorite logiku aplikacije i [Povezivanje s računom za integraciju](./app-service-logic-enterprise-integration-accounts.md "Naučite povezivanje poslovnog subjekta Integracija logike aplikaciju"). Taj račun sadrži shemi koju ćete koristiti za kodiranje XML podataka.  
2. Dodajte okidač **primitku zahtjeva – kada an HTTP zahtjev** logike aplikacije.  
![Snimka zaslona okidača za odabir](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. Dodajte nehijerarhijskom datotekom kodiranje akcija na sljedeći način:

    na. Odaberite znak **plus** .

    b. Odaberite **Dodaj akciju** vezu (pojavljuje se nakon što odaberete znak plus).

    c. U okvir za pretraživanje unesite *paušalni* da biste filtrirali sve akcije u onu koju želite koristiti.

    d. Odaberite mogućnost **Paušalni kodiranje datoteka** s popisa.   
![Snimka zaslona s Nehijerarhijskom datoteke kodiranje mogućnost](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. U dijaloškom okviru **Paušalni šifriranje datoteke** odaberite tekstni okvir **sadržaja** .  
![Snimka zaslona sadržaja tekstnog okvira](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Odaberite oznaku tijelo kao sadržaj koji želite šifrirati. Body oznake će popunjavanje sadržaja polja.     
![Snimka zaslona body oznake](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Potvrdite okvir **Naziv sheme** popisa pa odaberite sheme koju želite koristiti za kodiranje unos sadržaja.    
![Snimka zaslona naziv sheme okvir s popisom](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Spremite promjene.   
![Snimka zaslona s spremanje ikona](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

Sada ste završili postavljanje vaše kodiranja poveznik za nehijerarhijsku datoteku. U aplikaciji stvarnom svijetu, trebali biste spremiti kodiranih podatke u retku poslovnom aplikacijom, kao što su Salesforce. Ili možete poslati partnera kodiranih podatke na trgovina. Jednostavno možete dodati akciju slanja izlaz kodiranja akciju Salesforce ili partnera poslovnih pomoću bilo koju od drugih poveznika navedene.

Sada možete testirati i vaše poveznik za upućivanje zahtjeva za krajnju točku HTTP, a uključujući XML sadržaja u tijelu zahtjeva za.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>Kako stvoriti nehijerarhijsku datoteku dekodiranje poveznika

>[AZURE.NOTE] Da biste dovršili ove korake, morate koristiti datoteku sheme već prenijeli Integracija račun.

1. Dodajte okidač **primitku zahtjeva – kada an HTTP zahtjev** logike aplikacije.  
![Snimka zaslona okidača za odabir](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. Dodajte nehijerarhijskom datotekom dekodiranje akcija na sljedeći način:

    na. Odaberite znak **plus** .

    b. Odaberite **Dodaj akciju** vezu (pojavljuje se nakon što odaberete znak plus).

    c. U okvir za pretraživanje unesite *paušalni* da biste filtrirali sve akcije u onu koju želite koristiti.

    d. Odaberite mogućnost **Paušalni dekodiranje datoteka** s popisa.   
![Snimka zaslona s Nehijerarhijskom datoteke dekodiranje mogućnost](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- Odaberite kontrolu **sadržaja** . To daje popis sadržaja iz starije koraka koje možete koristiti kao sadržaja za dekodiranje. Obavijest o dostupnoj u *tijelo* iz dolazni zahtjev HTTP će se koristiti kao sadržaja za dekodiranje. Možete unijeti i sadržaj za dekodiranje izravno u kontrolu **sadržaja** .     
- Odaberite *Body* oznake. Obratite pozornost na to body oznake sada se nalazi u kontrolu **sadržaja** .
- Odaberite naziv sheme koju želite koristiti za dekodiranje sadržaja. Sljedeća slika pokazuje da je *OrderFile* naziv odabrana shema. Ovaj naziv sheme imali preneseni račun za integraciju prethodno.

 ![Snimka zaslona s Nehijerarhijskom datoteke dekodiranje dijaloškog okvira](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- Spremite promjene.  
![Snimka zaslona s spremanje ikona](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

Sada ste završili postavljanje dekodiranje poveznik nehijerarhijsku datoteku. U aplikaciji stvarnom svijetu, trebali biste spremiti dešifrirani podatke u retku poslovnih aplikacija kao što su Salesforce. Jednostavno možete dodati akciju izlaz dekodiranja akcija slanje Salesforce.

Sada možete testirati i vaše poveznik za upućivanje zahtjeva za krajnju točku HTTP i uključujući XML sadržaja koju želite dekodiranje u tijelu zahtjeva za.  

## <a name="next-steps"></a>Daljnji koraci
- [Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o Enterprise Integracija s programom paketa").  
