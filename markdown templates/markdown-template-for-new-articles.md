<properties
   pageTitle="Naslov stranice koja se prikazuje u pregledniku kartica i pretraživanje rezultata"
   description="Članak opis koji će se prikazivati na odredišne stranice i u većini rezultata pretraživanja"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Predložak markdown (članak Naslov 1 ili H1): naslov na vrh članka

Da biste kopirali u markdown iz ovog predloška, kopirajte u članku u vašem lokalnom repo ili kliknite gumb neobrađenog u korisničkom Sučelju GitHub, a zatim kopirajte na markdown.

  ![Zamjenski tekst ostavite prazno. Opišite slike.][8]

Uvod u odlomku: Lorem dolor amet, adipiscing elit. Phasellus interdum nulla risus, lacinia porta nisl imperdiet sed. Mauris dolor mauris, tempus sed lacinia serija nec, euismod koje nisu felis. Nunc semper porta ultrices. Nulla preuređena Maecenas, condimentum vitae automatskom sjesti amet, dignissim aliquet nisi.

## <a name="heading-2-h2"></a>Naslov 2 (H2)

Aenean sjesti amet leo serija nec purus placerat fermentum ac gravida odio. Aenean tellus lectus, faucibus u rhoncus u faucibus sed urna.  volutpat mi id purus ultrices iaculis serija nec koje nisu preuređena. [Tekst veze za vezu izvan azure.microsoft.com](http://weblogs.asp.net/scottgu). Dolor dictum Nullam na aliquam pharetra. Vivamus ac hendrerit mauris [primjer vezu teksta za veze na članak u mapi servisa](../articles/expressroute/expressroute-bandwidth-upgrade.md).

Dobiti dodatne 10 puta promet iz [Google]  [ gog] od iz [Yahoo]  [ yah] ili [MSN] [msn].

> [AZURE.NOTE] Napomena uvučeni tekst.  Riječ "bilješke" dodat će se tijekom publikacije. Lacus pretium EU-a za odjavu. Nullam purus po istočnom vremenu, iaculis sed po istočnom vremenu vnaj, euismod vehicula odio. Curabitur lacinia erat tristique iaculis rutrum, erat sem sodales nisi, nisi turpis condimentum EU-a na purus.

1. Aenean sjesti amet leo serija nec **Purus** placerat fermentum ac gravida odio.

2. Aenean tellus lectus, faucius u **Rhoncus** u faucibus sed urna. Suspendisse volutpat mi id purus ultrices iaculis serija nec koje nisu preuređena.

    ![Zamjenski tekst ostavite prazno. Prikupljanje automobila u trkaći crvene boje.][5]

3. Dolor dictum Nullam na aliquam pharetra. Vivamus ac hendrerit mauris. Sed dolor dui, condimentum postavljanje varius a vehicula pri nisl.

    ![Zamjenski tekst ostavite prazno][6]


Suspendisse volutpat mi id purus ultrices iaculis serija nec koje nisu preuređena. Dolor dictum Nullam na aliquam pharetra. Vivamus ac hendrerit mauris. Otrus informatus: [1 veze na drugu temu azure.microsoft.com dokumentacija](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Naslov (H2)

Lacus pretium EU-a za odjavu. Nullam purus po istočnom vremenu, iaculis sed po istočnom vremenu vnaj, euismod vehicula odio.

1. Curabitur lacinia erat tristique iaculis rutrum, erat sem sodales nisi, nisi turpis condimentum EU-a na purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum ante automatskom primis u faucibus orci luctus postavljanje ultrices posuere cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] Duis sed diam koje nisu <i>nisl molestie</i> pharetra eget na po istočnom vremenu. [Veza 2 na drugu temu azure.microsoft.com dokumentacija](web-sites-custom-domain-name.md)


Quisque commodo eros zmijeniti lectus euismod auctor eget sjesti amet leo. Proin faucibus suscipit tellus dignissim ultrices.

## <a name="heading-2-h2"></a>Naslov 2 (H2)

1. Maecenas sed condimentum nisi. Suspendisse potenti.

  + Fusce
  + Malesuada
  + Sem

2. Nullam u massa eu tellus tempus hendrerit.

    ![Zamjenski tekst ostavite prazno. Primjer 9 kanala videozapis.][7]

3. Quisque felis enim, fermentum Odsutan aliquam serija nec, pellentesque pulvinar magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

Vestibul ante automatskom primis u faucibus orci luctus postavljanje ultrices posuere cubilia Curae; Nullam ultricies, automatskom vitae volutpat hendrerit, purus diam pretium eros, vitae tincidunt nulla lorem sed turpis: [3 veze na drugu temu azure.microsoft.com dokumentacije](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    
