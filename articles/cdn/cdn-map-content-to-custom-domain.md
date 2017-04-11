<properties
     pageTitle="Upute za mapiranje Azure isporuku sadržaja (CDN) mrežni sadržaj prilagođenu domenu | Microsoft Azure"
     description="U ovoj se temi Demonstracija mapiranje CDN sadržaj prilagođene domene."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Kako mapirati prilagođene domene na krajnjoj točki sadržaja isporuke mreže (CDN)
Da biste mogli koristiti vlastiti naziv domene u URL-ovi za predmemoriranog sadržaja umjesto korištenja poddomenu azureedge.net možete mapirati prilagođenu domenu CDN krajnje.

Da biste mapirali prilagođene domene na krajnjoj točki CDN na dva načina:

1. [Stvorite CNAME zapis s registraru domene i mapirajte prilagođenu domenu i poddomenu CDN krajnja točka](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    CNAME zapis je DNS značajka koja karata domenu izvor, poput `www.contosocdn.com` ili `cdn.contoso.com`, u odredišnoj domeni. U ovom slučaju domenu izvor je prilagođenu domenu i poddomenu (poddomenu, kao što su **"www"** ili **cdn** potreban je uvijek). U odredišnoj domeni je vaš CDN krajnjoj točki.  

    Postupak mapiranje prilagođenu domenu s na krajnjoj točki CDN može, međutim, rezultirati kratak razdoblje nedostupnost za domenu dok registriranja domene na portalu za Azure.

2. [Dodavanje koraka do Srednja Registracija s **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Ako svoju prilagođenu domenu trenutno je podrške aplikacije s ugovor od razini usluge (SLA) koje je potrebno koji ima biti bez nedostupnost, možete koristiti poddomene Azure **cdnverify** možete unijeti na Srednja Registracija korak tako da korisnici će moći pristupiti domene tijekom DNS mapiranja traje mjesto.  

Kada registrirate prilagođene domene na jedan od gore navedeni postupci, želite da biste [potvrdili da prilagođene poddomene upućuje na krajnjoj točki CDN](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] Morate stvoriti CNAME zapis s registrar domene da biste mapirali domene krajnjoj CDN. CNAME zapisi mapiranje određene poddomene kao što su `www.contoso.com` ili `cdn.contoso.com`. Nije moguće da biste mapirali CNAME zapis za korijensku domenu, kao što su `contoso.com`.
>    
> Poddomenu mogu se povezati samo s jednog CDN krajnjoj točki. CNAME zapis koji ste stvorili će usmjeravanje sve promet adresirana poddomene navedeni krajnjoj.  Na primjer, ako se povezujete `www.contoso.com` s na krajnjoj točki CDN pa koje ne može povezati s drugim Azure krajnje točke, kao što su krajnje za račun za pohranu ili krajnja točka servisa u oblaku. Međutim, možete koristiti različite poddomene na istu domenu za krajnje točke različitih servisa. Isti krajnje CDN možete mapirati različite poddomene.
>
> Za krajnje točke **CDN Azure s Verizon** (standardna i Premium), imajte na umu da je zauzima **90 minuta** za prilagođenu domenu promjene proširiti CDN rub čvorove.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Registrirajte se prilagođene domene za krajnje Azure CDN

1.  Prijava na [Portal za Azure](https://portal.azure.com/).
2.  Kliknite **Pregledaj**, zatim **CDN profilima**, zatim profil CDN s krajnju točku koje želite mapirati prilagođenu domenu.  
3.  U plohu **CDN profila** kliknite krajnju točku CDN s kojom želite povezati poddomena.
4.  Pri vrhu plohu krajnju točku, kliknite gumb za **Dodavanje prilagođene domene** .  U plohu **dodati prilagođenu domenu** , vidjet ćete naziv glavnog računala krajnjoj točki izvedene iz CDN krajnje, da biste koristili u stvaranju novi CNAME zapis. Oblikovanje adresa naziv glavnog računala prikazat će se kao ** &lt;EndpointName >. azureedge.net**.  Možete kopirati ovaj glavno računalo za korištenje u stvaranju CNAME zapis.  
5.  Dođite do svojega registrara domene na web-mjesta, a zatim pronađite odjeljak za stvaranje DNS zapisa. Ne mogu pronaći u odjeljku kao što je **Naziv domene**, **DNS-a**ili **Naziv poslužitelja upravljanja**.
6.  Pronađite odjeljak za upravljanje CNAMEs. Možda ćete morati otvoriti stranicu Napredne postavke, a zatim potražite riječi, CNAME, pseudonim ili poddomene.
7.  Stvorite novi CNAME zapis koji mapira odabranom poddomene (na primjer, **"www"** ili **cdn**) na naziv glavnog računala navedene u plohu **dodati prilagođenu domenu** .
8.  Vratite se plohu **dodati prilagođenu domenu** i unesite prilagođenu domenu, uključujući poddomenu, u dijaloškom okviru. Na primjer, unesite naziv domene u obliku `www.contoso.com` ili `cdn.contoso.com`.   

    Azure će CNAME zapis Provjerite postoji li za naziv domene koji ste unijeli. Ako je na CNAME ispravno, prilagođene domene se provjerava.  Za krajnje točke **CDN Azure s Verizon** (standardna i Premium), može potrajati do 90 minuta za prilagođenu domenu postavke zbog primjene na sve čvorove CDN rub, no.  

    Imajte na umu da u nekim slučajevima može potrajati vremena za CNAME zapis za prijenos na poslužitelje naziva na Internetu. Ako domene ne provjerava se odmah, a smatrate točni CNAME zapis, pričekajte nekoliko minuta pa pokušajte ponovno.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Registrirajte se prilagođene domene za krajnje Azure CDN pomoću poddomene privremene cdnverify  

1. Prijava na [Portal za Azure](https://portal.azure.com/).
2. Kliknite **Pregledaj**, zatim **CDN profila**, zatim profil CDN s krajnju točku koje želite mapirati prilagođenu domenu.  
3. U plohu **CDN profila** kliknite krajnju točku CDN s kojom želite povezati poddomena.
4. Pri vrhu plohu krajnju točku, kliknite gumb za **Dodavanje prilagođene domene** .  U plohu **dodati prilagođenu domenu** , vidjet ćete naziv glavnog računala krajnjoj točki izvedene iz CDN krajnje, da biste koristili u stvaranju novi CNAME zapis. Oblikovanje adresa naziv glavnog računala prikazat će se kao ** &lt;EndpointName >. azureedge.net**.  Možete kopirati ovaj glavno računalo za korištenje u stvaranju CNAME zapis.
5. Dođite do svojega registrara domene na web-mjesta, a zatim pronađite odjeljak za stvaranje DNS zapisa. Ne mogu pronaći u odjeljku kao što je **Naziv domene**, **DNS-a**ili **Naziv poslužitelja upravljanja**.
6. Pronađite odjeljak za upravljanje CNAMEs. Možda ćete morati otvoriti stranicu Napredne postavke, a zatim potražite riječi **CNAME** **Pseudonim**ili **poddomene**.
7. Stvorite novi CNAME zapis i ponuditi poddomene pseudonim koji uključuje poddomene **cdnverify** . Na primjer, poddomena koju navedete će se u obliku **cdnverify.www** ili **cdnverify.cdn**. Navedite naziv glavnog računala, CDN krajnje, u obliku **cdnverify.&lt; EndpointName >. azureedge.net**.
8. Vratite se plohu **dodati prilagođenu domenu** i unesite prilagođenu domenu, uključujući poddomenu, u dijaloškom okviru. Na primjer, unesite naziv domene u obliku `www.contoso.com` ili `cdn.contoso.com`. Imajte na umu da u ovom ćete koraku ne morate počinjati poddomenu s **cdnverify**.  

    Azure će CNAME zapis Provjerite postoji li za cdnverify naziv domene koji ste unijeli.
9. Sada prilagođenu domenu provjerila Azure, ali promet na vaše domene nije još usmjeruje u svoje CDN krajnjoj točki. Nakon dovoljno dugo Čekanje da biste postavke prilagođene domene za prenijeti na rub čvorove CDN (90 minuta za **Azure CDN iz Verizon**, 1-2 minuta **CDN Azure s Akamai**), vratite DNS registrara na web-mjesto i stvorite drugi CNAME zapis koji mapira vaše poddomene vaše CDN krajnjoj točki. Na primjer, odrediti poddomena kao **"www"** ili **cdn**i naziv glavnog računala kao ** &lt;EndpointName >. azureedge.net**. Ovaj korak Registracija prilagođene domene je dovršena.
10. Na kraju, možete izbrisati CNAME zapis koji ste stvorili pomoću **cdnverify**, kao što je potrebno samo kao privremene korak.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Provjerite da prilagođene poddomene upućuje na krajnjoj točki CDN

- Nakon što dovršite Registracija prilagođene domene, može pristupiti sadržaju koji je predmemorirani pri CDN krajnjoj točki pomoću prilagođene domene.
Najprije provjerite imate li javno sadržaj koji je predmemorirani na krajnjoj. Ako, na primjer, ako je na krajnjoj točki CDN povezanih s računom za pohranu, u CDN predmemorira u javnim blob spremnika. Da biste testirali prilagođenu domenu, provjerite svoje spremnik postavljen dopustili pristup javno i da sadrži najmanje jedan blob.
- U pregledniku idite na adresu blob pomoću prilagođene domene. Na primjer, ako je vaša prilagođena domena `cdn.contoso.com`, URL-a u predmemoriji blob će biti sličan sljedeći URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Vidi također

[Kako omogućiti mreže za isporuku sadržaja (CDN) za Azure](./cdn-create-new-endpoint.md)  
