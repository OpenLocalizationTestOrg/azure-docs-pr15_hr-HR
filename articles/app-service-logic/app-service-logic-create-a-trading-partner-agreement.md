<properties 
   pageTitle="Stvaranje poslovnih partnerom u aplikacije servisa za Azure | Microsoft Azure" 
   description="Stvaranje trgovina ugovore partnera" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>Stvaranje poslovnih partnerom   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Partneri poslovnih su entiteti uvrštene u B2B komunikacije (tvrtke za tvrtke). Kada dva partnere uspostaviti odnos, to se naziva *ugovor*. Ugovor definirani temelji se na partnere želja komunikacije dviju da biste postigli i protokol ili prijenosa određene. Različite B2B protokoli i prijenosa podržava aplikacije servisa za Azure obuhvaćaju sljedeće:

- AS2 (primjenjivošću kako bi gubitka 2)
- EDIFACT (Sjedinjenih Američkih Država države elektroničke podataka za razmjenu za administraciju, trgovine i prijenosa (Poništavanje na EDIFACT))
- X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>Aplikacija za API BizTalk koje podržavaju B2B scenariji
Sljedeće aplikacije API omogućili ove mogućnosti pomoću obogaćenog i intuitivno sučelje na portalu za Azure:


## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk trgovina partnera Management (TPM)
- Stvaranje i upravljanje partnera, profili i identiteta
- Prostor za pohranu i upravljanje uređivanje shema
- Prostor za pohranu i upravljanje certifikata (koja se koristi u AS2 protocol)
- Stvaranje i upravljanje AS2 ugovora
- Stvaranje i upravljanje EDIFACT ugovore (obuhvaća grupnog slanja promjena na strani slanje)
- Stvaranje i upravljanje X12 ugovore (obuhvaća grupnog slanja promjena na strani slanje)

![][1]


## <a name="as2-connector"></a>Poveznik za AS2
- Izvršava AS2 ugovore kako je definirano u povezanim instanci TPM API aplikacije
- Informacije za obradu/praćenje AS2 površine za otklanjanje poteškoća


## <a name="biztalk-edifact"></a>BizTalk EDIFACT
- Izvršava EDIFACT ugovore kako je definirano u povezanim instanci TPM API aplikacije
- Informacije za obradu/praćenje EDIFACT površine za otklanjanje poteškoća
- Omogućuje upravljanje stanje serije (početak i kraj) definirana sporazume EDIFACT u povezanim instanci TPM API aplikacije


## <a name="biztalk-x12"></a>BizTalk X12
- Izvršava X12 ugovore kako je definirano u povezanim instanci TPM API aplikacije 
- Površine X12 obrada/praćenje informacije za otklanjanje poteškoća
- Omogućuje upravljanje stanje serije (početak i kraj) definirana X12 sporazume u povezanim instanci TPM API aplikacije

Kao prethodno navedeni AS2 X 12 i EDIFACT API aplikacije potreban aplikacije API TPM u funkciji prema očekivanjima.


## <a name="getting-started"></a>Početak rada
Da biste stvorili kotiranja ugovore partnera:

1. Stvaranje instance komponente poveznik za **Upravljanje partnera trgovina BizTalk** . Potreban je prazna baza podataka SQL funkciju. Prije početka obavezno prazna baza podataka dostupna i jeste li spremni za korištenje.
2. Prenesite sheme i certifikati prema potrebi po ugovore. To učiniti pregledavanja instancu TPM stvorili i koraka u dio "Sheme" i/ili "Certifikati"
3. Dođite do instancu TPM stvorili i koraka u dio za **partnere**
4. Stvaranje partnerima po želji. Uređivanje profila po potrebi i dodajte potrebne identiteta
5. Sada pomoću dijela **ugovore** da biste stvorili ugovore. Kada stvorite ugovor, morate odabrati protokol koji će se koristiti. Ostale mogućnosti konfiguracije temelje se na protokol koji ste odabrali.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
