<properties 
    pageTitle="Kako preusmjeriti USB uređaji RemoteApp Azure | Microsoft Azure" 
    description="Saznajte kako koristiti preusmjeravanje za uređaje USB Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Kako preusmjeriti USB uređaji RemoteApp Azure

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Preusmjeravanje omogućuje korisnicima pomoću USB uređaja povezanih s svoje računalo ili tablet s aplikacijama u Azure RemoteApp. Ako, na primjer, ako se zajednički koristi Skype pomoću servisa Azure RemoteApp vaši korisnici morati moći koristiti svoje kamere uređaja.

Prije nego što nastavite Dodatno, provjerite je li pročitajte informacije o preusmjeravanju USB u [Korištenje preusmjeravanje Azure RemoteApp](remoteapp-redirection.md). No u preporučuje nusbdevicestoredirect:s: * neće funkcionirati za USB web-kamera i možda neće funkcionirati za neke USB pisači ili USB multifunctional uređaja. Dizajnom i sigurnosnih vam razloga Azure RemoteApp administrator mora omogućiti preusmjeravanje klasa uređaja GUID ili ID instance uređaja korisnici mogli koristiti te uređaje.

Iako u ovom se članku govori o preusmjeravanju web kamere, možete koristiti sličan pristup da biste preusmjerili USB pisača i drugih USB multifunctional uređaja koji vas preusmjerava tako da na **nusbdevicestoredirect:s:*** naredba.

## <a name="redirection-options-for-usb-devices"></a>Mogućnosti za preusmjeravanje za USB uređaji
Azure RemoteApp koristi slične mehanizme za preusmjeravanje USB uređaja kao one koji su dostupni za udaljene radne površine. Pozadinsku tehnologiju omogućuje vam da odaberete način točan preusmjeravanje za dani uređaja, da biste dobili najbolje obje više razine i RemoteFX USB uređaj pomoću preusmjeravanje na **usbdevicestoredirect:s:** naredba. Postoje četiri elemenata sljedeću naredbu:

| Obrada narudžbe | Parametar           | Opis                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | Odabir svih uređaja koje nisu izdvojiti više razine preusmjeravanje. Napomena: dizajnom * ne funkcionira za USB web-kamera.  |
|                  | {Klasa uređaja GUID} | Odabir svih uređaja koje odgovaraju postavljanje predmete navedeni uređaj.                                                           |
|                  | USB\InstanceID      | Odabire USB uređaj za dani instancu ID.                                                                  |
| 2                | -USB\Instance ID    | Uklanja preusmjeravanja za određeni uređaj.                                                                 |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Preusmjeravanje USB uređaj pomoću klasa uređaja GUID
Da biste pronašli klasa uređaja GUID koje je moguće koristiti za preusmjeravanje na dva načina. 

Prva mogućnost jest korištenje [System-Defined uređaj postavljanje Tečajevi dostupni dobavljačima](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Odaberite predmet koja najviše odgovara uređaj povezan s lokalnim računalom. Za fotoaparata to može biti klasa uređaja za obradu slike ili videouređaj snimiti predmete. Morat ćete učiniti neke prebacivanje s klase uređaja da biste pronašli točan Klasa GUID koji funkcionira s lokalno pridružene USB uređaj (u našem slučaju web-kamere).

Bolji način ili druga mogućnost tako da slijedite ove korake da biste pronašli GUID klase za određeni uređaj:

1. Otvorite upravitelj uređaja, pronađite uređaj koji će biti preusmjereni i kliknite ga desnom tipkom miša, a zatim otvorite svojstva.
![Otvorite upravitelj uređaja](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Na kartici **Detalji** odaberite svojstvo **Guid predmete**. Vrijednost koja se pojavljuje je klasa GUID za tu vrstu uređaja.
![Svojstva kamere](./media/remoteapp-usbredir/ra-classguid.png)
3. Koristite vrijednost Guid klase da biste preusmjerili uređaje koji se podudaraju.

Ako, na primjer:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Možete kombinirati više uređaja preusmjeravanja u istom cmdlet. Na primjer: da biste preusmjerili lokalno spremište i USB web-kamere, cmdlet izgleda ovako:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Kada postavite preusmjeravanje po GUID klase svih uređaja koje zadovoljavaju taj GUID klase u određenu zbirku vas preusmjerava. Na primjer, ako postoji više računala na lokalnu mrežu koji imaju isti USB web-kamera, možete pokrenuti jednu cmdlet preusmjeravanje svih web-kamera.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Preusmjeravanje USB uređaj pomoću ID instance uređaja

Ako želite više kontrole preciznije, a želite upravljati preusmjeravanje po uređaj, možete koristiti parametar **USB\InstanceID** preusmjeravanje.

Najteži dio ovu metodu očekujem ID USB uređaj instance. Potreban vam je pristup računalu i određene USB uređaj. Zatim slijedite ove korake:

1. Omogućivanje preusmjeravanje uređaja u sesiji udaljene radne površine, kao što je opisano u [kako koristiti Moje uređaja i resursa u sesiji udaljene radne površine?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Otvorite udaljene radne površine i kliknite **Pokaži mogućnosti**.
3. Kliknite **Spremi kao** za spremanje trenutne postavke veze na datoteku RDP.  
    ![Spremanje postavki kao datoteku RDP](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Odaberite naziv datoteke i mjesto, primjerice "MyConnection.rdp" i "Ovaj PC\Documents", a zatim spremite datoteku.
5. Otvorite MyConnection.rdp datoteku u uređivaču teksta i pronaći ID instance uređaj koji želite preusmjeriti.

Sada pomoću instance ID u sljedeći cmdlet:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Pomozite nam da vam pomoći 
Jeste li znali da osim ocjena u ovom se članku i upućivanje komentare dolje ispod, koje možete unijeti promjene sam članak? Nešto nedostaje? Nešto nije u redu? Nije li moguće napisati nešto što je samo pregledniji? Pomicanje prema gore, a zatim kliknite **Uređivanje na GitHub** da biste unijeli promjene – one će dođite do nas za pregled, a zatim jednom smo odjaviti na njima, vidjet ćete promjene i poboljšanja ovdje.