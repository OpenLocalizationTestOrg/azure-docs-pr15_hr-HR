<properties 
    pageTitle="Dodatne informacije o tvrtki paket za integraciju dekodiranje AS2 poruke Connctor | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
    description="Saznajte kako koristiti partnere s aplikacijama paket Enterprise integracije i logika" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-as2-message"></a>Početak rada s dekodiranje AS2 poruke

Povezivanje s dekodiranje AS2 poruku da biste uspostavili sigurnosti i pouzdanosti prilikom prijenosa poruke. Pruža digitalno potpisivanje dešifriranje i acknowledgements putem poruka razmještaja obavijesti (MDN).

## <a name="create-the-connection"></a>Stvaranje veze

### <a name="prerequisites"></a>Preduvjeti

* Račun Azure; možete stvoriti [pomoću računa](https://azure.microsoft.com/free)

* Za korištenje dekodiranje AS2 poruke connector potreban je račun za integraciju. Podrobnije informirali o tome kako stvoriti [Račun integracije](./app-service-logic-enterprise-integration-create-integration-account.md), [partnere](./app-service-logic-enterprise-integration-partners.md) i [AS2 ugovor](./app-service-logic-enterprise-integration-as2.md)

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Povezivanje s dekodiranje AS2 poruke pomoću sljedećih koraka:

1. [Stvaranje aplikacije za logiku](./app-service-logic-create-a-logic-app.md) navedeni primjer.

2. Ovaj poveznik ne sadrži sve okidača. Da biste pokrenuli aplikaciju logike, kao što je zahtjev za pokretanje pomoću drugih okidača.  U dizajneru logike aplikaciju, dodajte okidač i dodali akciju.  Odaberite Prikaži Microsoft upravljani API-ji padajući popis, a zatim unesite "AS2" u okvir za pretraživanje.  Odaberite AS2 – dekodiranje AS2 poruke

    ![Pretraživanje AS2](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Ako već niste stvorili sve veze s računom za integraciju, Zatraži pojedinosti veze

    ![Stvaranje veze za integraciju](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Unesite detalje Integracija računa.  Svojstva zvjezdicom su potrebni

  	| Svojstvo   | Pojedinosti |
  	| --------   | ------- |
  	| Naziv veze *    | Unesite naziv neke za vezu |
  	| Integracija račun * | Unesite naziv računa za integraciju. Može li se račun integracije i logike aplikacije na istom mjestu Azure |

    Kada se dovrši, pojedinosti veze izgleda otprilike ovako

    ![Integracija veze](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Odaberite **Stvaranje**
    
6. Obratite pozornost na to je stvorena veza.  Sada nastavite s koracima u svojoj aplikaciji logike

    ![Integracija veze stvorili](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. Odaberite tijela i zaglavlja s izlaze zahtjev

    ![Navedite obavezna polja](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>Dekodiranje AS2 čini sljedeće

* Obrađuje AS2/HTTP zaglavlja
* Provjerava potpis (Ako je konfiguriran)
* Dešifrira poruke (Ako je konfiguriran)
* Dekomprimira poruku (Ako je konfiguriran)
* Usklađuje primljene MDN s izvorne odlazne poruke
* Ažuriranja i korelaciji zapisa u bazi podataka koje nisu priznavanje
* Piše zapise za izvješćivanje o pogreškama AS2 statusa
* Sadržaj tereta izlaz su base64 kodiranja
* Određuje hoće li se MDN je potrebno, i li na MDN mora biti sinkrono ili asinkronog ovisno o konfiguraciji u ugovoru AS2
* Stvara sinkrono ili asinkronog MDN (koja se temelji na ugovor o konfiguracijama)
* Postavlja svojstva i korelacije tokena na na MDN

##<a name="try-it-for-yourself"></a>Isprobajte sami

Zašto ne isprobajte sami. Kliknite [ovdje](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) da biste implementacija aplikacije potpuno radu logike vlastite korištenja značajke AS2 logike aplikacije 

## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o paket Enterprise Integracija") 