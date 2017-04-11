<properties
    pageTitle="Konfiguriranje naziv domene za vaše krajnju točku spremište blobova platforme | Microsoft Azure"
    description="Saznajte kako da biste mapirali korisnička domene krajnjoj spremišta blobova platforme za račun Azure prostora za pohranu na portalu klasični Azure."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Konfiguriranje prilagođenog naziva domene za vaše krajnju točku spremište blobova platforme

## <a name="overview"></a>Pregled

Možete konfigurirati prilagođene domene za pristup podacima blob na vašem računu Azure prostora za pohranu. Zadana krajnja točka blobova ima `<storage-account-name>.blob.core.windows.net`. Ako mapirate prilagođenu domenu i poddomenu, kao što je **www.contoso.com** krajnjoj blobova platforme za vaš račun za pohranu, a zatim korisnici mogu i pristup podacima blob u računu za pohranu putem te domene.

>[AZURE.IMPORTANT] Azure prostora za pohranu još podržava HTTPS pomoću prilagođene domene. Radimo na umu da korisnici mogli biti zainteresirani ovu značajku, a bit će dostupni u buduće izdanje.

Prilagođene domene na krajnjoj točki blobova platforme za vaš račun za pohranu na dva načina. Najjednostavniji način je da biste stvorili mapiranje prilagođenu domenu i poddomenu s blob krajnje CNAME zapis. CNAME zapis DNS značajka je koja mapira domenu izvor u odredišnoj domeni. U ovom slučaju domenu izvor je prilagođene domenu i poddomenu – Imajte na umu da je poddomena uvijek potrebno. Odredišna domena je krajnja točka vaše Blob servisa.

