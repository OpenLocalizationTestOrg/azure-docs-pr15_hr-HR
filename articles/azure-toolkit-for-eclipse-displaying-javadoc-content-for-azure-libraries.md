<properties
    pageTitle="Prikaz sadržaja Javadoc u Eclipse paketa Azure biblioteke za Java"
    description="Kako prikazati sadržaj Javadoc za Azure biblioteke u Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Prikaz sadržaja Javadoc u Eclipse paketa Azure biblioteke za Java #

Javadoc sadržaja za biblioteke Azure Java moguće je prikazati u okruženju sustava Eclipse povezivanjem Javadoc sadržaja u biblioteke Azure za Java. Sljedeći koraci pokazalo kako se koristi ta je funkcija unutar Eclipse.

Ovaj postupak pretpostavlja da ste već dodali Azure biblioteka za Java u vašem putu Sastavi.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Prikaz sadržaja Javadoc u Eclipse za Azure biblioteke Java ##

* Unutar Eclipse na Project Explorer u odjeljku **Biblioteke poziva** projekta, otvorite Kontekstni izbornik za biblioteku Azure za Java POSUDU. Na primjer, **microsoft-windowsazure-api-0.1.0.jar** (broj verzije može biti drugu, ovisi o tome koju verziju ste instalirali).
* Kliknite **Svojstva**.
* U dijaloškom okviru **Svojstva** , u lijevom oknu kliknite **Mjesto na Javadoc**. Prikazat će se dijaloški okvir **Javadoc mjesto** .
* Možete navesti **Javadoc URL-a**ili **Javadoc u arhivu**.
    * Ako se odlučite za određivanje **URL-a Javadoc**, pomoću URL-ove kao što su **http://dl.windowsazure.com/javadoc** ili **http://dl.windowsazure.com/storage/javadoc**.
    * Ako odlučite koristiti **Javadoc u arhivu**, možete odrediti vanjske datoteke ili datoteke radnog prostora.
    Odaberite, a zatim Pregledaj/provjeru prema potrebi. U sljedećem primjeru pridružuje u odgovarajuće Javadoc POSUDU koja sadrži preuzeta lokalno u mapu pod nazivom **c:\MyAzureJARs**Azure biblioteke za Java.
    ![][ic553487]
* *Neobavezno*: kliknite **Provjera valjanosti**. Potencijalni problemi s Javadoc POSUDU ovdje nije moguć.
* Kliknite **u redu**.

Kada je povezan s bibliotekom, Javadoc sadržaj će se prikazati unutar vaše IDE Eclipse. Ako, na primjer, ako `blob` je definirana vrsta `CloudBlockBlob` u kodu slijedi primjer Javadoc sadržaj koji se pojavljuje kada upišete `blob.acquireLease` u kodu:

![][ic553488]

## <a name="see-also"></a>Vidi također ##

[Azure komplet alata za Eclipse][]

[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse][]

[Instalacija alata za Azure za Eclipse][] 

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java][].

<!-- URL List -->

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png