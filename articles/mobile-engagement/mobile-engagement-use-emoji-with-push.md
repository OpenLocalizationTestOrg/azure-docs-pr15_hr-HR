<properties 
    pageTitle="Korištenje Emoji emotikona unutar Azure Mobile radnje" 
    description="Kako koristiti Emoji emotikona unutar automatske obavijesti"     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="use-emoji-emoticon-within-push-notifications"></a>Korištenje Emoji emotikon unutar automatske obavijesti

Možete uključiti Emoji emotikona u automatske obavijesti u nekoliko jednostavnih koraka: 

1. Najprije svih morate pronaći u Emoji koju želite poslati u poruci. Provjerite je li da Emoji birate neće podržavati ciljni uređaj kao uređaj manufactures potrebno neko vrijeme da biste dodali nove odobrene Emojis platforme uređaja. 

2. Na **Windows** – idite na sljedeću [vezu](http://apps.timwhitlock.info/emoji/tables/unicode) i kopirajte ikonu 'Nativni'.

    ![][7] 

3. **Mac** - možete pronaći Emojis u aplikacije rječnika u odjeljku uređivanje -> Emoji i simbole.

    ![][6] 

4. Sada idite na karticu **dosegne** na portalu Azure Mobile radnje. Odaberite vrstu automatske obavijesti (objava, ankete itd.). U ovom primjeru smo odaberite programa automatske objave.

5. Navedite različitih polja obavijesti o dok ne dođete do tekst obavijesti. Ovo je ćete dodati svoj emotikon Emoji. Možete odabrati da biste stavili u naslovu, poruka ili oboje. Morat ćete povucite i ispustite ili kopirate Emoji koje ćete pronaći iznad mjesta. 

    ![][1]

6. Ispunite polja za obavijesti i spremite ga. 

7. Kada pokrenete test ili aktivirali priopćenja prikazat će obavijest o s emotikon kao što je navedeno.   

    ![][3] ![][4] ![][5]

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

