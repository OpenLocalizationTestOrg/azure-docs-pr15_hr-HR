<properties 
    pageTitle="Naziv izdavača i izdavatelj ključ BizTalk Services | Microsoft Azure" 
    description="Saznajte kako dohvatiti naziv izdavača i izdavatelj ključ za servis Bus ili Access kontrola (ACS) BizTalk Services. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk servisa: Naziv izdavača i izdavatelj ključ

Servisa Azure BizTalk koristi naziv izdavača Bus usluge i ključ izdavač i naziv izdavača kontrole programa Access i izdavatelj ključ. Konkretno:

Zadatak | Koji su naziv izdavača i izdavatelj ključ
--- | ---
Implementacija aplikacije iz Visual Studio | Naziv izdavača kontrole programa Access i izdavatelj ključ
Konfiguriranje na portalu servisa Azure BizTalk | Naziv izdavača kontrole programa Access i izdavatelj ključ
Stvaranje LOB preusmjeravanje sa servisima prilagodnik BizTalk u Visual Studio | Naziv izdavača Bus servisa i izdavatelj ključ

U ovoj se temi navedeni koraci za dohvaćanje naziv izdavača i izdavatelj ključ. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Naziv izdavača kontrole programa Access i izdavatelj ključ
Naziv izdavača kontrole programa Access i izdavatelj ključ koriste sljedeće:

- Azure BizTalk servisnoj aplikaciji stvorene u Visual Studio: za uspješno uvođenje BizTalk servisnoj aplikaciji u Visual Studio Azure, unesete naziv izdavača kontrole programa Access i izdavatelj ključ. 
- Azure Portal BizTalk usluge: Kada stvaranje BizTalk servisa i otvorite Portal servisa BizTalk, naziv izdavača kontrole programa Access i izdavatelj ključ automatski registrirane za vaše implementacije sadrži jednake vrijednosti za kontrolu pristupa.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Kopiranje i lijepljenje naziv izdavača kontrole programa Access i izdavatelj ključ

1. Prijavite se na [portal za Azure klasični](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. U lijevom navigacijskom oknu odaberite **BizTalk Services**.
3. Odaberite servis za BizTalk. 
4. Odaberite **Podatke o vezi** na programskoj traci. Prostor naziva kontrole programa Access, zadani izdavač (naziv izdavača) i zadani ključ (izdavač ključ) navedene su i mogu biti kopiran i zalijepljen.  

Sažetak:  
Naziv izdavača = zadani izdavač  
Izdavač ključ = zadani ključ


Možete odabrati i **Otvaranje Portal za upravljanje ACS** da biste dobili vrijednosti za kontrolu pristupa:

1. Prijavite se na [portal za Azure klasični](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. U lijevom navigacijskom oknu odaberite **BizTalk Services**.
3. Odaberite servis za BizTalk.
4. Odaberite gumb podatke o vezi i odaberite **Otvori Portal za upravljanje ACS**.
5. Na portalu u odjeljku **Postavke servisa**, odaberite **Identiteta servisa**. Prikazat će se vaš identitet servisa koju vrijednost naziv izdavača kontrole programa Access. Odaberite vezu identiteta servisa da biste vidjeli lozinku vrijednost ključa izdavač. Vrijednosti se mogu kopirati.<br/><br/>
Na primjer, u **Identiteta servisa**vidjeti "vlasnik". "Vlasnik" je naziv izdavača kontrole za Access. Kada kliknete vezu "vlasnik", vidjet ćete **lozinku**. Kada kliknete vezu "Lozinku", vidjet ćete vrijednost. Ova vrijednost lozinke je izdavač ključ kontrole programa Access.  

Sažetak:  
Naziv izdavača = naziv identiteta servisa  
Izdavač ključ = vrijednost lozinke

U lijevom navigacijskom oknu, možete odabrati i **Servisa Active Directory** za dohvaćanje vrijednosti za kontrolu pristupa. 

> [AZURE.IMPORTANT]Prilikom stvaranja programa Access prostora za naziv kontrole pomoću **Servisa Active Directory**, identitet servisne **ne** stvara se automatski. Kada Dodjela BizTalk servis programa Access kontrola prostor naziva, identitet servisne pod nazivom "" (izdavač ime vlasnika), lozinka (izdavač ključ), a simetričnu ključ se automatski stvaraju.<br /> 
[Kako: korištenje servisa za upravljanje ACS za konfiguriranje identiteta servisa](http://go.microsoft.com/fwlink/p/?LinkID=303942) navedene su dodatne informacije na identiteta servisa za kontrolu pristupa.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Naziv izdavača Bus servisa i izdavatelj ključ
Naziv izdavača Bus servisa i izdavatelj ključ koriste BizTalk prilagodnik Services. U projektu BizTalk Services u Visual Studio, koristite BizTalk prilagodnik servisa za povezivanje s lokalnim sustavom za redak specifični za poslovanje (LOB). Da biste povezali, stvorite preusmjeravanja LOB i unesite pojedinosti LOB sustav. Kada to učinite, također unijeti naziv izdavača Bus servisa i izdavatelj ključ.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Dohvaćanje naziv izdavača Bus servisa i izdavatelj ključ

1. Prijavite se na [portal za Azure klasični](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. U lijevom navigacijskom oknu odaberite **Bus servisa**.
3. Odaberite vaš prostor naziva. Na programskoj traci odaberite **Podatke o vezi**. Prikazat će se **Zadana izdavač** (naziv izdavača) i **Zadani ključ** (izdavač ključ). Vrijednosti se mogu kopirati.  

Sažetak:  
Naziv izdavača = zadani izdavač  
Izdavač ključ = zadani ključ

## <a name="next"></a>Sljedeći
Dodatne teme servisa Azure BizTalk:

-  [Instaliranje servisa Azure BizTalk SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Vodiči za: Servisa Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Kako pokrenuti pomoću Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Vidi također
-  [Kako: pomoću servisa za upravljanje ACS konfigurirajte identiteta servisa](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [Servisi za BizTalk: Za razvojne inženjere, Basic, standardna i Premium izdanja grafikona](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk servisa: Azure pomoću portala za klasični dodjele resursa](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [Servisi za BizTalk: Dodjeljivanje Status grafikona](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk servisa: Kartica nadzorne ploče, Monitor i skaliranje](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk servisa: Sigurnosno kopiranje i vraćanje](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk usluge: ograničavanje](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
