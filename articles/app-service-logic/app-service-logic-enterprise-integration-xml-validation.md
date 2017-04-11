<properties 
    pageTitle="Pregled provjere valjanosti XML u paket Enterprise Integracija | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
    description="Saznajte kako funkcionira Provjera valjanosti u aplikacijama paket Enterprise integracije i logike" 
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

# <a name="enterprise-integration-with-xml-validation"></a>Enterprise Integracija s XML provjere valjanosti

## <a name="overview"></a>Pregled
Često u scenariji B2B partnerima ugovor potrebno da biste provjerili valjanost koje su valjane poruke koje se mogu razmjenjivati između međusobno prije pisanja obrada podataka. U paket za integraciju s Enterprise poveznik za provjeru valjanosti XML možete koristiti da biste provjerili valjanost dokumente sa shemom unaprijed definirane.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Kako provjeriti valjanost dokumenta s connector XML provjere valjanosti
1. Stvaranje logike aplikacije i [povezati ga na račun za integraciju](./app-service-logic-enterprise-integration-accounts.md "Naučite povezivanje poslovnog subjekta Integracija logike aplikaciju") koja sadrži će se koristiti za provjeru valjanosti podataka XML shemu.
2. Dodavanje okidač **primitku zahtjeva – kada an HTTP zahtjev** logike aplikacije  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. Dodavanje **Provjere valjanosti XML** akcije tako da prvo odabir **dodali akciju**  
4. U okvir za pretraživanje unesite *xml* da bi se sve akcije u onu koju želite koristiti za filtriranje 
5. Odaberite **XML provjere valjanosti**     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Odaberite tekstni okvir **sadržaja**  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Odaberite oznaku tijelo kao sadržaj koji će se provjeriti.   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Odaberite okvir s popisom **Naziv sheme** i odabrali sheme koju želite koristiti za provjeru valjanosti unos *sadržaja* iznad     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Spremanje rezultata rada  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

Sada ste završili postavljanje poveznik za provjeru valjanosti. U aplikaciji stvarnog svijeta, trebali biste spremiti provjerene podatke u aplikaciju LOB kao što su SalesForce. Jednostavno možete dodati akciju da biste poslali izlaznu provjeru Salesforce. 

Sada možete testirati i radnju provjere valjanosti tako da zahtjev HTTP krajnjoj.  

## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o paket Enterprise Integracija")   