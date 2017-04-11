<properties
    pageTitle="Prijava analitiku značajke za davatelja usluga | Microsoft Azure"
    description="Prijava analitiku pomoći upravlja davateljima usluga (MSPs), većih tvrtki neovisno dobavljačima softver (ISV) i davatelja usluge hostinga upravljanje i praćenje poslužitelji u klijentovoj lokalnog ili infrastrukture u oblaku."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Prijava analitiku značajke za davatelja usluga

Prijava analitiku može pridonijeti upravljanih davateljima usluga (MSPs), većih tvrtki, neovisni proizvođači softvera (ISV) i davatelja hostinga servis za upravljanje i nadzirati poslužitelji u klijentovoj lokalnog ili infrastrukture u oblaku. 

Većih tvrtki zajedničko korištenje mnogo sličnosti s davateljima usluga, osobito kada je središnje IT tima koji je odgovoran za upravljanje IT za mnogo različitih poslovne jedinice. Zbog jednostavnosti, ovaj dokument koristi termina *usluga* , ali ista funkcija nije dostupna za korporacije i ostali korisnici.

## <a name="cloud-solution-provider"></a>Oblak davatelju rješenja

Partnera i davatelji usluga koji su dio programa [Oblaka rješenje davatelj (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) prijava analitiku jedan je od Azure usluge dostupne na CSP pretplate. 

Za analizu zapisnika sljedeće značajke omogućene su u *Oblak davatelju rješenja* pretplate.

Kao *Davatelju rješenja oblaka* možete učiniti sljedeće:

+ Stvaranje radne prostore prijava analitiku pretplate na klijentu (korisnik).
+ Pristup radnim prostorima stvorio drugih korisnika. 
+ Dodavanje i uklanjanje korisničkog pristupa radni prostor putem upravljanja Azure korisnika. Kada u radnom prostoru na klijentu OMS portalu Upravljanje korisnicima stranice u odjeljku postavke nije dostupna
  - Prijava analitiku ne podržava pristupa na temelju uloga još - dodjeljivanja korisnik `reader` dozvola na portalu za Azure im omogućuje da promjene konfiguracije na portalu OMS

Da biste se prijavili pretplatu na klijentu, morate navesti identifikator klijenta. Identifikator klijenta često je zadnji dio adresu e-pošte koji se koristi za prijavu.

+ Na portalu OMS dodavanje `?tenant=contoso.com` u URL-a za portal. Na primjer,`mms.microsoft.com/?tenant=contoso.com`
+ U ljusci PowerShell, koristite na `-Tenant contoso.com` parametar prilikom korištenja `Add-AzureRmAccount` cmdleta
+ Identifikator klijenta automatski se dodaju kada koristite na `OMS portal` vezu s portala za Azure da biste otvorili i prijavite se na portal OMS u odabranom radnom prostoru

Kao *kupca* davatelju rješenja za oblak možete učiniti sljedeće:

+ Stvaranje zapisnika analize radne prostore u CSP pretplate
+ Radni prostori programa Access stvorio CSP
  -  Korištenje na `OMS portal` vezu s portala za Azure da biste otvorili i prijavite se na portal OMS u odabranom radnom prostoru
+ Prikaz i korištenje stranici Upravljanje korisnika u odjeljku postavke na portalu OMS

>[AZURE.NOTE] Rješenja za sigurnosno kopiranje i vraćanje web-mjesta za analizu zapisnika se ne može se povezati s oporavak servisa sigurnog i ne može konfigurirati CSP pretplate.

## <a name="managing-multiple-customers-using-log-analytics"></a>Upravljanje više korisnika pomoću zapisnika Analytics 

Preporučuje se stvaranje radnog prostora zapisnika Analytics za svakog korisnika kojima upravljate. Radni prostor analize zapisnika omogućuje:

+ Zemljopisnu lokaciju podaci koji se spremaju. 
+ Preciznosti za naplatu 
+ Odvajanja podataka 
+ Jedinstveni konfiguracije

Stvaranjem radnog prostora programa Groove kupac vam se staviti svaki korisničkih podataka i i pratite korištenje svaki klijent.

Dodatne informacije o kada i zašto za stvaranje više radnih prostora je opisan u [Upravljanje pristupom prijava analitiku] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Stvaranje i konfiguriranje radnih prostora za klijenta možete automatizirati pomoću [komponente PowerShell](log-analytics-powershell-workspace-configuration.md), [Predlošci Voditelj resursa](log-analytics-template-workspace-configuration.md), ili pomoću [REST API -JA](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

Korištenje predložaka resursima za konfiguraciju radnog prostora omogućuje vam osnovne konfiguracije koji se mogu koristiti za stvaranje i konfiguriranje radnih prostora. Možete biti sigurni da kao radne prostore stvaraju za kupce su automatski konfigurirana s potrebama. Kada ažurirate potrebama, predložak se ažurira i zatim ponovno primijeniti postojećih radnih prostora. Ovaj postupak omogućuje da čak i postojećih radnih prostora zadovoljava vaše nove standarde.    

Kada upravljanje više zapisnika analize radne prostore, preporučujemo integriranje pojedinom radnom prostoru sa sustavom postojeće ticketing operacije konzole pomoću funkcije [upozorenja](log-analytics-alerts.md) . Putem integracije s postojećim sustavima, osoblju za podršku možete Nastavite slijediti njihovih poznatih procesa. Prijava analitiku redovito provjerava pojedinom radnom prostoru protiv upozorenja kriteriju i generira upozorenja kada je potrebno akcija.

Izvršni razine izvješćima koja sažimanje podataka radne prostore možete koristiti Integracija između zapisnika analize i [PowerBI](log-analytics-powerbi.md). Ako je potrebno integrirati s drugih sustava za izvješćivanje API pretraživanja (putem komponente PowerShell ili [REST](log-analytics-log-search-api.md)) možete koristiti za izvođenje upita i izvoz rezultata pretraživanja.

## <a name="next-steps"></a>Daljnji koraci

+ Automatizirati stvaranje i konfiguriranje radnih prostora pomoću [predložaka Voditelj resursa](log-analytics-template-workspace-configuration.md)
+ Moguće automatizirati stvaranje radnih prostora pomoću [komponente PowerShell](log-analytics-powershell-workspace-configuration.md) 
+ Korištenje [upozorenja](log-analytics-alerts.md) radi integrirati s postojećim sustavima
+ Generiranje sažetka izvješća na temelju [PowerBI](log-analytics-powerbi.md)