Postupak mapiranje prilagođenu domenu s na krajnjoj točki blob može, međutim, rezultirati kratak razdoblje nedostupnost za domenu dok registriranja domene [Azure klasični Portal](https://manage.windowsazure.com). Ako svoju prilagođenu domenu trenutno je podrške aplikacije s ugovor od razini usluge (SLA) koje je potrebno koji ima biti bez nedostupnost, možete koristiti poddomene Azure **asverify** možete unijeti na Srednja Registracija korak tako da korisnici će moći pristupiti domene tijekom DNS mapiranja traje mjesto.

Sljedeća tablica prikazuje uzorak URL-ovi za pristup podacima blob u račun za pohranu pod nazivom **mystorageaccount**. Prilagođene domene registrirane za račun za pohranu je **www.contoso.com**:

Vrsta resursa|Oblici URL-a
---|---
Račun za pohranu|**Zadani URL:** http://mystorageaccount.blob.core.windows.net<p>**URL prilagođene domene:** http://www.contoso.com</td>
Blob|**Zadani URL:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**URL prilagođene domene:** http://www.contoso.com/mycontainer/myblob
Spremnik|**Zadani URL:** http://mystorageaccount.blob.core.windows.net/myblob ili http://mystorageaccount.blob.core.windows.net/$ korijenski/myblob<p>**URL prilagođene domene:** web-mjesto http://www.contoso.com/myblob ili u okvir za http://www.contoso.com/$ korijenski/myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Registrirajte se prilagođenu domenu za vaš račun za pohranu

Koristite ovaj postupak da biste registrirali prilagođene domene ako nemate opasnosti o stvaranju domene ukratko nije dostupan korisnicima ili prilagođenu domenu je trenutno nalaze aplikacije.

Ako svoju prilagođenu domenu trenutno je podrške aplikacije koja ne smije sadržavati bilo koju nedostupnost, pomoću postupka opisanog u <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">dnevniku prilagođenu domenu za vaš račun za pohranu pomoću poddomene privremene asverify</a>.

Da biste konfigurirali prilagođeni naziv domene, morate stvoriti novi CNAME zapis s registrar domena. CNAME zapis navodi pseudonim za naziv domene u ovom slučaju je karata adresu prilagođene domene krajnjoj spremište blobova platforme za vaš račun za pohranu.

Svaki registrara je slična, ali malo drugačiji način navodeći CNAME zapis, ali pojam je ista. Imajte na umu da mnoge paketa Registracija osnovni domene nude konfiguriranje DNS-a, pa ćete morati nadograditi paket za registraciju domene možete stvoriti CNAME zapis.

1.  [Azure klasični Portal](https://manage.windowsazure.com)idite na karticu **prostora za pohranu** .

2.  Na kartici **prostora za pohranu** kliknite naziv računa za pohranu za koje želite mapirati prilagođenu domenu.

3.  Kliknite karticu **Konfiguracija** .

4.  Pri dnu zaslona kliknite **Upravljanje domenom** da biste prikazali dijaloški okvir **Upravljanje domenom Prilagođeno** . U odjeljku teksta pri vrhu dijaloškog okvira, vidjet ćete informacije o stvaranju CNAME zapis. U ovom postupku Zanemari tekst koji se odnosi na poddomena **asverify** .

5.  Prijavite se na web-mjestu registrara DNS-a, a zatim idite na stranicu za upravljanje DNS-a. Ne mogu pronaći u odjeljku kao što je **Naziv domene**, **DNS-a**ili **Naziv poslužitelja upravljanja**.

6.  Pronađite odjeljak za upravljanje CNAMEs. Možda ćete morati otvoriti stranicu Napredne postavke, a zatim potražite riječi **CNAME** **Pseudonim**ili **poddomene**.

7.  Stvorite novi CNAME zapis i ponuditi pseudonim poddomenu, kao što su **"www"** ili **fotografija**. Zatim navedite naziv glavnog računala, što je Blob krajnje servisa, u obliku **mystorageaccount.blob.core.windows.net** (pri čemu je **mystorageaccount** ime vašeg računa za pohranu). Naziv glavnog računala da biste koristili navedeni su vam u tekstu dijaloški okvir **Upravljanje domenom Prilagođeno** .

8.  Kada stvorite CNAME zapis, vratili u dijaloški okvir **Upravljanje prilagođenu domenu** , a zatim unesite naziv prilagođene domene, uključujući poddomenu, u polje **Naziv prilagođene domene** . Na primjer, ako je vaša domena **contoso.com** , a vaš poddomena je **"www"**, unesite **www.contoso.com**; Ako je vaš poddomene **fotografije**, unesite **photos.contoso.com**. Imajte na umu da je potrebno poddomena.

9. Kliknite gumb **registrirati** da biste registrirali prilagođenu domenu.

    Ako registracija ne uspije, prikazat će se poruka **aktivan prilagođenu domenu**. Korisnici mogu odmah prikaz blob podataka na svojoj prilagođenoj domeni dok god imaju odgovarajuće dozvole.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Registrirajte se prilagođenu domenu za vaš račun za pohranu pomoću poddomene privremene asverify

Koristite ovaj postupak da biste registrirali prilagođene domene Ako svoju prilagođenu domenu trenutno podrške aplikacije pomoću programa SLA koji zahtijeva koji postojati bez isključiti. Stvaranjem CNAME koja upućuje na asverify. &lt;poddomena&gt;. &lt;customdomain&gt; za asverify. &lt;storageaccount&gt;. blob.core.windows.net, možete unaprijed registrirate domenu u sustavu Azure. Zatim stvorite drugi CNAME koja pokazuje od &lt;poddomena&gt;. &lt;customdomain&gt; da biste &lt;storageaccount&gt;. blob.core.windows.net, pri čemu promet za svoju prilagođenu domenu preusmjeravat će se vaš blob krajnjoj točki.

Poddomena asverify je posebno poddomene prepoznaje Azure. Stavljajući **asverify** da biste sami poddomene dopušta Azure prepoznati prilagođene domene bez izmjene DNS zapis za domenu. Nakon što izmijenite DNS zapis za domenu, će se mapirati krajnjoj blob s bez isključiti.

1.  [Azure klasični Portal](https://manage.windowsazure.com)idite na karticu **prostora za pohranu** .

2.  Na kartici **prostora za pohranu** kliknite naziv računa za pohranu za koje želite mapirati prilagođenu domenu.

3.  Kliknite karticu **Konfiguracija** .

4.  Pri dnu zaslona kliknite **Upravljanje domenom** da biste prikazali dijaloški okvir **Upravljanje domenom Prilagođeno** . U odjeljku teksta pri vrhu dijaloškog okvira, vidjet ćete informacije o stvaranju CNAME zapisi koji se koriste poddomene **asverify** .

5.  Prijavite se na web-mjestu registrara DNS-a, a zatim idite na stranicu za upravljanje DNS-a. Ne mogu pronaći u odjeljku kao što je **Naziv domene**, **DNS-a**ili **Naziv poslužitelja upravljanje**.

6.  Pronađite odjeljak za upravljanje CNAMEs. Možda ćete morati otvoriti stranicu Napredne postavke, a zatim potražite riječi **CNAME** **Pseudonim**ili **poddomene**.

7.  Stvorite novi CNAME zapis i ponuditi poddomene pseudonim koji uključuje poddomene asverify. Ako, na primjer, poddomene navedete će se u obliku **asverify.www** ili **asverify.photos**. Zatim navedite naziv glavnog računala, što je Blob krajnje servisa, u obliku **asverify.mystorageaccount.blob.core.windows.net** (pri čemu je **mystorageaccount** ime vašeg računa za pohranu). Naziv glavnog računala da biste koristili navedeni su vam u tekstu dijaloški okvir **Upravljanje domenom Prilagođeno** .

8.  Kada stvorite CNAME zapis, vratili u dijaloški okvir **Upravljanje prilagođenu domenu** , a zatim unesite naziv prilagođene domene u polje **Naziv prilagođene domene** . Na primjer, ako je vaša domena **contoso.com** , a vaš poddomena je **"www"**, unesite **www.contoso.com**; Ako je vaš poddomene **fotografije**, unesite **photos.contoso.com**. Imajte na umu da je potrebno poddomena.

9.  Kliknite potvrdni okvir koja vas obavještava da **Dodatno: preregister Moje prilagođene domene pomoću poddomene 'asverify'**.

10. Kliknite gumb **registrirati** preregister prilagođenu domenu.

    Ako je na preregistration uspije, prikazat će se poruka **aktivan prilagođenu domenu**.

11. U ovom trenutku prilagođenu domenu provjerila Azure, ali promet na vaše domene nije još usmjeruje na račun servisa za pohranu. Da biste dovršili postupak, vratite se web-mjestu registrara DNS i stvorite drugi CNAME zapis mapira vaše poddomene krajnju točku sustava Blob usluge. Na primjer, odrediti poddomena kao **"www"** ili **fotografija**i naziv glavnog računala kao **mystorageaccount.blob.core.windows.net** (pri čemu je **mystorageaccount** ime vašeg računa za pohranu). Ovaj korak Registracija prilagođene domene je dovršena.

12. Na kraju, možete izbrisati CNAME zapis koji ste stvorili pomoću **asverify**, kao što je potrebno samo kao privremene korak.

Korisnici mogu odmah prikaz blob podataka na svojoj prilagođenoj domeni dok god imaju odgovarajuće dozvole.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Provjerite je li prilagođenu domenu reference je vaš krajnja točka servisa blobova platforme

Da biste potvrdili svoju prilagođenu domenu uistinu mapirano na vaše krajnja točka servisa Blob, stvorite blob u spremniku javno unutar vašeg računa za pohranu. Nakon toga u web-pregledniku pomoću URI u sljedećem obliku pristup blob-om:

-   http://<*subdomain.customdomain*>/<*mycontainer*>/<*myblob*>

Ako, na primjer, možete koristiti sljedeće URI za pristup web-obrasca putem **photos.contoso.com** prilagođene poddomene mapira blob u spremniku vaše **myforms** :

-   http://photos.contoso.com/myforms/applicationform.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Unregister prilagođenu domenu s računa za pohranu

Odjava prilagođene domene, slijedite ove korake: 

1. Prijava na [Portal za Azure klasični](https://manage.windowsazure.com). 

2. U navigacijskom oknu kliknite **prostora za pohranu**. 

3. Na stranici **za pohranu** , kliknite naziv računa za pohranu za prikaz na nadzornoj ploči. 

5. Na vrpci kliknite **Upravljanje domenom**. 

6. U dijaloškom okviru **Upravljanje domenom Prilagođeno** kliknite **neregistriranja**. 


## <a name="additional-resources"></a>Dodatni resursi

-   [Kako mapirati prilagođene domene na krajnjoj točki sadržaja isporuke mreže (CDN)](../cdn/cdn-map-content-to-custom-domain.md)
