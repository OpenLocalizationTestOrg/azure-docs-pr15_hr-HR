<properties
     pageTitle="Korištenje Azure CDN | Microsoft Azure"
     description="U ovoj se temi objašnjava Omogućivanje sadržaja isporuke mreže (CDN) za Azure. Vodič vodi kroz stvaranje novog profila CDN i krajnjoj točki."
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
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Korištenje Azure CDN  

U ovoj se temi vodi kroz Omogućivanje Azure CDN tako da stvorite novi profil CDN i krajnjoj točki.

>[AZURE.IMPORTANT] Uvod u CDN funkcioniranja, kao i popis značajki, potražite u članku [Pregled CDN](./cdn-overview.md).

## <a name="create-a-new-cdn-profile"></a>Stvaranje novog profila CDN

Zbirka krajnje točke CDN je CDN profila.  Svaki profil sadrži jednu ili više CDN krajnje točke.  Možda želite koristiti više profila da biste organizirali CDN krajnje točke domene internet, web-aplikacije ili nekog drugog kriterija.

> [AZURE.NOTE] Prema zadanim postavkama, jedan Azure pretplate ograničeno je na osam CDN profila. Svaki profil CDN ograničeno je na deset CDN krajnje točke.
>
> Cijene CDN primjenjuje se na razini CDN profila. Ako želite koristiti kombinacije Azure CDN cijene razine potrebno više CDN profila.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Stvaranje nove CDN krajnje točke

**Da biste stvorili krajnju točku CDN**

1. [Portal za Azure](https://portal.azure.com)dođite do CDN profila.  Koje možda imaju prikvačena ga na nadzornu ploču u prethodnom koraku.  Ako vam ne možete pronaći ga tako da kliknete **Pregled**, a zatim **CDN profila**, a na profilu planirate dodati na krajnjoj točki za.

    Pojavit će se profil plohu CDN-a.

    ![CDN profila][cdn-profile-settings]

2. Kliknite gumb **Dodaj krajnjoj točki** .

    ![Dodavanje gumba za krajnje točke][cdn-new-endpoint-button]

    Pojavit će se plohu **Dodavanje krajnje točke** .

    ![Dodavanje plohu krajnje točke][cdn-add-endpoint]

3. Unesite **naziv** za ovaj CDN krajnjoj točki.  Ovaj naziv će se koristiti za pristup predmemorirani resursa na domeni `<endpointname>.azureedge.net`.

4. Na padajućem popisu **polazište vrsta** odaberite vrstu polazište.  Odaberite **prostor za pohranu** za račun za Azure prostora za pohranu, **u oblaku** za servis u Oblaku programa Azure, **Web-aplikacije** za Azure Web App ili **Prilagođeno polazište** za sve druge javno dostupnu web server origin (smještena u Azure ili nekom drugom mjestu).

    ![Vrsta CDN origin](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. Na padajućem popisu **polazište naziv glavnog računala** , odaberite ili unesite domenu izvor.  Na padajućem popisu prikazat će se popis svih dostupnih drugačijeg izvora vrste navedene u koraku 4.  Ako ste odabrali *prilagođene polazište* kao vaš **izvor upišite**, upisujete u domeni svoje prilagođene polazište.

6. U tekstni okvir **polazište put** unesite put do resursa želite predmemorije ili ostavite prazno da biste omogućili predmemorije bilo kojeg resursa na domeni koju ste naveli u koraku 5.

7. **Izvor zaglavlja glavnog računala**unesite zaglavlje domaćina želite CDN da biste poslali sa zahtjevom za svaki ili ostavite zadani.

    > [AZURE.WARNING] Neke vrste drugačijeg izvora, kao što su Azure prostora za pohranu i web-aplikacijama potreban je zaglavlje domaćina tako da odgovara domenu izvor. Osim ako imate na izvor koji zahtijeva zaglavlje domaćina razlikuje od svojoj domeni, ostavite zadanu vrijednost.

8. **Protokol** i **polazište priključak**, navedite protokoli i priključaka koji se koristi za pristup resursa na izvor.  Barem jedan protokol (HTTP ili HTTPS) mora biti odabran.
    
    > [AZURE.NOTE] **Polazište priključak** utječe samo na što priključak krajnje točke koristi za dohvaćanje podataka iz izvor.  Krajnja točka sam samo bit će dostupni završetka klijente na zadanom HTTP i HTTPS priključke (80 i 443), bez obzira na to **polazište priključak**.  
    >
    > Krajnje točke **CDN Azure s Akamai** ne dopuštaju cijeli raspon TCP priključak za drugačijeg izvora.  Popis priključaka polazište je dopušteno, potražite u članku [CDN Azure s Akamai dopuštene polazište priključci](https://msdn.microsoft.com/library/mt757337.aspx).  
    >
    > Pristup CDN sadržaja pomoću HTTPS ima sljedeća ograničenja:
    > 
    > - Morate koristiti SSL certifikat nudi na CDN. Certifikati trećih strana nisu podržani.
    > - Morate koristiti domenu CDN-ovi (`<endpointname>.azureedge.net`) pristup HTTPS sadržaja. HTTPS podrška nije dostupna za prilagođenih naziva domena (CNAMEs) jer u CDN ne podržava prilagođene certifikati trenutno.

9. Kliknite gumb **Dodaj** da biste stvorili novi krajnjoj točki.

10. Nakon stvaranja krajnju točku, prikazuje se popis krajnje točke profila. Prikaz popisa prikazuje se URL koristi za pristup predmemoriranog sadržaja, kao i domenu izvor.

    ![CDN krajnje točke][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Krajnja točka odmah nije dostupan za korištenje, kao što je potrebno vrijeme za registraciju proširiti kroz na CDN.  Prijenos će za <b>Azure CDN iz Akamai</b> profila, obično dovrši unutar jedne minute.  Profili <b>CDN Azure s Verizon</b> prijenos će obično dovrši unutar 90 minuta, ali u nekim slučajevima može trajati dulje.
    >    
    > Korisnici koji pokušate koristiti naziv domene CDN prije konfiguraciju krajnju točku propagira da biste na iskače primit će HTTP 404 odgovor šifre.  Ako je prošlo nekoliko sati ste stvorili na krajnjoj točki i se i dalje dobivate 404 odgovore, pogledajte [krajnje točke za otklanjanje poteškoća CDN vraćanje 404 statusi](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Vidi također
- [Omogućivanje predmemoriranja ponašanje zahtjeva s nizovima upita](cdn-query-string.md)
- [Upute za mapiranje CDN sadržaj prilagođene domene](cdn-map-content-to-custom-domain.md)
- [Unaprijed učitavanje imovine na krajnje Azure CDN](cdn-preload-endpoint.md)
- [Čišćenje Azure CDN krajnje točke](cdn-purge-endpoint.md)
- [Otklanjanje poteškoća s krajnje točke CDN vraćanje 404 statusi](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
