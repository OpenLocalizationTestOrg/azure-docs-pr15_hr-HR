<properties
   pageTitle="Vodič za stvaranje podatkovnog servisa za Marketplace | Microsoft Azure"
   description="Detaljne upute kako stvoriti, potvrđivanje i implementacija podatkovnog servisa za kupnju na Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Podatkovnog servisa objavljivanje vodič za Azure Marketplace

>[AZURE.IMPORTANT] **Trenutno ne možemo više nisu za uhodavanje sve nove izdavači podatkovnog servisa. Novi dataservices će dobiti odobrene za unos.** Ako imate aplikaciju SaaS tvrtke koje želite objaviti na AppSource možete pronaći dodatne informacije [u nastavku](https://appsource.microsoft.com/partners). Ako imate programa IaaS aplikacije ili servisa za razvojne inženjere koje želite objaviti na Azure Marketplace možete pronaći dodatne informacije [u nastavku](https://azure.microsoft.com/marketplace/programs/certified/).

Nakon dovršetka postupka korak 1, [stvaranja računa i registracije](marketplace-publishing-accounts-creation-registration.md), ne možemo upute s vodičem koju kroz [Općenito nisu tehničke prirode](marketplace-publishing-pre-requisites.md) i [Tehnički preduvjeti](marketplace-publishing-data-service-creation-prerequisites.md) za ponudu podatkovnog servisa na Azure Marketplace. Sad ćemo će vas voditi kroz korake za stvaranje ponude za podatkovnog servisa za [Portal za objavljivanje] [ link-pubportal] za Azure Marketplace.

## <a name="1---login-to-the-publishing-portal"></a>1. prijave za objavljivanje Portal.

Idite na [https://publish.windowsazure.com](https://publish.windowsazure.com. )

**Da biste postigli prvi put prijavite Portal za objavljivanje, koristite isti račun s kojim je profila tvrtke Prodavač registriran u centru za razvojne inženjere.**  (Naknadno možete dodati sve zaposlenika tvrtke ko-administrator na portalu za objavljivanje).

Ako je ovo prvi Prijava na portal za objavljivanje, kliknite pločicu **Objavi podatkovne usluge** .

## <a name="2---choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2. u navigacijski izbornik na lijevoj strani odaberite **Podatkovne usluge** .

  ![Crtanje](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3---create-a-new-data-service"></a>3. Stvaranje nove podatkovnog servisa

Unesite naslov za svoje nove podatke servisa nude, a zatim kliknite na "+" na desnoj strani.

  ![Crtanje](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4---review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4. Pregledajte podređenu izbornik u odjeljku novostvorenu podatkovnog servisa u navigacijski izbornik.

Na kartici **Prikaz** kliknite i pregledajte sve potrebne korake potrebne da biste objavili ispravno podatkovnog servisa na Azure Marketplace.

> [AZURE.TIP] Uvijek možete kliknite veze na stranici "Prikaz" ili kartice na ponudu podatkovnog servisa podizbornika na lijevoj strani.

## <a name="5---create-a-new-plan"></a>5. stvorite novu tarifu.

### <a name="offers-plans-transactions"></a>Nudi tarife transakcije.

Svaki ponudu može imati više tarife, ali morate imati barem jedan (1) Plan. Pretplaćeni krajnjim korisnicima tu ponudu mogu se pretplatiti zbog nekog od Plan za ponudu. Svaki plan definira kako krajnji korisnici će moći koristiti usluge.

Trenutno trgovine Windows Azure podržava samo mjesečne pretplate transakcije koji se temelje model za podatkovne usluge, odnosno krajnjim korisnicima plaćate mjesečnu naknadu prema cijena određeni plan su se pretplatili i moći ćete zauzeti svaki broj mjeseca transakcije definira plan.

Pojedine transakcije obično se definira kao broj zapisa podatkovnog servisa koji će vratiti na temelju upita koji se šalju servisu. Zadano je 100. Broj transakcija vratio svaki upit će se broj zapisa dijeli sa 100 i zaokružen na najbliži cijeli broj.

To je servisa Azure Marketplace sloja odgovornosti praćenje (metar) broj transakcije troše svaki upit.

> [AZURE.IMPORTANT] Nastavak korištenja servisa do kraja ciklusa njihove za mjesečne pretplate bit će blokirane krajnjim korisnicima koji dostigao ograničenje transakcije tijekom mjeseca.

> Plan ili jedan od tarifa možete (ali ne morate) obuhvaćaju neograničen broj transakcije.

### <a name="create-a-plan"></a>Stvorite plan.
1. Kliknite **"+"** pokraj "Dodaj novu tarifu".

2. Odaberite jednu od mogućnosti: korištenje **neograničeno** ili **ograničeno** za ovaj plan.  Ako je ograničeno zatim unesite broj transakcije plan omogućite zauzeti tijekom mjeseca.

    ![Crtanje](media/marketplace-publishing-data-service-creation/step-5.1.png)  

    Portal za objavljivanje i predlaže "Plan identifikator", koji će koristi za komunikaciju s krajnjim korisnicima naziv plan u korisničkom Sučelju i i tržište usluga koristi za prepoznavanje Plan. Po želji možete promijeniti "Planiranje identifikator".

    > [AZURE.NOTE] "Planiranje identifikator" mora biti jedinstvena u opsega svaki ponudu. Kao što je mnogo drugih identifikatora koji se koriste u identifikator planiranje Portal za objavljivanje će se zaključati nakon prve objavljivanje i radni nećete moći promijeniti ovaj identifikator.

3. Kliknite da biste prihvatili izboru.

4. Zatim će se zatražiti nekoliko dodatnih pitanja vezana uz novostvorenu tarifa.

    ![Crtanje](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Pitanje|Višekratnik.|
|----|----|
|**Taj je Plan besplatna i dostupna svijeta razini?**|Možete stvoriti plan potpuno slobodno-od-trošak. Ako je jedini plan za ponudu – to znači da objavljujete "Besplatne nude" na tržištu. Ako je samo za jednu (mali) tarifu, it vam nudi mogućnost nudi krajnjim korisnicima da biste saznali više o servisu s relativno malim brojem transakcija mjesečno.  Ako je odgovor 'Da', zatim bez dodatnih pitanja će se zatražiti.|

> [AZURE.NOTE] Krajnji korisnici uvijek možete nadograditi plaćene tarife.

|Pitanje|Višekratnik.|
|----|----|
|**Dostupna je besplatna probna verzija?**|Možete odabrati između "Bez probnu verziju" uopće ili dati mogućnost da biste koristili Plan za "Mjesec". Da biste koristili tu mogućnost možete unijeti krajnjim korisnicima mogućnost da biste shvatili prednosti ponuda besplatno za mjesec kao što su izdavači.|

> [AZURE.IMPORTANT] Krajnji korisnici samo moći kupiti besplatnu probnu verziju ako ih imate uspostaviti plaćanja instrument – primjerice kreditne kartice, enterprise ugovor.

> Nakon mjesec besplatnu probnu verziju, trgovine Windows Azure počet će puni kupci cijenu datum pretplate, osim ako je korisnik odabrao pokrenut otkazivanje pretplate. Nema posebne obavijesti objavit ćemo zajedno s krajnjim korisnicima.

|Pitanje|Višekratnik.|
|----|----|
|**Ovaj plan zahtijeva šifru promotivne ponude za kupnju?**| Izdavači imaju mogućnost ograničiti pristup svojim tarife servisa unosom posebno koda, pod nazivom "Promocode odgovora" određenim korisnicima. Samo krajnjim korisnicima koji će imati ovaj Promocode moći da biste se pretplatili na željenu tarifu. Ako odaberete "Ne", pristajete da svima iz područja gdje ponudu je dostupna (potražite u članku [Trgovine marketinške sadržaja vodič za](marketplace-publishing-push-to-staging.md) dodatne pojedinosti) moći ćete pretplata na ovaj plan. Bez dodatnih pitanja će se zatražiti.|
|**Sakriti ovaj plan od osoba koje nema valjanih promotivnog koda?**|Ako je odgovor na pitanje prethodno "Da" izdavača ima mogućnost da biste potpuno uklonili ovaj plan prikazivao u korisničkom Sučelju na tržištu. Znači, korisnici neće vidjeti ovaj plan u stranicu s detaljima u ponudu. Krajnji korisnici koji će primiti promocode kupiti, moći ćete pretplata pomoću ovog promocode.|

## <a name="6---create-your-marketplace-marketing-content"></a>6. Stvaranje na tržištu marketinškog sadržaja
Kako se navedite potrebne informacije na karticama **Marketing, određivanje cijena, podrška i kategorija** provjerite posjetiti [Trgovine marketinške sadržaja vodič](marketplace-publishing-push-to-staging.md) koja je zajednička za sve artefakte objavljenim u članku trgovine Windows Azure.  

## <a name="7---connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7. povezati ponudu u funkcioniranju servisa (SQL Azure temelji ili web-servisa koji se temelje).

Kliknite na podizborniku **Podatkovne usluge** .

Na gornjem dijelu stranice će se zatražiti možete unijeti na ponuda **prostora za naziv**.  

  ![Crtanje](media/marketplace-publishing-data-service-creation/step-7.png)

U nastavku pitanje će definirati kako u programu Publisher će izložiti novostvoreni ponudu Azure Marketplace. (Dodatne informacije potražite u članku [Podataka servisa tehničke pripremni vodič](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Crtanje](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Servis za bazu podataka za objavljivanje**

Kliknite **bazu podataka**. Pojavit će se na sljedećem mjestu:

  ![Crtanje](media/marketplace-publishing-data-service-creation/step-7.3.png)

Da biste stvorili mapiranje CSDL za skup podataka na temelju SQL Azure DB:

  ![Crtanje](media/marketplace-publishing-data-service-creation/step-7.4.png)

Te za svaku tablicu

  ![Crtanje](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Crtanje](media/marketplace-publishing-data-service-creation/step-7.6.png)

Ako web-usluge

  ![Crtanje](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] Pročitajte [mapiranja postojeće web-usluge OData kroz CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) za detaljne upute i primjeri za stvaranje web-servisa CSDL.

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste stvorili tu ponudu podatkovnog servisa, provjerite je li dovršili upute u [Trgovine marketinške sadržaja vodič za](marketplace-publishing-push-to-staging.md) Premještanje naprijed u [testiranje podatkovnog servisa u pripremna](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Vidi također
- [Početak rada: Kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md)
- Ako vas zanima objašnjenje cjelokupan OData mapiranje postupak i svrhu, pročitajte ovaj članak [Mapiranje OData servisa podataka](marketplace-publishing-data-service-creation-odata-mapping.md) da biste pregledali definicije, strukture i upute.
- Ako vas zanima učenje razumijevanje određene čvorove i njihovim parametrima pročitajte ovaj članak [Podataka usluge OData mapiranja čvorove](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definicije i objašnjenja, primjere i koristiti velika kontekst.
- Ako vas zanima pregled primjera, pročitajte ovaj članak [Podataka usluge OData mapiranja primjere](marketplace-publishing-data-service-creation-odata-mapping-examples.md) potražite u članku uzorak koda i razumijevanje Sintaksa koda i kontekst.


[link-pubportal]:https://publish.windowsazure.com
