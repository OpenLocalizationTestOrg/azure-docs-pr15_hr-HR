<properties 
    pageTitle="Stvaranje rješenja B2B paket Enterprise Integracija | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
    description="Informirajte se o primanju podataka pomoću značajke B2B Integracija paket Enterprise" 
    services="logic-apps" 
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

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>Informirajte se o primanju podataka pomoću značajke B2B Integracija paket Enterprise#

## <a name="overview"></a>Pregled ##

Dokument je dio paketa za integraciju Enterprise logike aplikacije. Pogledajte pregled da biste saznali više o [mogućnostima Integracija paket Enterprise](./app-service-logic-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Preduvjeti ##

Da biste koristili u AS2 i X12 akcije potrebno je račun za integraciju Enterprise

[Kako stvoriti račun za integraciju Enterprise](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>Kako koristiti logike aplikacije B2B poveznika ##

Nakon što ste stvorili račun integracije i dodali partnera i ugovore o da biste ga spremni ste za stvaranje logike aplikacije koji implementira tijek rada za tvrtke za tvrtke (B2B).

U ovom walkthru vidjet ćete kako koristiti u AS2 i X12 akcije koje će stvoriti aplikaciju za tvrtke za poslovne logike koja prima podatke iz poslovnih partnera.

1. Stvorite novi logike aplikacije i [povezati ga na račun za integraciju](./app-service-logic-enterprise-integration-accounts.md).  
2. Dodavanje okidač **primitku zahtjeva – kada an HTTP zahtjev** logike aplikacije  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. Dodajte akciju **Dekodiranje AS2** tako da prvo odabir **dodali akciju**  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. U okvir za pretraživanje unesite riječ **as2** da bi se sve akcije u onu koju želite koristiti za filtriranje  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. Odaberite akciju **AS2 - dekodiranje AS2 poruke**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. Kao što je prikazano, dodajte **tijelo** će potrajati kao ulaz. U ovom primjeru odaberite tijelu zahtjeva za HTTP koja ga je pokrenula logike aplikacije. Umjesto toga možete unijeti izraz za unos zaglavlja u polje**ZAGLAVLJA** :

    @triggerOutputs()['headers']

8. Dodavanje **zaglavlja** koji su potrebni za AS2. Te će biti HTTP zahtjev zaglavlja. U ovom primjeru odaberite zaglavlja HTTP zahtjev koja ga je pokrenula logike aplikacije.
9. Sada dodajte akciju dekodiranja X12 poruke tako da ponovno odaberete **Dodaj akciju**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. U okvir za pretraživanje unesite riječ **x12** da bi se sve akcije u onu koju želite koristiti za filtriranje  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. Odaberite na **X12-dekodiranje X12 poruke** akcije da biste dodali aplikaciju logike  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. Sada morate navesti ulaza u ovu akciju koji će biti izlaz akciju AS2 iznad. Sadržaj stvarne poruke se JSON objekta i kodira base64. Stoga morate navesti izraz kao što je unos pa unesite sljedeći izraz u polju unos **X12 PLOŠNA DEKODIRANJA prima PORUKE za datoteke**  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. Ovaj korak će dekodiranje X12 podataka primljenih od poslovnih partnera i će izlaz broj stavki u JSON objekta. Da biste omogućili partnera znati potvrde podatke možete poslati natrag odgovor koji sadrži na AS2 poruke razmještaja obavijesti (MDN) u akciju HTTP odgovor  
14. Dodajte akciju **odgovor** tako da odaberete **Dodaj akciju**   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. U okvir za pretraživanje unesite riječ **odgovor** da bi se sve akcije u onu koju želite koristiti za filtriranje  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. Odaberite akciju **odgovor** da biste ga dodali  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. Postavljanje polja **tijelo** odgovora pomoću sljedeći izraz za pristup na MDN iz izlaza akciju **dekodiranja X12 poruke**  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. Spremanje rezultata rada  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

Sada ste završili postavljanje B2B logike aplikacije. U aplikaciji stvarnom svijetu, trebali biste spremiti dešifrirani X12 podataka u LOB spremištu aplikacije ili podatke. Jednostavno možete dodati dodatne akcije za to ili pisanje prilagođene API-ji za povezivanje s LOB aplikacija i pomoću tih API-ji aplikaciji logike.

## <a name="features-and-use-cases"></a>Značajke i korištenje slučajeva ##

- U AS2 i X12 dekodiranje i kodiranje akcije omogućuju vam primaju podatke i slanje podataka u trgovina partnera putem standardnih industrijskih protokola korištenje logike aplikacija  
- Možete koristiti AS2 i X12 sa ili bez međusobno razmjene podataka poslovnih partneri po potrebi
- Akcije B2B olakšavaju stvaranje partnera i ugovore o računu za integraciju i korištenje ih poslovne inteligencije u aplikaciji logike  
- Produženjem logike aplikacije s druge akcije možete slati i primati podatke iz druge aplikacije i servise kao što su SalesForce  

## <a name="learn-more"></a>uči više ##

[Dodatne informacije o Integracija paket Enterprise](./app-service-logic-enterprise-integration-overview.md)  