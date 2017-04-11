<properties
    pageTitle="Popis poslovnih subjekata Azure prostora za pohranu"
    description="Upravljanje postavkama računa za pohranu korištenje kompleta alata za Azure za Eclipse"
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Popis poslovnih subjekata Azure prostora za pohranu #

Računi Azure prostora za pohranu omogućiti preuzimanje mjesta koja će se koristiti za JDK, poslužitelj aplikacije i proizvoljne komponente, kao i za pohranu stanja pri korištenju predmemoriranje. Eclipse održava popis poznatih prostora za pohranu računa koji su dostupni u radnom prostoru Eclipse projekte. Da biste otvorili dijaloški okvir **Računi za pohranu** , koji se koriste za upravljanje tog popisa unutar Eclipse, kliknite **prozor**, kliknite **Postavke**, proširite **Azure**, a zatim **Račune za pohranu**.

Sljedeće prikazuje dijaloški okvir **Računi za pohranu** .

![][ic719496]

U ovom dijaloškom okviru je moguće otvoriti i s vezom za **račune** na dijaloške okvire koji koristite za pohranu računa, kao što je sljedeće:

* Kartica **JDK** dijaloški okvir za **Konfiguraciju poslužitelja** .
* Kartici **poslužitelj** dijaloški okvir za **Konfiguraciju poslužitelja** .
* Dijaloški okvir **Dodavanje komponente** .
* Dijaloški okvir svojstva **Međuspremanje** .

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>Da biste uvezli svoje račune za pohranu pomoću datoteke postavke objavljivanja ##

1. U dijaloškom okviru **Računi za pohranu** , kliknite **Uvezi iz datoteke za OBJAVLJIVANJE postavke**.
2. (Ovaj korak preskočiti ako ste već spremili datoteku postavki Objavi na lokalno računalo) U dijaloškom okviru **Informacije o uvozu pretplate** kliknite **Preuzmi datoteku OBJAVI postavke**. Ako niste prijavljeni u račun za Azure, zatražit će se da biste se prijavili. Potom ćete morati Spremi programa Azure objavljivanje datoteka postavki. (Možete zanemariti dobivene upute prikazana na stranici za prijavu – nudi Azure portal i im su namijenjene korisnicima Visual Studio.) Spremite je na lokalno računalo.
3. I dalje u dijaloškom okviru **Informacije o uvozu pretplate** kliknite gumb **Pregledaj** , odaberite datoteku s postavkama Objavi koju ste prethodno spremili u lokalno i zatim kliknite **Otvori**.
4. Kliknite **u redu** da biste zatvorili dijaloški okvir **Informacije o uvozu pretplate** .

## <a name="to-create-a-new-storage-account"></a>Da biste stvorili novi račun za pohranu ##

1. U dijaloškom okviru **Računi za pohranu** , kliknite **Dodaj**.
2. U dijaloškom okviru **Dodavanje računa za pohranu** , kliknite **Novo**.
3. U dijaloškom okviru **Novi račun za pohranu** , navedite vrijednosti za sljedeće:
    * Naziv računa za pohranu.
    * Mjesto za pohranu računa.
    * Opis računa za pohranu.
    * Pretplata na kojoj pripada račun za pohranu.
4. Kliknite **u redu** da biste zatvorili dijaloški okvir **Novi račun za pohranu** .

Može potrajati nekoliko minuta za vaš račun za pohranu će biti stvoren. Nakon stvaranja, kliknite **u redu** da biste zatvorili dijaloški okvir **Dodavanje računa za pohranu** i na novi račun za pohranu dodat će se na popisu računa dostupan prostor za pohranu.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Da biste dodali postojećeg računa za pohranu na popis ##

1. Ako već nemate račun za Azure prostora za pohranu, stvorite ga tako da slijedite korake navedene u **Da biste stvorili novu sekciju račun za pohranu** iznad. (Osim toga, možete stvoriti novi račun za pohranu na [Portal za upravljanje Azure][].)
2. U dijaloškom okviru **Računi za pohranu** , kliknite **Dodaj**.
3. U dijaloškom okviru **Dodavanje računa za pohranu** unesite vrijednosti za **ime** i **Tipkovni prečac**. Naziv i pristup ključ za računa mora biti za postojeći račun Azure prostora za pohranu. Pomoću rubrike **prostora za pohranu** [Azure Portal za upravljanje][] da biste vidjeli imena pohranu računa i tipki. **Dodavanje računa za pohranu** dijalog izgledat će otprilike ovako.

    ![][ic719497]

4. Kliknite **u redu** da biste zatvorili dijaloški okvir **Dodavanje računa za pohranu** .

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Da biste izmijenili račun za pohranu za korištenje novog ključa za pristup ##

1. U dijaloškom okviru **Za pohranu računa** kliknite račun prostora za pohranu koji želite urediti, a zatim kliknite **Uredi**.
2. U dijaloškom okviru **Uređivanje pristup ključa za pohranu računa** izmijeniti vrijednost **Tipkovni prečac** .
3. Kliknite **u redu** da biste zatvorili dijaloški okvir **Uređivanje pristup ključa za pohranu računa** .

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Da biste uklonili račun za pohranu na popisu zajamčena Eclipse ##

1. U dijaloškom okviru **Računi za pohranu** kliknite račun za pohranu koji želite urediti, a zatim kliknite **Ukloni**.
2. Kliknite **u redu** kada se to od vas zatraži da biste uklonili račun za pohranu.

>[AZURE.NOTE] Uklanjanje računa za pohranu u sklopu dijaloškog okvira **Računi za pohranu** samo uklanja s popisa za pohranu račune koji su vidljivi unutar Eclipse. Uklanjanje računa za pohranu iz pretplate Azure. Uz to, računa za pohranu može izgledati ponovno na popisu kada Eclipse ponovno učitava pojedinosti pretplate.

## <a name="see-also"></a>Vidi također ##

[Azure komplet alata za Eclipse][]

[Instalacija alata za Azure za Eclipse][] 

[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse][]

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java][].

<!-- URL List -->

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Portal za upravljanje Azure]: http://go.microsoft.com/fwlink/?LinkID=512959
[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
